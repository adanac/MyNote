##消息推送
###知识点

```
1. 消息推送的状态
分为： 推送中，已推送，推送失败
2.消息事件类型
分为文本、多图文、关键字


1.消息推送列表页面：需要分页处理，而多图文页面不需要分页处理

场景描述：
1、商家管理人员在系统中维护信息推送内容；
2、推动后，微信可以接收相应的推动内容；
此页面是消息推送列表页面；数据按照维护时间倒序排序；

消息状态分为已推送、推送中、推送失败；推送中属于未到推送时间的数据；
需要根据消息状态判断操作功能；推送中：编辑、删除、复制；
已推送：删除、复制；推送失败：编辑、删除、复制；
点击复制后，直接跳转到编辑页面，但是带出所有信息，用户编辑完成后，保存推送后再次生成新数据；


新增/编辑
此页面所有字段全部必填；

推送时间可选择立即推送（即保存后就开始推送）；选择自定义，出现时间控件；
推送人员默认选中全部人员（即全部会员）；
消息内容：分为文本、多图文、关键字；选择后出现对应的内容框；文本可自定义；
多图文的数据选择为图文库中的内容；
关键字的数据选择为关键字回复中已启用的关键字数据；
多图文的维护逻辑与关键字回复中保持一致；
对于消息属性，此页面进行保存时，校验当前页数据，保存也只保存当前页面的数据；
```
###表的关系
```
总共有5张表：`wx_keyword` (关键字)`wx_msg_resouces_ref` (消息与多图文的映射表) `wx_msg_template`(消息模板表) `wx_push_task` （消息推送表）`wx_resources`(多图文表)
注意点：
1.区分文本、多图文与关键字
文本： msgType = 4&&contentType =1
多图文：msgType=4&&contentType=2
关键字：msgType=4(contentType=1 || contentType=2)
2.由于编辑时，消息类型是可以改变的，可由文本-->多图文，也可以由关键字-->文本
如果原回复不是关键字回复，则删除该推送任务的原消息模板，执行msgTemplateService.delMsgTemplate(tmpTask.getMsgId());

代码如下：
@Override
public Integer modPushTask(PushTaskDto taskDto) {
try {
//获取原来的推送任务
PushTaskDto tmpTask = getPushTask(taskDto.getId());
//获取原消息模板
MsgTemplateDto tmpMsg = msgTemplateService.getMsgTemplate(tmpTask.getMsgId());

//如果原回复不是关键字回复，则删除该推送任务的原消息模板
if(MsgTemplateType.passive.getValue()!=tmpMsg.getMsgType().intValue()){
msgTemplateService.delMsgTemplate(tmpTask.getMsgId());
}

//生成消息模板id，保存消息模板
String msgId = UUIDGenerator.getInstance().get64BitUUID();
MsgTemplateDto msgDto = taskDto.getMsgTemplateDto();
msgDto.setId(msgId);
msgTemplateService.addMsgTemplate(msgDto);

//给推送任务设置保存的消息模板Id
taskDto.setMsgId(msgId);
Map<String, Object> paramMap = DacUtils.convertToMap(taskDto);
baseDao.execute("pushTask.update", paramMap);
return RespStatus.success.getValue();
} catch (Exception e) {
logger.error("modPushTask:{}", e);
throw new SysException(e);
}
}


```


###执行过程跟踪
```
1.消息推送新增：
js:
// 新增/编辑文本 
        saveText :  function (oper) {
            $( '.btn-save1' ).attr( 'disabled' ,  true );
             var  taskId = $( '#taskId' ).val(); // 编辑才会有
             var  msgId = $( "#msgId" ).val();; // 编辑才会有
            
             var  title = $( '#title' ).val(); // 消息主题
             var  pushmode = $( 'input[name="pushmode"]:checked' ).val(); // 推送时间
             var  pushtime = "" ;
             if (pushmode ==  "1" ){ //立即推送
                 var  now =  new  Date();
                 var  pushDate = now.getFullYear()+ "-" +((now.getMonth()+1)<10? "0" : "" )+(now.getMonth()+1)+ "-" +(now.getDate()<10? "0" : "" )+now.getDate()+ " " +(now.getHours()<10? "0" : "" )+now.getHours()+ ":" +(now.getMinutes()<10? "0" : "" )+now.getMinutes()+ ":" +(now.getSeconds()<10? "0" : "" )+now.getSeconds();
                console.log(pushDate);
                pushtime = pushDate;
            } else { //自定义 时间
                 var  pushDate = $( "#pushDate" ).val();
                 var  pushHour = $( "#pushHour" ).val();
                pushtime = pushDate +  " " +pushHour+ ":00:00" ;
            }
             var  contentType = $( '.contentType:checked' ).val(); // 消息属性 肯定是1
             var  pubnum = $( '#pubnum' ).val();
             var  msgContent = $( "#msgContent" ).val();
             var  params = {
                title : title,
                pushType :  '0' , //全员推送
                pushmode : pushmode, //是否立即推送
                pushtime : pushtime,
                msgType :  '4' , // 消息类型 1: 关注回复 2:自动回复 3:被动消息 4:推送消息
                pubnum : pubnum,
                contentType : contentType,
                 // status:1,//初始值1，启用
                msgContent : msgContent
            };
             var  url = basePath +  '/message/' ;
             if  (taskId&&oper== "1" ) { // 编辑
                params.id = taskId;
                params.msgId = msgId;
                url +=  'mod.do' ;
            } else  { // 新增
                url +=  'add.do' ;
            }
             var  paramJson = encodeURI(encodeURI(JSON.stringify(params)));
            url +=  '?paramJson='  + paramJson;
            $.CommonUtils.ajaxPost({
                        url : url,
                        successFun :  function (data) {
                             if  (data.status ==  '1' ) {
                                layer.msg(data.errorMsg ||  '操作成功!' , {
                                            time : 1000
                                        },  function () {
                                            $.CommonUtils
                                                    .redirect(basePath
                                                            +  '/message/main.do' );
                                        });
                            }  else  {
                                layer.msg(data.errorMsg ||  '操作失败' );
                                $( '.btn-save1' ).attr( 'disabled' ,  false );
                            }
                        }
                    });         },  


// 新增/编辑图文
        saveImgText :  function (oper) {
            $( '.btn-save2' ).attr( 'disabled' ,  true );
             var  taskId = $( '#taskId' ).val(); // 编辑才会有
             var  msgId = $( "#msgId" ).val(); // 编辑才会有
            
             var  title = $( '#title' ).val(); // 消息主题
             var  pushmode = $( 'input[name="pushmode"]:checked' ).val(); // 推送时间
             var  pushtime = "" ;
             if (pushmode ==  "1" ){ //立即推送
                 var  now =  new  Date();
                 var  pushDate = now.getFullYear()+ "-" +((now.getMonth()+1)<10? "0" : "" )+(now.getMonth()+1)+ "-" +(now.getDate()<10? "0" : "" )+now.getDate()+ " " +(now.getHours()<10? "0" : "" )+now.getHours()+ ":" +(now.getMinutes()<10? "0" : "" )+now.getMinutes()+ ":" +(now.getSeconds()<10? "0" : "" )+now.getSeconds();
                console.log(pushDate);
                pushtime = pushDate;
            } else { //自定义 时间
                 var  pushDate = $( "#pushDate" ).val();
                 var  pushHour = $( "#pushHour" ).val();
                pushtime = pushDate +  " " +pushHour+ ":00:00" ;
            }
             var  contentType = $( '.contentType:checked' ).val(); // 消息属性 肯定是2
             var  pubnum = $( '#pubnum' ).val();
            
             // 取图文列表的值
             var  trs = $( "table.table tbody tr" );
             var  tempResList = [];
             for  ( var  i = 0; i < trs.length; i++) {
                 var  tempres = {};
                tempres.rid = $(trs[i]).children().first().find( 'input' )
                        .first().val();
                tempres.sort = $(trs[i]).children()[2].innerHTML;
                tempres.remark = $(trs[i]).children().first().find( 'input' )
                        .last().val();
                tempResList.push(tempres);
            }
            
             var  params = {
                title : title,
                pushType :  '0' , //全员推送
                pushmode : pushmode, //是否立即推送
                pushtime : pushtime,
                msgType :  '4' , // 消息类型 1: 关注回复 2:自动回复 3:被动消息 4:推送消息
                contentType : contentType,
                pubnum : pubnum,
                 // status:1,//初始值1，启用
                tempResList : tempResList
            };
            
             var  url = basePath +  '/message/' ;
             if  (taskId&&oper== "1" ) { // 编辑
                params.id = taskId;
                params.msgId = msgId;
                url +=  'mod.do' ;
            } else  { // 新增
                url +=  'add.do' ;
            }
             var  paramJson = encodeURI(encodeURI(JSON.stringify(params)));
            url +=  '?paramJson='  + paramJson;
            $.CommonUtils.ajaxPost({
                        url : url,
                        successFun :  function (data) {
                             if  (data.status ==  '1' ) {
                                layer.msg(data.errorMsg ||  '操作成功!' , {
                                            time : 1000
                                        },  function () {
                                            $.CommonUtils
                                                    .redirect(basePath
                                                            +  '/message/main.do' );
                                        });
                            }  else  {
                                layer.msg(data.errorMsg ||  '操作失败' );
                                $( '.btn-save2' ).attr( 'disabled' ,  false );
                            }
                        }
                    });
        },
        
         // 新增/编辑关键字
        saveKeyWord :  function (oper) {
            $( '.btn-save3' ).attr( 'disabled' ,  true );
             var  taskId = $( '#taskId' ).val(); // 编辑才会有
             var  msgId = $( "#msgId" ).val();; // 编辑才会有
            
             var  title = $( '#title' ).val(); // 消息主题
             var  pushmode = $( 'input[name="pushmode"]:checked' ).val(); // 推送时间
             var  pushtime = "" ;
             if (pushmode ==  "1" ){ //立即推送
                 var  now =  new  Date();
                 var  pushDate = now.getFullYear()+ "-" +((now.getMonth()+1)<10? "0" : "" )+(now.getMonth()+1)+ "-" +(now.getDate()<10? "0" : "" )+now.getDate()+ " " +(now.getHours()<10? "0" : "" )+now.getHours()+ ":" +(now.getMinutes()<10? "0" : "" )+now.getMinutes()+ ":" +(now.getSeconds()<10? "0" : "" )+now.getSeconds();
                console.log(pushDate);
                pushtime = pushDate;
            } else { //自定义 时间
                 var  pushDate = $( "#pushDate" ).val();
                 var  pushHour = $( "#pushHour" ).val();
                pushtime = pushDate +  " " +pushHour+ ":00:00" ;
            }
             var  contentType = $( '.contentType:checked' ).val(); // 消息属性 肯定是2
             var  pubnum = $( '#pubnum' ).val();
            
             var  params = {
                title : title,
                pushType :  '0' , //全员推送
                pushmode : pushmode, //是否立即推送
                pushtime : pushtime,
                msgType :  '3' , // 消息类型 1: 关注回复 2:自动回复 3:被动消息 4:推送消息
                contentType : contentType,
                pubnum : pubnum,
                 // status:1,//初始值1，启用
                msgId : msgId
            };
            
             var  url = basePath +  '/message/' ;
             if  (taskId&&oper== "1" ) { // 编辑
                params.id = taskId;
                url +=  'mod.do' ;
            }  else  { // 新增
                url +=  'add.do' ;
            }
             var  paramJson = encodeURI(encodeURI(JSON.stringify(params)));
            url +=  '?paramJson='  + paramJson;
            $.CommonUtils.ajaxPost({
                        url : url,
                        successFun :  function (data) {
                             if  (data.status ==  '1' ) {
                                layer.msg(data.errorMsg ||  '操作成功!' , {
                                            time : 1000
                                        },  function () {
                                            $.CommonUtils
                                                    .redirect(basePath
                                                            +  '/message/main.do' );
                                        });
                            }  else  {
                                layer.msg(data.errorMsg ||  '操作失败' );
                                $( '.btn-save3' ).attr( 'disabled' ,  false );
                            }
                        }
                    });         },   



2.Action:
@Controller
@RequestMapping (value =  "message" , produces =  "text/html;charset=UTF-8" )
public   class  PushMessageAction  extends  MmcAction {
/**
     * 保存消息推送
     * 
     *  @return
     */
     @ResponseBody
     @RequestMapping (value =  "add" , method = RequestMethod. POST , produces =  "application/json" )
     public  BaseResult add(HttpServletRequest  request ,  @RequestParam  String  paramJson ) {
        BaseResult  br ;
         try  {
             paramJson  = java.net.URLDecoder. decode ( paramJson ,  "utf-8" );
            CompanyInfoDto  companyInfoDto  =  this .getMerchantInfo( request );
            String  companyId  =  companyInfoDto .getCompanyId().toString();
            JSONObject  paramMap  = JSON. parseObject ( paramJson );
            LoginUser  loginUser  = (LoginUser)  request .getSession().getAttribute(MmcConstants. LOGIN_USER );
            Long  userId  =  loginUser .getUserId();
             paramMap .put( "createUserId" ,  userId .toString());
             paramMap .put( "updateUserId" ,  userId .toString());
             br  =  pushMessageService .addPushTask( paramMap ,  companyId );
        }  catch  (Exception  e ) {
             log .error( "addMsg error:"  +  e .getMessage());
             br  =  new  BaseResult(BaseResult. ERROR , MmcErrorCode. ERROR_MSG_UNKNOW );
        }
         return   br ;
    }
}


3.service:
public  BaseResult addPushTask(JSONObject  params , String  companyId )  throws  SysException {
        BaseResult  br  =  new  BaseResult();
        String  RESTURL  = getAddress( companyId );
        String  sign  = MD5. encode ( secretKey  +  companyId );
        String  url  =  RESTURL  +  "/push/add.do?sign="  +  sign ;
         try  {
            PushTaskDto  pushTaskDto  = buildPushTaskDto( params );
             if  ( pushTaskDto  ==  null ) {
                 br  =  new  BaseResult();
                 br .setErrorMsg( "消息属性选择图文，但没有发现图文内容" );
                 br .setStatus(BaseResult. ERROR );
                 return   br ;
            }
             @SuppressWarnings ( "unchecked" )
            Map<String, String>  parameters  = (Map<String, String>) JSON. toJSON ( pushTaskDto );
            String  data  = HttpUtil. doPost ( url ,  parameters , HttpUtil. BODYTYPE_JSON );
             br  = JSONObject. parseObject ( data , BaseResult. class );
        }  catch  (Exception  e ) {
             log .error(MmcErrorCode. CONN_APP_EXC_MSG );
             e .printStackTrace();
            setErrors( br );
        }
         return   br ;
    }


4.AppServer的Action:
/**
     * 保存消息推送
     * 
     *  @return
     */
     @ResponseBody
     @RequestMapping (value =  "/add" , method = RequestMethod. POST , produces =  "application/json" )
     public  BaseResult add( @RequestBody  PushTaskDto  taskDto , String  sign , String  companyId ) {
         log .info( "====start add====" );
        BaseResult  br  =  new  BaseResult();
         try  {
             // 检验必须参数
             if  (!isNotEmptyArgs( br ,  new  String[] {  "sign" ,  sign  })) {
                 return   br ;
            }
             // 检验签名串
             if  (!validateSign( br ,  sign ,  companyId )) {
                 return   br ;
            }
            Integer  flag  =  pushMessageService .addPushTask( taskDto );
             br .setContent( flag );
             br .setStatus(BaseResult. SUCCESS );
        }  catch  (Exception  e ) {
             log .error( "addMsg error:"  +  e .getMessage());
            setError( br );
        }
         return   br ;
    }


5.AppServer的Service:
@Service
public   class  PushMessageServiceImpl  implements  PushMessageService {
     @Reference (check =  false )
    PushTaskService  pushTaskService ;
     @Override
     public  Integer  addPushTask    (PushTaskDto  taskDto )  throws  SysException {
         return   pushTaskService . addPushTask    ( taskDto );
    }
} 

6.中台的service:
@Service("pushTaskService")
@com.alibaba.dubbo.config.annotation.Service(protocol = {"dubbo"})
public class PushTaskServiceImpl implements PushTaskService {
          @Autowired
private BaseDao baseDao;
          @Override
public Integer addPushTask(PushTaskDto taskDto) {
if(!taskDto.check()){
return RespStatus.paramError.getValue();
}
try {
//msgId为空表示推送的是图文消息或者文本消息，此时新增消息模板。如果是关键字则直接保存
if(taskDto.getMsgId()==null){
//生成消息模板id
String msgId = UUIDGenerator.getInstance().get64BitUUID();
MsgTemplateDto msgDto = taskDto.getMsgTemplateDto();
msgDto.setId(msgId);
msgTemplateService.addMsgTemplate(msgDto);

//给推送任务设置保存的消息模板Id
taskDto.setMsgId(msgId);
}

//设置默认的推送状态
taskDto.setStatus(PushTaskStatus.pushing.getValue());

String id = UUIDGenerator.getInstance().get64BitUUID();
taskDto.setId(id);

Map<String, Object> paramMap = DacUtils.convertToMap(taskDto);
baseDao.execute("pushTask.insert", paramMap);
return RespStatus.success.getValue();
} catch (Exception e) {
logger.error("addPushTask:{}", e);
throw new SysException(e);
}
}
}
7.mapper文件：
<sqlMap namespace="pushTask">

 <sql id="insert">
    <![CDATA[
       INSERT INTO wx_push_task (
ID,
MSGID,
PUBNUM_NUM,
TITLE,
PUSHMODE,
PUSHTIME,
PUSHTYPE,
`STATUS`,
UPDATE_TIME,
UPDATE_USER_ID,
CREATE_TIME,
CREATE_USER_ID
)
VALUES
(
:id,
:msgId,
:pubnum,
:title,
:pushmode,
:pushtime,
:pushType,
:status,
now(),
:updateUserId,
now(),
:createUserId
)
]]>
    </sql>
</sqlMap>









编辑消息模板：
1.js如上
2Action:
/**
     * 修改消息推送
     * 
     *  @return
     */
     @ResponseBody
     @RequestMapping (value =  "mod" , method = RequestMethod. POST , produces =  "application/json" )
     public  BaseResult mod(HttpServletRequest  request ,  @RequestParam  String  paramJson ) {
         log .info( "====modMsg====data:"  +  paramJson );
        BaseResult  br ;
         try  {
             paramJson  = java.net.URLDecoder. decode ( paramJson ,  "utf-8" );
            CompanyInfoDto  companyInfoDto  =  this .getMerchantInfo( request );
            String  companyId  =  companyInfoDto .getCompanyId().toString();
            JSONObject  paramMap  = JSON. parseObject ( paramJson );
             // Map<String, Object> paramMap = gson.fromJson(paramJson, new
             // TypeToken<Map<String, Object>>(){}.getType());
             log .info( "mod params:"  +  paramMap );
             // Map<String, Object> paramMap = gson.fromJson(paramJson, new
             // TypeToken<Map<String, Object>>() {
             // }.getType());
            LoginUser  loginUser  = (LoginUser)  request .getSession().getAttribute(MmcConstants. LOGIN_USER );
            Long  userId  =  loginUser .getUserId();
             paramMap .put( "createUserId" ,  userId .toString());
             paramMap .put( "updateUserId" ,  userId .toString());
             br  =  pushMessageService .modPushTask( paramMap ,  companyId );
        }  catch  (Exception  e ) {
             log .error( "modMsg error:"  +  e .getMessage());
             br  =  new  BaseResult(BaseResult. ERROR , MmcErrorCode. ERROR_MSG_UNKNOW );
        }
         return   br ;     }  
3.Service:
@Override
     public  BaseResult modPushTask(JSONObject  params , String  companyId )  throws  SysException {
        BaseResult  br  =  new  BaseResult();
        String  RESTURL  = getAddress( companyId );
        String  sign  = MD5. encode ( secretKey  +  companyId );
        String  url  =  RESTURL  +  "/push/mod?sign="  +  sign ;
         try  {
            PushTaskDto  pushTaskDto  = buildPushTaskDto( params );
             if  ( pushTaskDto  ==  null ) {
                 br  =  new  BaseResult();
                 br .setErrorMsg( "消息属性选择图文，但没有发现图文内容" );
                 br .setStatus(BaseResult. ERROR );
                 return   br ;
            }
             @SuppressWarnings ( "unchecked" )
            Map<String, String>  parameters  = (Map<String, String>) JSON. toJSON ( pushTaskDto );
            String  data  = HttpUtil. doPost ( url ,  parameters , HttpUtil. BODYTYPE_JSON );
             br  = JSONObject. parseObject ( data , BaseResult. class );
        }  catch  (Exception  e ) {
             log .error(MmcErrorCode. CONN_APP_EXC_MSG );
             e .printStackTrace();
            setErrors( br );
        }
         return   br ;
    }
4.AppServer中的action:
/**
     * 修改消息推送
     * 
     *  @return
     */
     @ResponseBody
     @RequestMapping (value =  "/mod" , method = RequestMethod. POST , produces =  "application/json" )
     public  BaseResult mod(HttpServletRequest  request ,  @RequestBody  PushTaskDto  taskDto ) {
         log .info( "====start mod====" );
        BaseResult  br  =  new  BaseResult();
         try  {
            Integer  flag  =  pushMessageService .modPushTask( taskDto );
             br .setContent( flag );
             br .setStatus(BaseResult. SUCCESS );
        }  catch  (Exception  e ) {
             log .error( "modMsg error:"  +  e .getMessage());
            setError( br );
        }
         return   br ;
    }
5.AppServer的Service:
@Override
     public  Integer modPushTask(PushTaskDto  taskDto )  throws  SysException {
         return   pushTaskService .modPushTask( taskDto );
    }
6.中台的Service:
@Override
public Integer modPushTask(PushTaskDto taskDto) {
try {
//获取原来的推送任务
PushTaskDto tmpTask = getPushTask(taskDto.getId());
//获取原消息模板
MsgTemplateDto tmpMsg = msgTemplateService.getMsgTemplate(tmpTask.getMsgId());

//如果原回复不是关键字回复，则删除该推送任务的原消息模板
if(MsgTemplateType.passive.getValue()!=tmpMsg.getMsgType().intValue()){
msgTemplateService.delMsgTemplate(tmpTask.getMsgId());

//msgId为空表示推送的是图文消息或者文本消息，此时新增消息模板。如果是关键字则直接保存
//生成消息模板id
String msgId = UUIDGenerator.getInstance().get64BitUUID();
MsgTemplateDto msgDto = taskDto.getMsgTemplateDto();
msgDto.setId(msgId);
msgTemplateService.addMsgTemplate(msgDto);

//给推送任务设置保存的消息模板Id
taskDto.setMsgId(msgId);
}

Map<String, Object> paramMap = DacUtils.convertToMap(taskDto);
baseDao.execute("pushTask.update", paramMap);
return RespStatus.success.getValue();
} catch (Exception e) {
logger.error("modPushTask:{}", e);
throw new SysException(e);
}
}
7.中台的mapper
<sql id="update">
    <![CDATA[
       update wx_push_task set
        <#if msgId?exists && msgId!="">
MSGID=:msgId,
</#if>
<#if pubnum?exists && pubnum!="">
PUBNUM_NUM=:pubnum,
</#if>
<#if title?exists && title!="">
TITLE=:title,
</#if>
<#if pushmode?exists && pushmode!="">
PUSHMODE=:pushmode,
</#if>
<#if pushtime?exists && pushtime!="">
PUSHTIME=:pushtime,
</#if>
<#if pushType?exists && pushType!="">
PUSHTYPE=:pushType,
</#if>
<#if updateUserId?exists && updateUserId!="">
UPDATE_USER_ID=:updateUserId,
</#if>
UPDATE_TIME=now()
WHERE 
ID = :id
]]>
    </sql> 

```
实现难点
```
无论新增还是编辑只要点击关键字、
和编辑时将选中的关键字带出来、
和初始时都要进行初始化。


initKeywordDefault:  function (){
             // 初始化下拉框并选中对应的关键字
             var  msgTemplateId = $( '#msgId' ).val();
             var  keyname = $( '#keyname' ).val();
            
             var  oMenu = $( '#kid' ).select2({
                placeholder :  "<div id='" +msgTemplateId+ "'>" +keyname+ "</div>" ,
                 // minimumInputLength: 1,
                
                initSelection :  function (element, callback) {
                     var  ev = element.val(); //获取<input type="text" id="kid2" />元素的值
                     callback({ //自定义属性名称要与formatSelection返回的属性名称一致
                         id: ev,
                         name: ev
                     });
                },
                id :  function (priv) {
                     return  priv.msgTemplateId;
                },
                formatResult :  function (word) { 
                     if  (word.msgTemplateId){
                         var  markup =  "<div id='"
                                + word.msgTemplateId
                                +  "'>"
                                + word.name +  "</div>" ;
                         return  markup;
                    }
                },  // omitted for brevity, see the source of this page  
                formatSelection :  function (repo) { 
                     return  repo.name;
                },  // omitted for brevity, see the source of this page
                dropdownCssClass :  "bigdrop" ,  // apply css that makes the
                                             // dropdown taller
                
                ajax : {  
                    url : basePath +  "/message/getKeyWord.do" ,
                    dataType :  'json' ,
                    quietMillis : 250,
                    data :  function (term, pageNumber) {
                         return  {
                            name : term,  // search term
                            pageSize:10,
                            pageNumber:pageNumber
                        };
                    },
                    results :  function (data, pageNumber) { 
                         if (data&&data.rows&&data.rows.length){     
                             var  more = (pageNumber*10)<data.total;     //用来判断是否还有更多数据可以加载
                             return  {
                                results:data.rows,more:more    
                            };
                        } else {
                             return  {
                                results : data.rows
                            };
                        }                
                    },
                    cache :  true
                },
                
                escapeMarkup :  function (m) { 
                     return  m;
                }
            });
             //将msgId的值传到页面
            $( '#kid' ).on( 'select2-selecting' , function (el){
                  var  choice = el.choice;
                  var  keyid = choice.id;
                  var  keyname = choice.name;
                  var  msgId = choice.msgTemplateId;
                 $( "#keyid" ).val(keyid);
                 $( "#keyname" ).val(keyname);
                 $( '#msgId' ).val(msgId);
            });         }   

```