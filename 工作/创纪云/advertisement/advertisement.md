##关于数据源
spring3.0如何解析jee:jndi-lookup
会去检索web容器中的JNDI资源名。正常情况是JNDI资源会配置在服务器下的server.xml或者context.xml中服务器启动后，该资源配置放入web容器中。在项目配置文件中声明数据源bean时，采用jee:jndi-lookup会在web容器中查找该资源名称 并转化为数据源bean.
spring-res.xml
```
< jee:jndi-lookup   id = "dataSource"   jndi-name = "jdbc/advertisement"   proxy-interface = "javax.sql.DataSource"   lookup-on-startup = "false"   />    
```
 tomcat的content.xml

```
< Resource   auth = "Container"   driverClassName = "com.mysql.jdbc.Driver"   maxActive = "5"   maxIdle = "2"   maxWait = "-1"   name = "jdbc/advertisement"   password = ""   type = "javax.sql.DataSource"   url = "jdbc:mysql://192.168.1.44:3306/advertisement?characterEncoding=UTF-8"   username = "root" /> </ Context >  
``` 

##广告管理 - 广告类型 - 新增


在本页面写js:
```
$(function()){
//跳转到添加广告资源的页面
    $("#add_btn").click(function(){
        window.location.href="${base}/adResource/toAddAdResource.do";             });  
```
 

```
```
<script type="text/javascript">
$(function(){
    //查询分页
    initDataGrid();
    bindEvent();
    //跳转到添加广告类型的页面
    $("#add_btn").click(function(){
        window.location.href="${base}/adType/toAddAdType.do";        
    });
    
}); </script> 
``` 
##广告管理 - 广告类型 -查询结果


主要分两大块，上面的是.ftl写的，下面的查询结果是E:\svn_code\advertisement-pom\advertisement-web\src\main\webapp\RES\advertisement\adType\adType.js根据id渲染出来的。












```
function initDataGrid(){        var datagrid = $("#dataGrid").datagrid({ 
                {
                field: "id",
                title: "操作",
                render: function( data ) {
                    var url = "";
                    if(0 == data.row.status)
                    {
                        url = "<a href='javascript:void(0);' onclick='modify_status("+data.row.id+","+1+")'>启用</a>";
                    }
                    else
                    {
                        url = "<a href='javascript:void(0);' onclick='modify_status("+data.row.id+","+0+")'>停用</a>";
                    }
                    return "<a href='${base}/adType/toModifyAdTypeById.do?id="+data.row.id+"'>编辑</a>|"+
                    "<a href='javascript:void(0);' onclick='modify_status("+data.row.id+","+2+")'>删除</a>|"+
                    "<a href='${base}/adResource/toFindAdResource.do?typeId="+data.row.id+"'>查看</a>|"+url;
                }
```
 请求后缀后为.do，是配置文件处理：
web.xml
```html
<context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>
        classpath:conf/spring/spring-dubbo.xml,         classpath:conf/spring/spring-servlet.xml, 
 

```


请求到controller：
```
/**
     * 跳转到修改广告类型的页面
     * 
     * @param id
     * @return
     */
    @RequestMapping(value = "/toModifyAdTypeById", method = RequestMethod.GET)
    public ModelAndView toModifyAdTypeById(String id)
    {
        ModelAndView mav = new ModelAndView(Constant.ADTYPE_PATH + "modifyAdType.ftl");
        try
        {
            AdType adType = adTypeService.findAdTypeById(id);
            mav.addObject("adType", adType);
            return mav;
        } catch (SysException e)
        {
            mav.addObject("");
            return mav;
        }     } 
```
跳转到freemarker下的modifyAdType.ftl

```html
<div class="row">
         <div class="panel panel-default">
            <div class="panel-body">
                   <form id="modify_form">
                         <input type="hidden" name = "id" value="${adType.id}" />
                      <div class="row">
                         <div class="col-md-6">
                             <label for="">广告类型Code：</label>
                             <input id="adTypeCode" style="width: 200px;display: inline-block" type="text" class="form-control" name="adTypeCode" value="${adType.adTypeCode}" >
                         </div>
                         <div class="col-md-6">
                             <label for="">广告类型名称：</label>
                             <input id="name" style="width: 200px;display: inline-block" type="text" class="form-control" name="name" value="${adType.name}">
                         </div>
                     </div>                     
                     <div class="row">
                         <div class="col-md-12">
                             <a id="modify_btn" class="btn btn-primary">修改</a>
                         </div>                     
                     </div>    
                </form>
            </div>
        </div>     </div>
</p>
```


修改信息后，点击修改按钮：
```
$(function(){
    $("#modify_btn").on("click",function(){
        var adTypeCode = $("#adTypeCode").val().trim();
        var name = $("#name").val().trim();
        if(confirm("确定修改？"))
        {
            if(adTypeCode=="")
            {
                alert("请填写广告类型Code");
                return;
            }
            if(name=="")
            {
                alert("请填写广告类型名称");
                return;
            }
            var actionUrl = "${base}/adType/modifyAdTypeById.do";
            $("#modify_form").ajaxSubmit({
                type :'post',
                url : actionUrl,
                success:function(data){
                    if(data.code=="0")
                    {
                        window.location.href="${base}/adType/toFindAdType.do";
                    }
                    else if(data.code=="8005")
                    {
                        alert(data.message);
                    }
                    else
                    {
                        alert("保存错误！");
                    }
                }
            });
        }
    })
});
```


发出请求到Controller：
`var actionUrl = "${base}/adType/modifyAdTypeById.do";`

##查询
 调用 service方法:
 `   result  =  adTypeService .modifyAdTypeById( adType );`
并返回一个map. `map .put( "code" , Constant. RESULT_SUCCESS );   //code表示操作数据库返回的值`
修改成功后，调用ajaxSubmit方法，跳转到 查询页面：
`    $("#modify_form").ajaxSubmit({  window.location.href="${base}/adType/toFindAdType.do"; `


查询页面的下拉选框：
```html
<select name = "id" style="width: 200px;display: inline-block" class="form-control">
                             <option value="">请选择</option>
                             <#list adTypeList as adType>
                             <option value='${adType.id}'>${adType.name}</option>
                             </#list>                              </select>  
```
###广告资源 - 操作 - 查看
请求/advertisement-web/adResource/toFindAdResource.do?typeId=11 ，
 ```

ModelAndView  mv  =  new  ModelAndView(Constant. ADRESOURCE_PATH  +  "findAdResource.ftl" );  
mv .addObject( "adTypeList" ,  adTypeList );
mv .addObject( "typeId" ,  typeId );
```
 查询结果依然是js再次发出请求：

```
<div class="panel-heading">查询结果</div>

<div id="dataGrid">
```
引入js 文件： <script src="${resRoot}/advertisement/adResource/adResource.js"></script>

调用  initDataGrid(typeid)方法。
```
<script type="text/javascript">
$(function(){
    var typeId = $('#typeId').val();
    if(typeId ==0)
    {
        typeId = "";
    }
    //查询分页
    initDataGrid(typeId);
    bindEvent();
    //跳转到添加广告资源的页面
    $("#add_btn").click(function(){
        window.location.href="${base}/adResource/toAddAdResource.do";        
    });
     });  
```


```
function  initDataGrid(typeId){
        console.info(typeId);
         var  urls =  "" ;
            if (typeId == "" )
        {
               urls =  "${base}/adResource/findAdResource.do" ;
               
        }
            else
           {
               urls =  "${base}/adResource/findAdResource.do?typeId=" +typeId;            }  
               var  datagrid = $( "#dataGrid" ).datagrid({                url:urls,   

```
 请求到Controller中，页面不需要跳转，因此没有用到ModelAndView对象

```
Pager<AdResourceDto>  result  =  adResourceService .findAdResourceForPage( adResource ,  adResource .getPaging(), adResource .getPage());  
dataGrid  =  new  DataGrid<AdResourceDto>( result .getTotalDataCount(),  result .getDatas());
```
 ###广告资源 - 新增

<a id="add_btn" class="btn btn-danger">新增</a>  
//跳转到添加广告资源的页面
    $("#add_btn").click(function(){
        window.location.href="${base}/adResource/toAddAdResource.do";        
    });
请求到Controller，要将广告类型传递到要跳转的页面中，mv.addObject()
```
@RequestMapping (value =  "/toAddAdResource" , method = RequestMethod. GET )
     public  ModelAndView toAddAdResource()
    {
        ModelAndView  mv  =  new  ModelAndView(Constant. ADRESOURCE_PATH  +  "addAdResource.ftl" );
        List<AdType>  adTypeList ;
         try
        {
             adTypeList  =  adTypeService .findAdTypeForList( null );
        }  catch  (Exception  e )
        {
             adTypeList  =  null ;
        }
         mv .addObject( "adTypeList" ,  adTypeList );
         return   mv ;
    }
```
跳转到新增页面后的各个元素：
1.广告类型
```
<select id = "typeId" name = "typeId" style="width: 200px;display: inline-block" class="form-control">
                                     <#list adTypeList as adType>
                                     <option value='${adType.id}'>${adType.name}</option>
                                     </#list>                                      </select>  
```
2.开始时间
```
<input type="text" id="startTime" name="startTime" class="shadowborder form-control border_radius_2 height_30" style="width: 200px;display: inline-block;cursor:pointer;" />  
$("#startTime").datetimepicker({
        language : 'zh-CN',
        weekStart : 1,
        todayBtn : 1,
        autoclose : 1,
        todayHighlight : 1,
        endDate:$("#endDate").val(),
        startView : 2,
        minView : 0,
        format : 'yyyy-mm-dd hh:ii:ss'
    })  

```
2.选择文件 和 开始上传

```
<div id="uploader" class="wu-example">
                                <!--用来存放文件信息-->
                                <div id="thelist" class="uploader-list"></div>
                                <div class="btns" style="clear: both;">
                                    <div id="picker">选择文件</div>
                                    <a id="ctlBtn" class="btn btn-default">开始上传</a>
                                </div>                             </div>  
```
引入js文件即可：` <script type="text/javascript" src="${resRoot}/common/webuploader/js/webuploader.js"></script>`
文件上传：
```
// 文件上传
jQuery( function () {
     var  $ = jQuery,
        $list = $( '#thelist' ),         $btn = $( '#ctlBtn' ),  
```
3.图片描述
注意name属性和数据库中的字段保持一样

`<input id="adDesc" name="adDesc" style="width: 200px;display: inline-block" type="text" class="form-control" >  `

 4.跳转类型
有三块内容 id ='adType' id ='content' id ='detail' ，后两个模块是子的，根据第1个的改变事件来显示或隐藏。
```
<div class="row top10">
                             <div class="col-md-2 text-right">跳转类型：</div>
                             <div class="col-md-4">
                                 <select id="adType" name="adType" style="width: 200px;display: inline-block" class="form-control">
                                     <option value="0">图片无跳转</option>
                                     <option value="1">图片链接跳转</option>
                                     <option value="2">功能模块</option>
                                     <option value="3">文本展示</option>
                                 </select>
                             </div>                     </div>   



<div id="content" class="row top10">
                             <div class="col-md-2 text-right">目标对象：</div>
                             <div class="col-md-10">
                             <textarea id="adContent" name="adContent" style="width: 200px;display: inline-block" type="text" class="form-control"></textarea>
                              </div>
                     </div>    
                     <div id="detail" class="row top10">
                             <input type="hidden" id = "adDetail" name = "adDetail" />
                            <div class="col-md-2 text-right">广告详情：</div>
                            <div class="col-md-10">
                                <script id="editor" name="fskuDetail" type="text/plain" style="width:100%;height:150px;"></script>
                            </div>                     </div>   

```
```
$("#adType").on("change",function(){
        var val = $(this).val();
        if(val=="0")
        {
            $("#content").hide();
            $("#detail").hide();
        }
        if(val=="1" || val=="2")
        {
            $("#content").show();
            $("#detail").hide();
        }
        if(val=="3")
        {
            $("#content").hide();
            $("#detail").show();
        }     })  
```
5.添加
为按钮添加点击事件，在执行前会先验证。如果验证成功后，请求 ${base}/adResource/toFindAdResource.do，跳转到 findAdResource.ftl
` <a id="save_btn" class="btn btn-primary">添加</a>`
 ```
$("#save_btn").on("click",function(){
        getImage();
        $("#adDetail").val(UE.getEditor('editor').getContent());
        if($("#adName").val().trim()=="")
        {
            alert("请填写广告资源名称");
            return;
        }
        if($("#startTime").val().trim()=="")
        {
            alert("请选择开始时间");
            return;
        }
        if($("#endTime").val().trim()=="")
        {
            alert("请选择结束时间");
            return;
        }
        if($("#adImageUrl").val().trim()=="")
        {
            alert("请上传图片");
            return;
        }
        if($("#orderNumber").val().trim()=="")
        {
            alert("填写显示顺序");
            return;
        }
        
        var actionUrl = "${base}/adResource/addAdResource.do";
        $("#save_form").ajaxSubmit({
            type :'post',
            url : actionUrl,
            success:function(data){
                if(data.code=="0")
                {
                    window.location.href="${base}/adResource/toFindAdResource.do";
                }
                else
                {
                    alert("保存错误！");
                }
            }
        });     });  
```


##广告资源 - 查询


1.` <a id="search_btn" class="btn btn-primary">查询</a>`
引入adResource.js:` <script src="${resRoot}/advertisement/adResource/adResource.js"></script>`
```
//搜索过滤
    $( '#search_btn' ).on( 'click' , function (){
        
         var  query_form = $( '#query_form' );
        
         var  parm = $.serializeObject(query_form);
        console.info(parm);
        $( "#dataGrid" ).datagrid(  "page" ,1); //重置为第一页
        $( "#dataGrid" ).datagrid(  "fetch" ,parm);
             });   

```
```

jQuery - 基于serializeArray的serializeObject 
jQuery有方法$.fn.serialize，可将表单序列化成字符串；有方法$.fn.serializeArray，可将表单序列化成数组。
如果需要其序列化为JSON对象，那么可以基于serializeArray编写方法serializeObject轻松实现：
jQuery.prototype.serializeObject=function(){  
    var obj=new Object();  
    $.each(this.serializeArray(),function(index,param){  
        if(!(param.name in obj)){  
            obj[param.name]=param.value;  
        }  
    });  
    return obj;  
};  
注：当表单中参数出现同名时，serializeObject会取第一个，而忽略后续的。


<form>  
    <input type="text" name="username" />  
    <input type="text" name="password" />  
</form> 


jQuery("form").serialize(); //"username=&password="  
jQuery("form").serializeArray(); //[{name:"username",value:""},{name:"password",value:""}]  
jQuery("form").serializeObject(); //{username:"",password:""}
```
2.请求到AdResouceController:
```
/**
     * 查询广告资源
     * 
     *  @param  AdResource
     *  @return
     */
     @ResponseBody
     @RequestMapping (value =  "/findAdResource" , method = RequestMethod. POST )
     public  DataGrid<AdResourceDto> findAdResource(AdResourceDto  adResource )
    {
        DataGrid<AdResourceDto>  dataGrid ;
         try
        {
            Pager<AdResourceDto>  result  =  adResourceService .findAdResourceForPage( adResource ,  adResource .getPaging(),
                     adResource .getPage());
             if  ( null  !=  result .getDatas() &&  result .getDatas().size() > 0)
            {
                 for  (AdResourceDto  adRes  :  result .getDatas())
                {
                    SimpleDateFormat  sdf  =  new  SimpleDateFormat( "yyyy-MM-dd HH:mm:ss" );
                    Long  startTime  =  sdf .parse( adRes .getStartTime()).getTime();
                    Long  endTime  =  sdf .parse( adRes .getEndTime()).getTime();
                    Long  nowTime  =  new  Date().getTime();
                     if  ( startTime  >  nowTime )
                    {
                         adRes .setFstatus(0);
                    }
                     if  ( nowTime  >=  startTime  &&  endTime  >=  nowTime )
                    {
                         adRes .setFstatus(1);
                    }
                     if  ( endTime  <  nowTime )
                    {
                         adRes .setFstatus(2);
                    }
                }
            }
             dataGrid  =  new  DataGrid<AdResourceDto>( result .getTotalDataCount(),  result .getDatas());
        }  catch  (Exception  e )
        {
             dataGrid  =  null ;
        }
         return   dataGrid ;     }  
```
3.请求到AdResouceServiceImpl:
```
public  Pager<AdResourceDto> findAdResourceForPage(AdResourceDto  adResource , Integer  pageSize , Integer  pageNumber )
    {
         log .info( "findAdResourceForPageIn========>[{}]" , JSON. toJSONString ( new  Object[] {  adResource ,  pageSize ,  pageNumber  }));
        Pager<AdResourceDto>  page ;
         try
        {
             page  =  adResourceBaseServiceImpl .findAdResourceForPage( adResource ,  pageSize ,  pageNumber );
             log .debug( "findAdResourceForPageOut========>[{}]" , JSON. toJSONString ( page ));
        }  catch  (Exception  e )
        {
             log .error( "findAdResourceForPage========>Exception[{}]" , JSON. toJSONString ( e ));
             page  =  null ;
        }
         return   page ;
    }
```
 4.请求到AdResouceBaseServiceImpl:

```
public  List<AdResourceDto> findAdResourceForList(AdResourceDto  adResource )
    {
        Map<String, Object>  map  = DacUtils. convertToMap ( adResource );
         return   baseDao .queryForList( "adResource.findAdResource" ,  map , AdResourceDto. class );     }  
```
5.请求到BaseDaoImpl:   implements   BaseDao
```
   @Override
     public  <T> List<T> queryForList(String  sqlId , Map<String, Object>  paramMap , Class<T>  elementType ) {
         return   dalClient .queryForList( sqlId ,  paramMap ,  elementType );
    }
```
 6.关于AdResouceDto:


此类中封装了查询结果中所包含的所有的列，将所有列对应转化为字段，并设置get/setter方法。


##广告资源 - 操作（编辑、删除、查看） 

###查看


adResource.js  --> initDataGrid(id) --> ` <a href='javascript:void(0);' onclick='find_btn(" +data.row.id+ ")'>查看</a>`
```
function  find_btn(id)
{
    window.location.href= "${base}/adResource/findAdResourceDetailById.do?id=" +id; }  
```
跳转到广告资源详情：  findAdResourceDetailById

` ModelAndView  mav  =  new  ModelAndView(Constant. ADRESOURCE_PATH  +  "adResourceDetail.ftl" ); `
`AdResourceDto  adResource  =  adResourceService .findAdResourceById( id );` `mav .addObject( "adResource" ,  adResource );   `

广告类型：` <input value="${adResource.name}" readonly style="width: 200px;display: inline-block" type="text" class="form-control">`
商品图片  :  <img src="${adResource.adImageUrl}" width="250px" height="250px">
返回： <a id="back_btn" class="btn btn-primary">返回</a>
```
$("#back_btn").on("click",function(){
            window.location.href="${base}/adResource/toFindAdResource.do";
        })
```
 
###编辑


###删除
adResource.js:` <a href='javascript:void(0);' onclick='delete_btn(" +data.row.id+ ")'>删除</a>`
```
function  delete_btn(id)
{
     if (confirm( "确定删除？" ))
    {
         var  actionUrl =  "${base}/adResource/modifyAdResourceStatusById.do" ;
        $.ajax({
            type : 'get' ,
            url : actionUrl,
            data :{ "id" :id},
            success: function (data){
                 if (data.code== "0" )
                {
                    window.location.href= "${base}/adResource/toFindAdResource.do" ;
                }
                 else
                {
                    alert(data.message);
                }
            }
        });
    } }   

```
请求Controller， modifyAdResourceStatusById

