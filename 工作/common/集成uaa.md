##omsTool项目集成uaa
##1.omsTooll-intf添加uaa依赖
###2.omsTool-pom配置cas


### 3.omsTool-service配置及实现


```
1-1.menu.java
package  com.bn.b2b.omsTool.uaa.entity;
import  com.bn.framework.uaa.common.model.TreeNode;
/**
 * Created by kai on 2015/6/17.
 */
public   class  Menu  extends  TreeNode{
     private  String target;
     public  Menu() {
    }
     public  String getTarget() {
         return  target;
    }
     public   void  setTarget(String target) {
         this .target = target;
    } }  
1-2.menu2.java
package  com.bn.b2b.omsTool.uaa.entity;
import  java.util.List;
/**
 * Created by kai on 2015/6/17.
 */
public   class  Menu2 {
     private  Menu menu;
     private  List<Menu> subMenuList;
     public  Menu2() {
    }
     public  Menu getMenu() {
         return  menu;
    }
     public   void  setMenu(Menu menu) {
         this .menu = menu;
    }
     public  List<Menu> getSubMenuList() {
         return  subMenuList;
    }
     public   void  setSubMenuList(List<Menu> subMenuList) {
         this .subMenuList = subMenuList;
    }
}
1-3.user.java
package  com.bn.b2b.omsTool.uaa.entity;
import  java.io.Serializable;
/**
 * Created by kai on 2015/6/8.
 */
public   class  User  implements  Serializable {
     private  Integer userId;
     private  String userName;
     private  String nickName;
     public  User() {
    }
     public  Integer getUserId() {
         return  userId;
    }
     public   void  setUserId(Integer userId) {
         this .userId = userId;
    }
     public  String getUserName() {
         return  userName;
    }
     public   void  setUserName(String userName) {
         this .userName = userName;
    }
     public  String getNickName() {
         return  nickName;
    }
     public   void  setNickName(String nickName) {
         this .nickName = nickName;
    }
}
1-4. UserQueryForm.java
package  com.bn.b2b.omsTool.uaa.entity;
import  java.io.Serializable;
/**
 * Created by kai on 2015/6/8.
 */
public   class  UserQueryForm  implements  Serializable {
     private  Integer userId;
     private  String userName;
     private  String nickName;
     public  UserQueryForm() {
    }
     public  Integer getUserId() {
         return  userId;
    }
     public   void  setUserId(Integer userId) {
         this .userId = userId;
    }
     public  String getUserName() {
         return  userName;
    }
     public   void  setUserName(String userName) {
         this .userName = userName;
    }
     public  String getNickName() {
         return  nickName;
    }
     public   void  setNickName(String nickName) {
         this .nickName = nickName;
    }
}
2-1. SupplyService.java
package  com.bn.b2b.omsTool.uaa.service;
/**
 * 供应商
 *  @author  chucun
 *
 */
public   interface  SupplyService {
    
     /**
     * 通过用户id查询供应商id
     *  @param  userId 用户id
     *  @return
     */
    String findSupplyIdByUserId(String userId);
}
2-2. SupplyServiceImpl.java
package  com.bn.b2b.omsTool.uaa.service;
import  java.util.List;
import  org.springframework.stereotype.Service;
import  com.alibaba.dubbo.config.annotation.Reference;
import  com.bn.framework.log.MyLogger;
import  com.bn.framework.log.MyLoggerFactory;
import  com.bn.umc.umcCenter.intf.company.dto.CompanyInfoDto;
import  com.bn.umc.umcCenter.intf.company.service.CompanyInfoService;
@ Service
public   class  SupplyServiceImpl  implements  SupplyService {
     private   static   final  MyLogger LOGGER = MyLoggerFactory.getLogger(SupplyServiceImpl. class );
     @ Reference(check= false )
     private  CompanyInfoService companyInfoService;
     @ Override
     public  String findSupplyIdByUserId(String userId) {
         try  {
            List<CompanyInfoDto> queryResult = companyInfoService.queryCompanyByUserId(Long.parseLong(userId));
             if  ( null  != queryResult && queryResult.size() > 0){
                Long supplierId = queryResult.get(0).getCompanyId();
                 if  (0 == supplierId){
                     //查询不到
                    LOGGER.error( "查询不到供应商id" );
                     return   null ;
                } else  {
                     return  supplierId +  "" ;
                }
            } else {
                LOGGER.error( "查询不到供应商id" );
                 return   null ;
            }
        } catch  (Exception e){
            LOGGER.error( "" ,e);
             return   null ;
        }
    }
}  
2-3. SysMenuService.java
package  com.bn.b2b.omsTool.uaa.service;
import  java.util.List;
import  com.bn.framework.uaa.common.model.TreeNode;
/**
 * 
 * 功能描述： Menu相关的类
 * 
 *  @author  作者 陈琦
 * 
 */
public   interface  SysMenuService {
     /**
     * 查询所有菜单信息
     * 
     *  @return  List SimpleJsonTree类型的List为了能使zTree生成树形结构
     */
    List<TreeNode> findAllJsonMenu(String systemId);
     /**
     * 
     * 菜单信息转化成zTree兼容的方式
     * 
     *  @param  sysList
     *  @return  impleJsonTree类型的List为了能使zTree生成树形结构
     */
//    List<TreeNode> changeToZTreeData(List<SimpleJsonTree> sysList);
}
2-3: SysMenuServiceImpl.java
/**
 * SUNING APPLIANCE CHAINS.
 * Copyright (c) 2012 - 2012 All Rights Reserved.
 */
package  com.bn.b2b.omsTool.uaa.service;
import  java.util.List;
import  org.slf4j.Logger;
import  org.slf4j.LoggerFactory;
import  org.springframework.beans.factory.annotation.Autowired;
import  com.bn.framework.uaa.common.model.TreeNode;
import  com.bn.framework.uaa.core.manager.MenuManager;
import  com.bn.framework.uaa.core.manager.SysMenuManager;
/**
 * 
 * 功能描述：菜单加载接口实现类
 * 
 *  @author  Lin Sen  <a> 11010072@cnsuning.com </a>
 */
//@Service("sysMenuService")
public   class  SysMenuServiceImpl  implements  SysMenuService {
    Logger logger = LoggerFactory.getLogger( this .getClass());
     @ Autowired
    SysMenuManager sysMenuManager;
     @ Autowired
    MenuManager menuManager;
     /*
     * (non-Javadoc)
     * @see com.suning.framework.ucc.menu.SysMenuService#findAllJsonMenu()
     */
     @ Override
     public  List<TreeNode> findAllJsonMenu(String systemId) {
        List<TreeNode> treeNodeList = menuManager.getMenuListBySystemId(systemId.toUpperCase());
         return  treeNodeList;
    }
//    /*
//     * (non-Javadoc)
//     * @see com.suning.framework.ucc.menu.SysMenuService#changeToZTreeData(java.util.List)
//     */
//    @Override
//    public List<TreeNode> changeToZTreeData(List<SimpleJsonTree> sysList) {
//        for (SimpleJsonTree menu : sysList) {
//            if (StringUtils.startsWith(menu.getUrl(), "NO_URL")) {
//                menu.setUrl(null);
//            } else if (StringUtils.startsWith(menu.getUrl(), "/")) {
//                menu.setUrl(menu.getUrl().substring(1));
//            }
////            if (StringUtils.isNotEmpty(menu.getTarget())) {
////                menu.setTarget(menu.getTarget());
////            } else {
//                menu.setTarget("mainFrame");
////            }
//        }
//
//        return sysList;
//    }
//
//    private List<SimpleJsonTree> findAll() {
//        List<SimpleJsonTree> treeDataLst = new ArrayList<SimpleJsonTree>();
//        InputStream input = SysMenuServiceImpl.class.getClassLoader().getResourceAsStream("tree-data.properties");
//        Properties properties = new Properties();
//        try {
//            properties.load(input);
//        } catch (Exception e) {
//            throw new RuntimeException("加载树形菜单数据失败！");
//        }
//
//        for (Object value : properties.values()) {
//            String valStr = (String) value;
//            String[] treeDatas = valStr.split(",");
//            SimpleJsonTree menu = new SimpleJsonTree();
//            menu.setId(treeDatas[0]);
//            menu.setpId(treeDatas[1]);
//            menu.setName(treeDatas[2]);
//            // if(!"NO_URL".equalsIgnoreCase(treeDatas[3])){
//            menu.setUrl(treeDatas[3]);
//            menu.setTarget(treeDatas[4]);
//            // }
//            treeDataLst.add(menu);
//        }
//        return treeDataLst;
//    }
//
//    private List<SimpleJsonTree> sortMenuList(List<SimpleJsonTree> menuList) {
//        Comparator<SimpleJsonTree> comparator = new Comparator<SimpleJsonTree>() {
//            public int compare(SimpleJsonTree s1, SimpleJsonTree s2) {
//                return -Integer.valueOf(s1.getId()).compareTo(Integer.valueOf(s2.getId()));
//            }
//        };
//        Collections.sort(menuList, comparator);
//        return menuList;
//    }
}
3-1. /omsTool-service/src/main/resources/conf/spring/common.xml
<? xml   version = "1.0"   encoding = "UTF-8" ?>
< beans   xmlns = "http://www.springframework.org/schema/beans"
        xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"   xmlns:jee = "http://www.springframework.org/schema/jee"
        xsi:schemaLocation = "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee.xsd" >
< bean   class = "com.bn.b2b.omsTool.uaa.service.SysMenuServiceImpl"   id = "sysMenuService" />
     < bean   id = "propertyConfig"
              class = "org.springframework.beans.factory.config.PropertyPlaceholderConfigurer" >
               < property   name = "location" >
                      < value > classpath:conf/uaa-setting.properties </ value >
               </ property >
               < property   name = "systemPropertiesModeName"   value = "SYSTEM_PROPERTIES_MODE_OVERRIDE"   />
        </ bean >
        < jee:jndi-lookup   id = "dataSource2"   jndi-name = "jdbc/uaa"
                         proxy-interface = "javax.sql.DataSource"   lookup-on-startup = "false"   />
        < bean   id = "uaaDacClient"   class = "com.bn.framework.dac.client.support.DefaultDacClient" >
                  <!-- SQL的Xml配置路径 -->
             < property   name = "sqlMapConfigLocation" >
                      < list >
                             < value > classpath*:conf/sqlMap/sqlMap_uaa_execute.xml </ value >
                             <!--如果是 Oracle则使用下面的sql-->
                             <!-- <value>classpath*:conf/sqlMap/sqlMap_uaa_query_oracle.xml</value>
                            <value>classpath*:conf/sqlMap/uaa_sqlMap_page_oraclel.xml</value> -->
                             < value > classpath*:conf/sqlMap/sqlMap_user.xml </ value >
                              <!--如果是 Mysql则使用下面的sql-->
                              < value > classpath*:conf/sqlMap/sqlMap_uaa_query_mysql.xml </ value >
                              < value > classpath*:conf/sqlMap/uaa_sqlMap_page_mysql.xml </ value >
                      </ list >
               </ property >
               < property   name = "dataSource"   ref = "dataSource2" />
        </ bean >
</ beans >
3-2. /omsTool-service/src/main/resources/conf/spring/uaa-manager.xml
<? xml   version = "1.0"   encoding = "UTF-8" ?>
< beans   xmlns = "http://www.springframework.org/schema/beans"
     xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"   xmlns:context = "http://www.springframework.org/schema/context"
     xsi:schemaLocation = "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd" >
     < context:component-scan   base-package = "com.bn.framework.uaa"   />
    
     < bean   id = "authConfigurationManager"   class = "com.bn.framework.uaa.core.manager.ext.AuthConfiguationManagerImpl" />
    
         < bean   id = "cacheManager"   class = "org.springframework.cache.ehcache.EhCacheCacheManager" >
         < property   name = "cacheManager" >
             < bean   class = "org.springframework.cache.ehcache.EhCacheManagerFactoryBean" >
                 < property   name = "configLocation"   value = "classpath:conf/ehcache.xml" />
                 < property   name = "shared"   value = "true" />
                 < property   name = "cacheManagerName"   value = "uaaCacheManager" />
             </ bean >
         </ property >
     </ bean >
     <!-- cache的默认实现为ehcache，若项目组有特殊需求，可自行开发或集成其他实现 -->
     < bean   id = "authorizationManager"
         class = "com.bn.framework.uaa.core.manager.AuthorizationManagerImpl" >
         < property   name = "authorizationCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "authorizationCache"   />
             </ bean >
         </ property >
         < property   name = "collectionCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "collectionCache"   />
             </ bean >
         </ property >
         < property   name = "authOfCollectionCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "authOfCollectionCache"   />
             </ bean >
         </ property >
         < property   name = "collectionOfAuthCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "collectionOfAuthCache"   />
             </ bean >
         </ property >
         < property   name = "collectionOfRoleCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "collectionOfRoleCache"   />
             </ bean >
         </ property >
         < property   name = "roleOfcollectionCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "roleOfcollectionCache"   />
             </ bean >
         </ property >
         < property   name = "appertainedCollectionCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "appertainedCollectionCache"   />
             </ bean >
         </ property >
         < property   name = "containedCollectionCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "containedCollectionCache"   />
             </ bean >
         </ property >
     </ bean >
     < bean   id = "roleManager"   class = "com.bn.framework.uaa.core.manager.RoleManagerImpl" >
         < property   name = "roleCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "roleCache"   />
             </ bean >
         </ property >
         < property   name = "childRoleCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "childRoleCache"   />
             </ bean >
         </ property >
         < property   name = "parentRoleCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "parentRoleCache"   />
             </ bean >
         </ property >
         < property   name = "userRoleCache" >
             < bean   factory-bean = "cacheManager"   factory-method = "getCache" >
                 < constructor-arg   value = "userRoleCache"   />
             </ bean >
         </ property >
     </ bean >
</ beans >
3-3. /omsTool-service/src/main/resources/conf/spring/uaa-security-core.xml
<? xml   version = "1.0"   encoding = "UTF-8" ?>
< beans   xmlns = "http://www.springframework.org/schema/beans"
     xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"   xmlns:context = "http://www.springframework.org/schema/context"
     xmlns:aop = "http://www.springframework.org/schema/aop"   xmlns:security = "http://www.springframework.org/schema/security"
     xsi:schemaLocation = "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.1.xsd
       http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.0.xsd
      http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security-3.1.xsd" >
     <!--【基本服务配置-开始】 -->
     < bean   id = "systemInfoGetter"   class = "com.bn.framework.uaa.common.system.UaaSystemInfoGetter" >
         < property   name = "system"   value = "CSCD"   />
     </ bean >
    
     < bean   id = "componentManager"   class = "com.bn.framework.uaa.common.util.ComponentManager"   />
    
     < bean   id = "authorizationService"
         class = "com.bn.framework.uaa.core.service.AuthorizationServiceImpl" >
         < property   name = "authorizationManager"   ref = "authorizationManager"   />
         < property   name = "roleManager"   ref = "roleManager"   />
         <!-- <property name="userDetailsService" ref="userDetailsService" /> -->
         < property   name = "selectors" >
             < list >
                 < ref   bean = "urlAuthorizationSelector" />
                 < ref   bean = "standardAuthorizationSelector" />
             </ list >
         </ property >
     </ bean >
     < bean   id = "urlAuthorizationSelector"
         class = "com.bn.framework.uaa.core.service.selector.UrlAuthorizationSelector" >
         < property   name = "authorizationManager"   ref = "authorizationManager"   />
     </ bean >
     < bean   id = "standardAuthorizationSelector"
         class = "com.bn.framework.uaa.core.service.selector.StandardAuthorizationSelector" >
         < property   name = "authorizationManager"   ref = "authorizationManager"   />
         < property   name = "evaluator" >
             < bean   class = "com.bn.framework.uaa.core.service.expression.SpelEvaluator"   />
         </ property >
         < property   name = "conversionService" >
             < bean   class = "org.springframework.context.support.ConversionServiceFactoryBean"   />
         </ property >
     </ bean >
     <!--【基本服务配置-结束】 -->
     <!--【树型结构数据过滤配置-开始】 -->
     < bean   id = "treeFilterAspect"   class = "com.bn.framework.uaa.common.access.tree.TreeFilterAspect" >
         < property   name = "filter" >
             < bean   class = "com.bn.framework.uaa.common.access.tree.FlatBasedTreeFilter" >
                 < property   name = "authorizationService"   ref = "authorizationService"   />
                 <!-- <property name="currentUserInfoGetter" ref="currentUserInfoGetter" /> -->
                 < property   name = "treeNodeInfoExtractor" >
                     < bean   class = "com.bn.framework.uaa.common.access.tree.DefaultTreeNodeInfoExtractor" >
                         < property   name = "idPropertyName"   value = "id"   />
                         < property   name = "urlPropertyName"   value = "url"   />
                         < property   name = "systemPropertyName"   value = "systemId"   />
                         < property   name = "accessPolicy"   value = "ID_BASED"   />
                     </ bean >
                 </ property >
             </ bean >
         </ property >
     </ bean >
     < aop:config   proxy-target-class = "false" >
         < aop:aspect   id = "treeAspect"   ref = "treeFilterAspect" >
             < aop:pointcut   id = "target"
                           expression = "execution(* com.bn.b2b.omsTool.uaa.service.SysMenuServiceImpl.findAllJsonMenu(..))"   />
             < aop:around   method = "filter"   pointcut-ref = "target"   />
         </ aop:aspect >
     </ aop:config >
     <!--【树型结构数据过滤配置-结束】 -->
</ beans >  
```
### 4.omsTool-web配置及实现


```
1-1. MyMenuController.java
/**
 * SUNING APPLIANCE CHAINS.
 * Copyright (c) 2012 - 2012 All Rights Reserved.
 */
package  com.bn.b2b.omsTool;
import  java.io.UnsupportedEncodingException;
import  java.lang.reflect.InvocationTargetException;
import  java.util.ArrayList;
import  java.util.List;
import  javax.servlet.http.HttpServletResponse;
import  org.apache.commons.beanutils.BeanUtils;
import  org.apache.commons.lang3.StringUtils;
import  org.springframework.beans.factory.annotation.Autowired;
import  org.springframework.stereotype.Controller;
import  org.springframework.ui.Model;
import  org.springframework.web.bind.annotation.RequestMapping;
import  com.alibaba.fastjson.JSON;
import  com.bn.b2b.omsTool.uaa.entity.Menu;
import  com.bn.b2b.omsTool.uaa.entity.Menu2;
import  com.bn.b2b.omsTool.uaa.service.SysMenuService;
import  com.bn.framework.uaa.common.model.TreeNode;
import  com.bn.framework.uaa.common.system.UaaSystemInfoGetter;
import  com.bn.framework.uaa.core.manager.SysMenuManager;
import  com.bn.framework.web.controller.BaseController;
/**
 * 〈一句话功能简述〉 <br>
 * Controller 〈功能详细描述〉跳转index页面
 */
@ Controller
@ RequestMapping( "/" )
public   class  MyMenuController  extends  BaseController{
     @ Autowired
    SysMenuService sysMenuService;
     @ Autowired
    SysMenuManager sysMenuManager;
     @ Autowired
     private  UaaSystemInfoGetter uaaSystemInfoGetter;
     @ RequestMapping(value =  "findAllJsonMenu" )
     public  String findAllJsonMenu(String callback, Model model, HttpServletResponse response,
            Long treeRootId,  boolean  cleanCache)  throws  UnsupportedEncodingException {
         if (treeRootId== null ) {
            treeRootId=1L;
        }
        String system = uaaSystemInfoGetter.getSystemName();
        List<TreeNode> treeList = sysMenuService.findAllJsonMenu(system);
        List<Menu> menuList = changeListToTreeData(treeList);
         long  rootId = 0;
        List<Menu2> menu2List =  new  ArrayList<Menu2>();
         if (menuList !=  null ){
             for (Menu menu : menuList){
                 if (menu.getParentId() == 0L){
                    rootId = menu.getId();
                     break ;
                }
            }
            List<Menu> subMenuList =  null ;
            Menu2 menu2 =  null ;
             for (Menu menu : menuList){
                 if (menu.getParentId() != 0L){
                     if (menu.getParentId().equals(rootId)){
                        menu2 =  new  Menu2();
                        subMenuList =  new  ArrayList<Menu>();
                        menu2.setSubMenuList(subMenuList);
                        menu2.setMenu(menu);
                        menu2List.add(menu2);
                    } else  {
                        subMenuList.add(menu);
                    }
                }
            }
        }
        String renderStr = callback+ "(" +JSON.toJSONString(menu2List)+ ")" ;
         return  ajaxJson(response, renderStr);
    }
     private  List<Menu> changeListToTreeData(List<TreeNode> treeList){
        List<Menu> menuList =  new  ArrayList<Menu>();
         for  (TreeNode item : treeList) {
             if  (StringUtils.isEmpty(item.getUrl())) {
                item.setUrl( null );
            } else   if  (StringUtils.startsWith(item.getUrl(),  "NO_URL" )) {
                item.setUrl( null );
            }  else   if  (StringUtils.startsWith(item.getUrl(),  "/" )) {
                item.setUrl(item.getUrl().substring(1));
            }
            Menu menuItem =  new  Menu();
             try  {
                BeanUtils.copyProperties(menuItem,item);
                menuItem.setTarget( "mainFrame" );
                menuList.add(menuItem);
            }  catch  (IllegalAccessException e) {
                e.printStackTrace();
            }  catch  (InvocationTargetException e) {
                e.printStackTrace();
            }
        }
         return  menuList;
    }
}
2-1. LoginFilter.java
package  com.bn.b2b.omsTool.filter;
import  java.io.IOException;
import  java.util.HashMap;
import  java.util.Map;
import  javax.servlet.Filter;
import  javax.servlet.FilterChain;
import  javax.servlet.FilterConfig;
import  javax.servlet.ServletContext;
import  javax.servlet.ServletException;
import  javax.servlet.ServletRequest;
import  javax.servlet.ServletResponse;
import  javax.servlet.http.HttpServletRequest;
import  javax.servlet.http.HttpServletResponse;
import  org.springframework.context.ApplicationContext;
import  org.springframework.web.context.support.WebApplicationContextUtils;
import  com.bn.b2b.omsTool.uaa.service.SupplyService;
import  com.bn.b2b.omsTool.util.CommonUtil;
/**
 * 供应商登录拦截
 */
public   class  LoginFilter  implements  Filter {
     private  Map<String, String> ignore; // ignore url
     private  SupplyService supplyService;
     @ Override
     public   void  init(FilterConfig filterConfig)  throws  ServletException {
        ServletContext servletContext = filterConfig.getServletContext();
        ApplicationContext applicationContext = WebApplicationContextUtils
                .getRequiredWebApplicationContext(servletContext);
        supplyService = applicationContext.getBean(SupplyService. class );
        ignore =  new  HashMap<String, String>();
        ignore.put( "/loginError.do" ,  "/loginError.do" );
        ignore.put( "/login.do" ,  "/login.do" );
    }
     @ Override
     public   void  doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain)
             throws  IOException, ServletException {
        HttpServletRequest req = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;
        String currentUrl = req.getServletPath();
         // ignore url
         if  (ignore.get(currentUrl) !=  null ) {
            filterChain.doFilter(servletRequest, servletResponse);
             return ;
        }
        doFilterEnableSSO(req, response, filterChain);
    }
     private   void  doFilterEnableSSO(HttpServletRequest req, HttpServletResponse response, FilterChain filterChain)
             throws  IOException, ServletException {
         // 启用
         // 从sso获取userId
        String loginUserId = req.getRemoteUser();
         if  ( null  == loginUserId ||  "" .equals(loginUserId)) {
             // 获取不到登录的用户
            redirect(req, response,  "/loginError.do" );
             return ;
        }  else  {
             // 获取登录供应商
            String supplierId = CommonUtil.getSupplierIdFromSession(req);
             if  (supplierId ==  null ) {
                supplierId = supplyService.findSupplyIdByUserId(loginUserId);
                 if (supplierId ==  null ){
                    supplierId =  "-1" ;
                }
                 // 加入session
                CommonUtil.addSupplierIdInSession(req, supplierId);
            }
            filterChain.doFilter(req, response);
        }
    }
     // 重定向
     public   void  redirect(HttpServletRequest request, HttpServletResponse response, String redirectUrl)
             throws  IOException {
        response.sendRedirect(request.getContextPath() + redirectUrl);
    }
     @ Override
     public   void  destroy() {
    }
}
2-2. PreAuthenticationFilter.java
package  com.bn.b2b.omsTool.filter;
import  javax.servlet.http.HttpServletRequest;
import  com.bn.framework.uaa.common.access.web.AbstractPreAuthenticationFilter;
import  com.bn.framework.uaa.common.model.Identity;
/**
 * 
 *  @description  权限过滤器
 *  @author  xuneng
 *  @date  2015 - 07 - 013
 */
public   class  PreAuthenticationFilter  extends  AbstractPreAuthenticationFilter  {
     /* (non-Javadoc)
     * @see com.suning.fraamework.uaa.access.web.AbstractPreAuthenticationFilter#getPreAuthenticatedPrincipal(javax.servlet.http.HttpServletRequest)
     */
     @ Override
     protected  Object getPreAuthenticatedPrincipal(HttpServletRequest request) {
         return  request.getRemoteUser();
    }
     /* (non-Javadoc)
     * @see com.suning.framework.uaa.access.web.AbstractPreAuthenticationFilter#successfulAuthentication(java.lang.Object, com.suning.framework.uaa.model.Identity)
     */
     @ Override
     protected  Object successfulAuthentication(Object principal, Identity identity) {
        identity.setId(String.valueOf(principal));
         return  identity;
    }
}
3-1. spring-servlet.xml添加
<context:component-scan base-package="org.jasig.cas.client">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>     </context:component-scan>  
3-2.sso-applicationContext.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="
          http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd
          http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
        <!--验证码生成器-->
<bean id="captchaProducer" class="com.google.code.kaptcha.impl.DefaultKaptcha">
    <property name="config">
     <bean class="com.google.code.kaptcha.util.Config">
      <constructor-arg type="java.util.Properties">
        <props>
          <prop key="kaptcha.textproducer.font.size">60</prop>
          <prop key="kaptcha.image.width">300</prop>
          <prop key="kaptcha.image.height">100</prop>
          <prop key="kaptcha.textproducer.char.string"></prop>
          <prop key="kaptcha.textproducer.char.length"></prop>
          <prop key="kaptcha.obscurificator.impl">com.google.code.kaptcha.impl.WaterRipple</prop>
         </props>
       </constructor-arg>
      </bean>
     </property>
</bean>
<!--cookie的路径设置-->
    <bean id="cookiePath" class="java.lang.String" >
        <constructor-arg value="${app-root}"/>
    </bean>
<bean id="webPropertyConfigService"          class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="locations">
            <list>
                <value>classpath:conf/main-setting-web.properties</value>
            </list>
        </property>
        <property name="placeholderPrefix" value="@{" />
        <property name="systemPropertiesModeName" value="SYSTEM_PROPERTIES_MODE_OVERRIDE" />
    </bean>
    <!--获取cas js的过滤器-->
    <bean id="CasScriptFilter" class="org.jasig.cas.client.util.CasScriptFilter">
        <property name="appRoot">
            <value>@{app-root}</value>
        </property>
        <property name="casProjectName" >
            <value>@{cas-project}</value>
        </property>
        <property name="casServerUrl">
            <value>@{cas.server.url}</value>
        </property>
        <property name="clientServerUrl">
            <value>@{client.server.url}</value>
        </property>
    </bean>
    <!--登出过滤器-->
    <bean id="CASSingleSignOutFilter" class="org.jasig.cas.client.session.SingleSignOutFilter"
         p:HANDLER-ref="logoutHandler"/>
    <!--登出的具体实现-->
    <bean id="logoutHandler" class="org.jasig.cas.client.session.SingleSignOutHandler"
          p:userCookieHandler-ref="userCookieHandler"/>
    <!--拦截请求，判断当前用户是否已登录，如果未登录则跳转到CAS server登录-->
    <bean id="CASAuthenticationFilter" class="org.jasig.cas.client.authentication.AuthenticationFilter"
          p:userCookieHandler-ref="userCookieHandler"/>
    <!--验证Ticket的有效性和初始化Session,把用户信息json序列化后，加密保存到cookie,不使用Session-->
    <bean id="CasValidationFilter" class="org.jasig.cas.client.validation.Cas30ProxyReceivingTicketValidationFilter"
            p:userCookieHandler-ref="userCookieHandler"
            p:casServerUserFieldPairToClientUserField-ref="casServerUserFieldPairToClientUserField"
            p:useSession="false"
           />
    <!--p:userClass-ref="usrInfoClass"-->
    <!--从cookie中获取用户相关的信息-->
    <bean id="UserThreadLocalFilter" class="org.jasig.cas.client.util.UserThreadLocalFilter"
          p:userCookieHandler-ref="userCookieHandler"
         />
    <!--p:userInfoClass-ref="usrInfoClass"-->
    <!--client 和 casServer之间 用户对象中字段的对应关系 key是本地usr bean对象属性的名称，value是casServer端usr bean对象属性的名称-->
    <util:map id="casServerUserFieldPairToClientUserField">
        <entry key="userName" value="userName"/>
        <entry key="userId" value="userId"/>
        <entry key="userType" value="userType"/>
        <entry key="role" value="role"/>
        <entry key="status" value="status"/>
        <entry key="shopId" value="shopId"/>
    </util:map>
<!--    <!–转换的User对象，用户可继承UserInfo对象，获取更多属性–>
    <!– 不推荐 ，通过继承的形式扩展UserInfo属性–>
    <bean id="usrInfoClass" class="java.lang.Class" factory-method="forName">
        <constructor-arg value="com.bn.MyUserInfo"/>
    </bean>-->
    <!--用户登录登出Cookie操作-->
    <bean id="userCookieHandler" class="org.jasig.cas.client.cookie.UserCookieHandler"
          p:userSSLLoginedFlagCookieGenerator-ref="userSSLLoginedFlagCookieGenerator"
          p:userLoginedFlagCookieGenerator-ref="userLoginedFlagCookieGenerator"
          p:userInfoCookieGenerator-ref="userInfoCookieGenerator"
          p:sessionTrackerCookieGenerator-ref="userSessionTrackerCookieGenerator"
          p:mobileUserSSLLoginedFlagCookieGenerator-ref="mobileUserSSLLoginedFlagCookieGenerator"
          p:mobileUserLoginedFlagCookieGenerator-ref="mobileUserLoginedFlagCookieGenerator"
          p:mobileUserInfoCookieGenerator-ref="mobileUserInfoCookieGenerator"
          p:mobileSessionTrackerCookieGenerator-ref="mobileUserSessionTrackerCookieGenerator"
          p:mobileLoginCookieGenerator-ref="mobileLoginCookieGenerator"
            />
<!--用户的信息Cookie的处理，默认存储在客户端一天-->
    <!--用户的信息Cookie的处理，默认存储在客户端一天-->
    <bean id="userInfoCookieGenerator" class="org.jasig.cas.client.cookie.CookieRetrievingCookieGenerator"
          c:casCookieValueManager-ref="userInfoCookieValueManager"
          p:cookieSecure="false"
          p:cookieMaxAge="86400"
          p:cookieName="UC"
          p:cookieHttpOnly="true"
          p:cookiePath-ref="cookiePath"/>
    <!--用户信息Cookie的实际操作类-->
    <bean id="userInfoCookieValueManager" class="org.jasig.cas.client.cookie.DefaultCasCookieValueManager"
          p:cipherExecutor-ref="userInfoCipherExecutor"/>
    <!--用户信息Cookie的加密，必须修改其值，secretKeyEncryption 为43个字符，secretKeySigning为86 个字符 -->
    <bean id="userInfoCipherExecutor" class="org.jasig.cas.client.util.DefaultCipherExecutor"
          c:secretKeyEncryption="1PbwSbnHeinpkZOSZjuSJ8yYpUrInm5aaV18J2Ar4rM" c:secretKeySigning="szxK-5_eJjs-aUj-64MpUZ-GPPzGLhYPLGl0wrYjYNVAGva2P0lLe6UGKGM7k8dWxsOVGutZWgvmY3l5oVPO3w"/>
<!--用户是否登录标识的Cookie处理-->
    <!--用户是否登录标识的Cookie处理-->
    <bean id="userLoginedFlagCookieGenerator" class="org.jasig.cas.client.cookie.CookieRetrievingCookieGenerator"
          c:casCookieValueManager-ref="userLoginFlagCookieValueManager"
          p:cookieSecure="false"
          p:cookieMaxAge="-1"
          p:cookieName="ULF"
          p:cookieHttpOnly="true"
          p:cookiePath-ref="cookiePath"/>
    <!--用户是否登录标识Cookie的实际操作类-->
    <bean id="userLoginFlagCookieValueManager" class="org.jasig.cas.client.cookie.DefaultCasCookieValueManager"
          p:cipherExecutor-ref="userLoginFlagCipherExecutor"/>
    <!--用户是否登录标识Cookie的加密必须修改其值，secretKeyEncryption 为43个字符，secretKeySigning为86 个字符 -->
    <bean id="userLoginFlagCipherExecutor" class="org.jasig.cas.client.util.DefaultCipherExecutor"
          c:secretKeyEncryption="1PbwSbnHeinpkZOSZjuSJ8yYpUrInm5aaV18J2Ar4rM" c:secretKeySigning="szxK-5_eJjs-aUj-64MpUZ-GPPzGLhYPLGl0wrYjYNVAGva2P0lLe6UGKGM7k8dWxsOVGutZWgvmY3l5oVPO3w"/>
<!--用户SSL场景下是否登录的标识的Cookie处理-->
    <!--用户SSL场景下是否登录的标识的Cookie处理-->
    <bean id="userSSLLoginedFlagCookieGenerator" class="org.jasig.cas.client.cookie.CookieRetrievingCookieGenerator"
          c:casCookieValueManager-ref="userSSLLoginFlagCookieValueManager"
          p:cookieSecure="true"
          p:cookieMaxAge="-1"
          p:cookieName="ULFSSL"
          p:cookieHttpOnly="true"
          p:cookiePath-ref="cookiePath"/>
    <!--用户SSL场景下是否登录的标识的实际操作类-->
    <bean id="userSSLLoginFlagCookieValueManager" class="org.jasig.cas.client.cookie.DefaultCasCookieValueManager"
          p:cipherExecutor-ref="userSSLLoginFlagCipherExecutor"/>
    <!--用户SSL场景下是否登录标识的Cookie的加密必须修改其值，secretKeyEncryption 为43个字符，secretKeySigning为86 个字符 -->
    <bean id="userSSLLoginFlagCipherExecutor" class="org.jasig.cas.client.util.DefaultCipherExecutor"
          c:secretKeyEncryption="1PbwSbnHeinpkZOSZjuSJ8yYpUrInm5aaV18J2Ar4rM" c:secretKeySigning="szxK-5_eJjs-aUj-64MpUZ-GPPzGLhYPLGl0wrYjYNVAGva2P0lLe6UGKGM7k8dWxsOVGutZWgvmY3l5oVPO3w"/>
<!--用户回话跟踪cookie的处理-->
    <!--用户回话跟踪cookie的处理-->
    <bean id="userSessionTrackerCookieGenerator" class="org.jasig.cas.client.cookie.CookieRetrievingCookieGenerator"
          c:casCookieValueManager-ref="userSessionTrackerCookieValueManager"
          p:cookieSecure="false"
          p:cookieMaxAge="86400"
          p:cookieName="sessionTracker"
          p:cookieHttpOnly="true"
          p:cookiePath-ref="cookiePath"/>
 <bean id="userSessionTrackerCookieValueManager" class="org.jasig.cas.client.cookie.NoOpCookieValueManager"/>
    <!--手机用户登录cookie相关信息的处理,下面的所有字段不要改动-->
    <!--手机用户 信息-->
    <bean id="mobileUserInfoCookieGenerator" class="org.jasig.cas.client.cookie.CookieRetrievingCookieGenerator"
          c:casCookieValueManager-ref="mobileUserInfoCookieValueManager"
          p:cookieSecure="false"
          p:cookieMaxAge="86400"
          p:cookieName="UC"
          p:cookieHttpOnly="true"
          p:cookiePath-ref="cookiePath"/>
    <bean id="mobileUserInfoCookieValueManager" class="org.jasig.cas.client.cookie.MobileCasCookieValueManager"
          p:cipherExecutor-ref="mobileUserInfoCipherExecutor"/>
    <bean id="mobileUserInfoCipherExecutor" class="org.jasig.cas.client.util.DefaultCipherExecutor"
          c:secretKeyEncryption="ExIAOmDvnT82l1xDvXV6HG6WBE43rzFeTV8BbXthMAW" c:secretKeySigning="JcPdyO3HO-ymETt1PSEHvNQbPNkSAItVABEvrGwrCL9OxeST2Wt4FuYH_sqfx9erBLs8mb_MkbfJgg6cdCtQBO"/>
    <!--手机用户 是否登录的标识-->
    <bean id="mobileUserLoginedFlagCookieGenerator" class="org.jasig.cas.client.cookie.CookieRetrievingCookieGenerator"
          c:casCookieValueManager-ref="mobileUserLoginFlagCookieValueManager"
          p:cookieSecure="false"
          p:cookieMaxAge="-1"
          p:cookieName="ULF"
          p:cookieHttpOnly="true"
          p:cookiePath-ref="cookiePath"/>
    <bean id="mobileUserLoginFlagCookieValueManager" class="org.jasig.cas.client.cookie.MobileCasCookieValueManager"
          p:cipherExecutor-ref="mobileUserLoginFlagCipherExecutor"/>
    <bean id="mobileUserLoginFlagCipherExecutor" class="org.jasig.cas.client.util.DefaultCipherExecutor"
          c:secretKeyEncryption="cN5vk0RGpHWVLufLZdPPi2m6a3HO4hpinE7IbQKaWie" c:secretKeySigning="yEgYY1poumdCohlUC7oTfCBSlycVDvdzXI6DgSB5neOLc-Odyrp7Gy9kvQPzLuB5yohIND_btpRLN-Ph4A2YPy"/>
    <!--手机用户 SSL场景下是否登录的标识-->
    <bean id="mobileUserSSLLoginedFlagCookieGenerator" class="org.jasig.cas.client.cookie.CookieRetrievingCookieGenerator"
          c:casCookieValueManager-ref="mobileUserSSLLoginFlagCookieValueManager"
          p:cookieSecure="true"
          p:cookieMaxAge="-1"
          p:cookieName="ULFSSL"
          p:cookieHttpOnly="true"
          p:cookiePath-ref="cookiePath"/>
    <bean id="mobileUserSSLLoginFlagCookieValueManager" class="org.jasig.cas.client.cookie.MobileCasCookieValueManager"
          p:cipherExecutor-ref="mobileUserSSLLoginFlagCipherExecutor"/>
    <bean id="mobileUserSSLLoginFlagCipherExecutor" class="org.jasig.cas.client.util.DefaultCipherExecutor"
          c:secretKeyEncryption="XxZ5qYt602f6sNFTNQuqAvHybD4yrjqc7LTJzdKZkTO" c:secretKeySigning="yEgYY1poumdCohlUC7oTfCBSlycVDvdzXI6DgSB5neOLc-Odyrp7Gy9kvQPzLuB5yohIND_btpRLN-Ph4A2YPy"/>
    <!--手机用户 回话跟踪cookie-->
    <bean id="mobileUserSessionTrackerCookieGenerator" class="org.jasig.cas.client.cookie.CookieRetrievingCookieGenerator"
          c:casCookieValueManager-ref="mobileUserSessionTrackerCookieValueManager"
          p:cookieSecure="false"
          p:cookieMaxAge="86400"
          p:cookieName="sessionTracker"
          p:cookieHttpOnly="true"
          p:cookiePath-ref="cookiePath"/>
    <bean id="mobileUserSessionTrackerCookieValueManager" class="org.jasig.cas.client.cookie.MobileCasCookieValueManager"
          p:cipherExecutor-ref="mobileUserSessionTrackerCipherExecutor"/>
    <bean id="mobileUserSessionTrackerCipherExecutor" class="org.jasig.cas.client.util.NoOpCipherExecutor" />
    <!--手机用户 登录标识cookie-->
    <bean id="mobileLoginCookieGenerator" class="org.jasig.cas.client.cookie.CookieRetrievingCookieGenerator"
          c:casCookieValueManager-ref="mobileLoginCookieValueManager"
          p:cookieSecure="false"
          p:cookieMaxAge="86400"
          p:cookieName="MOBILE"
          p:cookieHttpOnly="true"
          p:cookiePath-ref="cookiePath"/>
    <bean id="mobileLoginCookieValueManager" class="org.jasig.cas.client.cookie.MobileCasCookieValueManager"
          p:cipherExecutor-ref="mobileLoginCipherExecutor"/>
    <bean id="mobileLoginCipherExecutor" class="org.jasig.cas.client.util.DefaultCipherExecutor"
          c:secretKeyEncryption="7jpRe9tQlOX932COQI2QlJavvDjtIYFtOWpTyXuf04o" c:secretKeySigning="7YlxP7PHULT44U_9TM7jJSzu-UTUX06_MHiPf6a51ru13NLXzGH-EI34OJmO93jxQMm_CPe5mDqh3CR_qfxaql"/>
</beans>
3-3.ehcache.xml
<ehcache updateCheck="false" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.sf.net/ehcache.xsd">
    <diskStore path="java.io.tmpdir"/>
    <cacheManagerEventListenerFactory class="" properties=""/>
    
    <!-- multicastGroupPort需要保证与其他系统不重复，请联系权限项目组成员，进行端口注册  -->
    <!-- 若因未注册，配置了重复端口，造成权限缓存数据异常，请自行解决  -->
   <!--  <cacheManagerPeerProviderFactory
            class="net.sf.ehcache.distribution.RMICacheManagerPeerProviderFactory"
            properties="peerDiscovery=automatic,
                        multicastGroupAddress=224.0.0.1,
                        multicastGroupPort=4549, timeToLive=1"/> -->
    <!--配置ehcacheRMI缓存，使其可以在多台服务器 之间缓存同步-->
    <cacheManagerPeerProviderFactory
            class="net.sf.ehcache.distribution.RMICacheManagerPeerProviderFactory"
            properties="peerDiscovery=manual,
                        rmiUrls=//localhost:4567/roleCache|//localhost:4567/childRoleCache
                               |//localhost:4567/parentRoleCache|//localhost:4567/userRoleCache
                               |//localhost:4567/authorizationCache|//localhost:4567/collectionCache
                               |//localhost:4567/appertainedCollectionCache|//localhost:4567/containedCollectionCache
                               |//localhost:4567/authOfCollectionCache|//localhost:4567/collectionOfAuthCache
                               |//localhost:4567/collectionOfRoleCache|//localhost:4567/roleOfcollectionCache"/>
    <cacheManagerPeerListenerFactory
            class="net.sf.ehcache.distribution.RMICacheManagerPeerListenerFactory"
            properties="hostName=localhost, port=4567,socketTimeoutMillis=1000"/>
    <defaultCache maxElementsInMemory="10000"
                  eternal="true"
                  timeToIdleSeconds="0"
                  timeToLiveSeconds="0"
                  overflowToDisk="true"
                  diskPersistent="false"
                  diskExpiryThreadIntervalSeconds="120"
                  memoryStoreEvictionPolicy="LRU"/>
    <cache name="roleCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="childRoleCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="parentRoleCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    
    <cache name="userRoleCache"
           maxElementsInMemory="5000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    
    <cache name="authorizationCache"
           maxElementsInMemory="50000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    
    <cache name="collectionCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="appertainedCollectionCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="containedCollectionCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="authOfCollectionCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="collectionOfAuthCache"
           maxElementsInMemory="50000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="collectionOfRoleCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
    <cache name="roleOfcollectionCache"
           maxElementsInMemory="1000"
           eternal="true"
           overflowToDisk="false"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LFU">
        <cacheEventListenerFactory
                class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"
                properties="replicateAsynchronously=true, replicatePuts=true, replicateUpdates=true, replicateUpdatesViaCopy=true, replicateRemovals=true"/>
        <!--<bootstrapCacheLoaderFactory-->
                <!--class="net.sf.ehcache.distribution.RMIBootstrapCacheLoaderFactory"/>-->
    </cache>
</ehcache>
3-4.main-setting-web.properties
base= ${app-root}
resRoot= ${app-root}/RES
appDomain= ${app-domain}
jscssVersion= ${jscssVersion}
appVersion= ${project.version}
#sso\u96C6\u6210
cas.server.url= ${cas.server.url}
client.server.url= ${client.server.url}
app-root= ${app-root}
cas-project= ${cas-project}
3-5.uaa-setting.properties
appVersion= $[project.version]
envName= DEV
4-1foot.ftl
<script>
jQuery(document).ready(function() {    
        var header=$("#headerdiv"),menu=$("#menudiv"),theme_panel=$("#theme_paneldiv"),footer=$("#footerdiv"),time;
        
        
       //header.load("pages/common/header.ftl");
        //menu.load("pages/common/menu.ftl");
       // theme_panel.load("pages/common/theme-panel.ftl");
        //footer.load("pages/common/footer.ftl");
        time=window.setInterval(function(){     
            if(header.children("*").length>0&&menu.children("*").length>0){
                Metronic.init(); // init metronic core components
                Layout.init(); // init current layout
                QuickSidebar.init(); // init quick sidebar
                Demo.init(); // init demo features
                Menu();
                clearInterval(time);
            }
        },100)
         
        
        
        $('.date-picker').datepicker({
            rtl: Metronic.isRTL(),
            language: 'zh-CN',
            autoclose: true
        });
        
        //加载菜单
        var myTemplate = Handlebars.compile($("#meuns-template").html());
        $.getJSON("${base}/findAllJsonMenu.do?callback=?",{},
        function(data){
            $('ul.page-sidebar-menu').append(myTemplate(data));
            $('ul.page-sidebar-menu li:nth-child(11)').attr("class","active open");
        }, true)
    });
</script>
4-2.menu.ftl

    <!-- BEGIN SIDEBAR -->
    
    <div id="menu" class="page-sidebar-wrapper">
        <!-- DOC: Set data-auto-scroll="false" to disable the sidebar from auto scrolling/focusing -->
        <!-- DOC: Change data-auto-speed="200" to adjust the sub menu slide up/down speed -->
        <div class="page-sidebar navbar-collapse collapse">
            <!-- BEGIN SIDEBAR MENU -->
            <!-- DOC: Apply "page-sidebar-menu-light" class right after "page-sidebar-menu" to enable light sidebar menu style(without borders) -->
            <!-- DOC: Apply "page-sidebar-menu-hover-submenu" class right after "page-sidebar-menu" to enable hoverable(hover vs accordion) sub menu mode -->
            <!-- DOC: Apply "page-sidebar-menu-closed" class right after "page-sidebar-menu" to collapse("page-sidebar-closed" class must be applied to the body element) the sidebar sub menu mode -->
            <!-- DOC: Set data-auto-scroll="false" to disable the sidebar from auto scrolling/focusing -->
            <!-- DOC: Set data-keep-expand="true" to keep the submenues expanded -->
            <!-- DOC: Set data-auto-speed="200" to adjust the sub menu slide up/down speed -->
            <ul class="page-sidebar-menu " data-keep-expanded="false" data-auto-scroll="true" data-slide-speed="200">
                <!-- DOC: To remove the sidebar toggler from the sidebar you just need to completely remove the below "sidebar-toggler-wrapper" LI element -->
                <li class="sidebar-toggler-wrapper">
                    <!-- BEGIN SIDEBAR TOGGLER BUTTON -->
                    <div class="sidebar-title">
                    商家中心
                    </div>
                    <div class="sidebar-toggler">
                    </div>
                    <!-- END SIDEBAR TOGGLER BUTTON -->
                </li>
                <!-- DOC: To remove the search box from the sidebar you just need to completely remove the below "sidebar-search-wrapper" LI element -->
                <li class="sidebar-search-wrapper">
                    <!-- BEGIN RESPONSIVE QUICK SEARCH FORM -->
                    <!-- DOC: Apply "sidebar-search-bordered" class the below search form to have bordered search box -->
                    <!-- DOC: Apply "sidebar-search-bordered sidebar-search-solid" class the below search form to have bordered & solid search box -->
                    <form class="sidebar-search " action="extra_search.html" method="POST">
                        <a href="javascript:;" class="remove">
                        <i class="icon-close"></i>
                        </a>
                        <div class="input-group">
                            <input type="text" class="form-control" placeholder="Search...">
                            <span class="input-group-btn">
                            <a href="javascript:;" class="btn submit"><i class="icon-magnifier"></i></a>
                            </span>
                        </div>
                    </form>
                    <!-- END RESPONSIVE QUICK SEARCH FORM -->
                </li>         
                    
                 < script   id =" meuns - template "  type =" text / x - handlebars - template ">
                 {{#each this}}
                <li>
                    <a href="javascript:;">
                    <i class="icon-diamond"></i>
                    <span class="title" >{{menu.name}}</ span>
                    <span class="arrow "></span>
                    </a>
                    <ul class="sub-menu">
                    {{#each subMenuList}}
                        <li>
                            <a href=" {{url}} ">
                             {{name}}</ a>
                        </li>
                    {{/each}}
                    </ul>
                </li>
                 {{/each}}
                </ script >
                
            </ul>
            <!-- END SIDEBAR MENU -->
        </div>
    </div>
    <!-- END SIDEBAR -->  
```
### 5.web.xml


```
1.在web.xml添加filter,及mapping
<!-- 处理单点登录的js文件 -->
<filter>
    <filter-name>CasScriptFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>
<filter-mapping>
    <filter-name>CasScriptFilter</filter-name>
    <url-pattern>*.htm</url-pattern>
    <url-pattern>*.sso</url-pattern>
     <url-pattern>*.do</url-pattern>
</filter-mapping>
<!-- Filter to handle logout requests sent directly by the CAS server -->
<filter>
    <filter-name>CASSingleSignOutFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
        <param-name>targetFilterLifecycle</param-name>
        <param-value>true</param-value>
    </init-param>
    <init-param>
        <param-name>casServerUrlPrefix</param-name>
        <param-value>${cas.server.url}/${cas-project}</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CASSingleSignOutFilter</filter-name>
    <url-pattern>*.htm</url-pattern>
    <url-pattern>*.sso</url-pattern>
     <url-pattern>*.do</url-pattern>
</filter-mapping>
<!-- Define the protected urls of your application -->
<filter>
    <filter-name>CASAuthenticationFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
        <param-name>targetFilterLifecycle</param-name>
        <param-value>true</param-value>
    </init-param>
    <init-param>
        <param-name>casServerLoginUrl</param-name>
        <param-value>${cas.server.url}/${cas-project}/login</param-value>
    </init-param>
    <init-param>
        <param-name>serverName</param-name>
        <param-value>${client.server.url}</param-value>
    </init-param>
    <init-param>
        <param-name>ignorePattern</param-name>
        <param-value>proxyPage|isLogin|logout</param-value>
    </init-param>
</filter>
<!--过滤器的url-pattern可自定义-->
<filter-mapping>
    <filter-name>CASAuthenticationFilter</filter-name>
    <url-pattern>*.htm</url-pattern>
    <url-pattern>*.sso</url-pattern>
     <url-pattern>*.do</url-pattern>
</filter-mapping>
<!-- Define the urls on which you can validate a service ticket -->
<!-- #### change with your own CAS server and your host name #### -->
<filter>
    <filter-name>CasValidationFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
        <param-name>targetFilterLifecycle</param-name>
        <param-value>true</param-value>
    </init-param>
    <init-param>
        <param-name>casServerUrlPrefix</param-name>
        <param-value>${cas.server.url}/${cas-project}</param-value>
    </init-param>
    <init-param>
        <param-name>serverName</param-name>
        <param-value>${client.server.url}</param-value>
    </init-param>
    <init-param>
        <param-name>redirectAfterValidation</param-name>
        <param-value>false</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CasValidationFilter</filter-name>
    <url-pattern>*.htm</url-pattern>
    <url-pattern>*.sso</url-pattern>
     <url-pattern>*.do</url-pattern>
</filter-mapping>
<!--从cookie或request获取用户信息-->
<filter>
    <filter-name>UserThreadLocalFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
        <param-name>targetFilterLifecycle</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>UserThreadLocalFilter</filter-name>
    <url-pattern>*.htm</url-pattern>
    <url-pattern>*.sso</url-pattern>
     <url-pattern>*.do</url-pattern>
</filter-mapping>
<!-- 重新包装request，使其支持remoteUser等方法-->
<filter>
    <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>
    <filter-class>org.jasig.cas.client.util.HttpServleUsertRequestWrapperFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>
    <url-pattern>*.htm</url-pattern>
    <url-pattern>*.sso</url-pattern>
     <url-pattern>*.do</url-pattern>
</filter-mapping>
    <!-- 登陆过滤器 -->
    <filter>
        <filter-name>LoginFilter</filter-name>
        <filter-class>com.bn.b2b.omsTool.filter.LoginFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>LoginFilter</filter-name>
        <url-pattern>*.htm</url-pattern>
        <url-pattern>*.do</url-pattern>
    </filter-mapping>
    
    <!--预登陆过滤器  开始  UAA preAuthentication module start -->
    <filter>
        <filter-name>preAuthenticationFilter</filter-name>
        <filter-class>com.bn.b2b.omsTool.filter.PreAuthenticationFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>preAuthenticationFilter</filter-name>
        <url-pattern>*.htm</url-pattern>
        <url-pattern>*.do</url-pattern>
    </filter-mapping>
    <!--预登陆过滤器  结束  UAA preAuthentication module end-->
    <!--url 过滤   开始  URL security module start -->
    <filter>
        <filter-name>urlSecurityFilter</filter-name>
        <filter-class>com.bn.framework.uaa.common.access.web.UaaSecurityFilter</filter-class>
        <init-param>
            <param-name>system</param-name>
            <param-value>CRM</param-value>
        </init-param>
        <init-param>
            <param-name>errorPage</param-name>
            <param-value>/authority.html</param-value>
        </init-param>
    </filter>
   <filter-mapping>
       <filter-name>urlSecurityFilter</filter-name>
        <url-pattern>*.htm</url-pattern>
        <url-pattern>*.do</url-pattern>
   </filter-mapping>     <!--url 过滤   结束  URL security module end -->  
2.添加 servlet-mapping
<servlet-mapping>
        <servlet-name>action</servlet-name>
        <url-pattern>*.htm</url-pattern>
        <url-pattern>*.do</url-pattern>
        <url-pattern>*.sso</url-pattern>
    </servlet-mapping>
```
6.数据库配置

