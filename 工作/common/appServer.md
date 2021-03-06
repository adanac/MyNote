##关于mmc-web请求mmcApp-web(appServer)后，抛出异常问题
```
1.mmc-web中的service方法：
@Override
     public  BaseResult modCouponRule2(CouponRuleDto  dto )  throws  SysException {
         log .info( "DisRuleServiceImpl====>modCouponRule=====>CouponRuleDto:"  +  dto .toString());
        BaseResult  br  =  new  BaseResult();
        String  RESTURL  = getAppAddress( dto .getSuppId());
         // RESTURL = "http://192.168.1.173:8082/mmcApp-web";
        String  sign  = MD5. encode ( secretKey  +  dto .getSuppId());
        String  url  =  RESTURL  +  "/crule/mod?sign="  +  sign ;
         try  {
            Map<String, String>  parameters  =  JSONObject. fromObject ( dto ) ;
            String  data  = HttpUtil. doPost ( url ,  parameters , HttpUtil. BODYTYPE_JSON );
            JSONObject  jsonObj  = JSONObject. fromObject ( data );
             br  = (BaseResult) JSONObject. toBean ( jsonObj , BaseResult. class );
        }  catch  (Exception  e ) {
             br  =  new  BaseResult(BaseResult. ERROR , MmcErrorCode. ERROR_MSG_UNKNOW );
        }
         return   br ;     }  
传入的参数：
url:
http://192.168.2.104:8080/mmcApp-web/crule/mod?sign=295ffdec2493c5bb041efb39e4b3a3b1
parameters: ：
{createTime=, createUserId=, endTime=2016-05-19 23:59:59, faceValue=4.0, 
id=464fe3baec7b477db9eace9104dc71e5, issuedMode=1, issuedNum=333, name=111, satisfyMoney=33300.0, 
startTime=2016-05-12, status=0, suppId=1042, surplusNum=0, takeMaxNum=20, type=1, updateTime=, 
updateUserId=} 



appServer返回的数据：


jsonObj:
{"status":"1","errorCode":"","errorMsg":"","content":true}


Exception:
java.lang.NullPointerException


发现请求到Appserver后报错，找到原因为：appserver中的方法没有添加注解：@ResponseBody


2.appServer中的action方法：
@ResponseBody
     @RequestMapping (value =  "mod" , method = RequestMethod. POST , produces =  "application/json" )
     public  BaseResult mod( @RequestBody  CouponRuleDto  ruleDto , String  sign ) {
         log .info( "======start modCoupon======" );
        BaseResult  baseResult  =  new  BaseResult();
         try  {
             // 检验必须参数
             if  (!isNotEmptyArgs( baseResult ,  new  String[] {  "sign" ,  sign  })) {
                 return   baseResult ;
            }
             // 检验签名串
             if  (!validateSign( baseResult ,  sign ,  ruleDto .getSuppId())) {
                 return   baseResult ;
            }
            Boolean  flag  =  disRuleService .modCouponRule( ruleDto );
             if  ( flag ) {
                 baseResult .setContent( flag );
                 baseResult .setStatus(MmcAppErrorCode. SUCCESS );
            }  else  {
                 baseResult .setContent( flag );
                 baseResult .setStatus(MmcAppErrorCode. ERROR );
            }
        }  catch  (Exception  e ) {
             log .error( "modCouponRule error："  +  e .getMessage());
             baseResult .setStatus(MmcAppErrorCode. ERROR );
             baseResult .setErrorMsg( "modCouponRule error" );
        }
         return   baseResult ;     }   

```
##备注：HttpUtil处理请求工具类
```
package com.bn.o2o.mmc.utils;


import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.UnsupportedEncodingException;
import java.net.HttpURLConnection;
import java.net.URL;
import java.net.URLEncoder;
import java.nio.charset.Charset;
import java.util.Iterator;
import java.util.Map;


import org.springframework.http.HttpStatus;
import org.springframework.web.client.HttpClientErrorException;
import org.springframework.web.client.HttpStatusCodeException;


import com.bn.framework.log.MyLogger;
import com.bn.framework.log.MyLoggerFactory;


import net.sf.json.JSONObject;


/**
 * Http 连接处理工具
 * 
 * @author jwang
 * @date 2016年3月18日
 */
public final class HttpUtil {
private static final MyLogger logger = MyLoggerFactory.getLogger(HttpUtil.class);


public static final int BODYTYPE_PARAM = 0;
public static final int BODYTYPE_JSON = 1;


public static final String PARAM_CONNECT_TIMEOUT = "_connectTimeout"; // 连接超时参数
public static final String PARAM_READ_TIMEOUT = "_readTimeout"; // 响应超时参数
public static final int CONNECT_TIMEOUT = 3000;// （单位：毫秒)
public static final int READ_TIMEOUT = 6000;// （单位：毫秒)
public static final int TIMEOUT_WARN = 1000; // 接口请求超时报警


public static final String DEFAULT_ENCODING = "UTF-8";


private static final int BUFFER_SIZE = 4096;


public static void main(String[] args) throws Exception, Exception {
System.out.println(doGet("http://192.168.1.173:8082/mmcApp-web/demo/list.do"));
}


private HttpUtil() {
}


/**
* HTTP GET 请求
* 
* @param reqUrl
*            请求URL
* @return
* @throws HttpStatusCodeException,
*             Exception 返回正文信息
*/
public static String doGet(String restUrl) throws HttpStatusCodeException, Exception {
return doGet(restUrl, null, null);
}


/**
* HTTP GET 请求
* 
* @param reqUrl
*            请求URL
* @param connectTimeout
*            连接超时时间（单位：毫秒)
* @param readTimeout
*            响应超时时间（单位：毫秒)
* @return
* @throws HttpStatusCodeException,
*             Exception 返回正文信息
*/
public static String doGet(String restUrl, Integer connectTimeout, Integer readTimeout)
throws HttpStatusCodeException, Exception {
HttpURLConnection urlConn = null;
try {
URL url = new URL(restUrl);
urlConn = (HttpURLConnection) url.openConnection();
urlConn.setRequestMethod("GET");
urlConn.setRequestProperty("User-Agent", "O2O/Client");
if (connectTimeout == null) {
urlConn.setConnectTimeout(CONNECT_TIMEOUT);
} else {
urlConn.setConnectTimeout(connectTimeout);
}
if (readTimeout == null) {
urlConn.setReadTimeout(READ_TIMEOUT);
} else {
urlConn.setReadTimeout(readTimeout);
}


long time = System.currentTimeMillis();
String ret = getBody(urlConn);
long overtime = System.currentTimeMillis() - time;
if (logger.isWarnEnabled() && overtime > TIMEOUT_WARN) {
logger.warn("over time:" + overtime + " url:" + restUrl);
}
return ret;
} finally {
if (urlConn != null) {
urlConn.disconnect();
urlConn = null;
}
}
}


/**
* Http POST 请求
* 
* @param restUrl
*            请求URL
* @param parameters
*            请求参数
* @return
* @throws HttpStatusCodeException,
*             Exception 返回正文信息
*/
public static String doPost(String restUrl, Map<String, String> parameters)
throws HttpStatusCodeException, Exception {
return doMethod(restUrl, parameters, "POST");
};


/**
* Http POST 请求
* 
* @param restUrl
*            请求URL
* @param parameters
*            请求参数
* @param bodyType
*            0-普通参数提交,1-json格式提交
* @return
* @throws HttpStatusCodeException,
*             Exception 返回正文信息
*/
public static String doPost(String restUrl, Map<String, String> parameters, int bodyType)
throws HttpStatusCodeException, Exception {
return doMethod(restUrl, parameters, "POST", bodyType);
}


/**
* Http PUT 请求
* 
* @param restUrl
*            请求URL
* @param parameters
*            请求参数
* @return
* @throws HttpStatusCodeException,
*             Exception 返回正文信息
*/
public static String doPut(String restUrl, Map<String, String> parameters)
throws HttpStatusCodeException, Exception {
return doMethod(restUrl, parameters, "PUT");
}


/**
* Http PUT 请求
* 
* @param restUrl
*            请求URL
* @param parameters
*            请求参数
* @param bodyType
*            0-普通参数提交,1-json格式提交
* @return
* @throws HttpStatusCodeException,
*             Exception 返回正文信息
*/
public static String doPut(String restUrl, Map<String, String> parameters, int bodyType)
throws HttpStatusCodeException, Exception {
return doMethod(restUrl, parameters, "PUT", bodyType);
}


/**
* Http DELETE 请求
* 
* @param restUrl
*            请求URL
* @param parameters
*            请求参数
* @return
* @throws HttpStatusCodeException,
*             Exception 返回正文信息
*/
public static String doDelete(String restUrl, Map<String, String> parameters)
throws HttpStatusCodeException, Exception {
return doMethod(restUrl, parameters, "DELETE");
}


/**
* 上传文件
* 
* @param uploadUrl
*            上传URL
* @param parameters
*            参数
* @param fileParamName
*            文件参数名
* @param filename
*            文件名称
* @param contentType
*            类型
* @param data
*            文件内容
* @return
* @throws HttpStatusCodeException,
*             Exception 返回正文信息
*/
public static String doUploadFile(String uploadUrl, Map<String, String> parameters, String fileParamName,
String filename, String contentType, byte[] data) throws HttpStatusCodeException, Exception {
HttpURLConnection urlConn = null;
try {
urlConn = sendFormdata(uploadUrl, parameters, fileParamName, filename, contentType, data);
return getBody(urlConn);
} finally {
if (urlConn != null) {
urlConn.disconnect();
}
}
}


/*
* 请求方法
*/
private static String doMethod(String restUrl, Map<String, String> parameters, String method)
throws HttpStatusCodeException, Exception {
return doMethod(restUrl, parameters, method, BODYTYPE_PARAM);
}


/*
* 请求方法
*/
private static String doMethod(String restUrl, Map<String, String> parameters, String method, int bodyType)
throws HttpStatusCodeException, Exception {
HttpURLConnection urlConn = null;
try {
long time = System.currentTimeMillis();
urlConn = send(restUrl, parameters, method, bodyType);
String ret = getBody(urlConn);
long overtime = System.currentTimeMillis() - time;
if (logger.isWarnEnabled() && overtime > TIMEOUT_WARN) {
logger.warn("over time:" + overtime + " url:" + restUrl);
}
return ret;
} finally {
if (urlConn != null) {
urlConn.disconnect();
urlConn = null;
}
}
}


/**
* POST数据表单
* 
* @param restUrl
* @param parameters
* @param connectTimeout
*            连接超时时间
* @return
* @throws Exception
*/
private static HttpURLConnection send(String restUrl, Map<String, String> parameters, String method, int bodyType)
throws Exception {
HttpURLConnection urlConn = null;
try {
String connectTimeout = parameters.remove(PARAM_CONNECT_TIMEOUT);
String readTimeout = parameters.remove(PARAM_READ_TIMEOUT);
String params = "";
URL url = new URL(restUrl);
urlConn = (HttpURLConnection) url.openConnection();
urlConn.setRequestMethod("POST");
urlConn.setRequestProperty("User-Agent", "O2O/Client");
urlConn.setConnectTimeout(valueOf(connectTimeout, CONNECT_TIMEOUT));
urlConn.setConnectTimeout(valueOf(readTimeout, READ_TIMEOUT));
if (bodyType == BODYTYPE_JSON) {
params = JSONObject.fromObject(parameters).toString();
urlConn.setRequestProperty("Content-Type", "application/json");
} else {
params = generatorParamString(parameters);
}
urlConn.setDoOutput(true);
byte[] b = params.getBytes();
urlConn.getOutputStream().write(b, 0, b.length);
urlConn.getOutputStream().flush();


} finally {
if (urlConn != null) {
urlConn.getOutputStream().close();
}
}
return urlConn;
}


/**
* 上传文件表单
* 
* @param uploadUrl
* @param parameters
* @param fileParamName
* @param filename
* @param contentType
* @param data
* @return
* @throws Exception
*/
private static HttpURLConnection sendFormdata(String uploadUrl, Map<String, String> parameters,
String fileParamName, String filename, String contentType, byte[] data) throws Exception {
HttpURLConnection urlConn = null;
OutputStream os = null;
try {
URL url = new URL(uploadUrl);
urlConn = (HttpURLConnection) url.openConnection();
urlConn.setRequestMethod("POST");
String timeout = parameters.remove(PARAM_CONNECT_TIMEOUT);
urlConn.setConnectTimeout(valueOf(timeout, CONNECT_TIMEOUT));
urlConn.setDoOutput(true);


urlConn.setRequestProperty("connection", "keep-alive");


String boundary = "-----------------------------114975832116442893661388290519"; // 分隔符
urlConn.setRequestProperty("Content-Type", "multipart/form-data; boundary=" + boundary);


boundary = "--" + boundary;
StringBuffer params = new StringBuffer();
if (parameters != null) {
for (Iterator<String> iter = parameters.keySet().iterator(); iter.hasNext();) {
String name = iter.next();
String value = parameters.get(name);
params.append(boundary + "\r\n");
params.append("Content-Disposition: form-data; name=\"" + name + "\"\r\n\r\n");
params.append(URLEncoder.encode(value, DEFAULT_ENCODING));
// params.append(value);
params.append("\r\n");
}
}


StringBuilder sb = new StringBuilder();
// sb.append("\r\n");
sb.append(boundary);
sb.append("\r\n");
sb.append("Content-Disposition: form-data; name=\"" + fileParamName + "\"; filename=\"" + filename
+ "\"\r\n");
sb.append("Content-Type: " + contentType + "\r\n\r\n");
byte[] fileDiv = sb.toString().getBytes();
byte[] endData = ("\r\n" + boundary + "--\r\n").getBytes();
byte[] ps = params.toString().getBytes();


os = urlConn.getOutputStream();
os.write(ps);
os.write(fileDiv);
os.write(data);
os.write(endData);


os.flush();


} finally {
if (os != null) {
os.close();
}
}
return urlConn;
}


/**
* Return the body of the message as an input stream.
* 
* @return the input stream body
* @throws IOException
*             in case of I/O Errors
*/
private static String getBody(HttpURLConnection urlConn) throws Exception {
ByteArrayOutputStream out = new ByteArrayOutputStream(BUFFER_SIZE);
if (urlConn.getResponseCode() != 200) {
copy(urlConn.getErrorStream(), out);
throw new HttpClientErrorException(HttpStatus.valueOf(urlConn.getResponseCode()),
urlConn.getResponseMessage(), out.toByteArray(), Charset.forName(DEFAULT_ENCODING));
} else {
copy(urlConn.getInputStream(), out);
return new String(out.toByteArray(), DEFAULT_ENCODING);
}
}


/**
* 将parameters中数据转换成用"&"链接的http请求参数形式
* 
* @param parameters
* @return
* @throws UnsupportedEncodingException
*/
public static String generatorParamString(Map<String, String> parameters) throws UnsupportedEncodingException {
StringBuffer params = new StringBuffer();
if (parameters != null) {
for (Iterator<String> iter = parameters.keySet().iterator(); iter.hasNext();) {
String name = iter.next();
String value = parameters.get(name);
if (value != null) {
params.append(name).append("=");
params.append(URLEncoder.encode(value, DEFAULT_ENCODING));
}
if (iter.hasNext()) {
params.append("&");
}
}
}
return params.toString();
}


/**
* Copy the contents of the given InputStream to the given OutputStream.
* Closes both streams when done.
* 
* @param in
*            the stream to copy from
* @param out
*            the stream to copy to
* @return the number of bytes copied
* @throws IOException
*             in case of I/O errors
*/
public static int copy(InputStream in, OutputStream out) throws IOException {
try {
int byteCount = 0;
if (in == null) {
return byteCount;
}
byte[] buffer = new byte[BUFFER_SIZE];
int bytesRead = -1;
while ((bytesRead = in.read(buffer)) != -1) {
out.write(buffer, 0, bytesRead);
byteCount += bytesRead;
}
out.flush();
return byteCount;
} finally {
try {
in.close();
} catch (IOException ex) {
}
try {
out.close();
} catch (IOException ex) {
}
}
}


private static Integer valueOf(String val, Integer defaultVal) {
if (null != val) {
try {
defaultVal = Integer.valueOf(val);
} catch (NumberFormatException e) {
}
}
return defaultVal;
}
}
```
##应用举例，查询列表
```
1.mmc-web中的action:
@ResponseBody
     @RequestMapping (value =  "ajaxList" )
     public  String ajaxList(HttpServletRequest  request , BootstrapPage  page , CouponRuleDto  param ) {
         log .info( "DisRuleAction====>ajaxList====>param:"  +  param .toString());
         try  {
             if  ( null  !=  param .getStartTime() && ! "" .equals( param .getStartTime())) {
                 param .setStartTime( param .getStartTime());
            }
             if  ( null  !=  param .getEndTime() && ! "" .equals( param .getEndTime())) {
                 param .setEndTime( param .getEndTime());
            }
            CompanyInfoDto  companyInfoDto  =  this .getMerchantInfo( request );
            String  suppId  =  companyInfoDto .getCompanyId().toString();
             // 从session中获取供应商id
            PagerParam  pageParam  =  new  PagerParam();
             pageParam .setPageNumber( page .getPageNumber());
             pageParam .setPageSize( page .getPageSize());
            String  name  = java.net.URLDecoder. decode ( param .getName(),  "utf-8" );
            Pager<CouponRuleDto>  pager  =  disRuleService .queryList2( suppId ,  name ,  param .getStatus(),
                     param .getStartTime(),  param .getEndTime(),  param .getType(),  pageParam );
            List<CouponRuleDto>  infoList  =  pager .getDatas();
            BootstrapTable<CouponRuleDto>  data  =  new  BootstrapTable<CouponRuleDto>( infoList ,
                     pager .getTotalDataCount() ==  null  ? 0 :  pager .getTotalDataCount(),  pager .getPageNumber());
             return  JSONObject. fromObject ( data ).toString();
        }  catch  (Exception  e ) {
             log .error( "DisRuleAction====>ajaxList====>error：" ,  e .toString());
             return   "" ;
        }     }  
2.mmc-web中的service方法：
@Override
     public  Pager<CouponRuleDto> queryList2(String  suppid , String  name , Integer  status , String  startTime , String  endTime ,
            Integer  type , PagerParam  pageParam )  throws  SysException {
         log .info( "DisRuleServiceImpl====>queryList====>name:"  +  name  +  ", status:"  +  status );
        Pager<CouponRuleDto>  rules  =  null ;
        String  RESTURL  = getAppAddress( suppid );
        String  sign  = MD5. encode ( secretKey  +  suppid );
        String  url  =  RESTURL  +  "/crule/getCouponList.do?" ;
         try  {
             if  (!StringUtils. isEmpty ( suppid )) {
                 url  +=  "suppId="  +  suppid ;
            }
             if  (!StringUtils. isEmpty ( name )) {
                 url  +=  "&name="  +  name ;
            }
             if  ( status  !=  null ) {
                 url  +=  "&status="  +  status .toString();
            }
             if  (!StringUtils. isEmpty ( startTime )) {
                 url  +=  "&startTime="  +  startTime ;
            }
             if  (!StringUtils. isEmpty ( endTime )) {
                 url  +=  "&endTime="  +  endTime ;
            }
             if  ( type  !=  null ) {
                 url  +=  "&type="  +  type .toString();
            }
             url  +=  "&pageSize="  +  pageParam .getPageSize() +  "&pageNumber="  +  pageParam .getPageNumber() +  "&sign="
                    +  sign ;
            String  result  = HttpUtil. doGet ( url );
             if  (StringUtils. isEmpty ( result )) {
                 rules  =  new  Pager<CouponRuleDto>();
            }  else  {
                BaseResult  br  = com.alibaba.fastjson.JSONObject. parseObject ( result , BaseResult. class );
                 if  ( br .getStatus().equals(BaseResult. ERROR )) {
                     return   null ;
                }
                com.alibaba.fastjson.JSONObject  re  = (com.alibaba.fastjson.JSONObject)  br .getContent();
                 rules  =  new  Pager<>();
                com.alibaba.fastjson.JSONArray  datas  =  re .getJSONArray( "rows" );
                 // skus = JSON.toJavaObject(re, Pager.class);
                List<CouponRuleDto>  ruleDtos  = com.alibaba.fastjson.JSONArray. parseArray ( datas .toJSONString(),
                        CouponRuleDto. class );
                 rules .setDatas( ruleDtos );
                 rules .setIsLastPage( re .getBoolean( "indexNumber" ));
                 rules .setPageCount( pageParam .getPageSize());
                 rules .setPageNumber( pageParam .getPageNumber());
                 rules .setPageSize( pageParam .getPageSize());
                 rules .setTotalDataCount( re .getInteger( "total" ));
            }
        }  catch  (Exception  e ) {
             e .printStackTrace();
        }
         return   rules ;
    }
3.mmcApp-web中的Action方法：
@ResponseBody
     @RequestMapping (value =  "getCouponList" , method = RequestMethod. GET , produces =  "application/json" )
     public  BaseResult getCouponList(CouponRuleDto  param , Integer  pageNumber , Integer  pageSize , String  sign ) {
         log .info( "======start getCouponList====="  +  param .toString());
        BaseResult  baseResult  =  new  BaseResult();
         try  {
             // 检验必须参数
             if  (!isNotEmptyArgs( baseResult ,  new  String[] {  "sign" ,  sign  })) {
                 return   baseResult ;
            }
             // 检验签名串
             if  (!validateSign( baseResult ,  sign ,  param .getSuppId())) {
                 return   baseResult ;
            }
             if  ( null  !=  param .getStartTime() && ! "" .equals( param .getStartTime())) {
                 param .setStartTime( param .getStartTime());
            }
             if  ( null  !=  param .getEndTime() && ! "" .equals( param .getEndTime())) {
                 param .setEndTime( param .getEndTime());
            }
            String  suppId  =  param .getSuppId();
            PagerParam  pageParam  =  new  PagerParam();
             pageParam .setPageNumber( pageNumber );
             pageParam .setPageSize( pageSize );
            String  name  = java.net.URLDecoder. decode ( param .getName() ==  null  ?  ""  :  param .getName(),  "utf-8" );
            Pager<CouponRuleDto>  pager  =  disRuleService .queryList( suppId ,  name ,  param .getStatus(),  param .getStartTime(),
                     param .getEndTime(),  param .getType(),  pageParam );
            List<CouponRuleDto>  infoList  =  pager .getDatas();
            BootstrapTable<CouponRuleDto>  data  =  new  BootstrapTable<CouponRuleDto>( infoList ,
                     pager .getTotalDataCount() ==  null  ? 0 :  pager .getTotalDataCount(),  pager .getPageNumber());
             baseResult .setContent( data );
             baseResult .setStatus(MmcAppErrorCode. SUCCESS );
        }  catch  (Exception  e ) {
             log .error( "====getCouponList error：" ,  e .getMessage());
             baseResult .setStatus(MmcAppErrorCode. ERROR );
             baseResult .setErrorMsg( "getCouponList error" );
        }
         return   baseResult ;
    }
4mmcApp中的service方法：（跟修改为调用appServer中的方法一样，调用中台）
@Override
     public  Pager<CouponRuleDto> queryList(String  suppid , String  name , Integer  status , String  startTime , String  endTime ,
            Integer  type , PagerParam  pageParam )  throws  SysException {
         log .info( "DisRuleServiceImpl====>queryList====>name:"  +  name  +  ", status:"  +  status );
        Pager<CouponRuleDto>  dataPage  =  null ;
         try  {
             dataPage  =  couponRuleService .queryList( suppid ,  name ,  status ,  startTime ,  endTime ,  type ,  pageParam );
        }  catch  (Exception  e ) {
             log .error( "DisRuleServiceImpl-->queryList====>error" ,  e .toString());
        }
         return   dataPage ;
    }  

```
##应用举例：添加 
```
1-1.mmc-web中的Action:
@RequestMapping (value =  "add" , method = RequestMethod. POST )
     public  String add(HttpServletRequest  request , HttpServletResponse  response , XSActivityDto  activity ) {
        CompanyInfoDto  companyInfoDto  =  this .getMerchantInfo( request );
        String  suppId  =  companyInfoDto .getCompanyId().toString();
         activity .setSuppId( suppId );
        BaseResult  br  =  disCountService .addActivity2( activity );
         return  ajaxJson( response , JSONObject. fromObject ( br ).toString());     }  
1-2.mmc-web中的service:
@Override
     public  BaseResult addActivity2(XSActivityDto  dto ) {
         log .info( "DisCountServiceImpl====>addActivity=====>XSActivityDto:"  +  dto .toString());
        BaseResult  br  =  new  BaseResult();
        String  RESTURL  = getAppAddress( dto .getSuppId());
         try  {
            String  url  =  RESTURL  +  "/xs/add.do" ;
            Map<String, String>  parameters  =  JSONObject. fromObject ( dto ) ;
            String  data  = HttpUtil. doPost ( url ,  parameters , HttpUtil. BODYTYPE_JSON );
            JSONObject  jsonObj  = JSONObject. fromObject ( data );
             br  = (BaseResult) JSONObject. toBean ( jsonObj , BaseResult. class );
        }  catch  (Exception  e ) {
             br .setContent(MmcErrorCode. ERROR_MSG_UNKNOW );
             br .setStatus(BaseResult. ERROR );
        }
         return   br ;
    }
2-1.mmcApp-web中的 Action:

@ResponseBody
     @RequestMapping (value =  "add" , method = RequestMethod. POST , produces =  "application/json" )
     public  BaseResult add( @RequestBody  XSActivityDto  activity ) {
         log .info( "======start addActivity======" );
        BaseResult  baseResult  =  new  BaseResult();
         try  {
             baseResult  =  disCountService .addActivity( activity );
        }  catch  (Exception  e ) {
             log .error( "addActivity error："  +  e .getMessage());
             baseResult .setStatus(MmcAppErrorCode. ERROR );
             baseResult .setErrorMsg( "addActivity error" );
        }
         return   baseResult ;     }  
2-2.mmcApp-web中的Service:
@Override
     public  BaseResult addActivity(XSActivityDto  dto ) {
        BaseResult  br  =  new  BaseResult();
         // 添加校验
         // 判断开始时间和结束时间
        String  start  =  dto .getStartDttm(); // 开始时间
        String  end  =  dto .getEndDttm(); // 结束时间
        Date  resStart  = DateUtils. parse ( start , DateUtils. DEFAULT_YEAR_MONTH_DAY_HMS ); // 转换后的开始时间
        Date  resEnd  = DateUtils. parse ( end , DateUtils. DEFAULT_YEAR_MONTH_DAY_HMS ); // 转换后的结束时间
         if  ( resStart .compareTo( resEnd ) != -1)  // 比较开始和结束时间,如果开始时间小于结束时间
        {
             br .setStatus(BaseResult. ERROR );
             br .setErrorMsg( "开始时间不能大于结束时间" );
             return   br ;
        }
         try  {
             dto .setStatus(XSActivityStatusEnum. STATUS_HAS_START .getValue());
            String  uuid  =  xSActivityService .addActivity( dto );
             if  (!StringUtils. isEmpty ( uuid )) {
                ActivityDto  activity  =  new  ActivityDto();
                setValueFromXSAactivity( dto ,  activity );
                 activity .setActivityId( uuid );
                 activity .setType(ActivityTypeEnum. TYPE_XS .getValue());
                 activity .setId(DefaultSequenceGenerator. getInstance ().uuid());
                 activityService .addActivity( activity );
            }
             br .setContent( uuid );
             br .setStatus(BaseResult. SUCCESS );
        }  catch  (Exception  e ) {
             log .error( "添加活动失败" ,  e );
             br .setStatus(BaseResult. ERROR );
             br .setErrorMsg( "添加活动失败" );
        }
         return   br ;
    }
```
##应用举例3：查询
```
import  com.alibaba.fastjson.JSON;
import  com.alibaba.fastjson.JSONArray; import  com.alibaba.fastjson.JSONObject;   

1.mmc-web中的service:
     /**
     * 功能描述:根据商品ID，检索商品  <br>
     */
     public  BaseResultDto<SpuDto> querySpuById(String  spuId , String  companyId ) {
        BaseResultDto<SpuDto>  brd  =  new  BaseResultDto<>();
        BaseResult  br  =  new  BaseResult();
        String  sign  = MD5. encode ( secretKey  +  spuId );
         try  {
            String  url  = getAddress( companyId ) +  "/spu/querySpuById/"  +  spuId  +  "?sign="  +  sign ;
            String  result  = HttpUtil. doGet ( url );
             br  = JSONObject. parseObject ( result , BaseResult. class );
             brd .setStatus( br .getStatus());
            SpuDto  dto  = JSON. toJavaObject ((JSONObject)  br .getContent(), SpuDto. class );
             brd .setResult( dto );
        }  catch  (Exception  e ) {
             log .error(MmcErrorCode. CONN_APP_EXC_MSG );
             e .printStackTrace();
            setErrors( brd );
        }
         return   brd ;     }  
2.mmcApp-web中的action:
     /**
     * 需要验证签名的字段为： spuId 查询SpuId
     */
     @ResponseBody
     @RequestMapping (value =  "/querySpuById1/{spuId}" , method = RequestMethod. GET )
     public  BaseResult querySpuById1( @PathVariable  String  spuId ,String  sign ) {
         log .info( "querySpuById1====InParams====>spuId:"  +  spuId );
        BaseResult  baseResult  =  new  BaseResult();
         try  {
             if  (!isNotEmptyArgs( baseResult ,  new  String[] {  "sign" ,  sign  })) {
                 return   baseResult ;
            }
             // 检验签名串
             if  (!validateSign( baseResult ,  sign ,  spuId )) {
                 return   baseResult ;
            }
             baseResult  =  productService .querySpuById1( spuId );
        }  catch  (Exception  e ) {
             log .error( "querySpuById1 error: "  +  e .getMessage());
             baseResult .setStatus(MmcAppErrorCode. ERROR );
             baseResult .setErrorCode(MmcAppErrorCode. DEMO_ERROR );
             baseResult .setErrorMsg( "获取商品信息失败" );
        }
         return   baseResult ;     }  
3.mmcApp-web中的service:
       /**
     * 功能描述:根据商品ID，检索商品  <br>
     */
     public  BaseResult querySpuById1(String  spuId )
    {
        BaseResult  result  =  new  BaseResult();
        SpuDto  spuDto  =  spuQueryService .getSpuBySpuId( spuId );
        SearchGoodsDto  sDto  =  goodsSearchService .getBySkuId( spuId );
        String  className  = listToString2( sDto );
         spuDto .setClassPath( className );
         result .setStatus(BaseResultDto. SUCCESS );
         result .setContent(JSON. toJSON ( spuDto ));
         return   result ;
    }  

 

```



