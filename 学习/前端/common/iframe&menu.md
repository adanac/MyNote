##iframe.ftl
```
<!DOCTYPE html>
<html lang="en" style="height: 100%;">
<head>
<meta charset="UTF-8">
    <title>创纪云|零售业全渠道O2O平台</title>
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta content="width=device-width, initial-scale=1" name="viewport" />
    <meta content="" name="description" />
    <meta content="" name="author" />
    <!-- 图标库,bootstrap -->
    <link href="../../../assets/plugins/font-awesome/css/font-awesome.min.css" rel="stylesheet" type="text/css" />
    <link href="../../../assets/plugins/simple-line-icons/simple-line-icons.min.css" rel="stylesheet" type="text/css" />
    <link href="../../../assets/plugins/bootstrap/css/bootstrap.min.css" rel="stylesheet" type="text/css" />
    <!-- 下拉菜单，可输入 -->
    <link rel="stylesheet" type="text/css" href="../../../assets/plugins/bootstrap-select/bootstrap-select.min.css" />
    <link rel="stylesheet" type="text/css" href="../../../assets/plugins/select2/select2.css" />
    <link rel="stylesheet" type="text/css" href="../../../assets/plugins/jquery-multi-select/css/multi-select.css" />
    <!-- 全局样式 -->
    <link href="../../../assets/css/components.css" id="style_components" rel="stylesheet" type="text/css" />
    <link href="../../../assets/css/plugins.css" rel="stylesheet" type="text/css" />
    <link href="../../../assets/plugins/layout/css/layout.css" rel="stylesheet" type="text/css" />
    <link href="../../../assets/plugins/layout/css/themes/darkblue.css" rel="stylesheet" type="text/css" id="style_color" />






    <!-- 独立样式 -->
    <link href="css/style.css" rel="stylesheet" type="text/css" />
    <link href="css/custom.css" rel="stylesheet" type="text/css" />
    <link href="css/xys.css" rel="stylesheet" type="text/css" />
    <link rel="stylesheet" href="../../../assets/plugins/bootstrap-table/css/bootstrap-table.min.css">
</head>
<body style="height: 100%;    overflow-x: hidden;">


<div class="page-content" style="padding:10px;">
                <!-- 主题颜色 -->
                <div id="theme_paneldiv"></div>
                <!-- BEGIN PAGE HEADER-->
                <h3 class="page-title">
                商品管理 <small>商家管理商品</small>
                </h3>
                <div class="page-bar">
                    <ul class="page-breadcrumb">
                        <li>
                            <i class="fa fa-home"></i>
                            <a href="product-list.html">商品管理中心</a>
                            <i class="fa fa-angle-right"></i>
                        </li>
                        <li>
                            <a href="product-list.html">商品管理</a>
                        </li>
                    </ul>
                </div>
                <!-- END PAGE HEADER-->
                <div class="clearfix">
                </div>
                <!-- ##########主要内容开始########## -->
                <div class="page-top">
                    <div class="row">
                        <div class="col-md-12">
                            <div class="form-group">
                                <label class="control-label text-right">商品名称</label>
                                <div class="control-btn">
                                    <input type="text" class="value-null" maxlength="20">
                                </div>
                            </div>
                            <div class="form-group">
                                <label class="control-label text-right">商品编码</label>
                                <div class="control-btn">
                                    <input type="text"  class="value-null max6" maxlength="20">
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12">
                            <div class="form-group">
                                <label class="control-label text-right">分类</label>
                                <div class="control-btn">
                                    <select class="form-control select2me" required data-placeholder=" ">
                                        <option value="">匪类</option>
                                    </select>
                                </div>
                            </div>
                            <div class="">
                                <label class="control-label text-right" style="width:90px;"></label>
                                <div class="control-btn">
                                    <button class="btn btn-primary wn120 f_l"> <i class="fa fa-search"></i> 查询
                                    </button>
                                    <button class="btn btn-warning wn120 f_l"> <i class="fa fa-undo"></i> 清除条件
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-12">
                        <div class="tabbable-line boxless tabbable-reversed">
                            <ul class="nav nav-tabs">
                                <li class="active">
                                    <a href="#tab1" data-toggle="tab" aria-expanded="true">全部商品</a>
                                </li>
                                <li class="">
                                    <a href="#tab2" data-toggle="tab" aria-expanded="false">已发布</a>
                                </li>
                                <li class="">
                                    <a href="#tab3" data-toggle="tab" aria-expanded="false">未发布</a>
                                </li>
                            </ul>
                            <div class="tab-content">
                                <div class="tab-pane active" id="tab1">
                                    <!-- BEGIN TAB PORTLET-->
                                    <div class="portlet box blue">
                                        <div class="portlet-title">
                                            <div class="caption">商品管理</div>
                                            <div class="actions">
                                                <a href="javascript:;" class="btn btn-default btn-sm">
                                                    <i class="fa fa-upload"></i> 上架
                                                </a>
                                                <a href="javascript:;" class="btn btn-default btn-sm">
                                                    <i class="fa fa-download"></i> 下架
                                                </a>
                                            </div>
                                        </div>
                                        <div class="portlet-body">
                                            <!-- 列表demo 开始 -->
                                            <!-- 表格 -->
                                            <table id="table sample_2" role="grid" aria-describedby="sample_2_info" data-toggle="table" data-url="../../../assets/plugins/bootstrap-table/data/product.json" data-side-pagination="server" data-pagination="true" data-height="" class="table table-hover dataTable no-footer table-striped table-bordered" style="width: 100%;border:1px solid #ddd;">
                                                <thead>
                                                    <tr>
                                                        <th data-width="50" data-field="number" data-align="center" data-formatter="idFormatter">
                                                            <input type="checkbox" class="group-checkable checkbox_all" id="checkbox_all">
                                                        </th>
                                                        <th data-width="80" data-field="state" data-align="center">商品编码
                                                            
                                                        </th>
                                                        <th data-width="80" data-field="distribution" data-align="center">商品图片</th>
                                                        <th data-width="200" data-field="store" data-align="center">商品名称</th>
                                                        <th data-width="80" data-field="total" data-align="center">分类</th>
                                                        <th data-width="50" data-field="vip" data-align="center">价格</th>
                                                        <th data-width="50" data-field="iphone" data-align="center">库存</th>
                                                        <th data-width="50" data-field="payment" data-align="center">商品状态</th>
                                                        <th data-width="150" data-field="operation" data-align="center">操作</th>
                                                    </tr>
                                                </thead>
                                            </table>
                                            <!-- 表格 -->
                                            
                                            <!-- 列表demo 结束 -->
                                        </div>
                                    </div>
                                    <!-- END TAB PORTLET-->
                                </div>
                                <div class="tab-pane" id="tab2">
                                    <!-- BEGIN TAB PORTLET-->
                                    <div class="portlet box blue">
                                        <div class="portlet-title">
                                            <div class="caption">商品管理</div>
                                            <div class="actions">
                                                <a href="javascript:;" class="btn btn-default btn-sm">
                                                    <i class="fa fa-upload"></i> 上架
                                                </a>
                                                <a href="javascript:;" class="btn btn-default btn-sm">
                                                    <i class="fa fa-download"></i> 下架
                                                </a>
                                            </div>
                                        </div>
                                        <div class="portlet-body">
                                            <!-- 列表demo 开始 -->
                                            <!-- 表格 -->
                                            <table class="table table-striped table-bordered table-hover dataTable no-footer ta_c product_list_table" id="sample_2" role="grid" aria-describedby="sample_2_info">
                                                <thead>
                                                    <tr>
                                                        <th class="ta_c wb5">
                                                            <input type="checkbox" class="group-checkable checkbox_all" id="checkbox_all">
                                                        </th>
                                                        <th class="ta_c wb10">商品编码</th>
                                                        <th class="ta_c wb10">商品图片</th>
                                                        <th class="ta_c wb20">商品名称</th>
                                                        <!-- <th class="ta_c ">品牌</th> -->
                                                        <th class="ta_c wb10">分类</th>
                                                        <th class="ta_c ">价格</th>
                                                        <th class="ta_c wb5">库存</th>
                                                        <th class="ta_c wn110">商品状态</th>
                                                        <th class="ta_c wn175">操作</th>
                                                    </tr>
                                                </thead>
                                                <tbody>
                                                    <!-- loop -->
                                                    <tr>
                                                        <td>
                                                            <input type="checkbox" class="group-checkable" name="the_checkbox" value="1">
                                                        </td>
                                                        <td>
                                                            <a href="R-product-detail.html">1000000001</a>
                                                        </td>
                                                        <td>
                                                            <img src="images/pro_pic001.jpg" class="pro_pic"></td>
                                                        <td>
                                                            <a href="R-product-detail.html">婴儿抱被新生儿春秋冬款可脱胆纯棉宝宝包被抱毯加厚冬季婴童用品</a>
                                                        </td>
                                                        <td>抱被</td>
                                                        <td>¥88.00</td>
                                                        <td>99</td>
                                                        <td>已发布</td>
                                                        <td class="pro_tdlink">
                                                            <a class="btn btn-xs yellow copy" title="复制链接"><i class="fa fa-paste"></i></a>
                                                            <a href="R-product-detail.html" class="btn btn-xs green" title="查看"><i class="fa fa-search"></i></a>
                                                            <a href="R-product-edit.html" class="btn btn-xs blue" title="编辑"><i class="fa fa-pencil"></i></a>
                                                            <a href="javascript:;" class="btn btn-xs yellow" title="上架" disabled><i class="fa fa-upload"></i></a>
                                                            <a href="javascript:;" class="last btn btn-xs red" title="下架"><i class="fa fa-download"></i></a>
                                                        </td>
                                                    </tr>
                                                    <tr>
                                                        <td>
                                                            <input type="checkbox" class="group-checkable" name="the_checkbox" value="1">
                                                        </td>
                                                        <td>
                                                            <a href="R-product-detail.html">1000000002</a>
                                                        </td>
                                                        <td>
                                                            <img src="images/pro_pic002.jpg" class="pro_pic"></td>
                                                        <td>
                                                            <a href="R-product-detail.html">纯棉婴儿衣服新生儿礼盒婴儿服满月宝宝礼物母婴用品</a>
                                                        </td>
                                                        <td>抱被</td>
                                                        <td>¥55.00</td>
                                                        <td>99</td>
                                                        <td>已发布</td>
                                                        <td class="pro_tdlink">
                                                            <a class="btn btn-xs yellow copy" title="复制链接"><i class="fa fa-paste"></i></a>
                                                            <a href="R-product-detail.html" class="btn btn-xs green" title="查看"><i class="fa fa-search"></i></a>
                                                            <a href="R-product-edit.html" class="btn btn-xs blue" title="编辑"><i class="fa fa-pencil"></i></a>
                                                            <a href="javascript:;" class="btn btn-xs yellow" title="上架" disabled><i class="fa fa-upload"></i></a>
                                                            <a href="javascript:;" class="last btn btn-xs red" title="下架"><i class="fa fa-download"></i></a>
                                                        </td>
                                                    </tr>
                                                    <!-- end -->
                                                </tbody>
                                            </table>
                                            <!-- 表格 -->
                                            <div class="row">
                                                <div class="col-md-5 col-sm-12">
                                                    <select class="form-control select2me wn50" name="pro_cate" data-placeholder="分页数量">
                                                        <option value="2">5</option>
                                                        <option value="3">15</option>
                                                        <option value="3">20</option>
                                                    </select>
                                                    <span class="inline ma_l5">行/页</span>
                                                    <span class="inline ma_l10">第1-5行 共1125行</span>
                                                </div>
                                                <div class="col-md-7 col-sm-12">
                                                    <div class="dataTables_paginate paging_simple_numbers f_r" id="sample_editable_1_paginate">
                                                        <ul class="pagination ma0">
                                                            <li class="paginate_button previous disabled" aria-controls="sample_editable_1" tabindex="0" id="sample_editable_1_previous">
                                                                <a href="#">
                                                                    <i class="fa fa-angle-left"></i>
                                                                </a>
                                                            </li>
                                                            <li class="paginate_button active" aria-controls="sample_editable_1" tabindex="0">
                                                                <a href="#">1</a>
                                                            </li>
                                                            <li class="paginate_button " aria-controls="sample_editable_1" tabindex="0">
                                                                <a href="#">2</a>
                                                            </li>
                                                            <li class="paginate_button next" aria-controls="sample_editable_1" tabindex="0" id="sample_editable_1_next">
                                                                <a href="#">
                                                                    <i class="fa fa-angle-right"></i>
                                                                </a>
                                                            </li>
                                                        </ul>
                                                    </div>
                                                </div>
                                            </div>
                                            <!-- 列表demo 结束 -->
                                        </div>
                                    </div>
                                    <!-- END TAB PORTLET-->
                                </div>
                                <div class="tab-pane" id="tab3">
                                    <!-- BEGIN TAB PORTLET-->
                                    <div class="portlet box blue">
                                        <div class="portlet-title">
                                            <div class="caption">商品管理</div>
                                            <div class="actions">
                                                <a href="javascript:;" class="btn btn-default btn-sm">
                                                    <i class="fa fa-upload"></i> 上架
                                                </a>
                                                <a href="javascript:;" class="btn btn-default btn-sm">
                                                    <i class="fa fa-download"></i> 下架
                                                </a>
                                            </div>
                                        </div>
                                        <div class="portlet-body">
                                            <!-- 列表demo 开始 -->
                                            <!-- 表格 -->
                                            <table class="table table-striped table-bordered table-hover dataTable no-footer ta_c product_list_table" id="sample_2" role="grid" aria-describedby="sample_2_info">
                                                <thead>
                                                    <tr>
                                                        <th class="ta_c wb5">
                                                            <input type="checkbox" class="group-checkable checkbox_all" id="checkbox_all">
                                                        </th>
                                                        <th class="ta_c wb10">商品编码</th>
                                                        <th class="ta_c wb10">商品图片</th>
                                                        <th class="ta_c wb20">商品名称</th>
                                                        <!-- <th class="ta_c ">品牌</th> -->
                                                        <th class="ta_c wb10">分类</th>
                                                        <th class="ta_c ">价格</th>
                                                        <th class="ta_c wb5">库存</th>
                                                        <th class="ta_c wn110">商品状态</th>
                                                        <th class="ta_c wn175">操作</th>
                                                    </tr>
                                                </thead>
                                                <tbody>
                                                    <!-- loop -->
                                                    <tr>
                                                        <td>
                                                            <input type="checkbox" class="group-checkable" name="the_checkbox" value="1">
                                                        </td>
                                                        <td>
                                                            <a href="R-product-detail.html">1000000001</a>
                                                        </td>
                                                        <td>
                                                            <img src="images/pro_pic001.jpg" class="pro_pic"></td>
                                                        <td>
                                                            <a href="R-product-detail.html">婴儿抱被新生儿春秋冬款可脱胆纯棉宝宝包被抱毯加厚冬季婴童用品</a>
                                                        </td>
                                                        <td>抱被</td>
                                                        <td>¥88.00</td>
                                                        <td>99</td>
                                                        <td>未发布</td>
                                                        <td class="pro_tdlink">
                                                            <a class="btn btn-xs yellow copy" title="复制链接"><i class="fa fa-paste"></i></a>
                                                            <a href="R-product-detail.html" class="btn btn-xs green" title="查看"><i class="fa fa-search"></i></a>
                                                            <a href="R-product-edit.html" class="btn btn-xs blue" title="编辑"><i class="fa fa-pencil"></i></a>
                                                            <a href="javascript:;" class="btn btn-xs yellow" title="上架"><i class="fa fa-upload"></i></a>
                                                            <a href="javascript:;" class="last btn btn-xs red" title="下架" disabled><i class="fa fa-download"></i></a>
                                                        </td>
                                                    </tr>
                                                    <!-- end -->
                                                </tbody>
                                            </table>
                                            <!-- 表格 -->
                                            <div class="row">
                                                <div class="col-md-5 col-sm-12">
                                                    <select class="form-control select2me wn50" name="pro_cate" data-placeholder="分页数量">
                                                        <option value="2">5</option>
                                                        <option value="3">15</option>
                                                        <option value="3">20</option>
                                                    </select>
                                                    <span class="inline ma_l5">行/页</span>
                                                    <span class="inline ma_l10">第1-5行 共1125行</span>
                                                </div>
                                                <div class="col-md-7 col-sm-12">
                                                    <div class="dataTables_paginate paging_simple_numbers f_r" id="sample_editable_1_paginate">
                                                        <ul class="pagination ma0">
                                                            <li class="paginate_button previous disabled" aria-controls="sample_editable_1" tabindex="0" id="sample_editable_1_previous">
                                                                <a href="#">
                                                                    <i class="fa fa-angle-left"></i>
                                                                </a>
                                                            </li>
                                                            <li class="paginate_button active" aria-controls="sample_editable_1" tabindex="0">
                                                                <a href="#">1</a>
                                                            </li>
                                                            <li class="paginate_button " aria-controls="sample_editable_1" tabindex="0">
                                                                <a href="#">2</a>
                                                            </li>
                                                            <li class="paginate_button next" aria-controls="sample_editable_1" tabindex="0" id="sample_editable_1_next">
                                                                <a href="#">
                                                                    <i class="fa fa-angle-right"></i>
                                                                </a>
                                                            </li>
                                                        </ul>
                                                    </div>
                                                </div>
                                            </div>
                                            <!-- 列表demo 结束 -->
                                        </div>
                                    </div>
                                    <!-- END TAB PORTLET-->
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <!-- ##########主要内容结束########## -->
    </div>
    <script src="../../../assets/scripts/jquery.min.js"></script>


<!-- 弹出层 -->
    <script src="../../../assets/plugins/layer/layer.js" type="text/javascript"></script>
    <!-- 最大数字 -->
    <script src="../../../assets/plugins/bootstrap-maxlength/bootstrap-maxlength.min.js" type="text/javascript"></script>
    <!-- 下拉菜单，可输入 -->
    <script type="text/javascript" src="../../../assets/plugins/bootstrap-select/bootstrap-select.min.js"></script>
    <script type="text/javascript" src="../../../assets/plugins/select2/select2.min.js"></script>
    <script type="text/javascript" src="../../../assets/plugins/jquery-multi-select/js/jquery.multi-select.js"></script>
     <!-- 表格插件 -->
    <script src="../../../assets/plugins/bootstrap-table/js/bootstrap-table.js"></script>


    <!-- 组件库 -->
    <script src="../../../assets/scripts/Assembly.js"></script>
    <script>
   
    $(function(){
   


        //最大字数限制控件
        $('*[maxlength]').maxlength({
            alwaysShow: true,
            limitReachedClass: "label label-danger",
        });
        
        $.extend({
            suc:function(){
                $(".copy").on("click",function(){
                    $('.hrefBox').css("display","block")
                    layer.open({
                        type: 1,
                        shade: false,
                        title: "复制链接", //不显示标题
                        content: $('.hrefBox'), //捕获的元素
                        cancel: function(index){
                            layer.close(index);
                            
                          }
                    })
                    /*var textElem = document.getElementById("song");
                    textSelect(textElem, 0, 5);*/
                    assembly.checkAll({
                        target:$("#song")
                    },2)
                    
                })
            }
        })


        //清空
        /*$(".btn-warning").on("click",function(){
            $(".value-null").val(null);
            $("#select2-chosen-2").html(null)
        });*/


        assembly.checkAll({
            target:$(".btn-warning"),
            valueNull:$(".value-null")
        },1);


        var checkff=1;
        $(".checkbox_all").on("change",function(){
            if(checkff===1){
                assembly.Select($(this).parents("table").find(".group-checkable"),1);
                checkff=2
            }else if(checkff===2){
                assembly.Select($(this).parents("table").find(".group-checkable"),2);
                checkff=1;
            }
            
        });


        //只能输入数字
        assembly.limitNumber($(".limit"));




        //选中文字功能
        var textSelect = function(o, a, b){
            //o是当前对象，例如文本域对象
            //a是起始位置，b是终点位置
            var a = parseInt(a, 10), b = parseInt(b, 10);
            var l = o.value.length;
            if(l){
                //如果非数值，则表示从起始位置选择到结束位置
                if(!a){
                    a = 0;    
                }
                if(!b){
                    b = l;    
                }
                //如果值超过长度，则就是当前对象值的长度
                if(a > l){
                    a = l;    
                }
                if(b > l){
                    b = l;    
                }
                //如果为负值，则与长度值相加
                if(a < 0){
                    a = l + a;
                }
                if(b < 0){
                    b = l + b;    
                }
                if(o.createTextRange){//IE浏览器
                    var range = o.createTextRange();         
                    range.moveStart("character",-l);              
                    range.moveEnd("character",-l);
                    range.moveStart("character", a);
                    range.moveEnd("character",o.value.length);
                    range.select();
                }else{
                    o.setSelectionRange(a, o.value.length);
                    o.focus();
                }
            }
        };
    })




    </script>
</body>
</html>
```


##menu.ftl
```
<!DOCTYPE html>
<html lang="en" class="no-js" style="height: 100%;overflow:hidden">
<head>
    <meta charset="utf-8" />
    <title>创纪云|零售业全渠道O2O平台</title>
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta content="width=device-width, initial-scale=1" name="viewport" />
    <!-- 图标库,bootstrap -->
    <link href="${base}/RES/assets/plugins/font-awesome/css/font-awesome.min.css" rel="stylesheet" type="text/css" />
    <link href="${base}/RES/assets/plugins/simple-line-icons/simple-line-icons.min.css" rel="stylesheet" type="text/css" />
    <link href="${base}/RES/assets/plugins/bootstrap/css/bootstrap.min.css" rel="stylesheet" type="text/css" />
    <!-- 全局样式 -->
    <link href="${base}/RES/assets/css/components.css" id="style_components" rel="stylesheet" type="text/css" />
    <link rel="stylesheet" href="${base}/RES/assets/plugins/layer/skin/layer.css">
    <link href="${base}/RES/assets/css/plugins.css" rel="stylesheet" type="text/css" />
    <link href="${base}/RES/assets/plugins/layout/css/layout.css" rel="stylesheet" type="text/css" />
    <link href="${base}/RES/assets/plugins/layout/css/themes/darkblue.css" rel="stylesheet" type="text/css" id="style_color" />
    <link href="../css/custom.css" rel="stylesheet" type="text/css" />
    <script>
        function iFrameHeight() {


            var ifm= document.getElementById("iframepage");


            var subWeb = document.frames ? document.frames["iframepage"].document : ifm.contentDocument;


                if(ifm != null && subWeb != null) {


                ifm.height = subWeb.body.scrollHeight;


                }
            ifm.style.display="block";
            


        }


    </script>
    <link rel="shortcut icon" href="favicon.ico" />
</head>


<body class="page-header-fixed page-quick-sidebar-over-content page-sidebar-closed-hide-logo" style="height: 100%;overflow: hidden;">


    <!-- 头部 -->
    <div id="headerdiv">
        <div id="header"> 
        <div class="page-header navbar navbar-fixed-top">
            <div class="page-header-inner">
                <div class="page-logo">
                    <a href="javascript:;">
                    <img src="${base}/RES/assets/images/logo.png" alt="logo" class="logo-default"/>
                    </a>
                    <div class="menu-toggler sidebar-toggler hide">
                    </div>
                </div>


                <div class="page-header-title">零售业全渠道O2O平台</div>


                <a href="javascript:;" class="menu-toggler responsive-toggler" data-toggle="collapse" data-target=".navbar-collapse">
                </a>
                <div class="top-menu">
                    <ul class="nav navbar-nav pull-right">
                        <li>
                            <div class="header">南京分店</div>
                        </li>
                        <li class="dropdown dropdown-user">
                            <a href="javascript:;" class="dropdown-toggle" data-toggle="dropdown" data-hover="dropdown" data-close-others="true">
                            <img alt="" class="img-circle" src="${base}/RES/assets/images/avatar3_small.jpg"/>
                            <span class="username username-hide-on-mobile">刘聪 </span>
                            <i class="fa fa-angle-down"></i>
                            </a>
                            <ul class="dropdown-menu dropdown-menu-default">
                                <li>
                                    <a href="extra_profile.html">
                                    <i class="icon-user"></i> My Profile </a>
                                </li>
                                <li>
                                    <a href="page_calendar.html">
                                    <i class="icon-calendar"></i> My Calendar </a>
                                </li>
                                <li>
                                    <a href="inbox.html">
                                    <i class="icon-envelope-open"></i> My Inbox <span class="badge badge-danger">
                                    3 </span>
                                    </a>
                                </li>
                                <li>
                                    <a href="page_todo.html">
                                    <i class="icon-rocket"></i> My Tasks <span class="badge badge-success">
                                    7 </span>
                                    </a>
                                </li>
                                <li class="divider">
                                </li>
                                <li>
                                    <a href="extra_lock.html">
                                    <i class="icon-lock"></i> Lock Screen </a>
                                </li>
                                <li>
                                    <a href="login.php">
                                    <i class="icon-key"></i> Log Out </a>
                                </li>
                            </ul>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
        </div>
        <div class="clearfix"></div></div>
    <div class="page-container" style="overflow:hidden">
        <!-- 菜单 -->
        <div id="menudiv" class="page-sidebar-wrapper">
            <div id="menu" class="page-sidebar-wrapper" style="position:fixed;">
                <div class="page-sidebar navbar-collapse collapse">
                    <ul class="page-sidebar-menu " data-keep-expanded="false" data-auto-scroll="true" data-slide-speed="200">
                        <li class="sidebar-toggler-wrapper">
                            <div class="sidebar-title">
                                商户中心
                            </div>
                            <div class="sidebar-toggler">
                            </div>
                        </li>
                        <li class="sidebar-search-wrapper">
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
                        </li>
                        
                        <li>
                            <a href="javascript:;">
                                <i class="glyphicon glyphicon-cog"></i>
                                <span class="title">商品管理中心</span>
                                <span class="arrow "></span>
                            </a>
                            <ul class="sub-menu">
                                <li>
                                    <a href="../R-product.html">
                                        <i class="glyphicon glyphicon-qrcode "></i> 商品管理
                                    </a>
                                </li>
                                <li>
                                    <a href="../R-product-publish.html">
                                        <i class="glyphicon glyphicon-barcode "></i> 商品发布
                                    </a>
                                </li>
                            </ul>
                        </li>
                        
                        
                    </ul>
                </div>
            </div>
        </div>


        <!-- 正文 -->
        <div class="page-con" style="margin-left: 235px;margin-top: 0;">
            <div class="page-iframe" style="position: relative;left: 0;background-color: #fff;">
                <iframe id="iframepage" src="../iframe-1.html" width="100%" height="100%" frameborder="0" onLoad="iFrameHeight()"></iframe>
                
                <div class="load" style="background: url(${base}/RES/assets/plugins/layer/skin/default/loading-1.gif) no-repeat;width:37px;height: 37px;position: absolute;left: 50%;top: 50%;margin-left: -16.5px;margin-top: -16.5px;display: none;"></div>
            </div>
        </div>




    </div>
    
    
    <!-- 弹出链接 -->
    <div class="hrefBox" style="display: none;">
        <textarea name="" style="border:none;outline: none;width: 300px;text-indent: 3px;" value="http://www.baidu.com" cols="30" rows="10" id="song">http://www.baidu.com</textarea>
    </div>
    
    <!-- BEGIN FOOTER -->
    <div id="footer" class="page-footer">
        <div class="page-footer-inner" style="width: 100%;text-align: center;">
             2015 © 江苏创纪云网络科技有限公司. <a href="111111111111" target="_blank">请购买!</a>
        </div>
        <div class="scroll-to-top">
            <i class="icon-arrow-up"></i>
        </div>
    </div>
    <script src="${base}/RES/assets/scripts/jquery.min.js" type="text/javascript"></script>
    <script src="${base}/RES/assets/plugins/bootstrap/js/bootstrap.min.js" type="text/javascript"></script>
    <script src="${base}/RES/assets/plugins/layer/layer.js"></script>
    <script src="${base}/RES/assets/scripts/metronic.js" type="text/javascript"></script>
    <script src="${base}/RES/assets/plugins/layout/scripts/layout.js" type="text/javascript"></script>
    <script src="${base}/RES/assets/plugins/layout/scripts/quick-sidebar.js" type="text/javascript"></script>
    <script src="${base}/RES/assets/plugins/layout/scripts/demo.js" type="text/javascript"></script>
    <script src="${base}/RES/assets/scripts/Assembly.js" type="text/javascript"></script>
    <script src="../scripts/add_demo.js" type="text/javascript"></script>
   
    <script>
    jQuery(document).ready(function() {




        var header_hei=$("#headerdiv").height(),
            body_hei=document.documentElement.offsetHeight,
            height=body_hei-header_hei-100;
        
        $(".page-iframe").css("height",height+"px")
        var a=document.getElementsByTagName("a");
        for (var i = 0; i < a.length; i++) {
            a[i].onclick=function(event){
                event.preventDefault();


                var reg=/\.html/g,
                    href=this.getAttribute("href");
                if(reg.test(href)){
                    $(".load").css("display","block");
                    var last=$("#iframepage");
                    last.attr("id","iframe");
                    var iframe=document.createElement("iframe");
                    iframe.src=href;
                    iframe.width="100%";
                    iframe.id="iframepage";
                    iframe.setAttribute("height",height);
                    iframe.style.height=height+"px";
                    iframe.style.opacity="0";
                    iframe.style.overflow="hidden";
                    iframe.style.border="none";
                    
                    $(".page-iframe").append(iframe);
                    
                    $("#iframepage").on("load",function(){
                        $(".load").css("display","none");
                        last.remove();
                        iFrameHeight();
                        $("#iframepage").animate({
                            opacity:"1",
                            filter:"alpha(opacity=100)" //标准的css透明度，在大部分的标准浏览器Firefox, Safari, and Opera都有效
                        },160);
                    });
                    //alert(1)
                    //$("#iframepage").attr("src",href);
                }


                return false;
            }
        }
        //assembly.menu();
        Metronic.init(); // init metronic core components
        Layout.init(); // init current layout
        QuickSidebar.init(); // init quick sidebar
        //Demo.initvalue(); // init demo features


    });
    </script>


</body>
<!-- END BODY -->


</html>




<!-- BEGIN SIDEBAR -->

<div id="menu" class="page-sidebar-wrapper" style="position:fixed;">
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
                    运营中心
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

<li>
<a href="javascript:;">
<i class="icon-home"></i>
<span class="title">服务设置</span>
<span class="arrow "></span>
</a>
<ul class="sub-menu">

<li>
<a href="O-OfficialAccountsmanage-DEPT-pub.html">
<i class="icon-bulb"></i>
公众号管理</a>
</li>
<li>
<a href="O-OfficialAccountsmanage-midservice.html">
<i class="icon-bulb"></i>
中台服务地址设置</a>
</li>
</ul>
</li>
<li>
<a href="O-OfficialAccountsmanage-DEPT.html">
<i class="icon-home"></i>
<span class="title">微商城交易查询</span>
</a>
</li>


</ul>
<!-- END SIDEBAR MENU -->
</div>
</div>
<!-- END SIDEBAR -->
```