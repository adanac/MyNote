web.xml


index.html
```
< script >
location.href= "page/cust-list.html" ; </ script >   

```
cust-list.html


my_ajax
```
function  my_ajax(url,setting){
     var  para = {url:url,type: "post" };
    para.success = ajaxSuccess;
    para.error = ajaxError;
    para.data = setting.data== null ?  new  Object():setting.data;
    
    para.beforeSend = ajaxbeforeSend;
    para.success = ajaxSuccess;
    para.error = ajaxError;
     if  (setting.contentType!= null ) 
        para.contentType=setting.contentType;
     if  (setting.dataType!= null )
        para.dataType = setting.dataType;
    $.ajax(para);
    
     function  ajaxbeforeSend(jqXHR,p){
        console.log( "ajax call start. url=" +p.url+ " para=" +p.data);
    }
    
     function  ajaxSuccess(data,status,jaXHR){
        console.log( "ajax call success. status=" +status+ " data type=" + typeof (data));        
        console.log( "ajax call result result:" +JSON.stringify(data));
        
         if  (setting.success)    
            setting.success(data,status,jaXHR);    
        
         if  (setting.successjson){
             if  (!data.success) alert( "调用服务返回异常,错误码：" +data.code);
             else  setting.successjson(data.content);
        }
    }
     function  ajaxError(jqXHR,status,errorMsg){
        console.log( "ajax call error. status=" +status+ " msg=" +errorMsg);
        wa_notice( "网络不给力" );
         if  (setting.error)
            setting.error(jqXHR,status,errorMsg);
    } }   

```


##查看详情页面，发现有两列都可以实现






wa_gopara()方法
jplugin_common.js


addParamToLocation()方法，将url中的参数进行了编码加密。
url:


http://localhost:8082/firstapp-web/page/cust-detail.html?_WEB_PARA_=eyJjdXN0SWQiOiIxIn0=



##返回列表页面，跟跳转到详情页面的逻辑是一样的，也要执行addParamToLocation方法




可以看出来Object{}编码的的密文为e30=
url：http://localhost:8082/firstapp-web/page/cust-list.html?_WEB_PARA_=e30=
##跳转到新增与跳转到详情是同样的逻辑。
##新增保存


cust-new.html



com.haiziwang.study.firstapp.Plugin



com.haiziwang.study.firstapp.svc.impl.CustomerServiceImpl



com.haiziwang.study.firstapp.mapper.ICustomerMapper



ICustomerMapper.xml

```
<? xml   version = "1.0"   encoding = "UTF-8"   ?>
<! DOCTYPE   mapper
   PUBLIC   "-//mybatis.org//DTD Mapper 3.0//EN"
   "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
< mapper   namespace = "com.haiziwang.study.firstapp.mapper.ICustomerMapper" >
                                       
     < insert   id = "insert"   parameterType = "com.haiziwang.platform.study.firstapp.api.dbo.Customer"   useGeneratedKeys = "true"   keyProperty = "custId"   >
        insert into 
            customer (cust_name,cust_address,sex,cust_level,create_date)
            values(#{custName},#{custAddress},#{sex},#{custLevel},#{createDate})
     </ insert >
    
     < select   id = "find"   resultType = "com.haiziwang.platform.study.firstapp.api.dbo.Customer" >
        select 
            cust_id custId,
            cust_name custName,
            cust_address custAddress,
            sex,
            status,
            cust_level custLevel,
            create_date createDate 
        from 
            customer 
        where 
            cust_id = #{custId} 
     </ select >
     < delete   id = "delete"   >
        delete from 
            customer
        where
            cust_id = #{custId}
     </ delete >
     < update   id = "update"   parameterType = "com.haiziwang.platform.study.firstapp.api.dbo.Customer" >
        update 
            customer 
        set 
            cust_id = #{custId},
            cust_name = #{custName},
            cust_address = #{custAddress},
            sex = #{sex},
            status = #{status},
            cust_level = #{custLevel},
            create_date = #{createDate}
        where 
            cust_id=#{custId}
     </ update >
    
     <!-- parameterType可以不写，因为mapper层加了注解@Param -->
     < update   id = "updateStatus"   >
        update 
            customer 
        set  
            status=#{status}
        where 
            cust_id=#{custId}
     </ update >
     < select   id = "queryWithPage"   resultType = "com.haiziwang.platform.study.firstapp.api.dbo.Customer" >
        select 
            cust_id custId,
            cust_name custName,
            cust_address custAddress,
            sex,
            cust_level custLevel,
            create_date createDate 
        from 
            customer 
        where 
             <![CDATA[
            status  <>  'D' 
             ]]>
         < if   test = "query.custId !=null  and query.custId != '' " >
            and (cust_id = #{query.custId})
         </ if >
        
         < if   test = "query.custName !=null and query.custName != ''" >
            and (cust_name like CONCAT('%',#{query.custName},'%') ) 
         </ if >
         < if   test = "query.custLevel !=null and  query.custLevel !=''" >
            and (cust_level = #{query.custLevel} ) 
         </ if >
         < if   test = "query.status !=null and  query.status !=''" >
            and (status = #{query.status}   ) 
         </ if >
     </ select >
    
     < select   id = "queryAllCustomer"   resultType = "com.haiziwang.platform.study.firstapp.api.dbo.Customer" >
        select 
            cust_id custId,
            cust_name custName,
            cust_address custAddress,
            sex,
            cust_level custLevel,
            create_date createDate 
        from 
            customer 
        where 
             <![CDATA[
            status  <>  'D' 
             ]]>
     </ select >
     < select   id = "queryEmp"   resultType = "com.haiziwang.platform.study.firstapp.api.dbo.Emp" >
        select 
            *
        from 
            m_emp
        
     </ select >
     < insert   id = "addEmp"   parameterType = "com.haiziwang.platform.study.firstapp.api.dbo.Emp" >
        insert into    m_emp
            (login_pwd,phone,remark,real_name,status,create_time)
        values
            (#{loginPwd},#{phone},#{remark},#{realName},#{status},now())
     </ insert > </ mapper >   

```




##升级


应该与basic-config.properties中的app-code保持一致


只有这样才不会mapper报错。


##jplugin-test
```
问题一：org.adanac.jplugin.study.Plugin 中 添加的过滤器类，doFilter方法缺少filterChain参数，如何办？
```





