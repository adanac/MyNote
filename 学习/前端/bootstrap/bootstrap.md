##layer弹层的使用
```
< div   id = "Popup3"   class = "layer_form2"   style =" display :  none ;" >
         < div   class = "row" >
             < div   class = "col-md-12 ta_c pa_t3" > 是否删除？ </ div >
             < div   class = "col-md-12 ta_c pa_t20" >
                 < button   class = "btn btn-primary wn120 Popup3-confirm" > 确认 </ button >
                 < button   class = "btn btn-warning wn120 Popup3-close" > 取消 </ button >
             </ div >
         </ div >      </ div >  
 

function  delMsg(id){
    layer.open({
        type : 1,
        title :  '' ,
        closeBtn : 1,
        area :  '415px' ,
        shadeClose :  true ,
        content : $( '#Popup3' ),
        success :  function () {
             var  PoConfirm = $( ".Popup3-confirm" );
            PoConfirm.unbind( "click" );
            PoConfirm.on( "click" ,  function () {
                         var  url = basePath +  '/message/del.do?id='  + id;
                        $.ajax({
                            type :  "post" ,
                            url : url,
                            async:  false ,
                            dataType :  "json" ,
                            success :  function (data) {
                                 if  (data.status ==  "1" ) {
                                    layer.msg(data.errorMsg ||  '操作成功!' , {
                                        time : 1000
                                    },
                                     function () {//操作成功后调用的函数
                                        $.CommonUtils
                                                .redirect(basePath
                                                        +  '/message/main.do' );
                                    });
                                }  else  {
                                    layer.msg(data.errorMsg ||  '操作失败' );
                                }
                            }
                        });
                        layer.closeAll();
                        layer.msg( "删除成功!" );
                    });
            $( '.Popup3-close' ).on( 'click' ,  function (event) {
                        layer.closeAll();
                         return   false ;
                    });
        }
    });
         }   

```
##bootstrap4速查表
```
http://hackerthemes.com/bootstrap-cheatsheet

```
##bootstrap时间控件
```
1.样式与js：
<link rel="stylesheet" type="text/css" href="${resRoot}/assets/plugins/bootstrap-datetimepicker/css/bootstrap-datetimepicker.css" type="text/javascript"></script>
 <script type="text/javascript" src="${resRoot}/assets/plugins/bootstrap-datetimepicker/js/bootstrap-datetimepicker.js"></script>


2.使用：只选择到年月日
< input   id = "credvalidity"   type = "text"   class = "form-control validate[required]"   />   

$('#credvalidity').datetimepicker({ 
        　　minView: "month", //选择日期后，不会再跳转去选择时分秒 
        　　format: "yyyy-mm-dd", //选择日期后，文本框显示的日期格式 
        　　language: 'zh-CN', //汉化 
        　　autoclose:true //选择日期后自动关闭 
        }).on('changeDate',checkCredvalidity);
function  checkCredvalidity() {
         var  evalue = $( "#credvalidity" ).val();
         var  now =  new  Date();
         var  year = now.getFullYear();        //年
         var  month = now.getMonth() + 1;      //月
         var  day = now.getDate(); 
        
         var  dB = year +  "-" ;
         if (month < 10){
            dB +=  "0" ;
        }
            dB += month +  "-" ;
         if (day < 10){
            
            dB +=   "0" ;
        }
        dB += day +  " " ;
         if  (dB > evalue) {
            console.log( "证件有效期无效,应超过当前时间" );
             return   false ;
        } else {
             return   true ;
        }        }   



3.使用（选择到时分秒）：
< input   type = "text"   class = "form-control today validate[required]"   id = "startDttm"   name = "startDttm"   size = "16"   value = " ${activity.startDttm} "   placeholder = "" >  
< input   type = "text"   class = "form-control today validate[required]"   id = "endDttm"   name = "endDttm"   size = "16"   value = " ${activity.endDttm} "   placeholder = "" >  

$( '#startDttm' ).datetimepicker({ 
          　　minView:  "hour" ,  //选择日期后，不会再跳转去选择时分秒 
          　　format:  "yyyy-mm-dd hh:ii:ss" ,  //选择日期后，文本框显示的日期格式 
          　　language:  'zh-CN' ,  //汉化 
          　　autoclose: true   //选择日期后自动关闭 
          }).on( 'changeDate' ,checkDttm( "startDttm" ));
        
        $( '#endDttm' ).datetimepicker({ 
          　　minView:  "hour" ,  //选择日期后，不会再跳转去选择时分秒 
          　　format:  "yyyy-mm-dd hh:ii:ss" ,  //选择日期后，文本框显示的日期格式 
          　　language:  'zh-CN' ,  //汉化 
          　　autoclose: true   //选择日期后自动关闭            }).on( 'changeDate' ,checkDttm( "endDttm" ));   

function  checkDttm(tag) {
     var  evalue = $( "#" +tag+ "" ).val();     console.log(evalue);  
} 



4.注意：
如果input中的value不符合时间格式：yyyy-mm-dd HH:mm:ss时，当点击时会显示1899年的日期。
解决方法：对时间格式化< input   type = "text"   class = "form-control today validate[required]"   id = "startDttm"   name = "startTime"   value = " ${cr.startTime?datetime!'' } "   size = "16"   value = ""   placeholder = "" >   



5.设置步长：（ minuteStep,默认为5)
 $( '#startDttm' ).datetimepicker({ 
          　　minView:  "hour" ,  //选择日期后，不会再跳转去选择时分秒 
          　　format:  "yyyy-mm-dd hh:ii:00" ,  //选择日期后，文本框显示的日期格式 
          　　language:  'zh-CN' ,  //汉化 
             minuteStep: 30,
          　　autoclose: true   //选择日期后自动关闭 
          }).on( 'changeDate' ,checkDttm( "startDttm" ));
        
        $( '#endDttm' ).datetimepicker({ 
          　　minView:  "hour" ,  //选择日期后，不会再跳转去选择时分秒 
          　　format:  "yyyy-mm-dd hh:ii:00" ,  //选择日期后，文本框显示的日期格式 
          　　language:  'zh-CN' ,  //汉化 
             minuteStep: 30,
          　　autoclose: true   //选择日期后自动关闭            }).on( 'changeDate' ,checkDttm( "endDttm" ));   

```

##bootstrap刷新表格
```
1.最简单的方式：
function  find(){
    $( '#goodsadd_list' ).bootstrapTable( 'refresh' ); }   

2.有可能会用到的方式：
< script >
     var $table = $('#table');
     $(function () {
         $table.bootstrapTable({
             data: [{
                 "id": 0,
                 "name": "Item 0",
                 "price": "$0"
             }]
         });
         $('#button').click(function () {
             $table.bootstrapTable('refresh', {url: '../json/data1.json'});
         });
     });
</ script >


```
##bootstrap表格的查询
```
1.页面
< input   id = "title"   type = "text"   class = "check-text phone"   maxlength = "11" >  
 

< button   onclick = "$('#msg_list').bootstrapTable('refresh');"   class = "btn btn-ajax btn-primary f_l"   style =" width : 120px ;" >   < i   class = "fa fa-search" ></ i >  查询

</ button >
< button   class = "btn btn-warning f_l btn-reset"   style =" width : 120px ;" >   < i   class = "fa fa-undo" ></ i >  清除条件
</ button >


< table   id = "msg_list"   data-side-pagination = "server"   data-pagination = "true"   data-height = ""   class = "table table-hover dataTable no-footer table-striped table-bordered"   style =" width :  100% ; border : 1px solid #ddd ;" >
</ table >
2.js
var  queryParams =  function  (params){
     var  temp = {
        limit: params.limit,   //页面大小
        offset: params.offset,  //页码
        title: encodeURI($( "#title" ).val())//根据title模糊查询
    };
     return  temp;
}
function  queryMsgList() {
    $( '#msg_list' ).bootstrapTable({
        method:  'get' ,
        url: basePath +  "/message/msgList.do" ,
        queryParams: queryParams, //传递参数
        striped:  true ,       //是否显示行间隔色
        cache:  false ,       //是否使用缓存，默认为true
        pagination:  true ,      //是否显示分页
        sortable:  true ,  //是否排序
        sidePagination:  "server" , //服务端分页
        pageNumber:1,       //初始化加载第一页，默认第一页
        pageSize: 5,       //每页的记录行数（*）
        pageList: [5, 10, 20],   //可供选择的每页的行数（*）
        columns: [{
            field:  'title' ,
            title:  '主题' ,
            align:  'center' ,
            valign:  'middle'
        }, {
            field:  'msgTemplateDto' ,
            title:  '推送内容' ,
            align:  'center' ,
            valign:  'middle' ,
            formatter: function (value,row,index){
                 var  content = value.msgContent;
                 return  content;
            }
        }, {
            field:  'pname' ,
            title:  '推送人员' ,
            align:  'center' ,
            valign:  'middle' ,
            formatter: function (value,row,index){
                 var  type = row.pushType;
                 if (type == 1){
                     return   "全部会员" ;
                } else {
                     return   "部分会员" ;
                }
            }
        }, {
            field:  'pushtime' ,
            title:  '推送时间' ,
            align:  'center' ,
            valign:  'middle' ,
            formatter: function (value,row,index){
                 if (value !=  '' ){
                     var  kk = value.substring(0,10);
                     return  kk;
                }
            }
        }, {
            field:  'status' ,
            title:  '状态' ,
            align:  'center' ,
            valign:  'middle' ,
            formatter: function (value,row,index){
                 var  status =  "<span class=\"label label-sm label-success\">推送中</span>" ;
                 if (value == 1){
                    status =  "<span class=\"label label-sm label-info\">已推送</span>" ;
                } else   if (value == 3){
                    status =  "<span class=\"label label-sm label-warning\">推送失败</span>" ;
                }
                    return  status;
            }
        },{
            title:  '操作' ,
            field:  'id' ,
            align:  'center' ,
            formatter: function (value,row,index){  
               var  temp= "" ;
               var  status = row.status;
               if (status==1){ //已推送
                  temp =  "<a href='javascript:delMsg(" +row.id+ ");' class='btn btn-xs red' title='删除'><i class='fa fa-times'></i></a>"
                  + "<a href='${base}/message/editMsg.do?id=" +row.id+ "' class='btn btn-xs green' title='复制'><i class='fa fa-copy'></i></a>" ;
              } else { //2推送中  3推送失败
                       temp =  "<a href='${base}/message/editMsg.do?id=" +row.id+ "' class='btn btn-xs blue' title='编辑'><i class='fa fa-pencil'></i></a>"
                   + "<a href='javascript:delMsg(" +row.id+ ");' class='btn btn-xs red' title='删除'><i class='fa fa-times'></i></a>"
                   + "<a href='${base}/message/editMsg.do?id=" +row.id+ "' class='btn btn-xs green' title='复制'><i class='fa fa-copy'></i></a>" ;
              }
               return  temp;
            } 
        }]                
       });   }   

3.action:
@Controller
@RequestMapping (value =  "message" , produces =  "text/html;charset=UTF-8" ) public   class  MessageAction  extends  MmcAction {  
     /**
     * 查询消息推送列表
     * 
     *  @return
     */
     @ResponseBody
     @RequestMapping ( "msgList" )
     public  String msgList(HttpServletRequest  request , PushTaskDto  msg , PagerParam  param ) {
         log .info( "====msgList====msg:"  +  msg .toString());
        BootstrapTable<PushTaskDto>  data  =  new  BootstrapTable<>();
         try  {
            Pager<PushTaskDto>  pager  =  msgService .listMsg( msg ,  param );
            List<PushTaskDto>  infoList  =  pager .getDatas();
             data  =  new  BootstrapTable<PushTaskDto>( infoList ,
                     pager .getTotalDataCount() ==  null  ? 0 :  pager .getTotalDataCount(),  pager .getPageNumber());
        }  catch  (Exception  e ) {
             log .error( "msgList error:"  +  e .getMessage());
        }
         return  JSONObject. fromObject ( data ).toString();
    }
} 
4.mapper文件：
<sql id="queryList">
    <![CDATA[
       SELECT
t.ID AS id,
t.MSGID AS msgId,
t.PUBNUM_NUM AS pubnum,
t.TITLE AS title,
t.PUSHMODE AS pushmode,
t.PUSHTIME AS pushtime,
t.PUSHTYPE AS pushType,
t.`STATUS` AS STATUS,
t.UPDATE_TIME AS updateTime,
t.UPDATE_USER_ID AS updateUserId,
t.CREATE_TIME AS createTime,
t.CREATE_USER_ID AS createUserId
FROM
wx_push_task t
WHERE 
1=1
<#if pubnum?exists && pubnum!="">
and t.PUBNUM_NUM=:pubnum
</#if>
<#if PUSHTYPE?exists && pushType!="">
and t.pushType=:pushType
</#if>
<#if status?exists && status!="">
and t.`STATUS`=:status
</#if>
<#if pushmode?exists && pushmode!="">
and t.PUSHMODE=:pushmode
</#if>
<#if title?exists && title!="">
and  t.TITLE LIKE CONCAT('%', :title, '%')
</#if>
<#if msgId?exists && msgId!="">
and t.MSGID=:msgId
</#if>
ORDER BY t.UPDATE_TIME DESC
]]>
    </sql>
5.SQL:
SELECT
t.ID AS id,
t.MSGID AS msgId,
t.PUBNUM_NUM AS pubnum,
t.TITLE AS title,
t.PUSHMODE AS pushmode,
t.PUSHTIME AS pushtime,
t.PUSHTYPE AS pushType,
t.`STATUS` AS STATUS,
t.UPDATE_TIME AS updateTime,
t.UPDATE_USER_ID AS updateUserId,
t.CREATE_TIME AS createTime,
t.CREATE_USER_ID AS createUserId
FROM
wx_push_task t
WHERE 
1=1
/*<#if pubnum?exists && pubnum!="">
and t.PUBNUM_NUM=:pubnum
</#if>
<#if PUSHTYPE?exists && pushType!="">
and t.pushType=:pushType
</#if>
<#if status?exists && status!="">
and t.`STATUS`=:status
</#if>
<#if pushmode?exists && pushmode!="">
and t.PUSHMODE=:pushmode
</#if>
<#if title?exists && title!="">*/
AND  t.TITLE LIKE CONCAT('%', 'a', '%')
/*</#if>
<#if msgId?exists && msgId!="">
and t.MSGID=:msgId
</#if>*/
ORDER BY t.UPDATE_TIME DESC
``` ##bootstrap表格与序号
```
1.页面
< table   id = "alipay_list"   data-side-pagination = "server"   data-pagination = "true"   data-height = ""   class = "table table-hover dataTable no-footer table-striped table-bordered"   style =" width :  100% ; border : 1px solid #ddd ;" > </ table >   

2.js
var  queryParams =  function  (params){
         var  temp = {
            limit: params.limit,   //页面大小
            offset: params.offset,  //页码
        };
         return  temp;
    }
     function  queryAlipayList() {
        console.log( '${basePath}' );
        $( '#alipay_list' ).bootstrapTable({
            method:  'get' ,
            url: basePath +  "/payment/alipayList.do" ,
            queryParams: queryParams, //传递参数
            striped:  true ,       //是否显示行间隔色
            cache:  false ,       //是否使用缓存，默认为true
            pagination:  true ,      //是否显示分页
            sortable:  true ,  //是否排序
            sidePagination:  "server" , //服务端分页
            pageNumber:1,       //初始化加载第一页，默认第一页
            pageSize: 5,       //每页的记录行数（*）
            pageList: [5, 10, 20],   //可供选择的每页的行数（*）
            columns: [{
                field:  'number' ,
                title:  '序号' ,
                align:  'center' ,
                valign:  'middle' ,
                formatter:  function  (value, row, index) {
                     var  num = index+1;
                     if (num<10){
                         return   "0" +num;
                    }
                     return  num;
                }
            }, {
                field:  'account' ,
                title:  '支付宝账号' ,
                align:  'center' ,
                valign:  'middle' ,
                formatter: function (value,row,index){
                     var  temp =  "<a href='${base}/payment/toAlipayApplied.do?id=" +row.id+ "'>" +row.account+ "</a>" ;
                        return  temp;
                }
            }, {
                field:  'payId' ,
                title:  'PayID' ,
                align:  'center' ,
                valign:  'middle' ,
            }, {
                field:  'aliases' ,
                title:  '别名' ,
                align:  'center' ,
                valign:  'middle'
            }, {
                field:  'applyDate' ,
                title:  '申请日期' ,
                align:  'center' ,
                valign:  'middle'
            }, {
                field:  'approvalDate' ,
                title:  '审核日期' ,
                align:  'center' ,
                valign:  'middle'
            }, {
                field:  'status' ,
                title:  '状态' ,
                align:  'center' ,
                valign:  'middle' ,
                formatter: function (value,row,index){
                     var  status =  "<span class=\"label label-sm label-success\">已申请</span>" ;
                     if (value == 1){
                        status =  "<span class=\"label label-sm label-info\">已审核</span>" ;
                    } else   if (value == 2){
                        status =  "<span class=\"label label-sm label-warning\">被驳回</span>" ;
                    } else   if (value == 3){
                        status =  "<span class=\"label label-sm label-danger\">已终止</span>" ;
                    }
                        return  status;
                }
            },{
                title:  '操作' ,
                field:  'id' ,
                align:  'center' ,
                formatter: function (value,row,index){  
                   var  temp;
                   var  status = row.status;
                   if (status==0 || status==3){
                       temp =  "<span class=\"glyphicon glyphicon-edit\"></span>" ;
                  } else   if (status==1){
                      temp =  "<a href=\"${base}/payment/toAlipayEdit.do?id=" +row.id+ "\">"  +
                               "<span style=\"cursor:pointer\" class=\"glyphicon glyphicon-edit font-blue\" title=\"编辑\"></span></a>" ;
                  } else   if (status==2){
                      temp =  "<a href='${base}/payment/toAlipayReject.do?id=" +row.id+ "'>"  +
                               "<span style='cursor:pointer' class='glyphicon glyphicon-edit font-blue' title='编辑'></span></a>" ;
                  }
                   return  temp;
                } 
            }]                
           });       }   

```