##限时促销
###业务需求
```
10营销中心业务需求
10.1限时促销
10.1.1业务简介
商家人员，登录零售全渠道O2O平台商家中心后，可以对自己售卖中的商品设置各种营销活动，目前我们系统只支持限时促销。
商家对其售卖中的商品做限时促销管理，包括浏览限时促销活动，新建、修改、终止限时促销管理。
限时促销活动是针对单品做一口价、打折的一种促销，有2点业务规则：
1） 一个单品不允许同时参加2个有效的限时促销；
2） 支持对同一个买家限购数量控制，不可购买超过限购的数量。




10.1.4系统用例


用例编码 Uc_yxxscx_01
用例名称 浏览限时促销
1. 拥有限时促销管理权限的商家员工登录系统后，点击限时促销
2. 系统显示促销主页面
活动名称默认空；
活动状态默认空（即所有状态）；下拉选择框，
可选项为未开始|已开始|已结束|空
活动时间范围默认上月1号到当前日期；
列表显示满足活动时间范围的限时促销，已开始排在前面，未开始的排在已开始的后面。已结束按照促销时间排序显示在后面。
列表特殊字段说明： 
    活动状态 = 未开始   ，活动时间在当前日期之后且是否终止为否；
= 已开始   ，活动时间包含当前日期且是否终止为否；
= 已结束   ，活动时间在当前日期之前且是否终止为否
操作，活动状态决定操作内容
    修改  终止 ，  活动状态 = 未开始|已开始 且 是否终止为否；
    查看 ，  活动状态 = 已结束。
3. 商家员工维护查询条件，点击查询
4. 系统检索出满足条件的限时促销活动显示在列表里，特殊条件说明如下
    活动状态 = 空时，说明活动状态不作为查询条件；
    活动时间范围，在这个时间范围内有效的促销都查出来，举例：
    查询条件：20150701 至 20150710
    限时活动1：活动开始结束时间为 20150601 至 20150630
    限时活动2：活动开始结束时间为 20150601 至 20150701
    限时活动3：活动开始结束时间为 20150702 至 20150703
    限时活动4：活动开始结束时间为 20150601 至 20150801
    限时活动5：活动开始结束时间为 20150709 至 20150801
    限时活动6：活动开始结束时间为 20150720 至 20150801
    则，查询出来的活动应该包含2,3,4,5
不包含 1,6
7．商家员工在促销列表里点击复制链接，即可复制该活动的链接


用例编码 Uc_yxxscx_02
用例名称 新建限时促销
1. 商家员工点击限时促销主页面的‘创建活动’按钮
2. 系统打开限时促销设置页面
活动名称默认空；
活动时间范围日期默认空，时间默认00:00；
日期可调用日期控件维护，时间下拉选择框，选项：00:00 到 23:00
优惠方式：
限时折扣默认勾上，默认一口价；
3. 用户可点击活动图片进行上传，可以本地上传也可以在图片空间选择一张。
3. 商家员工维护优惠商品，点击‘请选择’
    系统判断必须选择一种限时折扣方式，若已有选择商品，则限时折扣方式不可更改，根据限时折扣方式的不同，系统打开不同的选择商品页面，选择维护好促销商品后，返回到促销设置页面
4. 商家员工可以删除选中的多余的促销商品
5. 员工点击‘保存’
系统判断活动结束时间必须大于活动开始时间，还有必填项的判断，
    判断通过后保存用户维护的限时促销活动。返回到限时促销主页面。


用例编码 Uc_yxxscx_03
用例名称 选择促销商品
1. 员工在限时促销设置页面，想要添加优惠商品时（点击‘选择’或点击‘继续添加’）
2.  系统根据折扣方式的不同打开相应的选择商品页面
商品列表中默认显示此商家的前200个已上架的，未参加状态为（已开始 或 未开始）且是否终止为否的限时促销活动的商品（运营平台是自营商品），按照商品上架时间倒序排序，
   如果限时促销活动的折扣方式是打折，则列表字段包含折扣，折后价，无一口价；
   如果限时促销活动的折扣方式是一口价，则列表字段包含一口价，无折扣和折后价；
3. 员工维护查询条件点击搜索按钮，系统在数据库中查询满足搜索条件（支持模糊查询）且满足当前促销限制（参照第2点）的所有商品。
4. 员工可勾选列表中的复选框，也可以点全选，选中所有商品 （库存>1的才可勾选）
5. 对于选中的商品，则一口价，折扣，每人限购可以维护
6. 员工维护列表中的一口价，折扣，每人限购，可以批量维护，勾选上全部相同，则当前组商品设置一样    
7. 员工点‘确定’，系统把维护的商品信息带入到限时促销设置页面；
    若选择返回，则此次维护的商品作废，回到显示促销设置页面。




用例编码 Uc_yxxscx_04
用例名称 修改限时促销
1. 商家员工点击限时促销主页面的活动列表中的‘修改’
2. 系统显示限时促销设置页面，打开当前行的限时促销信息
    活动开始时间不可修改；
    其他活动内容类似新建限时促销活动，可以修改
3. 商家员工点击‘保存’
4. 系统保存用户维护的限时促销活动             
    需要判断，活动结束时间必须大于活动开始时间，还有必填项判断。


用例编码 Uc_yxxscx_05
用例名称 查看限时促销详情
1. 商家员工点击限时促销主页面的活动列表中的‘查看’
2. 系统显示限时促销设置页面，打开当前行的限时促销信息
   所有信息只可查看，不可修改。
   隐藏保存按钮
3. 商家员工点击‘返回’
4. 系统返回到限时促销主页面。


用例编码 Uc_yxxscx_06
用例名称 终止限时促销
1. 商家员工点击限时促销主页面的活动列表中的‘终止’
2. 系统提示用户‘您确认终止当前限时促销活动吗？’
   待商家员工确认后，系统终止当前行的限时促销活动。
   系统刷新促销活动列表。
```
##表的关系
```
```

##优惠券
###业务
```
```
###表的关系
```
```