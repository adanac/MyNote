上篇博客介绍了同源策略和跨域访问概念，其中提到跨域常用的基本方式：JSONP和CORS。
  那这篇博客就介绍JSONP方式。
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
  
    具体的JS实现JSONP代码。

  JS中：

   
[html]   view plain copy print ?

   < script >   
  var  url  =  "http://localhost:8080/crcp/rcp/t99eidt/testjson.do?jsonp=callbackfunction" ;   
  var  script  =  document .createElement('script');   
  script.setAttribute('src', url);  //load javascript    
  document.getElementsByTagName('head')[0].appendChild(script);   
  
  //回调函数  
   function callbackfunction(data){  
var  html = JSON .stringify(data.RESULTSET);  
alert(html);  
     }  



  服务器代码Action：
  后台返回的json外面需要由回调函数包裹。具体的方法如下：
  
[html]   view plain copy print ?

public class TestJson extends ActionSupport{  
  
    @Override  
    public String execute() throws Exception {  
        try {  
            JSONObject  jsonObject = new  JSONObject();  
            List  list = new  ArrayList();  
            for(int  i = 0 ;i < 4 ;i++){  
                Map  paramMap = new  HashMap();  
                paramMap.put("bank_no", 100+i);  
                paramMap.put("money_type", i);  
                paramMap.put("bank_name", i);  
                paramMap.put("bank_type", i);  
                paramMap.put("bank_status", 0);  
                paramMap.put("en_sign_ways", 1);  
                list.add(paramMap);  
            }  
            JSONArray  rows = JSONArray .fromObject(list);  
            jsonObject.put("RESULTSET", rows);  
            HttpServletRequest  request = ServletActionContext .getRequest();  
            HttpServletResponse  response = ServletActionContext .getResponse();  
            response.setContentType("text/javascript");  
              
              
            boolean  jsonP  =  false ;  
            String  cb  =  request .getParameter("jsonp");  
            if (cb != null) {  
                 jsonP  =  true ;  
                System.out.println("jsonp");  
                response.setContentType("text/javascript");  
            } else {  
                System.out.println("json");  
                response.setContentType("application/x-json");  
            }  
            response.setCharacterEncoding("UTF-8");  
            Writer  out  =  response .getWriter();  
            if (jsonP) {  
                out.write(cb + "("+jsonObject.toString()+")");  
                System.out.println(jsonObject.toString());  
            }  
            else{  
                out.write(jsonObject.toString());  
                 System.out.println(jsonObject.toString());  
            }  
        } catch (Exception e) {  
            e.printStackTrace();  
        }  
         
        return null;  
    }  
}  
 
 JQUERY实现JSONP代码。
 Jquery从1.2版本开始也支持JSONP的实现。
[html]   view plain copy print ?

$(function(){  
     jQuery.getJSON("http://localhost:8080/crcp/rcp/t99eidt/testjson.do? jsonp =?",function(data)  
{   
  var  html = JSON .stringify(data.RESULTSET);  
$("#testjsonp").html(html);  
}  
     );   
});  
  第一个？代表后面是参数，与咱们一般调用一样。重要的是第二个？，则是jquery动态给你生成毁掉函数名称。

至于后台代码和上述一致，使用同一个后台。


JQUERY中Ajax实现JSONP代码。
[html]   view plain copy print ?

 $.ajax({  
type:"GET",  
async :false,  
url:"http://localhost:8080/crcp/rcp/t99eidt/testjson.do",  
dataType:"jsonp",  
success:function(data){  
var  html = JSON .stringify(data.RESULTSET);  
$("#testjsonp").html(html);  
},  
error:function(){  
alert("error");  
}  
  
});  
    注意：这种形式，默认的参数是callback，而不是会是其他。则action代码中获取calback值则
    String cb=request.getParameter("callback");
    并且生成的回调函数，默认也是类似上述一大串数字。
    根据Ajax手册，更改callback名称以及回调函数名称。
    http://www.w3school.com.cn/jquery/ajax_ajax.asp 
    jsonp:jsonp,则请求的地址为： http://localhost:8080/crcp/rcp/t99eidt/testjson.do?jsonp=自动生成回调函数名
    jsonpCallback:callbackfunction,则请求的地址为：
     http://localhost:8080/crcp/rcp/t99eidt/testjson.do?jsonp= callbackfunction
    最后返回前台的是：
    callbackfunction(具体的json值)


    其中上述JS实现JSONP代码中，若不是动态拼接script脚本，而是直接写script标签，类似如下：
   <script type="text/javascript" src=""></script>
   若这样写的话，通过debug发现，的确正确返回了，但是一直提示找不到回调函数。即使js也提供了回调函数【各个浏览器都测试】
   若要通过JS来显示，则通过代码动态create script标签。


   JSONP跨域方式，很方便，同时也支持大多部分浏览器，但是唯一缺点是，只支持GET提交方式，不支持其他POST提交。
   若url地址传输的参数过多，如何实现呢？下篇博客会讲解另一种跨域方案CROS原理以及具体调用示例。 本文转载自：http://blog.csdn.net/yuebinghaoyuan/article/details/32706277