
```
advanced.jsp
< div   class = "form-actions" >
                         < div   class = "row" >
                             < div   class = "col-md-offset-5 col-md-9" >
                                 < button   type = "button"   class = "btn green"   id = "query" >< i   class = "fa fa-check" ></ i >  查询 </ button >
                                
                                 < a   href = " ${ctx} /views/advanceddetail.jsp" >< button   type = "button"   class = "btn default" > 创建促销规则 </ button ></ a >
                             </ div >
                         </ div >                      </ div >   


Request URL:
http://localhost:8083/jplugin-study/cust/promotionQuery.do?code=&actname=&ruletype=0&status=0&from=&creatername=&site=1&pageno=1&pagesize=10&sellerId=&_=1476338978984

Request Method:
GET
$( "#query" ).click( function (){
        getPromotionList(1,10);
    });
    
    
     function  getPromotionList(pageno,pagesize)
    {
        $( '#sample_6 tbody' ).html( '' );
        
         var  arr=$( "input[name='timetest']" ).val().split( '至' );
         var  from=arr[0];
         var  to=arr[1];
         var  code=$( "input[name='code']" ).val();  
         var  actname=$( "input[name='actname']" ).val();
         var  ruletype=$( "#ruletype  option:selected" ).val();
         var  status=$( "#status  option:selected" ).val();
        
         var  creatername=$( "input[name='creatername']" ).val();
         var  site=$( "#site  option:selected" ).val();
         var  sellerId = $( "#sellId" ).val();
         var  sellType = $( "#sellType" ).val();
         if (sellType == 1){
             if (sellerId ==  "请填商家ID"  || sellerId ==  "" ){
                alert( "请填写商家ID" );
                 return   false ;
            }
        }
        $.ajax({
            url: '${ctx}/cust/promotionQuery.do' ,
            data:{
                 "code" :code,
                 "actname" :encodeURI(actname),
                 "ruletype" :ruletype,
                 "status" :status,
                 "from" :from,
                 "to" :to,
                 "creatername" :encodeURI(creatername),
                 "site" :site,
                 "pageno" :pageno,
                 "pagesize" :pagesize,
                 "sellerId" :sellerId
                },
            dataType: 'json' ,
            cache: false ,
            type: 'get' ,
            success: function (data){
                 var  dlgHtml =  "" ;
                 if (data.resultCode ==  "1" )
                {
                     var  prplist = data.databody;
                    
                     var  sellModel =  "" ;
                     var  siteStr =  "" ;
                     for ( var  i=0;i<prplist.length;i++)
                    {
                         if (prplist[i][ "sellerId" ] == 0){
                            sellModel =  "自营" ;
                            siteStr = getSiteName(prplist[i][ "siteId" ]);
                        } else {
                            sellModel =  "联营" ;
                            siteStr =  "--" ;
                        }
                         if (prplist[i][ "state" ]==2)
                        {
                            dlgHtml = dlgHtml +  "<tr><td><a href='#' data-target='#myModal_autocomplete' onclick='showPromotion(" +prplist[i][ "ruleId" ]+ ")' role='button' data-toggle='modal'>" +prplist[i][ "ruleId" ]+ "</a></td><td>" +siteStr+ "</td><td>" +sellModel+ "</td><td>" + getLocalTime(prplist[i][ "createTime" ])+ "</td><td>" +prplist[i][ "ruleName" ]+ "</td><td>" +getPromotionType(prplist[i][ "ruleType" ])+ "</td><td>" +getLocalTime(prplist[i][ "startTime" ])+ "</td><td>" +getLocalTime(prplist[i][ "endTime" ])+ "</td><td>" +getPromotionStatus(prplist[i][ "state" ])+ "</td><td>" +prplist[i][ "creater" ]+ "</td><td>" +prplist[i][ "approval" ]+ "</td><td><button type='button' class='btn default' onclick='SetPromotionStatus(" +prplist[i][ "ruleId" ]+ ",4,0)' >作废</button></td></tr>" ;
                        }
                         else   if (prplist[i][ "state" ]==4 || prplist[i][ "state" ]==3)
                        {
                            dlgHtml = dlgHtml +  "<tr><td><a href='#' data-target='#myModal_autocomplete' onclick='showPromotion(" +prplist[i][ "ruleId" ]+ ")' role='button' data-toggle='modal'>" +prplist[i][ "ruleId" ]+ "</a></td><td>" +siteStr+ "</td><td>" +sellModel+ "</td><td>" + getLocalTime(prplist[i][ "createTime" ])+ "</td><td>" +prplist[i][ "ruleName" ]+ "</td><td>" +getPromotionType(prplist[i][ "ruleType" ])+ "</td><td>" +getLocalTime(prplist[i][ "startTime" ])+ "</td><td>" +getLocalTime(prplist[i][ "endTime" ])+ "</td><td>" +getPromotionStatus(prplist[i][ "state" ])+ "</td><td>" +prplist[i][ "creater" ]+ "</td><td>" +prplist[i][ "approval" ]+ "</td><td></td></tr>" ;
                        }
                         else
                        {
                            dlgHtml = dlgHtml +  "<tr><td><a href='#' data-target='#myModal_autocomplete' onclick='showPromotion(" +prplist[i][ "ruleId" ]+ ")' role='button' data-toggle='modal'>" +prplist[i][ "ruleId" ]+ "</a></td><td>" +siteStr+ "</td><td>" +sellModel+ "</td><td>" + getLocalTime(prplist[i][ "createTime" ])+ "</td><td>" +prplist[i][ "ruleName" ]+ "</td><td>" +getPromotionType(prplist[i][ "ruleType" ])+ "</td><td>" +getLocalTime(prplist[i][ "startTime" ])+ "</td><td>" +getLocalTime(prplist[i][ "endTime" ])+ "</td><td>" +getPromotionStatus(prplist[i][ "state" ])+ "</td><td>" +prplist[i][ "creater" ]+ "</td><td>" +prplist[i][ "approval" ]+ "</td><td><button type='button' class='btn default' onclick='SetPromotionStatus(" +prplist[i][ "ruleId" ]+ ",2,1)'>审核通过</button><button type='button' class='btn default' onclick='SetPromotionStatus(" +prplist[i][ "ruleId" ]+ ",2,2)'>审核拒绝</button><button type='button' class='btn default' onclick='SetPromotionStatus(" +prplist[i][ "ruleId" ]+ ",4,0)' >作废</button></td></tr>" ;
                        }
                    }
                    
                    $( '#sample_6 tbody' ).html(dlgHtml);
                    
                     if (data.totalRecord%pagesize==0)
                    {
                        setPage(data.totalRecord/pagesize,data.totalRecord,pageno);
                    }
                     else
                    {
                        setPage(parseInt(data.totalRecord/pagesize)+1,data.totalRecord,pageno);
                    }
                    
                    
                     //setPage(data.totalRecord,36,pageno);
                }
                 else
                {
                    setPage(0,0,1);
                }
            }
        });       }   





function  setPage(totalPage,totalRecords,pageNo)
    {    
         if (!pageNo){
            pageNo = 1;
        }
        kkpager.generPageHtml({
            pno : pageNo,
             //总页码
            total : totalPage,
             //总数据条数
            totalRecords : totalRecords,
            mode :  'click' ,  //设置为click模式
             //链接前部
            hrefFormer :  'pager_test' ,
             //链接尾部
            hrefLatter :  '.html' ,
            click :  function (n){
                 //处理完后可以手动条用selectPage进行页码选中切换
                 this .selectPage(n);
                
                getPromotionList(n,10);
                 //alert(n);
            }
        }, true );     }  








public   class  CustomerController  extends  BaseService {
    Logger  logger  = ServiceFactory. getService (ILogService. class )
            .getLogger( "com.haiziwang.jplugin.study.ctrl.CustomerController" );
    Logger  apiLogger  = ServiceFactory. getService (ILogService. class ).getSpecicalLogger( "apiLogger.log" );
    IPromotionService  promotionService  = RuleServiceFactory. getRuleService (IPromotionService. class );
    IBetrayService  betrayService  = RuleServiceFactory. getRuleService (IBetrayService. class );
     public   void  promotionQuery()  throws  ParseException {
        WebStubCntl  stub  = CppClientProxyFactory. createCppWebStubCntl ();
        RespJson  resjon  =  new  RespJson();
         logger .info( "查询促销开始........." );
        String  code  = getParam( "code" );
        String  actname  = NormalUtil. decodeToString (getParam( "actname" ));
        String  ruletype  = getParam( "ruletype" );
        String  status  = getParam( "status" );
        String  from  = getParam( "from" );
        String  to  = getParam( "to" );
        String  creatername  = NormalUtil. decodeToString (getParam( "creatername" ));
        String  site  = getParam( "site" );
        Integer  pageno  = Integer. parseInt (getParam( "pageno" ));
        Integer  pagesize  = Integer. parseInt (getParam( "pagesize" ));
        String  sellerId  = getParam( "sellerId" );
         // 172.172.177.29
         final  GetPromotionListReq  req  =  new  GetPromotionListReq();
         req .setMachineKey( this . MachineKey  +  "haiziwang213123" );
         req .setSource( this . Source );
         req .setSceneId( this . SceneId );
        PromotionQueryPo  pqp  =  new  PromotionQueryPo();
         pqp .setVersion(1);
         pqp .setPageSize( pagesize );
         pqp .setStartIndex( pageno );
         if  (!NormalUtil. isNullOrEmpty ( code )) {
             if  (!NormalUtil. isNumeric ( code )) {
                 resjon .setResultCode( "0" );
                 resjon .setMsg( "编号请输入数字" );
                renderJson(JSON. toJSONString ( resjon ));
            }
             pqp .setRuleId(Integer. parseInt ( code ));
        }
         if  (!NormalUtil. isNullOrEmpty ( ruletype )) {
             if  (Integer. parseInt ( ruletype ) > 0) {
                 pqp .setRuleType(Integer. parseInt ( ruletype ));
            }
        }
         if  (!NormalUtil. isNullOrEmpty ( status )) {
             if  (Integer. parseInt ( status ) > 0) {
                 pqp .setState(Integer. parseInt ( status ));
            }
        }
         if  (!NormalUtil. isNullOrEmpty ( actname )) {
             pqp .setRuleName( actname );
            ;
        }
         if  (!NormalUtil. isNullOrEmpty ( creatername )) {
             pqp .setCreater( creatername );
            ;
        }
         if  (!NormalUtil. isNullOrEmpty ( from )) {
             pqp .setStartTime(NormalUtil. GetLongByDate ( from ));
        }
         if  (!NormalUtil. isNullOrEmpty ( to )) {
             pqp .setEndTime(NormalUtil. GetLongByDate ( to ));
        }
         if  (!NormalUtil. isNullOrEmpty ( site )) {
             pqp .setSiteId(Integer. parseInt ( site ));
        }
         logger .info( "sellerId:"  +  sellerId );
         if  (!NormalUtil. isNullOrEmpty ( sellerId )) {
             pqp .setSellerId(Long. parseLong ( sellerId ));
        }
         req .setPromotionQueryPo( pqp );
         logger .info( "查询促销入参："  +  req .toString());
         final  GetPromotionListResp  resp  =  new  GetPromotionListResp();
         resp .setPromotionRuleInfoList( new  Vector<PromotionRulePo>());
         try  {
             stub .invoke( req ,  resp , 1024 * 1024);
             // logger.info("查询促销出参："+resp.toString());
            List<PromotionRulePo>  prplist  =  resp .getPromotionRuleInfoList();
             if  ( prplist  !=  null  &  prplist .size() > 0) {
                 if  (!NormalUtil. isNullOrEmpty ( code )) {
                     logger .info( "sellId:"  +  prplist .get(0).getSellerId());
                     // 限制商家
                    Map<String, Object>  sellMap  = getAvailableSellers( prplist .get(0).getSellerId());
                     resjon .setAdditionalbody( sellMap );
                    ConditionStagePo  d  =  prplist .get(0).getConditionStageInfo();
                    ProductInfoDbo  proinfo  =  new  ProductInfoDbo();
                    ConditionStagePo  scpo  =  new  ConditionStagePo();
                     scpo .setConditionStageType( d .getConditionStageType());
                     scpo .setDiscountType( d .getDiscountType());
                     proinfo .setConditionStagePo( scpo );
                    List<String>  includeCate  =  new  Vector<String>();
                    List<String>  excludeCate  =  new  Vector<String>();
                    List<String>  includeBrand  =  new  Vector<String>();
                    List<String>  excludeBrand  =  new  Vector<String>();
                    List<String>  includeSku  =  new  Vector<String>();
                    List<String>  excludeSku  =  new  Vector<String>();
                     // productPathQuery类目
                     // 限制类型的品类
                    Map<String, Object>  params  =  new  HashMap<String, Object>();
                     params .put( "categoryType" , 0);
                     params .put( "salePriceId" ,  code );
                    List<Map<String, Object>>  includeCategoryList  =  promotionService .selectShowCategory( params );
                     // 获得优惠券的限制类型的详细品类
                    List<Map<String, Object>>  includeRangeList  = getCouponRangeList( includeCategoryList );
                     proinfo .setIncludeRangeList( includeRangeList );
                     // 限制类型的select下拉框
                    List<Map<String, Object>>  usedCategoryList  = initCategortList( includeCategoryList );
                    String  usedCategorySelect  = initCategorySelect( usedCategoryList );
                     proinfo .setAvailableCategory( usedCategorySelect );
                     // 排除类型的品类
                     params .put( "categoryType" , 1);
                    List<Map<String, Object>>  excludeCategoryList  =  promotionService .selectShowCategory( params );
                    List<Map<String, Object>>  unusedCategoryList  = initCategortList( excludeCategoryList );
                    String  unusedCategorySelect  = initCategorySelect( unusedCategoryList );
                     proinfo .setUnavailableCategory( unusedCategorySelect );
                     // 获得优惠券的排除品类
                    List<Map<String, Object>>  exRangeList  = getCouponRangeList( excludeCategoryList );
                     proinfo .setExcludeRangeList( exRangeList );
                     // promotionAttrQuery品牌attrid=55
                    List<OptionDdo>  allbrandlist  = getBrandList();
                     if  ( prplist .get(0).getBrandIncludeList().size() > 0) {
                         for  (uint32_t  brand  :  prplist .get(0).getBrandIncludeList()) {
                             for  (OptionDdo  onebrand  :  allbrandlist ) {
                                 if  ( brand .longValue() ==  onebrand .getOptionId()) {
                                     includeBrand .add(
                                             "("  + Long. toString ( onebrand .getOptionId()) +  ")"  +  onebrand .getName());
                                }
                            }
                        }
                    }
                     if  ( prplist .get(0).getBrandExcludeList().size() > 0) {
                         for  (uint32_t  brand  :  prplist .get(0).getBrandExcludeList()) {
                             for  (OptionDdo  onebrand  :  allbrandlist ) {
                                 if  ( brand .longValue() ==  onebrand .getOptionId()) {
                                     excludeBrand .add(
                                             "("  + Long. toString ( onebrand .getOptionId()) +  ")"  +  onebrand .getName());
                                }
                            }
                        }
                    }
                     // productQueryBySkuId商品
                     if  ( prplist .get(0).getProductIncludeList().size() > 0) {
                         for  (uint64_t  product  :  prplist .get(0).getProductIncludeList()) {
                             includeSku .add( "("  + Long. toString ( product .longValue()) +  ")"
                                    + GetSkuById( product .longValue()).getOrdinaryInfo().getSkuTitle());
                        }
                    }
                     if  ( prplist .get(0).getProductExcludeList().size() > 0) {
                         for  (uint64_t  product  :  prplist .get(0).getProductExcludeList()) {
                             excludeSku .add( "("  + Long. toString ( product .longValue()) +  ")"
                                    + GetSkuById( product .longValue()).getOrdinaryInfo().getSkuTitle());
                        }
                    }
                     proinfo .setIncludeCategoryNameList( includeCate );
                     proinfo .setExcludeCategoryNameList( excludeCate );
                     proinfo .setIncludeBrandNameList( includeBrand );
                     proinfo .setExcludeBrandNameList( excludeBrand );
                     proinfo .setIncludeSkuNameList( includeSku );
                     proinfo .setExcludeSkuNameList( excludeSku );
                     resjon .setResverbody( proinfo );
                     resjon .setDataPicBody(NormalUtil. GetProConObject ( d .getConditionInfoList()));
                     logger .info( "getDiscountType:"  +  d .getDiscountType());
                     // 满返券信息组织
                     if  ( d .getDiscountType() == 4) {
                        Map<uint32_t, ConditionPo>  conditionPoList  =  d .getConditionInfoList();
                        List<Map<String, Object>>  list  =  new  ArrayList<Map<String, Object>>();
                        Map<String, Object>  map  =  null ;
                        Map<String, Object>  couponMap  =  null ;
                         for  (Map.Entry<uint32_t, ConditionPo>  entry  :  conditionPoList .entrySet()) {
                             map  =  new  HashMap<String, Object>();
                             map .put( "couponWorth" ,  entry .getKey().toString());
                            List<Map<String, Object>>  couponMapList  =  new  ArrayList<Map<String, Object>>();
                             for  (Map.Entry<String, ReturnCouponInfoPo>  coupon  :  entry .getValue().getCouponBatchList()
                                    .entrySet()) {
                                 couponMap  =  new  HashMap<String, Object>();
                                 couponMap .put( "couponBatchId" ,  coupon .getKey());
                                 couponMap .put( "couponAmt" ,  coupon .getValue().getCouponAmt());
                                 couponMap .put( "couponNum" ,  coupon .getValue().getCouponNum());
                                 couponMapList .add( couponMap );
                            }
                             map .put( "couponObj" ,  couponMapList );
                             list .add( map );
                        }
                         logger .info( "list:"  + JSON. toJSONString ( list ));
                        NormalUtil. sort ( list ,  "couponWorth" , 0);
                         resjon .setOtherbody( list );
                    }  else   if  ( d .getDiscountType() == 6) {
                         // 组合购为定价
                        Map<uint32_t, ConditionPo>  conditionPoList  =  d .getConditionInfoList();
                        List<Map<String, Object>>  list  =  new  ArrayList<Map<String, Object>>();
                        Map<String, Object>  map  =  null ;
                         for  (Map.Entry<uint32_t, ConditionPo>  entry  :  conditionPoList .entrySet()) {
                             map  =  new  HashMap<String, Object>();
                             map .put( "groupCouponWorth" ,  entry .getValue().getPromotionPrice());
                             map .put( "groupCouponNum" ,  entry .getKey().toString());
                             list .add( map );
                        }
                         logger .info( "list:"  + JSON. toJSONString ( list ));
                        NormalUtil. sort ( list ,  "groupCouponNum" , 0);
                         resjon .setOtherbody( list );
                    }
                }
                 // Json格式错误conditionStageInfo
                 for  (PromotionRulePo  pr  :  prplist ) {
                     pr .setConditionStageInfo( null );
                }
                 resjon .setResultCode( "1" );
                 resjon .setDatabody( prplist );
                 resjon .setMsg( "" );
                 resjon .setTotalRecord( resp .getTotalNum());
                renderJson(JSON. toJSONString ( resjon ));
            }  else  {
                 resjon .setResultCode( "0" );
                 resjon .setMsg( "no data" );
                renderJson(JSON. toJSONString ( resjon ));
            }
        }  catch  ( final  Exception  e1 ) {
             logger .error( "查询促销出错："  +  e1 .toString());
             resjon .setResultCode( "0" );
             resjon .setMsg( "Query Error" );
            renderJson(JSON. toJSONString ( resjon ));
             e1 .printStackTrace();
        }
    }
} 

```





