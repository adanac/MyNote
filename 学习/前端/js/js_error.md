##Uncaught Error: Syntax error, unrecognized expression: unsupported pseudo: option
```
原因：写法错误，  $( "#selectedsellers :option" ).each( function (){}
修复：
          var  sellerId = [];
        $( "#selectedsellers option" ).each( function (){
            sellerId.push($( this ).val());         });   

```
## Math min bug
```
var minPrice = Math.min.apply(null,["8","7","","5"]);
var maxPrice = Math.max.apply(null,["8","7","","5"]);
alert(minPrice+","+maxPrice);
如果传入的数组 有一个为"",则 min = 0.


```
##Uncaught TypeError: appPrice.indexOf is not a function
```
var appPrice = "";
if(prplist[0].mutiPrice!=undefined){
appPrice = prplist[0].mutiPrice;
}
if(appPrice.indexOf(";")!=-1){
appPrice = getMemPrice(prplist[0].mutiPrice)
}
原因：appPrice = 1100.为金额，不是字符串。
修复： if(appPrice.toString().indexOf(";")!=-1)
```
##ajaxfileupload-jQuery.handleError is not a function修复办法
```
jQuery.handleError is not a function 报错原因是：
1.handlerError只在jQuery-1.4.2之前的版本中存在，jQuery-1.4.2之后的版本中都没有这个函数了。
2.如果返回的dataType: “json”, 是json格式的，则还需要添加httpData方法。
```
需要在ajaxFileupload.js中增加两个方法


```
完整的ajaxfileupload.js代码：

jQuery.extend({
    
    createUploadIframe:  function (id, uri)
    {
             //create frame
             var  frameId =  'jUploadFrame'  + id;
             var  iframeHtml =  '<iframe id="'  + frameId +  '" name="'  + frameId +  '" style="position:absolute; top:-9999px; left:-9999px"' ;
             if (window.ActiveXObject)
            {
                 if ( typeof  uri==  'boolean' ){
                    iframeHtml +=  ' src="'  +  'javascript:false'  +  '"' ;
                }
                 else   if ( typeof  uri==  'string' ){
                    iframeHtml +=  ' src="'  + uri +  '"' ;
                }    
            }
            iframeHtml +=  ' />' ;
            jQuery(iframeHtml).appendTo(document.body);
             return  jQuery( '#'  + frameId).get(0);            
    },
    createUploadForm:  function (id, fileElementId, data)
    {
         //create form    
         var  formId =  'jUploadForm'  + id;
         var  fileId =  'jUploadFile'  + id;
         var  form = jQuery( '<form  action="" method="POST" name="'  + formId +  '" id="'  + formId +  '" enctype="multipart/form-data"></form>' );    
         if (data)
        {
             for ( var  i  in  data)
            {
                jQuery( '<input type="hidden" name="'  + i +  '" value="'  + data[i] +  '" />' ).appendTo(form);
            }            
        }        
         var  oldElement = jQuery( '#'  + fileElementId);
         var  newElement = jQuery(oldElement).clone();
        jQuery(oldElement).attr( 'id' , fileId);
        jQuery(oldElement).before(newElement);
        jQuery(oldElement).appendTo(form);
        
         //set attributes
        jQuery(form).css( 'position' ,  'absolute' );
        jQuery(form).css( 'top' ,  '-1200px' );
        jQuery(form).css( 'left' ,  '-1200px' );
        jQuery(form).appendTo( 'body' );        
         return  form;
    },
    ajaxFileUpload:  function (s) {
         //  TODO  introduce global settings, allowing the client to modify them for all requests, not only timeout        
        s = jQuery.extend({}, jQuery.ajaxSettings, s);
         var  id =  new  Date().getTime()        
         var  form = jQuery.createUploadForm(id, s.fileElementId, ( typeof (s.data)== 'undefined' ? false :s.data));
         var  io = jQuery.createUploadIframe(id, s.secureuri);
         var  frameId =  'jUploadFrame'  + id;
         var  formId =  'jUploadForm'  + id;        
         // Watch for a new set of requests
         if  ( s.global && ! jQuery.active++ )
        {
            jQuery.event.trigger(  "ajaxStart"  );
        }            
         var  requestDone =  false ;
         // Create the request object
         var  xml = {}   
         if  ( s.global )
            jQuery.event.trigger( "ajaxSend" , [xml, s]);
         // Wait for a response to come back
         var  uploadCallback =  function (isTimeout)
        {            
             var  io = document.getElementById(frameId);
             try  
            {                
                 if (io.contentWindow)
                {
                     xml.responseText = io.contentWindow.document.body?io.contentWindow.document.body.innerHTML: null ;
                     xml.responseXML = io.contentWindow.document.XMLDocument?io.contentWindow.document.XMLDocument:io.contentWindow.document;
                     
                } else   if (io.contentDocument)
                {
                     xml.responseText = io.contentDocument.document.body?io.contentDocument.document.body.innerHTML: null ;
                    xml.responseXML = io.contentDocument.document.XMLDocument?io.contentDocument.document.XMLDocument:io.contentDocument.document;
                }                        
            } catch (e)
            {
                jQuery.handleError(s, xml,  null , e);
            }
             if  ( xml || isTimeout ==  "timeout" ) 
            {                
                requestDone =  true ;
                 var  status;
                 try  {
                    status = isTimeout !=  "timeout"  ?  "success"  :  "error" ;
                     // Make sure that the request was successful or notmodified
                     if  ( status !=  "error"  )
                    {
                         // process the data (runs the xml through httpData regardless of callback)
                         var  data = jQuery.uploadHttpData( xml, s.dataType );    
                         // If a local callback was specified, fire it and pass it the data
                         if  ( s.success )
                            s.success( data, status );
    
                         // Fire the global callback
                         if ( s.global )
                            jQuery.event.trigger(  "ajaxSuccess" , [xml, s] );
                    }  else
                        jQuery.handleError(s, xml, status);
                }  catch (e) 
                {
                    status =  "error" ;
                    jQuery.handleError(s, xml, status, e);
                }
                 // The request was completed
                 if ( s.global )
                    jQuery.event.trigger(  "ajaxComplete" , [xml, s] );
                 // Handle the global AJAX counter
                 if  ( s.global && ! --jQuery.active )
                    jQuery.event.trigger(  "ajaxStop"  );
                 // Process result
                 if  ( s.complete )
                    s.complete(xml, status);
                jQuery(io).unbind()
                setTimeout( function ()
                                    {     try  
                                        {
                                            jQuery(io).remove();
                                            jQuery(form).remove();    
                                            
                                        }  catch (e) 
                                        {
                                            jQuery.handleError(s, xml,  null , e);
                                        }                                    
                                    }, 100)
                xml =  null
            }
        }
         // Timeout checker
         if  ( s.timeout > 0 ) 
        {
            setTimeout( function (){
                 // Check to see if the request is still happening
                 if ( !requestDone ) uploadCallback(  "timeout"  );
            }, s.timeout);
        }
         try  
        {
             var  form = jQuery( '#'  + formId);
            jQuery(form).attr( 'action' , s.url);
            jQuery(form).attr( 'method' ,  'POST' );
            jQuery(form).attr( 'target' , frameId);
             if (form.encoding)
            {
                jQuery(form).attr( 'encoding' ,  'multipart/form-data' );                  
            }
             else
            {    
                jQuery(form).attr( 'enctype' ,  'multipart/form-data' );            
            }            
            jQuery(form).submit();
        }  catch (e) 
        {            
            jQuery.handleError(s, xml,  null , e);
        }
        
        jQuery( '#'  + frameId).load(uploadCallback    );
         return  {abort:  function  () {}};    
    },
    uploadHttpData:  function ( r, type ) {
         var  data = !type;
        data = type ==  "xml"  || data ? r.responseXML : r.responseText;
         // If the type is "script", eval it in global context
         if  ( type ==  "script"  )
            jQuery.globalEval( data );
         // Get the JavaScript object, if JSON is used.
         if  ( type ==  "json"  )
            eval(  "data = "  + data );
         // evaluate scripts within html
         if  ( type ==  "html"  )
            jQuery( "<div>" ).html(data).evalScripts();
         return  data;
    },
    
    handleError:  function  (s, xhr, status, e) {
         if  (s.error) {
            s.error.call(s.context || s, xhr, status, e);
        }
         if  (s.global) {
            (s.context ? jQuery(s.context) : jQuery.event).trigger( "ajaxError" , [xhr, s, e]);
        }
    },
    httpData:  function  (xhr, type, s) {
         var  ct = xhr.getResponseHeader( "content-type" ),
        xml = type ==  "xml"  || !type && ct && ct.indexOf( "xml" ) >= 0,
        data = xml ? xhr.responseXML : xhr.responseText;
         if  (xml && data.documentElement.tagName ==  "parsererror" )
             throw   "parsererror" ;
         if  (s && s.dataFilter)
            data = s.dataFilter(data, type);
         if  ( typeof  data ===  "string" ) {
             if  (type ==  "script" )
                jQuery.globalEval(data);
             if  (type ==  "json" )
                data = window[ "eval" ]( "("  + data +  ")" );
        }
         return  data;
    }
})
```
## Uncaught ReferenceError:XXXX is not  
```
 temp =  "<a href='javascript:void(0);' onclick='delMsg(" +id+ ");'></a>";

js函数delMsg() 如果传入的参数为字符串，就会报此错。



正确的写法如下：
<a href='javascript:void(0);' onclick=\"delMsg('" +id+ "');\"></a>

if (status==1){ //已推送
                  temp =  "<a href='javascript:void(0);' onclick=\"delMsg('" +id+ "');\" class='btn btn-xs red' title='删除'><i class='fa fa-times'></i></a>"
                  + "<a href='${base}/message/toModMsg.do?id=" +id+ "' class='btn btn-xs green' title='复制'><i class='fa fa-copy'></i></a>" ;               }  




```
## Uncaught SyntaxError: Unexpected token ILLEGAL
```
问题一：

<input id="btn_0_4EAE4F474C91156086C0D4EA7E983C69C215B649" type="button" value="连接" onclick="middleware_connect(0, 4EAE4F474C91156086C0D4EA7E983C69C215B649)">




经过查看源码可以发现“ onclick="middleware_connect(0, 4EAE4F474C91156086C0D4EA7E983C69C215B649)" ”，第二个参数是字符串，却没有使用引号括起来，所以引发了此异常。




加上引号后，问题解决：


<input id="btn_0_4EAE4F474C91156086C0D4EA7E983C69C215B649" type="button" value="连接" onclick="middleware_connect('0', '4EAE4F474C91156086C0D4EA7E983C69C215B649')">

问题二：
原因：java代码中的对象goodLists中有空格导致页面eval处理时，解析不了空格，导致数据从空格往后就没有了。


JSONArray oldGoodsList = JSONArray.fromObject(dataList);
String goodList = proStr(oldGoodsList.toString()); mav.addObject("oldGoodsList", goodList);  



 <input type="hidden" id="oldGoodsList" value=eval("("+${oldGoodsList}+")") /> 
//存放条码编码集合
var barCodeList = new Array();
//存放条码编码对应商品信息的集合
var goodsList = new Array();
var temp = $("#oldGoodsList").val();
var strlen0 = temp.length;
var strlen1 = eval(strlen0+"-"+5);
var temp0 = temp.substring(9,strlen1);
var ods = eval(temp0);
console.log(ods)
for(var i=0;i<ods.length;i++){
    goodsList.push(ods[i]);
    barCodeList.push(ods[i]['barCode']);
} 



备注：
goodsList:[{"barCode":"111111111111111111111111111111111111111111111111","name":"旺旺 仙贝 加量装 540g","ruleId":"4589968756864dbea1b85c7bb57d918d","skuId":"051445b719924a138f174b0adebf80b9","spuId":"7c72cae9fb9346d286440e06e35d08c7"}]
eval("("+${oldGoodsList}+")")::[{"barCode":"111111111111111111111111111111111111111111111111","name":"旺旺"


```
##Uncaught SyntaxError: Unexpected token )
```
有可能js代码有问题：
 success: function(data) {
        console.log(data)
//         var source = eval('('+data+')');
//         console.log(source)
        }
eval报错。
```
##Uncaught ReferenceError: 某个函数 is not defined
```
虽然引入了相关函数，但页面中引入的顺序不对导致的。
<script src="${resRoot}/assets/scripts/metronic.js" type="text/javascript"></script>     <script src="${resRoot}/assets/plugins/layout/scripts/layout.js" type="text/javascript"></script> 
比如layout.js引用metronic.js的东西，如果metronic.js引入放在下面就会报错。 

``` ##referenceerror is not defined
```
jsp 页面中引入了 jQuery ,却用不了。< script type = "text/javascript" src = "../js/jquery-1.8.3.js" ></ script >
--原因分析： js / jquery - 1.8 . 3.js 文件放到了/ WEB - INF 目录下，所以引用不了导致。
--解决方法：将 js / jquery - 1.8 . 3.js 拿到外面，可放到 WebContent 目录下。
```
###SyntaxError: missing ] after element list
```
dataType:"json"改为dataType:"html" 或 "text" 或 "text/html"

```
###关于页面跳转
现象：页面验证失败后，页面不应该跳转却跳转了。
原因：页面中有嵌套，而a标签却加了href=""。
解决：将a标签的href属性去掉即可。





