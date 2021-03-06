1、抽象类Analyzer

其主要包含两个接口，用于生成TokenStream：
TokenStream tokenStream(String fieldName, Reader reader);
TokenStream reusableTokenStream(String fieldName, Reader reader) ;


所谓TokenStream，后面我们会讲到，是一个由分词后的Token结果组成的流，能够不断的得到下一个分成的Token。

为了提高性能，使得在同一个线程中无需再生成新的TokenStream对象，老的可以被重用，所以有reusableTokenStream一说。

所以Analyzer中有CloseableThreadLocal< Object > tokenStreams = new CloseableThreadLocal< Object >();成员变量，保存当前线程原来创建过的TokenStream，可用函数setPreviousTokenStream设定，用函数getPreviousTokenStream得到。

在reusableTokenStream函数中，往往用getPreviousTokenStream得到老的TokenStream对象，然后将TokenStream对象reset以下，从而可以从新开始得到Token流。

让我们看一下最简单的一个Analyzer:

 public final class SimpleAnalyzer extends Analyzer {

   @Override

   public TokenStream tokenStream(String fieldName, Reader reader) {

     //返回的是将字符串最小化，并且按照空格分隔的Token

     return new LowerCaseTokenizer(reader);

   }

   @Override

   public TokenStream reusableTokenStream(String fieldName, Reader reader) throws IOException {

     //得到上一次使用的TokenStream，如果没有则生成新的，并且用setPreviousTokenStream放入成员变量，使得下一个可用。

     Tokenizer tokenizer = (Tokenizer) getPreviousTokenStream();

     if (tokenizer == null) {

       tokenizer = new LowerCaseTokenizer(reader);

       setPreviousTokenStream(tokenizer);

     } else

       //如果上一次生成过TokenStream，则reset。

       tokenizer.reset(reader);

     return tokenizer;

   }

 }


  2、TokenStream抽象类

TokenStream主要包含以下几个方法：
boolean incrementToken()用于得到下一个Token。
public void reset() 使得此TokenStrean可以重新开始返回各个分词。 


和原来的TokenStream返回一个Token对象不同，Lucene 3.0的TokenStream已经不返回Token对象了，那么如何保存下一个Token的信息呢。

在Lucene 3.0中，TokenStream是继承于AttributeSource，其包含Map，保存从class到对象的映射，从而可以保存不同类型的对象的值。

在TokenStream中，经常用到的对象是TermAttributeImpl，用来保存Token字符串；PositionIncrementAttributeImpl用来保存位置信息；OffsetAttributeImpl用来保存偏移量信息。

所以当生成TokenStream的时候，往往调用AttributeImpl tokenAtt = (AttributeImpl) addAttribute(TermAttribute.class)将TermAttributeImpl添加到Map中，并保存一个成员变量。

在incrementToken()中，将下一个Token的信息写入当前的tokenAtt，然后使用TermAttributeImpl.term()得到Token的字符串。

  3、几个具体的TokenStream

在索引的时候，添加域的时候，可以指定Analyzer，使其生成TokenStream，也可以直接指定TokenStream：

public Field(String name, TokenStream tokenStream);

下面介绍两个单独使用的TokenStream 3.1、NumericTokenStream

上一节介绍NumericRangeQuery的时候，在生成NumericField的时候，其会使用NumericTokenStream，其incrementToken如下：

 public boolean incrementToken() {

   if (valSize == 0)

     throw new IllegalStateException("call set???Value() before usage");

   if (shift >= valSize)

     return false;

   clearAttributes();

   //虽然NumericTokenStream欲保存数字，然而Lucene的Token只能保存字符串，因而要将数字编码为字符串，然后存入索引。

   final char[] buffer;

   switch (valSize) {

     //首先分配TermBuffer，然后将数字编码为字符串

     case 64:

       buffer = termAtt.resizeTermBuffer(NumericUtils.BUF_SIZE_LONG);

       termAtt.setTermLength(NumericUtils.longToPrefixCoded(value, shift, buffer));

       break;

     case 32:

       buffer = termAtt.resizeTermBuffer(NumericUtils.BUF_SIZE_INT);

       termAtt.setTermLength(NumericUtils.intToPrefixCoded((int) value, shift, buffer));

       break;

     default:

       throw new IllegalArgumentException("valSize must be 32 or 64");

   }

   typeAtt.setType((shift == 0) ? TOKEN_TYPE_FULL_PREC : TOKEN_TYPE_LOWER_PREC);

   posIncrAtt.setPositionIncrement((shift == 0) ? 1 : 0);

   shift += precisionStep;

   return true;

 }


 public static int intToPrefixCoded(final int val, final int shift, final char[] buffer) {

   if (shift>31 || shift<0)

     throw new IllegalArgumentException("Illegal shift value, must be 0..31");

   int nChars = (31-shift)/7 + 1, len = nChars+1;

   buffer[0] = (char)(SHIFT_START_INT + shift);

   int sortableBits = val ^ 0x80000000;

   sortableBits >>>= shift;

   while (nChars>=1) {

     //int按照每七位组成一个utf-8的编码，并且字符串大小比较的顺序同int大小比较的顺序完全相同。

     buffer[nChars--] = (char)(sortableBits & 0x7f);

     sortableBits >>>= 7;

   }

   return len;

 }


  3.2、SingleTokenTokenStream

SingleTokenTokenStream顾名思义就是此TokenStream仅仅包含一个Token，多用于保存一篇文档仅有一个的信息，如id，如time等，这些信息往往被保存在一个特殊的Token(如ID:ID, TIME:TIME)的倒排表的payload中的，这样可以使用跳表来增加访问速度。

所以SingleTokenTokenStream返回的Token则不是id或者time本身，而是特殊的Token，"ID:ID", "TIME:TIME"，而是将id的值或者time的值放入payload中。

 //索引的时候

 int id = 0; //用户自己的文档号

 String tokenstring = "ID";

 byte[] value = idToBytes(); //将id装换为byte数组

 Token token = new Token(tokenstring, 0, tokenstring.length);

 token.setPayload(new Payload(value));

 SingleTokenTokenStream tokenstream = new SingleTokenTokenStream(token);

 Document doc = new Document();

 doc.add(new Field("ID", tokenstream));

 ……

 //当得到Lucene的文档号docid，并不想构造Document对象就得到用户的文档号时

 TermPositions tp = reader.termPositions("ID:ID");

 boolean ret = tp.skipTo(docid);

 tp.nextPosition();

 int payloadlength = tp.getPayloadLength();

 byte[] payloadBuffer = new byte[payloadlength];

 tp.getPayload(payloadBuffer, 0);

 int id = bytesToID(); //将payloadBuffer转换为用户id
4、Tokenizer也是一种TokenStream

 public abstract class Tokenizer extends TokenStream {

   protected Reader input;

   protected Tokenizer(Reader input) {

     this.input = CharReader.get(input);

   }

   public void reset(Reader input) throws IOException {

     this.input = input;

   }

 }


以下重要的Tokenizer如下，我们将一一解析：
CharTokenizer
LetterTokenizer
LowerCaseTokenizer

WhitespaceTokenizer

ChineseTokenizer
CJKTokenizer
EdgeNGramTokenizer
KeywordTokenizer
NGramTokenizer
SentenceTokenizer
StandardTokenizer
4.1、CharTokenizer

CharTokenizer是一个抽象类，用于对字符串进行分词。

在构造函数中，生成了TermAttribute和OffsetAttribute两个属性，说明分词后除了返回分词后的字符外，还要返回offset。

 offsetAtt = addAttribute(OffsetAttribute.class);

 termAtt = addAttribute(TermAttribute.class);


其incrementToken函数如下：

 public final boolean incrementToken() throws IOException {

   clearAttributes();

   int length = 0;

   int start = bufferIndex;

   char[] buffer = termAtt.termBuffer();

   while (true) {

     //不断读取reader中的字符到buffer中

     if (bufferIndex >= dataLen) {

       offset += dataLen;

       dataLen = input.read(ioBuffer);

       if (dataLen == -1) {

         dataLen = 0;

         if (length > 0)

           break;

         else

           return false;

       }

       bufferIndex = 0;

     }

     //然后逐一遍历buffer中的字符

     final char c = ioBuffer[bufferIndex++];

     //如果是一个token字符，则normalize后接着取下一个字符，否则当前token结束。  

     if (isTokenChar(c)) {

       if (length == 0)

         start = offset + bufferIndex - 1;

       else if (length == buffer.length)

         buffer = termAtt.resizeTermBuffer(1+length);

       buffer[length++] = normalize(c);

       if (length == MAX_WORD_LEN)

         break;

     } else if (length > 0)

       break;

   }

   termAtt.setTermLength(length);

   offsetAtt.setOffset(correctOffset(start), correctOffset(start+length));

   return true;

 }


CharTokenizer是一个抽象类，其isTokenChar函数和normalize函数由子类实现。

其子类WhitespaceTokenizer实现了isTokenChar函数：

 //当遇到空格的时候，当前token结束

 protected boolean isTokenChar(char c) {

   return !Character.isWhitespace(c);

 }


其子类LetterTokenizer如下实现isTokenChar函数：

 protected boolean isTokenChar(char c) {

   return Character.isLetter(c);

 }


LetterTokenizer的子类LowerCaseTokenizer实现了normalize函数，将字符串转换为小写：

 protected char normalize(char c) {

   return Character.toLowerCase(c);

 }


  4.2、ChineseTokenizer

其在初始化的时候，添加TermAttribute和OffsetAttribute。

其incrementToken实现如下：

 public boolean incrementToken() throws IOException {

     clearAttributes();

     length = 0;

     start = offset;

     while (true) {

         final char c;

         offset++;

         if (bufferIndex >= dataLen) {

             dataLen = input.read(ioBuffer);

             bufferIndex = 0;

         }

         if (dataLen == -1) return flush();

         else

             c = ioBuffer[bufferIndex++];

         switch(Character.getType(c)) {

         //如果是英文下小写字母或数字的时候，则属于同一个Token，push到buffer中  

         case Character.DECIMAL_DIGIT_NUMBER:

         case Character.LOWERCASE_LETTER:

         case Character.UPPERCASE_LETTER:

             push(c);

             if (length == MAX_WORD_LEN) return flush();

             break;

         //中文属于OTHER_LETTER，当出现中文字符的时候，则上一个Token结束，并将当前字符push到buffer中

         case Character.OTHER_LETTER:

             if (length>0) {

                 bufferIndex--;

                 offset--;

                 return flush();

             }

             push(c);

             return flush();

         default:

             if (length>0) return flush();

             break;

         }

     }

 }


  4.3、KeywordTokenizer

KeywordTokenizer是将整个字符作为一个Token返回的。

其incrementToken函数如下：

 public final boolean incrementToken() throws IOException {

   if (!done) {

     clearAttributes();

     done = true;

     int upto = 0;

     char[] buffer = termAtt.termBuffer();

     //将字符串全部读入buffer，然后返回。

     while (true) {

       final int length = input.read(buffer, upto, buffer.length-upto);

       if (length == -1) break;

       upto += length;

       if (upto == buffer.length)

         buffer = termAtt.resizeTermBuffer(1+buffer.length);

     }

     termAtt.setTermLength(upto);

     finalOffset = correctOffset(upto);

     offsetAtt.setOffset(correctOffset(0), finalOffset);

     return true;

   }

   return false;

 }


  4.4、CJKTokenizer

其incrementToken函数如下：

  

 public boolean incrementToken() throws IOException {

     clearAttributes();

     while(true) {

       int length = 0;

       int start = offset;

       while (true) {

         //得到当前的字符，及其所属的Unicode块

         char c;

         Character.UnicodeBlock ub;

         offset++;

         if (bufferIndex >= dataLen) {

             dataLen = input.read(ioBuffer);

             bufferIndex = 0;

         }

         if (dataLen == -1) {

             if (length > 0) {

                 if (preIsTokened == true) {

                     length = 0;

                     preIsTokened = false;

                 }

                 break;

             } else {

                 return false;

             }

         } else {

             c = ioBuffer[bufferIndex++];

             ub = Character.UnicodeBlock.of(c);

         }

         //如果当前字符输入ASCII码

         if ((ub == Character.UnicodeBlock.BASIC_LATIN) || (ub == Character.UnicodeBlock.HALFWIDTH_AND_FULLWIDTH_FORMS)) {

             if (ub == Character.UnicodeBlock.HALFWIDTH_AND_FULLWIDTH_FORMS) {

               int i = (int) c;

               if (i >= 65281 && i <= 65374) {

                 //将半型及全型形式Unicode转变为普通的ASCII码

                 i = i - 65248;

                 c = (char) i;

               }

             }

             //如果当前字符是字符或者"_" "+" "#"

             if (Character.isLetterOrDigit(c) || ((c == '_') || (c == '+') || (c == '#'))) {

                 if (length == 0) {

                     start = offset - 1;

                 } else if (tokenType == DOUBLE_TOKEN_TYPE) {

                     offset--;

                     bufferIndex--;

                     if (preIsTokened == true) {

                         length = 0;

                         preIsTokened = false;

                         break;

                     } else {

                         break;

                     }

                 }

                 //将当前字符放入buffer

                 buffer[length++] = Character.toLowerCase(c);

                 tokenType = SINGLE_TOKEN_TYPE;

                 if (length == MAX_WORD_LEN) {

                     break;

                 }

             } else if (length > 0) {

                 if (preIsTokened == true) {

                     length = 0;

                     preIsTokened = false;

                 } else {

                     break;

                 }

             }

         } else {

             //如果非ASCII字符

             if (Character.isLetter(c)) {

                 if (length == 0) {

                     start = offset - 1;

                     buffer[length++] = c;

                     tokenType = DOUBLE_TOKEN_TYPE;

                 } else {

                   if (tokenType == SINGLE_TOKEN_TYPE) {

                         offset--;

                         bufferIndex--;

                         break;

                     } else {

                         //非ASCII码字符，两个字符作为一个Token

                        //(如"中华人民共和国"分词为"中华"，"华人"，"人民"，"民共"，"共和"，"和国")

                         buffer[length++] = c;

                         tokenType = DOUBLE_TOKEN_TYPE;

                         if (length == 2) {

                             offset--;

                             bufferIndex--;

                             preIsTokened = true;

                             break;

                         }

                     }

                 }

             } else if (length > 0) {

                 if (preIsTokened == true) {

                     length = 0;

                     preIsTokened = false;

                 } else {

                     break;

                 }

             }

         }

     }

     if (length > 0) {

       termAtt.setTermBuffer(buffer, 0, length);

       offsetAtt.setOffset(correctOffset(start), correctOffset(start+length));

       typeAtt.setType(TOKEN_TYPE_NAMES[tokenType]);

       return true;

     } else if (dataLen == -1) {

       return false;

     }

   }

 }
4.5、SentenceTokenizer

其是按照如下的标点来拆分句子："。，！？；,!?;"

让我们来看下面的例子：

 String s = "据纽约时报周三报道称，苹果已经超过微软成为美国最有价值的  科技公司。这是一个不容忽视的转折点。";

 StringReader sr = new StringReader(s);

 SentenceTokenizer tokenizer = new SentenceTokenizer(sr);

 boolean hasnext = tokenizer.incrementToken();

 while(hasnext){

   TermAttribute ta = tokenizer.getAttribute(TermAttribute.class);

   System.out.println(ta.term());

   hasnext = tokenizer.incrementToken();

 }


 结果为：

 据纽约时报周三报道称，
 苹果已经超过微软成为美国最有价值的
 科技公司。
 这是一个不容忽视的转折点。


其incrementToken函数如下：

 public boolean incrementToken() throws IOException {

   clearAttributes();

   buffer.setLength(0);

   int ci;

   char ch, pch;

   boolean atBegin = true;

   tokenStart = tokenEnd;

   ci = input.read();

   ch = (char) ci;

   while (true) {

     if (ci == -1) {

       break;

     } else if (PUNCTION.indexOf(ch) != -1) {

       //出现标点符号，当前句子结束，返回当前Token

       buffer.append(ch);

       tokenEnd++;

       break;

     } else if (atBegin && Utility.SPACES.indexOf(ch) != -1) {

       tokenStart++;

       tokenEnd++;

       ci = input.read();

       ch = (char) ci;

     } else {

       buffer.append(ch);

       atBegin = false;

       tokenEnd++;

       pch = ch;

       ci = input.read();

       ch = (char) ci;

       //当连续出现两个空格，或者\r\n的时候，则当前句子结束，返回当前Token

       if (Utility.SPACES.indexOf(ch) != -1

           && Utility.SPACES.indexOf(pch) != -1) {

         tokenEnd++;

         break;

       }

     }

   }

   if (buffer.length() == 0)

     return false;

   else {

     termAtt.setTermBuffer(buffer.toString());

     offsetAtt.setOffset(correctOffset(tokenStart), correctOffset(tokenEnd));

     typeAtt.setType("sentence");

     return true;

   }

 }


  5、TokenFilter也是一种TokenStream

来对Tokenizer后的Token作过滤，其使用的是装饰者模式。

 public abstract class TokenFilter extends TokenStream {

   protected final TokenStream input;

   protected TokenFilter(TokenStream input) {

     super(input);

     this.input = input;

   }

 }


  5.1、ChineseFilter

其incrementToken函数如下：

 public boolean incrementToken() throws IOException {

     while (input.incrementToken()) {

         char text[] = termAtt.termBuffer();

         int termLength = termAtt.termLength();

        //如果不被停词表过滤掉

         if (!stopTable.contains(text, 0, termLength)) {

             switch (Character.getType(text[0])) {

             //如果是英文且长度超过一，则算一个Token，否则不算一个Token

             case Character.LOWERCASE_LETTER:

             case Character.UPPERCASE_LETTER:

                 if (termLength>1) {

                     return true;

                 }

                 break;

            //如果是中文则算一个Token

             case Character.OTHER_LETTER:

                 return true;

             }

         }

     }

     return false;

 }


举例：

 String s = "Javaeye: IT外企那点儿事。1.外企也就那么会儿事。";

 StringReader sr = new StringReader(s);

 ChineseTokenizer ct = new ChineseTokenizer(sr);

 ChineseFilter filter = new ChineseFilter(ct);

 boolean hasnext = filter.incrementToken();

 while(hasnext){

   TermAttribute ta = filter.getAttribute(TermAttribute.class);

   System.out.println(ta.term());

   hasnext = filter.incrementToken();

 }


 结果为：

 javaeye
 外
 企
 那
 点
 儿
 事
 外
 企
 也
 就
 那
 么
 会
 儿
 事


  5.2、LengthFilter

其incrementToken函数如下：

 public final boolean incrementToken() throws IOException {

   while (input.incrementToken()) {

     int len = termAtt.termLength();

     //当当前字符串的长度在指定范围内的时候则返回。

     if (len >= min && len <= max) {

         return true;

     }

   }

   return false;

 }


举例如下：

 String s = "a it has this there string english analyzer";

 StringReader sr = new StringReader(s);

 WhitespaceTokenizer wt = new WhitespaceTokenizer(sr);

 LengthFilter filter = new LengthFilter(wt, 4, 7);

 boolean hasnext = filter.incrementToken();

 while(hasnext){

   TermAttribute ta = filter.getAttribute(TermAttribute.class);

   System.out.println(ta.term());

   hasnext = filter.incrementToken();

 }


 结果如下：

 this
 there
 string
 english


  5.3、LowerCaseFilter

其incrementToken函数如下：

 public final boolean incrementToken() throws IOException {

   if (input.incrementToken()) {

     final char[] buffer = termAtt.termBuffer();

     final int length = termAtt.termLength();

     for(int i=0;i<length;i++)

       //转小写

       buffer[i] = Character.toLowerCase(buffer[i]);

     return true;

   } else

     return false;

 }


  5.4、NumericPayloadTokenFilter

 public final boolean incrementToken() throws IOException {

   if (input.incrementToken()) {

     if (typeAtt.type().equals(typeMatch))

       //设置payload

       payloadAtt.setPayload(thePayload);

     return true;

   } else {

     return false;

   }

 }


  5.5、PorterStemFilter

其成员变量PorterStemmer stemmer，其实现著名的stemming算法是The Porter Stemming Algorithm，其主页为 http://tartarus.org/~martin/PorterStemmer/ ，也可查看其论文 http://tartarus.org/~martin/PorterStemmer/def.txt 。

通过以下网页可以进行简单的测试： Porter's Stemming Algorithm Online [ http://facweb.cs.depaul.edu/mobasher/classes/csc575/porter.html ]

cars –> car

driving –> drive

tokenization –> token

其incrementToken函数如下：

 public final boolean incrementToken() throws IOException {

   if (!input.incrementToken())

     return false;

   if (stemmer.stem(termAtt.termBuffer(), 0, termAtt.termLength()))

     termAtt.setTermBuffer(stemmer.getResultBuffer(), 0, stemmer.getResultLength());

   return true;

 }


举例：

 String s = "Tokenization is the process of breaking a stream of text up into meaningful elements called tokens.";

 StringReader sr = new StringReader(s);

 LowerCaseTokenizer lt = new LowerCaseTokenizer(sr);

 PorterStemFilter filter = new PorterStemFilter(lt);

 boolean hasnext = filter.incrementToken();

 while(hasnext){

   TermAttribute ta = filter.getAttribute(TermAttribute.class);

   System.out.println(ta.term());

   hasnext = filter.incrementToken();

 }


 结果为：

 token
 is
 the
 process
 of
 break
 a
 stream
 of
 text
 up
 into
 meaning
 element
 call
 token


  5.6、ReverseStringFilter

 public boolean incrementToken() throws IOException {

   if (input.incrementToken()) {

     int len = termAtt.termLength();

     if (marker != NOMARKER) {

       len++;

       termAtt.resizeTermBuffer(len);

       termAtt.termBuffer()[len - 1] = marker;

     }

     //将token反转

     reverse( termAtt.termBuffer(), len );

     termAtt.setTermLength(len);

     return true;

   } else {

     return false;

   }

 }


 public static void reverse( char[] buffer, int start, int len ){

   if( len <= 1 ) return;

   int num = len>>1;

   for( int i = start; i < ( start + num ); i++ ){

     char c = buffer[i];

     buffer[i] = buffer[start * 2 + len - i - 1];

     buffer[start * 2 + len - i - 1] = c;

   }

 }


举例：

 String s = "Tokenization is the process of breaking a stream of text up into meaningful elements called tokens.";

 StringReader sr = new StringReader(s);

 LowerCaseTokenizer lt = new LowerCaseTokenizer(sr);

 ReverseStringFilter filter = new ReverseStringFilter(lt);

 boolean hasnext = filter.incrementToken();

 while(hasnext){

   TermAttribute ta = filter.getAttribute(TermAttribute.class);

   System.out.println(ta.term());

   hasnext = filter.incrementToken();

 }


 结果为：

 noitazinekot
 si
 eht
 ssecorp
 fo
 gnikaerb
 a
 maerts
 fo
 txet
 pu
 otni
 lufgninaem
 stnemele
 dellac
 snekot


  5.7、SnowballFilter

其包含成员变量SnowballProgram stemmer，其是一个抽象类，其子类有EnglishStemmer和PorterStemmer等。

 public final boolean incrementToken() throws IOException {

   if (input.incrementToken()) {

     String originalTerm = termAtt.term();

     stemmer.setCurrent(originalTerm);

     stemmer.stem();

     String finalTerm = stemmer.getCurrent();

     if (!originalTerm.equals(finalTerm))

       termAtt.setTermBuffer(finalTerm);

     return true;

   } else {

     return false;

   }

 }


举例：

 String s = "Tokenization is the process of breaking a stream of text up into meaningful elements called tokens.";

 StringReader sr = new StringReader(s);

 LowerCaseTokenizer lt = new LowerCaseTokenizer(sr);

 SnowballFilter filter = new SnowballFilter(lt, new EnglishStemmer());

 boolean hasnext = filter.incrementToken();

 while(hasnext){

   TermAttribute ta = filter.getAttribute(TermAttribute.class);

   System.out.println(ta.term());

   hasnext = filter.incrementToken();

 }


 结果如下：

 token
 is
 the
 process
 of
 break
 a
 stream
 of
 text
 up
 into
 meaning
 element
 call
 token


  5.8、TeeSinkTokenFilter

TeeSinkTokenFilter可以使得已经分好词的Token全部或者部分的被保存下来，用于生成另一个TokenStream可以保存在其他的域中。

我们可用如下的语句生成一个TeeSinkTokenFilter：

 TeeSinkTokenFilter source = new TeeSinkTokenFilter(new WhitespaceTokenizer(reader));


然后使用函数newSinkTokenStream()或者newSinkTokenStream(SinkFilter filter)生成一个SinkTokenStream：

 TeeSinkTokenFilter.SinkTokenStream sink = source.newSinkTokenStream();


其中在newSinkTokenStream(SinkFilter filter)函数中，将新生成的SinkTokenStream保存在TeeSinkTokenFilter的成员变量sinks中。

在TeeSinkTokenFilter的incrementToken函数中：

 public boolean incrementToken() throws IOException {

   if (input.incrementToken()) {

     //对于每一个Token，依次遍历成员变量sinks

     AttributeSource.State state = null;

     for (WeakReference<SinkTokenStream> ref : sinks) {

       //对于每一个SinkTokenStream，首先调用函数accept看是否接受，如果接受则将此Token也加入此SinkTokenStream。

       final SinkTokenStream sink = ref.get();

       if (sink != null) {

         if (sink.accept(this)) {

           if (state == null) {

             state = this.captureState();

           }

           sink.addState(state);

         }

       }

     }

     return true;

   }

   return false;

 }


SinkTokenStream.accept调用SinkFilter.accept，对于默认的ACCEPT_ALL_FILTER则接受所有的Token：

 private static final SinkFilter ACCEPT_ALL_FILTER = new SinkFilter() {

   @Override

   public boolean accept(AttributeSource source) {

     return true;

   }

 };


这样SinkTokenStream就能够保存下所有WhitespaceTokenizer分好的Token。

当我们使用比较复杂的分成系统的时候，分词一篇文章往往需要耗费比较长的时间，当分好的词需要再次使用的时候，再分一次词实在太浪费了，于是可以用上述的例子，将分好的词保存在一个TokenStream里面就可以了。

如下面的例子：



 String s = "this is a book";

 StringReader reader = new StringReader(s);

 TeeSinkTokenFilter source = new TeeSinkTokenFilter(new WhitespaceTokenizer(reader));

 TeeSinkTokenFilter.SinkTokenStream sink = source.newSinkTokenStream();

 boolean hasnext = source.incrementToken();

 while(hasnext){

   TermAttribute ta = source.getAttribute(TermAttribute.class);

   System.out.println(ta.term());

   hasnext = source.incrementToken();

 }

 System.out.println("---------------------------------------------");

 hasnext = sink.incrementToken();

 while(hasnext){

   TermAttribute ta = sink.getAttribute(TermAttribute.class);

   System.out.println(ta.term());

   hasnext = sink.incrementToken();

 }


 结果为：

 this
 is
 a
 book
 ---------------------------------------------
 this
 is
 a
 book


当然有时候我们想在分好词的一系列Token中，抽取我们想要的一些实体，保存下来。

如下面的例子：

   String s = "Japan will always balance its national interests between China and America.";

   StringReader reader = new StringReader(s);

   TeeSinkTokenFilter source = new TeeSinkTokenFilter(new LowerCaseTokenizer(reader));

   //一个集合，保存所有的国家名称

   final HashSet<String> countryset = new HashSet<String>();

   countryset.add("japan");

   countryset.add("china");

   countryset.add("america");

   countryset.add("korea");

   SinkFilter countryfilter = new SinkFilter() {

     @Override

     public boolean accept(AttributeSource source) {

       TermAttribute ta = source.getAttribute(TermAttribute.class);

       //如果在国家名称列表中，则保留

       if(countryset.contains(ta.term())){

         return true;

       }

       return false;

     }

   };

   TeeSinkTokenFilter.SinkTokenStream sink = source.newSinkTokenStream(countryfilter);

   //由LowerCaseTokenizer对语句进行分词，并把其中的国家名称保存在SinkTokenStream中

   boolean hasnext = source.incrementToken();

   while(hasnext){

     TermAttribute ta = source.getAttribute(TermAttribute.class);

     System.out.println(ta.term());

     hasnext = source.incrementToken();

   }

   System.out.println("---------------------------------------------");

   hasnext = sink.incrementToken();

   while(hasnext){

     TermAttribute ta = sink.getAttribute(TermAttribute.class);

     System.out.println(ta.term());

     hasnext = sink.incrementToken();

   }

 }


 结果为：

 japan
 will
 always
 balance
 its
 national
 interests
 between
 china
 and
 america
 ---------------------------------------------
 japan
 china
 america


  6、不同的Analyzer就是组合不同的Tokenizer和TokenFilter得到最后的TokenStream

  6.1、ChineseAnalyzer

 public final TokenStream tokenStream(String fieldName, Reader reader) {

     //按字分词，并过滤停词，标点，英文

     TokenStream result = new ChineseTokenizer(reader);

     result = new ChineseFilter(result);

     return result;

 }


举例："This year, president Hu 科学发展观" 被分词为 "year","president","hu","科","学","发","展","观" 6.2、CJKAnalyzer

 public final TokenStream tokenStream(String fieldName, Reader reader) {

     //每两个字组成一个词，并去除停词

     return new StopFilter(StopFilter.getEnablePositionIncrementsVersionDefault(matchVersion), new CJKTokenizer(reader), stopTable);

 }


举例："This year, president Hu 科学发展观" 被分词为"year","president","hu","科学","学发","发展","展观"。 6.3、PorterStemAnalyzer

 public TokenStream tokenStream(String fieldName, Reader reader) {

     //将转为小写的token，利用porter算法进行stemming

     return new PorterStemFilter(new LowerCaseTokenizer(reader));

 }


  6.4、SmartChineseAnalyzer

 public TokenStream tokenStream(String fieldName, Reader reader) {

     //先分句子

     TokenStream result = new SentenceTokenizer(reader);

     //句子中分词组

     result = new WordTokenFilter(result);

     //用porter算法进行stemming

     result = new PorterStemFilter(result);

     //去停词

     if (!stopWords.isEmpty()) {

       result = new StopFilter(StopFilter.getEnablePositionIncrementsVersionDefault(matchVersion), result, stopWords, false);

     }

     return result;

 }


  6.5、SnowballAnalyzer

 public TokenStream tokenStream(String fieldName, Reader reader) {

     //使用标准的分词器

     TokenStream result = new StandardTokenizer(matchVersion, reader);

    //标准的过滤器  

     result = new StandardFilter(result);

    //转换为小写  

     result = new LowerCaseFilter(result);

     //去停词

     if (stopSet != null)

       result = new StopFilter(StopFilter.getEnablePositionIncrementsVersionDefault(matchVersion), result, stopSet);

     //根据设定的stemmer进行stemming

     result = new SnowballFilter(result, name);

     return result;

 }


  7、Lucene的标准分词器 7.1、StandardTokenizerImpl.jflex

和QueryParser类似，标准分词器也需要词法分析，在原来的版本中，也是用javacc，当前的版本中，使用的是jflex。

jflex也是一个词法及语法分析器的生成器，它主要包括三部分，由%%分隔：
用户代码部分：多为package或者import
选项及词法声明
语法规则声明


用于生成标准分词器的flex文件尾StandardTokenizerImpl.jflex，如下:



 import org.apache.lucene.analysis.Token;

 import org.apache.lucene.analysis.tokenattributes.TermAttribute;

 %% //以上是用户代码部分，以下是选项及词法声明

 %class StandardTokenizerImpl //类名

 %unicode

 %integer //下面函数的返回值

 %function getNextToken //进行词法及语法分析的函数

 %pack

 %char

 %{ //此之间的代码之间拷贝到生成的java文件中

 public static final int ALPHANUM          = StandardTokenizer.ALPHANUM;

 public static final int APOSTROPHE        = StandardTokenizer.APOSTROPHE;

 public static final int ACRONYM           = StandardTokenizer.ACRONYM;

 public static final int COMPANY           = StandardTokenizer.COMPANY;

 public static final int EMAIL             = StandardTokenizer.EMAIL;

 public static final int HOST              = StandardTokenizer.HOST;

 public static final int NUM               = StandardTokenizer.NUM;

 public static final int CJ                = StandardTokenizer.CJ;

 public static final int ACRONYM_DEP       = StandardTokenizer.ACRONYM_DEP;

 public static final String [] TOKEN_TYPES = StandardTokenizer.TOKEN_TYPES;

 public final int yychar()

 {

     return yychar;

 }

 final void getText(Token t) {

   t.setTermBuffer(zzBuffer, zzStartRead, zzMarkedPos-zzStartRead);

 }

 final void getText(TermAttribute t) {

   t.setTermBuffer(zzBuffer, zzStartRead, zzMarkedPos-zzStartRead);

 }

 %}

 THAI       = [\u0E00-\u0E59]

 //一系列字母和数字的组合

 ALPHANUM   = ({LETTER}|{THAI}|[:digit:])+

 //省略符号，如you're

 APOSTROPHE =  {ALPHA} ("'" {ALPHA})+

 //缩写，如U.S.A.

 ACRONYM    =  {LETTER} "." ({LETTER} ".")+

 ACRONYM_DEP    = {ALPHANUM} "." ({ALPHANUM} ".")+

 // 公司名称如AT&T， Excite@Home .

 COMPANY    =  {ALPHA} ("&"|"@") {ALPHA}

 // 邮箱地址

 EMAIL =  {ALPHANUM} (("."|"-"|"_") {ALPHANUM})* "@" {ALPHANUM} (("."|"-") {ALPHANUM})+

 // 主机名

 HOST  =  {ALPHANUM} ((".") {ALPHANUM})+

 NUM  = ({ALPHANUM} {P} {HAS_DIGIT}

            | {HAS_DIGIT} {P} {ALPHANUM}

            | {ALPHANUM} ({P} {HAS_DIGIT} {P} {ALPHANUM})+

            | {HAS_DIGIT} ({P} {ALPHANUM} {P} {HAS_DIGIT})+

            | {ALPHANUM} {P} {HAS_DIGIT} ({P} {ALPHANUM} {P} {HAS_DIGIT})+

            | {HAS_DIGIT} {P} {ALPHANUM} ({P} {HAS_DIGIT} {P} {ALPHANUM})+)

 //标点

 P  = ("_"|"-"|"/"|"."|",")

 //至少包含一个数字的字符串

 HAS_DIGIT  = ({LETTER}|[:digit:])* [:digit:] ({LETTER}|[:digit:])*

 ALPHA  = ({LETTER})+

 //所谓字符，即出去所有的非字符的ASCII及中日文。

 LETTER = !(![:letter:]|{CJ})

 //中文或者日文

 CJ  = [\u3100-\u312f\u3040-\u309F\u30A0-\u30FF\u31F0-\u31FF\u3300-\u337f\u3400-\u4dbf\u4e00-\u9fff\uf900-\ufaff\uff65-\uff9f]

 //空格

 WHITESPACE = \r\n | [ \r\n\t\f]

 %% //以下是语法规则部分，由于是分词器，因而不需要进行语法分析，则全部原样返回

 {ALPHANUM}                                                     { return ALPHANUM; }

 {APOSTROPHE}                                                   { return APOSTROPHE; }

 {ACRONYM}                                                      { return ACRONYM; }

 {COMPANY}                                                      { return COMPANY; }

 {EMAIL}                                                        { return EMAIL; }

 {HOST}                                                         { return HOST; }

 {NUM}                                                          { return NUM; }

 {CJ}                                                           { return CJ; }

 {ACRONYM_DEP}                                                  { return ACRONYM_DEP; }

  


下面我们看下面的例子，来说明StandardTokenizerImpl的功能：

 String s = "I'm Juexian, my email is forfuture1978@gmail.com. My ip address is 192.168.0.1, AT&T and I.B.M are all great companies.";

 StringReader reader = new StringReader(s);

 StandardTokenizerImpl impl = new StandardTokenizerImpl(reader);

 while(impl.getNextToken() != StandardTokenizerImpl.YYEOF){

     TermAttributeImpl ta = new TermAttributeImpl();

     impl.getText(ta);

     System.out.println(ta.term());

 }


 结果为：

 I'm
 Juexian
 my
 email
 is
 forfuture1978@gmail.com
 My
 ip
 address
 is
 192.168.0.1
 AT&T
 and
 I.B.M
 are
 all
 great
 companies


  7.2、StandardTokenizer

其有一个成员变量StandardTokenizerImpl scanner;

其incrementToken函数如下：



 public final boolean incrementToken() throws IOException {

   clearAttributes();

   int posIncr = 1;

   while(true) {

     //用词法分析器得到下一个Token以及Token的类型

     int tokenType = scanner.getNextToken();

     if (tokenType == StandardTokenizerImpl.YYEOF) {

       return false;

     }

     if (scanner.yylength() <= maxTokenLength) {

       posIncrAtt.setPositionIncrement(posIncr);

       //得到Token文本

       scanner.getText(termAtt);

       final int start = scanner.yychar();

       offsetAtt.setOffset(correctOffset(start), correctOffset(start+termAtt.termLength()));

       //设置类型

       typeAtt.setType(StandardTokenizerImpl.TOKEN_TYPES[tokenType]);

       return true;

     } else

       posIncr++;

   }

 }


  7.3、StandardFilter

其incrementToken函数如下：

 public final boolean incrementToken() throws java.io.IOException {

   if (!input.incrementToken()) {

     return false;

   }

   char[] buffer = termAtt.termBuffer();

   final int bufferLength = termAtt.termLength();

   final String type = typeAtt.type();

   //如果是省略符号，如He's，则去掉's

   if (type == APOSTROPHE_TYPE && bufferLength >= 2 &&

       buffer[bufferLength-2] == '\'' && (buffer[bufferLength-1] == 's' || buffer[bufferLength-1] == 'S')) {

     termAtt.setTermLength(bufferLength - 2);

   } else if (type == ACRONYM_TYPE) {

    //如果是缩略语I.B.M.，则去掉.

     int upto = 0;

     for(int i=0;i<bufferLength;i++) {

       char c = buffer[i];

       if (c != '.')

         buffer[upto++] = c;

     }

     termAtt.setTermLength(upto);

   }

   return true;

 }


  7.4、StandardAnalyzer

 public TokenStream tokenStream(String fieldName, Reader reader) {

     //用词法分析器分词

     StandardTokenizer tokenStream = new StandardTokenizer(matchVersion, reader);

     tokenStream.setMaxTokenLength(maxTokenLength);

     //用标准过滤器过滤  

     TokenStream result = new StandardFilter(tokenStream);

     //转换为小写

     result = new LowerCaseFilter(result);

     //去停词  

     result = new StopFilter(enableStopPositionIncrements, result, stopSet);

     return result;

 }


举例如下：

 String s = "He's Juexian, His email is forfuture1978@gmail.com. He's an ip address 192.168.0.1, AT&T and I.B.M. are all great companies.";

 StringReader reader = new StringReader(s);

 StandardAnalyzer analyzer = new StandardAnalyzer(Version.LUCENE_CURRENT);

 TokenStream ts = analyzer.tokenStream("field", reader);

 boolean hasnext = ts.incrementToken();

 while(hasnext){

   TermAttribute ta = ts.getAttribute(TermAttribute.class);

   System.out.println(ta.term());

   hasnext = ts.incrementToken();

 }


 结果为：

 he
 juexian
 his
 email
 forfuture1978@gmail.com
 he
 ip
 address
 192.168.0.1
 at&t
 ibm
 all
 great
 companies


  8、不同的域使用不同的分词器 8.1、PerFieldAnalyzerWrapper

有时候，我们想不同的域使用不同的分词器，则可以用PerFieldAnalyzerWrapper进行封装。

其有两个成员函数：
Analyzer defaultAnalyzer：即当域没有指定分词器的时候使用此分词器
Map<String,Analyzer> analyzerMap = new HashMap<String,Analyzer>()：一个从域名到分词器的映射，将根据域名使用相应的分词器。


其TokenStream函数如下：

 public TokenStream tokenStream(String fieldName, Reader reader) {

   Analyzer analyzer = analyzerMap.get(fieldName);

   if (analyzer == null) {

     analyzer = defaultAnalyzer;

   }

   return analyzer.tokenStream(fieldName, reader);

 }


举例说明：

 String s = "Hello World";
 PerFieldAnalyzerWrapper analyzer = new PerFieldAnalyzerWrapper(new SimpleAnalyzer());
 analyzer.addAnalyzer("f1", new KeywordAnalyzer());
 analyzer.addAnalyzer("f2", new WhitespaceAnalyzer());

 TokenStream ts = analyzer.reusableTokenStream("f1", new StringReader(s));
 boolean hasnext = ts.incrementToken();
 while(hasnext){
   TermAttribute ta = ts.getAttribute(TermAttribute.class);
   System.out.println(ta.term());
   hasnext = ts.incrementToken();
 }

 System.out.println("---------------------------------------------");

 ts = analyzer.reusableTokenStream("f2", new StringReader(s));
 hasnext = ts.incrementToken();
 while(hasnext){
   TermAttribute ta = ts.getAttribute(TermAttribute.class);
   System.out.println(ta.term());
   hasnext = ts.incrementToken();
 }

 System.out.println("---------------------------------------------");

 ts = analyzer.reusableTokenStream("none", new StringReader(s));
 hasnext = ts.incrementToken();
 while(hasnext){
   TermAttribute ta = ts.getAttribute(TermAttribute.class);
   System.out.println(ta.term());
   hasnext = ts.incrementToken();
 }


 结果为：

 Hello World
 ---------------------------------------------
 Hello
 World
 ---------------------------------------------
 hello
 world
