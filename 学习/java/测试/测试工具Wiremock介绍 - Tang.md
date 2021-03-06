
WireMock是一个开源的测试工具，支持HTTP响应存根、请求验证、代理/拦截、记录和回放。最直接的用法： 
为Web/移动应用构建Mock Service
快速创建Web API原型
模拟Web Service中错误返回
录制HTTP请求和回放


 一般开发项目都会把前端组和Service组分开，当进度不一致时，可以根据接口构建Mock Service对和模拟不同输入/数据/场景，这样不至于影响两组的开发进度。构建Mock Service方法很多，node.js大概五句代码，另一测试工具soapUI也可做到，同时还可以对Service进行功能/性能测试，功能齐全。Wiremock好在轻便，一个jar包基本够用了，当然，也可以把它引用写进测试代码里。

官网地址： http://wiremock.org/

Jar下载： http://repo1.maven.org/maven2/com/github/tomakehurst/wiremock/1.57/wiremock-1.57-standalone.jar Mock Service

下载后，在命令行中运行：（需要java JDK环境）

java -jar wiremock-
1.57
-standalone.jar –port 
9999
 --verbose

  （不指定端口默认为8080； 开启日志：--verbose。更多参数，请至： http://wiremock.org/running-standalone.html 。 把命令放在bat文件中比较方便）

启动后在同目录下生成两个空的文件夹：__files和mappings。__files是放上传/下载/录制文件用的，mappings放你想要的Service返回数据和Url mapping.

在mappings文件夹下随便创建一个*.json文件：

（注意，添加修改mapping文件后，都需要重启服务才能生效）

{
    
"request"
: {
        
"method": "GET"
,
        
"url": "/api/mytest"

    },
    
"response"
: {
        
"status": 200
,
        
"body": "More content\n"

    }
}


 

在Fiddler/浏览器/curl命令调用: http://localhost:9999/api/mytest

 

这是一个HTTP GET的例子，还可以在response中自定义返回的头headers:

"response"
: {

            
"status": 200
,
            
"body": "Hello world!"
,
            
"headers"
: {
                    
"Content-Type": "text/plain"

            }

}


 HTTP方法支持GET, POST, PUT, DELETE, HEAD, TRACE, OPTIONS等，自定义头、数据模板(bodyPatterns，如下例，如不符合，抛出404错误)，URL Template，Query参数匹配，显示指定文件内容等。

如以下例子：  POST: http://localhost:9999/api/products

{
    
"request"
: {
        
"method": "POST"
,
        
"url": "/api/products"
,
          
"bodyPatterns"
: [
                 {
"equalToJson" : "{ \"name\": \"new product\", \"creator\": \"tester\", \"createTime\": \"2015-09-07\" }", "jsonCompareMode": "LENIENT"
}
         ]
    },
    
"response"
: {
        
"status": 201
,
        
"body": "Add successfully."
,
         
"headers"
:{
                   
"x-token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxx"

         }
    }
}
PUT: http://localhost:9999/api/products/1

{
    
"request"
: {
        
"method": "PUT"
,
        
"url": "/api/products/1"
,
          
"bodyPatterns"
: [
            {
"equalToJson" : "{ \"id\": 1, \"name\": \"new product\", \"creator\": \"tester\", \"createTime\": \"2015-09-07\" }", "jsonCompareMode": "LENIENT"
}
         ]
    },
    
"response"
: {
        
"status": 200
,
        
"body": "Update successfully."
,
         
"headers"
:{
                  
"x-token":" xxxxxxxxxxxxxxxxxxxxxxxxxxxx"

         }
    }
}
DELETE: http://localhost:9999/api/products/1

{
    
"request"
: {
        
"method": "DELETE"
,
        
"url": "/api/products/1"
 
    },
    
"response"
: {
        
"status": 204
,      
         
"headers"
:{
                   
"x-token":" xxxxxxxxxxxxxxxxxxxxxxxxxxxx"

         }
    }
}
 URL Matching: http://localhost:9999/api/products/1（2/3 ...）

{
    
"request"
: {
        
"method": "GET"
,
        
"urlPattern": "/api/products/[0-9]+"

    },
    
"response"
: {
        
"status": 200

    }
}
  Query参数匹配：http://localhost:9999/api/products?search=china

{
    
"request"
: {
        
"method": "GET"
,
        
"urlPath": "/api/products"
,
        
"queryParameters"
: {
            
"search"
: {
                
"contains": "
chin
"

            }
        }
    },
    
"response"
: {
            
"status": 200
,
             
"headers":{ "Content-Type": "application/json"
},
              
"body": "{ \"id\": 7, \"name\": \"shan zai\", \"from\":\"China\" },{ \"id\": 7, \"name\": \"shan zai\", \"from\":\"China(RPC)\" }"

    }
}
  返回文件内容：http://localhost:9999/file/1

{
    
"request"
: {
        
"method": "GET"
,
        
"url": "/file/1"

    },
    
"response"
: {
        
"status": 200
,
        
"bodyFileName": "test.xml"（或：”xmlfiles/test.xml”）

    }
}


(在__files文件夹下建好所调文件，路径为相对__files文件夹。)  模拟错误

{
  
"request"
 : {
    
"url" : "/unknown.html"
,
    
"method" : "GET"

  },
  
"response"
 : {
    
"status" : 404
,   
    
"headers"
 : {     
      
"Content-Type" : "text/html; charset=utf-8"
    
    }
  }
}


 

{
    
"request"
: {
        
"method": "GET"
,
        
"url": "/fault"

    },
    
"response"
: {
        
"fault": "MALFORMED_RESPONSE_CHUNK"

    }
}



 

录制HTTP请求及回放 

Wiremock的录制过程是启动一个代理服务，截取HTTP请求和响应，在mappings文件夹中创建一json文件记录下请求地址和响应概要，在__files文件夹下创建一文件包含响应内容；当重启Standalone进程时，那些记录下的请求响应就会作为Mock Service生成。 

启动录制服务： 

java -jar wiremock-
1.57
-standalone.jar --proxy-all=
"
http://localhost:7777
"
 --record-mappings –verbose

默认的代理服务端口是8080，即之后发向 http://localhost:7777 的请求，可以用 http://localhst:8080/ 来代理。

代理前：



代理后：



录制记录：



 生成mapping文件和响应内容文件：



 

{
  
"request"
 : {
    
"url" : "/test.aspx"
,
    
"method" : "GET"

  },
  
"response"
 : {
    
"status" : 200
,
    
"
bodyFileName
" : "
body-test.aspx-WK0fD.json
"
,
    
"headers"
 : {
      
"Cache-Control" : "private"
,
      
"Content-Type" : "text/html; charset=utf-8"
,
      
"Server" : "Microsoft-IIS/7.5"
,
      
"X-AspNet-Version" : "2.0.50727"
,
      
"X-Powered-By" : "ASP.NET"
,
      
"Date" : "Tue, 08 Sep 2015 03:14:36 GMT"
,
      
"Content-Length" : "61"

    }
  }
}




 

录制完重启服务，验证刚才录制的请求是否生效：



 

更多的功能，如集成到测试代码、场景模拟（比较接近于真实的情景），请参阅官网： http://wiremock.org/

 