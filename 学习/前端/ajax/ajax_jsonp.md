##Ajax请求中的async:false/true的作用
官方的解释是：
async Boolean Default : true
By default , all requests are sent asynchronous ( e . g . this is set to true by default ). If you need synchronous requests , set this option to false . Note that synchronous requests may temporarily lock the browser , disabling any actions while the request is active .
async . 默认是 true ，即为异步方式， $ . Ajax 执行后，会继续执行 ajax 后面的脚本，直到服务器端返回数据后，触发 $ . Ajax 里的 success 方法，这时候执行的是两个线程。若要将其设置为 false ，则所有的请求均为同步请求，在没有返回值之前，同步请求将锁住浏览器，用户其它操作必须等待请求完成才可以执行。
下面查看一个示例：
```
var temp ;
$ . ajax ({
       async : false ,
       type : "POST" ,      url : defaultPostData . url ,
    dataType : 'json' ,
    success : function ( data ) {
    temp = data ;

      }
    });
alert ( temp );
```
这个 ajax 请求为同步请求，在没有返回值之前， alert ( temp )是不会执行的。
如果 async 设置为： true ，则不会等待 ajax 请求返回的结果，会直接执行 ajax 后面的语句。
不过上面设置同步请求的方法，有网友曾经反馈将 async 设成 false 后, 原意是想返回数据了再执行 $ . Ajax 后面的脚本, 没想到这个地方却导致了在火狐浏览器下出现闪屏( Firefox 11.0 )，滚动条下拉到底部触发 ajax 的情况。最后只能将 async : false 注释掉， 也就是 async 为 ture 的情况下，成功解决了火狐浏览器滚动条下拉到底部触发 ajax 出现闪屏的问题。
##ajax与jsonp
ajax和jsonp这两种技术在调用方式上“看起来”很像，目的也一样，都是请求一个url，然后把服务器返回的数据进行处理;



但ajax和jsonp其实本质上是不同的东西。ajax的核心是通过XmlHttpRequest获取非本页内容，而jsonp的核心则是动态添加<script>标签来调用服务器提供的js脚本。

所以说，其实ajax与jsonp的区别不在于是否跨域，ajax通过服务端代理一样可以实现跨域，jsonp本身也不排斥同域的数据的获取。

##jsonp详解

1jsonp原理 


JSONP原理
 在同源策略下，在某个服务器下的页面是无法获取到该服务器以外的数据的，但img、iframe、script等标签是个例外，这些标签可以通过src属性请求到其他服务器上的数据。
  而JSONP就是通过script节点src调用跨域的请求。
  当我们通过JSONP模式请求跨域资源时，服务器返回给客户端一段javascript代码，这段javascript代码自动调用客户端回调函数。
举个例子
  客户端http://localhost:8080访问服务器http://localhost:11111/user，正常情况下，这是不允许的。因为这两个URL是不同域的。
  
  若我们使用JSONP格式发送请求的话？
  http://localhost:11111/user?callback=callbackfunction
  则服务器返回的数据如下：
  callbackfunction({"id":1,"name":"test"})
  仔细看看服务器返回的数据，其实就是一段javascript代码，这就是函数名（参数）格式。
  服务器返回后，则自动执行callbackfunction函数。
  因此，客户端需要callbackfunction函数，以便使用JSONP模式返回javascript代码后自动执行其回调函数。


  注意：其中url地址中的callback和callbackfunction是随意命名的。
2.实现jsonp
JS实现JSONP代码

```
<script>  
  var url = "http://localhost:8080/crcp/rcp/t99eidt/testjson.do?jsonp=callbackfunction";   
  var script = document.createElement('script');   
  script.setAttribute('src', url);  //load javascript    
  document.getElementsByTagName('head')[0].appendChild(script);   
  
  //回调函数  
   function callbackfunction(data){  
var html=JSON.stringify(data.RESULTSET);  
alert(html);  
     }
</script>


```
Jquery实现JSONP代码

```
$(function(){
     jQuery.getJSON("http://localhost:8080/crcp/rcp/t99eidt/testjson.do?jsonp=?",function(data)
{ 
  var html=JSON.stringify(data.RESULTSET);
$("#testjsonp").html(html);
}
     ); 
});


```
Ajax实现JSONP代码

```
     $.ajax({
    type:"GET",
    async :false,
    url:"http://localhost:8080/crcp/rcp/t99eidt/testjson.do",
    dataType:"jsonp",
    success:function(data){
    var html=JSON.stringify(data.RESULTSET);
    $("#testjsonp").html(html);
    },
    error:function(){
    alert("error");
    }
   
    });
```