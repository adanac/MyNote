
    Json转换工具实在之多，最近又听说FastJson对Java序列化和反序列化最优，相比 Java自带序列化、Json-lib、Jackson等。不过本人很青睐Gson，但是好像用的人也不是很多，项目中用的最多的就是垃圾Json-lib了，虽然烂，但是还是的继续使用着，因为项目在我来之前已经搭建了，不过现在我推荐使用Jackson，官网说到这是Json工具中最快的一个，当然是有一点吹牛的，因为他没有拿数据说话！之所以推荐Jackson，Jackson确实速度快，而且Spring内部原生支持Jackson对Json的转换，也就是只要我们在XML中配置然后添加Jackson包，就大功告成了！具体如何操作可以参见博客：


Spring MVC Rest 学习一  http://my.oschina.net/heweipo/blog/337581

    对于其他的Json包，诸如simpleJson等，这些东西本人没有什么了解，不过可以参考如下地址，看看maven仓库中的介绍：


Maven Repository  http://mvnrepository.com/search?q=Json

    好了，不多说了，下面开始把本人归纳总结的Map、List 与 Json之间转换的代码贴上，确实很简单。


1、判断是否满足Json格式
    
/**
	     * 该字符串可能转为 JSONObject 或 JSONArray
	     * @param string
	     * @return
	     */

	    
public
 static 
boolean
 mayBeJSON(
String
 
string
) {
			
return
 ((
string
 
!=
 
null
) 
&&
 (((
"null"
.
equals
(
string
))
					
||
 ((
string
.
startsWith(
"["
)) 
&&
 (
string
.
endsWith(
"]"
))) 
||
 ((
string

					
.
startsWith(
"{"
)) 
&&
 (
string
.
endsWith(
"}"
))))));
		}
	    
	    
/**
	     * 该字符串可能转为JSONObject
	     * @param string
	     * @return
	     */

	    
public
 static 
boolean
 mayBeJSONObject(
String
 
string
) {
			
return
 ((
string
 
!=
 
null
) 
&&
 (((
"null"
.
equals
(
string
))
					 
||
 ((
string
.
startsWith(
"{"
)) 
&&
 (
string
.
endsWith(
"}"
))))));
		}
	    
	    
/**
	     * 该字符串可能转为 JSONArray
	     * @param string
	     * @return
	     */

	    
public
 static 
boolean
 mayBeJSONArray(
String
 
string
) {
			
return
 ((
string
 
!=
 
null
) 
&&
 (((
"null"
.
equals
(
string
))
					
||
 ((
string
.
startsWith(
"["
)) 
&&
 (
string
.
endsWith(
"]"
))))));
		}



2、Json 与 Map 的转换
 
/**
	  *函数注释：parseJSON2Map()<br>
	  *时间：2014-10-28-上午10:50:21<br>
	  *用途：该方法用于json数据转换为<Map<String, Object>
	  *@param jsonStr
	  *@return
	  */

	    
public
 
static
 Map<
String
, 
Object
> parseJSON2Map(
String
 jsonStr){  
	        Map<
String
, 
Object
> 
map
 = 
new
 
HashMap
<
String
, 
Object
>();  
	        
//最外层解析  

	        
JSONObject
 json = 
JSONObject
.fromObject(jsonStr);  
	        
for
(
Object
 k : json.keySet()){  
	            
Object
 v = json.
get
(k);   
	            
//如果内层还是数组的话，继续解析  

	            
if
(v 
instanceof
 
JSONArray
){  
	                List<Map<
String
, 
Object
>> list = 
new
 ArrayList<Map<
String
,
Object
>>();  
	                Iterator<
JSONObject
> it = ((
JSONArray
)v).iterator();  
	                
while
(it.hasNext()){  
	                    
JSONObject
 json2 = it.next();  
	                    list.
add
(parseJSON2Map(json2.toString()));  
	                }  
	                
map
.put(k.toString(), list);  
	            } 
else
 {  
	                
map
.put(k.toString(), v);  
	            }  
	        }  
	        
return
 
map
;  
	    }  
	    
	    
/**
              * 函数注释：parseJSON2MapString()<br>
	      * 用途：该方法用于json数据转换为<Map<String, String><br>
	      * 备注：***<br> 
	      */

	    
public
 
static
 Map<
String
, 
String
> parseJSON2MapString(
String
 jsonStr){  
	        Map<
String
, 
String
> 
map
 = 
new
 
HashMap
<
String
, 
String
>();  
	        
//最外层解析  

	        
JSONObject
 json = 
JSONObject
.fromObject(jsonStr);  
	        
for
(
Object
 k : json.keySet()){ 
	            
Object
 v = json.
get
(k);   
	            
if
(
null
!=v){
	            	
map
.put(k.toString(), v.toString());  
	            }
	        }  
	        
return
 
map
;  
	    }



3、Json 与 List  的转换
/**
	 *函数注释：parseJSON2List()<br>
	 *用途：该方法用于json数据转换为List<Map<String, Object>><br>
	 */

	 
public
 static 
List
<
Map
<
String
, Object
>>
 parseJSON2List(
String
 jsonStr){  
	        JSONArray jsonArr 
=
 JSONArray
.
fromObject(jsonStr);  
	        
List
<
Map
<
String
, Object
>>
 
list
 
=
 
new
 ArrayList
<
Map
<
String
,Object
>>
();  
	        Iterator
<
JSONObject
>
 it 
=
 jsonArr
.
iterator();  
	        
while
(it
.
hasNext()){  
	            JSONObject json2 
=
 it
.
next();  
	            
list
.
add(parseJSON2Map(json2
.
toString()));  
	        }  
	        
return
 
list
;  
	    }
	 
	
/**
	* 函数注释：parseJSON2ListString()<br>
	* 用途：该方法用于json数据转换为List<Map<String, String>><br>
	*/

	 
public
 static 
List
<
Map
<
String
, 
String
>>
 parseJSON2ListString(
String
 jsonStr){  
		        JSONArray jsonArr 
=
 JSONArray
.
fromObject(jsonStr);  
		        
List
<
Map
<
String
, 
String
>>
 
list
 
=
 
new
 ArrayList
<
Map
<
String
,
String
>>
();  
		        Iterator
<
JSONObject
>
 it 
=
 jsonArr
.
iterator();  
		        
while
(it
.
hasNext()){  
		            JSONObject json2 
=
 it
.
next();  
		            
list
.
add(parseJSON2MapString(json2
.
toString()));  
		        }  
		        
return
 
list
;  
		    }



4、List 或者 Map 与 Json的转换
JSONObject.
from
Object（obj）.
to
String();
JSONArray.
from
Object（obj）.
to
String();



5、为什么不使用 JSONObject.toBean

    我的理由是：这个方法我相信用过的人都是恨之入骨，至少我是这样的，以前写过一个javaBean，结果硬是要javaBean中的属性和Json的属性完全相同，二者转换才不会报错。

6、提醒

    Json格式是要求value必须有双引号，否则就不是标准的 Json， 那么，在上面提供的方法中，假设有个 Map<String,Object> 然而这个Object有可能是一个List<Map<String,Object>>,那么想要获取里面的List对象时，可千万不要调用parseJSON2Map，原因是他得到的值会把value中的双引号全部去掉，那么这个结果是不符合Json规范的，后面在解析List就会出错。
net
.sf
.json
.JSONException
: Unquotted string



正确的做法应该是 采用 JSONObject先解析成功JSONObject，然后再用JSONObject对象获取List的Json字符串，然后调用上面的parseJSON2ListString，这样就成功了！

7、提供一些Json帮助网站以及工具


1）Json在线校验： http://www.bejson.com/

2）Json在线解析：火狐浏览器插件 json-handler 非常不错

3）Json-lib 所需要的jar ：  http://download.csdn.net/detail/wp562846864/7238979








