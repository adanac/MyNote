促销管理页面：
1.选择规则类型：满赠、满换购、组合购
2.输入商品编号，点击【添加】时根据【标识位，是否是组合购商品】判断是否是组合码商品，如果是，提示【组合购商品不可作为赠品出售！请更换！】


3. 商品范围设置添加，一行HTML页面。
温馨提示：请注意组合码商品是否需要参与促销！
advanceddetail.jsp


```
issetCombParentSkuInfoList    商品所属组合码商品信息列表 标志位

```
```


//获取是否组合购商品信息
                     var  skuinfo = data.skuBody;
                     if (skuinfo!= null ){
                         var  isComb = skuinfo.combParentSkuInfoList_u;
                         //是否是组合购商品
                         if (isComb != 0){
                             var  tip =  "*组合购商品" ;
                             if (ruleType == 2){
                                tip +=  "不可作为赠品出售" ;
                            } else   if (ruleType == 3){
                                tip +=  "不可参与换购" ;
                            } else {
                                tip +=  "不可参与组合购" ;
                            }
                                tip +=  "！请更换！"
                            $( "#validateInfoSpan" ).text(tip);
                            $( "#validateInfo" ).css( "display" , "block" );
                             return   false ;
                        }                     }   

```


赠品管理

1.类型买A赠B时：
主商品不用判是否为组合购商品
【赠品查询】时， 要判断是否为组合购商品。如果 输入的值是【组合码sku】时，提示 “ *组合码商品不可作为赠品出售！请更换！ ” 。并且不允许 提交。
2.类型买X件赠Y件：
【主商品】要判断是否为组合购商品。
views/pmSingleDetail.jsp














多价管理
1.商品信息中，输入商品id，查询商品信息，根据【标识位】判断是否为组合码商品，如果是，在原来5列的基础上增加一列【组合码明细，（商品id,商品数,分摊比例）】。
2.多价规则中，【提交】操作时，对定价金额作校验，【 （定价金额 *每行SKU分摊比例）/包含商品数不允许除不尽（小数点后两位） 】，如果除不尽，则不能提交,alert提示。
3.增加时间默认设置

促销时间段没有选择的时候 ，默认为当前月。






##展示明细：salesdetail.jsp

获取组合码商品明细信息com.haiziwang.jplugin.study.dbo.SkuInfoPo.getCombChildSkuInfoList(),获取不了。


需要设置一下协议： filter里边property给第7位设置一下
 





```
每个list的实例

combParentSkuId :  9990047
combParentSkuId_u :  1
combSkuId :  43
combSkuId_u :  1
combSkuNum :  5
combSkuNum_u :  1
shareRatio :  3333
shareRatio_u :  1
size :  83
type :  0
type_u :  0
version :  0
version_u :  0
```


```
function  getCombList(combList){
         var  combInfo =  "" ;
         for ( var  i=0; i<combList.length; i++){
            combInfo += combList[i].combSkuId;
            combInfo +=  " x" ;
            combInfo += combList[i].combSkuNum;
            combInfo +=  " " ;
            combInfo += combList[i].shareRatio;
            combInfo +=  "%<br/>" ;
        } 
        console.log( "combInfo-->" +combInfo);
         return  combInfo;     }   

```


##定价金额作校验

发现的问题，要获取的属性在两个实体里获取不到。

ordinaryInfo.skuPurchPrice  。直接获取却是可以的。


定价 金额做强制校验： （定价金额 *每行SKU分摊比例）/包含商品数不允许除不尽（小数点后两位）。 否则 不允许提交。




疑惑：skuid 9990047【原因是分摊比例没有除100】

页面通过了整除验证，均可以整除，后台返回码为10245表示不可以整除。












 





赠品导入


需 校验赠品是否为【组合码】，若是则导入失败，提示【 “ 组合码商品不可作为赠品出售！】 ”

pmSingleImport.jsp










多价导入
也需要进行校验，有可能也是判断是否是组合码商品。
salesImport.jsp





















提交时注意
1.交页面屏蔽的getCookie方法打开。


2.






3.salesimport.jsp


4.pmSingleImport.jsp


3与4页面不用提交。




## 多价 提供修改功能。仅支持修改特供价




所有 商品都展示    修改    摁扭 。已维护 特供价 的支持修改，未维护特供价的商品支持补充。



 

点击 修改展示弹窗，已维护特供价则展示之前价格，未维护则为空。

修改 条件：

待审核(status=1) ， 生效中 (status=2) ，已作废 (status=4) 状态均支持修改。

驳回 状态 (status=3) 不支持修改。

```

multiPriceQueryList.do

sales.jsp


```


状态

```


function  getMultiPriceStatus(nS) {
     switch (nS)
    {
         case  1:
               return   '待审核' ;
           break ;
         case  2:
             return   '审核通过' ;
           break ;
         case  3:
               return   '驳回' ;
           break ;
         case  4:
             return   '已作废' ;
           break ;
         default :
               return   '未知类型' ;
    } }   




```




可看出，status不为2，4；即为更新，因此，页面updateMultprice方法中，status参数我给到的是0.










疑问1：去除登录方法 getLoginUserInfo后，作废选项看不到了，发现

审核和作废的权限的展示是通过css样式 style="display:none"来隐藏的， 在查询列表的方法中,







而是否有审核、作废权限是通过登录方法 getLoginUserInfo去设置的。










##特供价sso配置相应的权限

http://test.sso.haiziwang.com/sso-web/common/main.jsp

添加资源 、创建角色










##配置数据库



由于此次更新中在Plugin.java中注册了新的数据源salecenter_database ;登录sso-web配置应用中心【 http://test.site.haiziwang.com/main.html 】，配置数据库：salecenter_database 
 








sales.jsp

```


< div   class = "col-md-offset-5 col-md-9" >
                                 < button   type = "button"   class = "btn green"   id = "query"   data-resource = "#im_sales_conf,#im_sales_create,#im_sales_delete,#im_sales_specost_modify" >< i   class = "fa fa-check" ></ i >  查询 </ button >
                                 < input   type = "hidden"   id = "im_sales_conf"   />
                                 < input   type = "hidden"   id = "im_sales_create"   />
                                 < input   type = "hidden"   id = "im_sales_delete"   />
                                 < input   type = "hidden"   id = "im_sales_specost_modify"   />
                                 < a   id = "salesCreate"   style =" display : none ;"  href = " ${ctx} /views/salesdetail.jsp" >< button   type = "button"   class = "btn default" > 创建多价信息 </ button ></ a >
                                 < a   id = "salesDetail"   style =" display : none ;"  href = "#"   data-target = "#myModal_autocomplete"   role = "button"   class = "btn red"   data-toggle = "modal" > 多价信息 </ a >                              </ div >   




```

查询多价列表信息时，添加判断：




特供价不高于当前商品的定价金额。







问题一：在 multiPriceUpdate中添加了参数，不知对其他方法是否有影响










## 组合购商品池上限开到500

促销管理-》创建促销规则-》促销规则类型选择组合购时，将200修改为500.







目前C++数据库字段还没有修改。修改过后，我们java后台只需要修改那个提示即可。




##多价信息Bug




点击查询多价信息，查询 出来的会员价的定价不对。

调用 接口 multiPriceQuery（），


final  GetMultiPriceRuleResp  resp  =  new  GetMultiPriceRuleResp();
List<PriceRuleDdo>  prplist  =  resp . getVecPriceRuleInfoOut ();

retList  = initPriceRuleDdoList( prplist );



PriceRuleDdo 中C++没有返回对应商品的vipPriceInfo信息。





而查询列表信息，调用 接口 multiPriceQueryList（），

final  GetPriceRefListResp  resp  =  new  GetPriceRefListResp();   


List<PricePubRefPo>  prplist  =  resp .getPriceRefList();  


// 初始化多价列表,添加商品名称 prpMapList  = initPrpList( prplist );   




  PricePubRefPo 返回了对应商品的vipPriceInfo信息。





##组合码商品，页面校验

```


//组合码商品
         var  ttl = $( "#sample_6 thead th" ).length;
         if (ttl == 3){
             var  resId =  new  Array();
             var  flag =  true ;
             var  combListJson = $( "#combListJson" ).val();
             var  combList = eval(combListJson);
             for ( var  j=0; j<combList.length; j++){
                 var  comb = combList[j];
                 var  ratio = formatFloat(formatFloat(comb.shareRatio, 1) / 10000, -1);
                 var  ratio2 = comb.shareRatio;
                 var  ratio3 = accDiv(comb.shareRatio,10000);
                cosole.log(ratio3);
                 var  temp = formatFloat(formatFloat(mutiPrice, 1) * ratio, -1); 
                 var  check = formatFloat(formatFloat(temp, 1) / comb.combSkuNum, -1);
                
                console.log( 'check:' +check);
                 if (check.toString().indexOf( "." )!=-1){
                     var  len = check.toString().split( "." )[1].length;
                      if (len > 2){
                         flag =  false ;
                         resId.push(comb.combSkuId);
                     }
                     //console.log(comb.combSkuId+","+comb.combSkuName);
                }
            }
             if (!flag){
                alert( "组合码商品Id为：" +resId+ "的商品，定价金额不符合要求！请重输入！\r\n定价金额*每行SKU分摊比例/包含商品数不允许除不尽（小数点后两位）" );
                 return ;
            }         }  




会出现的问题：33.33% / 10000 = 0.333300000000000007这样的问题，于是将校验放到了java代码中：
// 是否为组合码商品，如果是组合码商品，对其作定价金额校验
             if  ( skuInfo .getCombChildSkuInfoList() !=  null  &&  skuInfo .getCombChildSkuInfoList().size() > 0) { // 组合码商品
                Vector<SkuCombInfoPo>  combChildSkuInfoList  =  skuInfo .getCombChildSkuInfoList();
                 for  ( int   i  = 0;  i  <  combChildSkuInfoList .size();  i ++) {
                    SkuCombInfoPo  combInfoPo  =  combChildSkuInfoList .get( i );
                    BigDecimal  sp  =  new  BigDecimal( mutiPrice ); // 定价金额
                    BigDecimal  sr  =  new  BigDecimal(Long. toString ( combInfoPo .getShareRatio()));
                    BigDecimal  count  =  new  BigDecimal(Long. toString ( combInfoPo .getCombSkuNum()));
                    BigDecimal  spr  =  sp .multiply( sr ).multiply( count );
                    BigDecimal  temp  =  spr .divide( new  BigDecimal(1000000));
                    String  check  =  temp .toString();
                     if  ( check .indexOf( "." ) >= 0) {
                         int   dot  =  check .indexOf( "." );
                        String  dotNum  =  check .substring( dot  + 1,  check .length());
                         if  ( dotNum .length() > 2) {
                             flag  =  false ;
                             sb .append( combInfoPo .getCombSkuId() +  "," );
                        }
                    }
                }
                 if  (! flag ) {
                     sb .toString().substring(0,  sb .length() - 1);
                     sb .insert(0,  "添加失败，组合码商品ID为:" ).append( "的商品，定价金额不符合要求！请重输入！" );
                }

            }  


```



