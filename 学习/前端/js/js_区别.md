##浏览器编码的函数 escape(),encodeURI(),encodeURIComponent()
```
1、escape()
escape()是js编码函数中最古老的一个。虽然这个函数现在已经不提倡使用了，但是由于历史原因，很多地方还在使用它，所以有必要先从它讲起。
实际上，escape()不能直接用于URL编码，它的真正作用是返回一个字符的Unicode编码值。比如“春节”的返回结果是%u6625%u8282，也就是说在Unicode字符集中，“春”是第6625个（十六进制）字符，“节”是第8282个（十六进制）字符。


例如：
javascript:escape("春节");
//输出 "%u6625%u8282"


javascript:escape("hello word");
//输出 "hello%20word"
还有两个地方需要注意。


首先，无论网页的原始编码是什么，一旦被Javascript编码，就都变为unicode字符。也就是说，Javascipt函数的输入和输出，默认都是Unicode字符。这一点对下面两个函数也适用。


 javascript:escape("\u6625\u8282");
//输出 "%u6625%u8282"


 javascript:unescape("%u6625%u8282");
//输出 "春节"


 javascript:unescape("\u6625\u8282");
//输出 "春节"
其次，escape()不对“+”编码。但是我们知道，网页在提交表单的时候，如果有空格，则会被转化为+字符。服务器处理数据的时候，会把+号处理成空格。所以，使用的时候要小心。


2、encodeURI()

它着眼于对整个URL进行编码，因此除了常见的符号以外，对其他一些在网址中有特殊含义的符号“; / ? : @ & = + $ , #”，也不进行编码。编码后，它输出符号的utf-8形式，并且在每个字节前加上%。


enter image description here
它对应的解码函数是decodeURI()。


enter image description here
需要注意的是，它不对单引号'编码。


3、encodeURIComponent()


最后一个Javascript编码函数是encodeURIComponent()。与encodeURI()的区别是，它用于对URL的组成部分进行个别编码，而不用于对整个URL进行编码。


因此，“; / ? : @ & = + $ , #”，这些在encodeURI()中不被编码的符号，在encodeURIComponent()中统统会被编码。至于具体的编码方法，两者是一样。


enter image description here
它对应的解码函数是decodeURIComponent()。


encodeURIComponent()相比encodeURI()要更加彻底。


例如：


<html>
<body>
<script type="text/javascript">
var test1="http://www.haorooms.com/My first/";
var nn=encodeURI(test1);
var now=decodeURI(test1);

var test1="http://www.haorooms.com/My first/";
var bb=encodeURIComponent(test1);
var nnow=decodeURIComponent(bb);
</script>
</body>
</html>
输出结果是：
http://www.haorooms.com/My%20first/
http://www.haorooms.com/My first/
http%3A%2F%2Fwww.haorooms.com%2FMy%20first%2F
http://www.haorooms.com/My first/
总结
escape()不能直接用于URL编码，它的真正作用是返回一个字符的Unicode编码值。比如"春节"的返回结果是%u6625%u8282，，escape()不对"+"编码 主要用于汉字编码，现在已经不提倡使用。

encodeURI()是Javascript中真正用来对URL编码的函数。 编码整个url地址，但对特殊含义的符号"; / ? : @ & = + $ , #"，也不进行编码。对应的解码函数是：decodeURI()。

encodeURIComponent() 能编码"; / ? : @ & = + $ , #"这些特殊字符。对应的解码函数是decodeURIComponent()。

假如要传递带&符号的网址，所以用encodeURIComponent()
```

## innerHTML和 innerText 如何区分
```
共同点：innerHTML和innerText都会把元素内内容替换掉。

<div id="test"> 
 <span style="color:red">test1</span> test2 
</div>

test.innerHTML = "<span style="color:red">test1</span> test2";
test.innerText = "test1 test2";
test.outerHTML = "<div id="test"><span style="color:red">test1</span> test2</div>";


innerHTML是符合W3C标准的属性，而innerText只适用于IE浏览器，因此，尽可能地去使用innerHTML，而少用innerText，如果要输出不含HTML标签的内容，可以使用innerHTML取得包含HTML标签的内容后，再用正则表达式去除HTML标签，下面是一个简单的符合W3C标准的示例：
<a href="javascript:alert(document.getElementById('test').innerHTML.replace(/<.+?>/gim,''))">无HTML,符合W3C标准</a>
```

##substr 与 substring
```
js区别：L
var s = "hello";
s.substring(1，3);//相当于从位置为1的字符截取到位置为2的字符，得到子串为："el"


var s = "hello";
s.substr(1，3);//从下标为1的字符开始截取3个字符长度，最后子串为：ell


var s = "hello";
s.substr(3)//"lo"


var s = "hello";
s.substr(-3,2)//即从倒数第三个字符开始起截取2个长度，获得："ll"
实例：
var test = "12345,112,32,19";
var ind = test.lastIndexOf(",");//12
console.log('index:'+ind);//12
console.log(test)//12345,112,32,19
console.log(test.substring(0,ind));//12345,112,32
console.log(test.substr(0,ind));//12345,112,32
```

##判断数组是否为空,null与undefined区别
```
js中的数据类型 
字符串、数字、布尔、数组、对象、Null、Undefined
===全等比较
比较2个相同类型的对象，如果类型不同，就直接返回false，如果类型相同，那就比较具体的值或具体的引用地址


var arr=null;
if(arr===null){
    console.log('arr is null');
}
arr=undefined;
if(arr===null){
    console.log('arr is null');
}else if(arr===undefined){
    console.log('arr is undefined');
}
//arr赋值一个数组对象
arr=[];
//if(arr.length==0){
if(arr.length===0){
   console.log('arr is empty');
}
```

##indexOf()方法返回值区别
```
-1 没有找到该字符串
0  该字符串位于第一个字符
1  该字符串位于第二个字符
          String  tag  =  ".22222" ;
        System. out .println( tag .indexOf( "." )); // 0
        System. out .println( tag .indexOf( "." ) >= 0); // true
        System. out .println( tag .indexOf( "." ) > 0); // false         System. out .println( tag .indexOf( "." ) == 0); // true   

``` ##JS中Null与Undefined的区别
```

在JavaScript中存在这样两种原始类型:Null与Undefined。


Undefined类型只有一个值，即undefined。当声明的变量还未被初始化时，变量的默认值为undefined。
Null类型也只有一个值，即null。null用来表示尚未存在的对象，常用来表示函数企图返回一个不存在的对象。


js 代码 
var oValue;  
alert(oValue == undefined); //output "true"  


这段代码显示为true,代表oVlaue的值即为undefined，因为我们没有初始化它。

alert(null == document.getElementById('notExistElement')); //output "true"


当页面上不存在id为"notExistElement"的DOM节点时，这段代码显示为"true"，因为我们尝试获取一个不存在的对象。

alert(typeof undefined); //output "undefined"  
alert(typeof null); //output "object"  


第一行代码很容易理解，undefined的类型为Undefined；第二行代码却让人疑惑，为什么null的类型又是Object了呢？其实这是JavaScript最初实现的一个错误，后来被ECMAScript沿用下来。在今天我们可以解释为，null即是一个不存在的对象的占位符，但是在实际编码时还是要注意这一特性。


alert(null == undefined); //output "true"  

ECMAScript认为undefined是从null派生出来的，所以把它们定义为相等的。但是，如果在一些情况下，我们一定要区分这两个值，那应该怎么办呢？可以使用下面的两种方法。


alert(null === undefined); //output "false"  
alert(typeof null == typeof undefined); //output "false"  


使用typeof方法在前面已经讲过，null与undefined的类型是不一样的，所以输出"false"。而===代表绝对等于，在这里null === undefined输出false。


<html>
 <head></head>
 <body>
 <script>
var oValue;  
alert(oValue == undefined); //output "true"  
alert(null == document.getElementById('notExistElement')); //output "true" 
alert(typeof undefined); //output "undefined"  
alert(typeof null); //output "object"  
alert(null == undefined); //output "true"  
alert(null === undefined); //output "false"  
alert(typeof null == typeof undefined); //output "false"  
</script>
 </body>
</html>
```
##创建 js对象和java对象的不同
面向对象分为基于原型的面向对象和基于模板的面向对象。
Java：基于模板的面向对象。
```
class A
{
   private String name;
   public void fun(){

   }
}

A a = new A();
a.fun();
```
JavaScript：基于原型的面向对象，基于事件的网页脚本语言。
```
<script>
    function CreateObject() {

    }

    CreateObject.prototype = {
        constructor: CreateObject,  // 可特意声明constructor指向 CreateObject
        name: 'xxx',
        age: '11',
        children: ['aaa', 'bbb'],
        getName: function() {
            return this.name;
        }
    }

    var p = new CreateObject();
    console.log(p.name); // 'xxx'
</script>
```