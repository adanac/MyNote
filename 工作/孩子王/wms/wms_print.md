##新增模块


printer-add-controller.js
```
wmsApp.controller( 'printerAddController' , [ '$scope' ,  '$rootScope' , '$modal' , '$compile' , '$timeout' ,  '$q' ,  '$modalInstance' ,  'masterDataService' , 'printerManageService' ,  'DTOptionsBuilder' ,  'DTColumnDefBuilder' ,
     function  ($scope, $rootScope,$modal, $compile, $timeout, $q, $modalInstance, masterDataService, printerManageService, DTOptionsBuilder, DTColumnDefBuilder) {
        $scope.criteria = {
            toWhSysNo:  '' ,
            printNode:  ''
        };
        
         /**
          *   提交到后台的bean对象
          */
        $scope.printerBean = {
            whSysNo:  '' ,
            printerNo:  '' ,
            printerDesc:  '' ,
            address:  '' ,
            printNode:  '' ,
            clientIp:  '' ,
            comments:  ''
        };
        
         /**
          *   获取仓库列表
          */
        $scope.toWhOptions = [];
         if  ($scope.toWhOptions.length === 0) {
            masterDataService.getWhList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.toWhOptions = reply.data;
                }
            });
        }
        $scope.changeToWh =  function  (toWh) {
            $scope.criteria.toWhSysNo = !(toWh.index) ?  ''  : $scope.toWhOptions[toWh.index].sysNo;
        };
        
         /**
          *   获取打印环节   下拉
          */
        $scope.printNodeOptions = [];
         if  ($scope.printNodeOptions.length === 0) {
            masterDataService.getPrintNodeList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.printNodeOptions = reply.data;
                }
            });
        }
        $scope.changePrintNode =  function  (printNode) {
            $scope.criteria.printNode = !(printNode.index) ?  ''  : $scope.printNodeOptions[printNode.index].key;
        };
         /**
          *   保存
          */
        $scope.savePrinter =  function  () {
             if  ($scope.toWhSysNo ==  ""  || $scope.toWhSysNo ==  undefined ) {
                alert( "请选择仓库" )
                 return   false ;
            }
             if  ($scope.printerNo ==  "" ) {
                alert( "请输入打印机名称" )
                 return   false ;
            }
             if  ($scope.address ==  "" ){
                alert( "请输入打印机ip" )
                 return   false ;
            }
             if  ($scope.printNode ==  "" ){
                alert( "请输入打印环节" )
                 return   false ;
            }
             if  ($scope.clientIp ==  "" ){
                alert( "请输入客户端ip" )
                 return   false ;
            }
            
            $scope.printerBean.whSysNo = parseInt($scope.toWhSysNo);
            $scope.printerBean.printerNo = $scope.printerNo;
            $scope.printerBean.printerDesc = $scope.printerDesc;
            $scope.printerBean.address = $scope.address;
            $scope.printerBean.printNode = $scope.criteria.printNode;
            $scope.printerBean.clientIp = $scope.clientIp;
            $scope.printerBean.comments = $scope.comments;
            
            $rootScope.loadingPromise = savePrinter($scope.printerBean).success( function  (res) {
                 if (res.resultCode ==  "0" ){
                    alert(res.resultMsg);
                } else {
                    alert( "新增成功!" );
                    $modalInstance.close(1);
                }
            });
            
        };
         var  savePrinter =  function (printerBean){
             return  printerManageService.savePrinter(printerBean)
        };
        
         /**
          *   对话框关闭响应
          */
        $scope.close =  function  () {
//            $modalInstance.close('refresh');
            $modalInstance.dismiss( 'close' );
        };     }]);   

```


printer-add.html
```
<div class="">
    <div class="modal-content">
        <div class="modal-header">
            <button type="button" class="close" ng-click="close()">×</button>
            <h4 class="modal-title">新增打印机</h4>
        </div>
        <div class="modal-body" style="height: 250px;">
            <!-- Row 1 -->
            <div class="form-inline col-md-12">
                <div class="control-group col-md-4">
                     <div class="bolder ">仓库：</div>
                     <div custom-select="s.sysNo as s.whName for s in toWhOptions | filter: { whName: $searchTerm } track by s.sysNo" ng-model="toWhSysNo"></div>
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">打印机名称：</div>
                    <input type="text" class="form-control" maxlength="64" autocomplete="off" ng-model="printerNo">
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">打印机描述：</div>
                    <input type="text" class="form-control" maxlength="128" autocomplete="off" ng-model="printerDesc">
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">打印机ip：</div>
                    <input type="text" class="form-control" maxlength="128" autocomplete="off" ng-model="address">
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">打印环节：</div>
                    <select class="form-control" id="printNode" data-ng-model="printNode.index"
                                ng-change="changePrintNode(printNode)">
                        <option value="">--请选择--</option>
                        <option ng-repeat="printNode in printNodeOptions" ng-value="$index">{{printNode.value}}</option>
                    </select>
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">客户端ip：</div>
                    <input type="text" class="form-control" maxlength="50" autocomplete="off" ng-model="clientIp">
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">备注：</div>
                    <input type="text" class="form-control" maxlength="256" autocomplete="off" ng-model="comments">
                </div>
            </div>
            
            <div class="clearfix"></div>
            <!-- Row 2 -->
            <div class="form-inline col-md-12 pull-right">
                <div class="control-group pull-right">
                    <input type="button" value="保存" class="btn btn-success" ng-click="savePrinter()">
                </div>
            </div>
            
        </div>
    </div> </div>   

```


printer-manage-service.js
```
/**
  *   Desc:      打印机管理服务
  */
wmsApp.factory( 'printerManageService' , [ 'restProxyService' ,  'WMS_API_PROVIDER' ,  'WMS_API_END_POINT' ,
     function  (restProxyService, WMS_API_PROVIDER, WMS_API_END_POINT) {
         /**
          *   共享变量
          */
         var  allocationStrategySysNo;
        
         /**
          *   共享变量
          */
         var  sysNo;
        
        
         /**
          *   函数列表
          */
         return  {
            getPrinterManageList:  function  (params) {
                 return  restProxyService.sendHttpGet(WMS_API_PROVIDER, WMS_API_END_POINT +  '/mst-printer-manage/list' , params);
            },
            getPrinterInfo:  function  (params) {
                 return  restProxyService.sendHttpGet(WMS_API_PROVIDER, WMS_API_END_POINT +  '/mst-printer-manage/info' , params);
            },
            savePrinter:  function  (params) {
                 return  restProxyService.sendHttpPost(WMS_API_PROVIDER, WMS_API_END_POINT +  '/mst-printer-manage/add' , params);
            },
            updatePrinter:  function  (params) {
                 return  restProxyService.sendHttpPost(WMS_API_PROVIDER, WMS_API_END_POINT +  '/mst-printer-manage/update' , params);
            },
             /**
              *   Getter   &   Setter   函数
              */
            getAllocationStrategySysNo:  function  () {
                 return  allocationStrategySysNo;
            },
            setAllocationStrategySysNo:  function  (aSysNo) {
                allocationStrategySysNo = aSysNo;
            },
            
             /**
              *   Getter   &   Setter   函数
              */
            getSysNo:  function  () {
                 return  sysNo;
            },
            setSysNo:  function  (aSysNo) {
                sysNo = aSysNo;
            }
        }
    } ]);   

```


MstPrinterManager.java
```
package  com.haiziwang.kwms.common.domain.bean;
import  java.io.Serializable;
import  java.util.Date;
public   class  MstPrinterManage  implements  Serializable
{
     private   static   final   long  serialVersionUID = 1L;
    
     /**
     * 内部主键
     */
     private  Long sysNo;
     /**
     * 打印机名称
     */
     private  String printerNo;
     /**
     * 打印机描述
     */
     private  String printerDesc;
    
     /**
     * 打印环节
     */
     private  Integer printNode;
    
     private  String printNodeName;
    
     /**
     * 客户端Ip
     */
     private  String clientIp;
    
     /**
     * 打印机 完整存取地址
     */
     private  String address;
     /**
     * 仓库内部主键
     */
     private  String whSysNo;
     /**
     * 仓库编码
     */
     private  String whNo;
     /**
     * 仓库名称
     */
     private  String whName;
     /**
     * 备注
     */
     private  String comments;
     /**
     * 是否有效：1是 0否
     */
     private  Integer yn;
     /**
     * 时间戳
     */
     private  Date ts;
     /**
     * 创建时间
     */
     private  Date createTime;
     private  Date updateTime;
     /**
     * 创建人
     */
     private  String createUser;
     private  String updateUser;
     private  Integer start;
     private  Integer rows;
     public  Long getSysNo()
    {
         return  sysNo;
    }
     public   void  setSysNo(Long sysNo)
    {
         this .sysNo = sysNo;
    }
     public  String getPrinterNo()
    {
         return  printerNo;
    }
     public   void  setPrinterNo(String printerNo)
    {
         this .printerNo = printerNo;
    }
     public  String getPrinterDesc()
    {
         return  printerDesc;
    }
     public   void  setPrinterDesc(String printerDesc)
    {
         this .printerDesc = printerDesc;
    }
    
     public  Integer getPrintNode() {
         return  printNode;
    }
     public   void  setPrintNode(Integer printNode) {
         this .printNode = printNode;
    }
     public  String getPrintNodeName() {
         return  printNodeName;
    }
     public   void  setPrintNodeName(String printNodeName) {
         this .printNodeName = printNodeName;
    }
     public  String getClientIp() {
         return  clientIp;
    }
     public   void  setClientIp(String clientIp) {
         this .clientIp = clientIp;
    }
     public  String getAddress() {
         return  address;
    }
     public   void  setAddress(String address) {
         this .address = address;
    }
     public  Integer getYn() {
         return  yn;
    }
     public   void  setYn(Integer yn) {
         this .yn = yn;
    }
     public  String getWhNo()
    {
         return  whNo;
    }
     public   void  setWhNo(String whNo)
    {
         this .whNo = whNo;
    }
     public  String getWhName()
    {
         return  whName;
    }
     public   void  setWhName(String whName)
    {
         this .whName = whName;
    }
     public  String getComments()
    {
         return  comments;
    }
     public   void  setComments(String comments)
    {
         this .comments = comments;
    }
     public  String getWhSysNo() {
         return  whSysNo;
    }
     public   void  setWhSysNo(String whSysNo) {
         this .whSysNo = whSysNo;
    }
     public  Date getUpdateTime() {
         return  updateTime;
    }
     public   void  setUpdateTime(Date updateTime) {
         this .updateTime = updateTime;
    }
     public  String getCreateUser() {
         return  createUser;
    }
     public   void  setCreateUser(String createUser) {
         this .createUser = createUser;
    }
     public  String getUpdateUser() {
         return  updateUser;
    }
     public   void  setUpdateUser(String updateUser) {
         this .updateUser = updateUser;
    }
     public  Date getTs()
    {
         return  ts;
    }
     public   void  setTs(Date ts)
    {
         this .ts = ts;
    }
     public  Date getCreateTime()
    {
         return  createTime;
    }
     public   void  setCreateTime(Date createTime)
    {
         this .createTime = createTime;
    }
     public  Integer getStart()
    {
         return  start;
    }
     public   void  setStart(Integer start)
    {
         this .start = start;
    }
     public  Integer getRows()
    {
         return  rows;
    }
     public   void  setRows(Integer rows)
    {
         this .rows = rows;
    }
}
```
MstPrinterManageListResDto.java
```
package  com.haiziwang.kwms.common.domain.dto.mst;
import  java.util.List;
import  com.haiziwang.kwms.common.domain.bean.MstPrinterManage;
import  com.haiziwang.kwms.common.domain.dto.BaseResDto;
public   class  MstPrinterManageListResDto  extends  BaseResDto
{
     private   static   final   long  serialVersionUID = 1L;
     private  Integer totalCont; //记录总数
     private  Integer totalPage; //记录总页数
     private  List<MstPrinterManage> list; //列表数据
     public  Integer getTotalCont()
    {
         return  totalCont;
    }
     public   void  setTotalCont(Integer totalCont)
    {
         this .totalCont = totalCont;
    }
     public  Integer getTotalPage()
    {
         return  totalPage;
    }
     public   void  setTotalPage(Integer totalPage)
    {
         this .totalPage = totalPage;
    }
     public  List<MstPrinterManage> getList()
    {
         return  list;
    }
     public   void  setList(List<MstPrinterManage> list)
    {
         this .list = list;
    }
     public   static   long  getSerialversionuid()
    {
         return  serialVersionUID;
    }
}
```
MstPrinterManageReqDto.java

```
package  com.haiziwang.kwms.common.domain.dto.mst;
import  java.util.Date;
import  com.haiziwang.kwms.common.domain.dto.PageReqDto;
public   class  MstPrinterManageReqDto  extends  PageReqDto
{
    
     private   static   final   long  serialVersionUID = 1L;
     /**
     * 内部主键
     */
     private  Long sysNo;
     /**
     * 打印机名称
     */
     private  String printerNo;
     /**
     * 打印环节
     */
     private  Integer printNode;
    
     /**
     * 打印机描述
     */
     private  String printerDesc;
    
     /**
     * 打印机Ip
     */
     private  String address;
    
     /**
     * 接入该打印机的客户端ip
     */
     private  String clientIp;
     /**
     * 仓库内部主键
     */
     private  String whSysNo;
     /**
     * 仓库编码
     */
     private  String whNo;
     /**
     * 仓库名称
     */
     private  String whName;
     /**
     * 备注
     */
     private  String comments;
     /**
     * 是否有效：1是 0否
     */
     private  Integer yn;
     /**
     * 时间戳
     */
     private  Date ts;
     /**
     * 创建时间
     */
     private  Date createTime;
     private  Date updateTime;
     /**
     * 创建人
     */
     private  String createUser;
     private  String updateUser;
     private  Integer start; //当前页数
     private  Integer rows; //每页显示记录数
     /**
     * 构造函数
     *
     *  @param  reqId 请求ID
     *  @param  start 待查询的开始索引
     *  @param  rows  返回记录条数
     */
     public  MstPrinterManageReqDto( int  reqId,  int  start,  int  rows) {
         super (reqId, start, rows);
    }
     public  MstPrinterManageReqDto(){
    }
     public  String getWhSysNo() {
         return  whSysNo;
    }
     public   void  setWhSysNo(String whSysNo) {
         this .whSysNo = whSysNo;
    }
     public  Long getSysNo()
    {
         return  sysNo;
    }
     public   void  setSysNo(Long sysNo)
    {
         this .sysNo = sysNo;
    }
     public  String getPrinterNo()
    {
         return  printerNo;
    }
     public   void  setPrinterNo(String printerNo)
    {
         this .printerNo = printerNo;
    }
     public  String getPrinterDesc()
    {
         return  printerDesc;
    }
     public   void  setPrinterDescDesc(String printerDesc)
    {
         this .printerDesc = printerDesc;
    }
    
     public  Integer getPrintNode() {
         return  printNode;
    }
     public   void  setPrintNode(Integer printNode) {
         this .printNode = printNode;
    }
     public  String getClientIp() {
         return  clientIp;
    }
     public   void  setClientIp(String clientIp) {
         this .clientIp = clientIp;
    }
    
     public  String getAddress() {
         return  address;
    }
     public   void  setAddress(String address) {
         this .address = address;
    }
     public   void  setPrinterDesc(String printerDesc) {
         this .printerDesc = printerDesc;
    }
     public  Integer getYn() {
         return  yn;
    }
     public   void  setYn(Integer yn) {
         this .yn = yn;
    }
     public  Date getUpdateTime() {
         return  updateTime;
    }
     public   void  setUpdateTime(Date updateTime) {
         this .updateTime = updateTime;
    }
     public  String getCreateUser() {
         return  createUser;
    }
     public   void  setCreateUser(String createUser) {
         this .createUser = createUser;
    }
     public  String getUpdateUser() {
         return  updateUser;
    }
     public   void  setUpdateUser(String updateUser) {
         this .updateUser = updateUser;
    }
     public  String getWhNo()
    {
         return  whNo;
    }
     public   void  setWhNo(String whNo)
    {
         this .whNo = whNo;
    }
     public  String getWhName()
    {
         return  whName;
    }
     public   void  setWhName(String whName)
    {
         this .whName = whName;
    }
     public  String getComments()
    {
         return  comments;
    }
     public   void  setComments(String comments)
    {
         this .comments = comments;
    }
     public  Date getTs()
    {
         return  ts;
    }
     public   void  setTs(Date ts)
    {
         this .ts = ts;
    }
     public  Date getCreateTime()
    {
         return  createTime;
    }
     public   void  setCreateTime(Date createTime)
    {
         this .createTime = createTime;
    }
     public  Integer getRows() {
         return  rows;
    }
     public   void  setRows(Integer rows) {
         this .rows = rows;
    }
     public  Integer getStart() {
         return  start;
    }
     public   void  setStart(Integer start) {
         this .start = start;
    }
     public   static   long  getSerialversionuid() {
         return  serialVersionUID;
    }
}
```
MstPrinterManageResDto.java

```
package  com.haiziwang.kwms.common.domain.dto.mst;
import  java.util.Date;
import  com.haiziwang.kwms.common.domain.dto.BaseResDto;
public   class  MstPrinterManageResDto  extends  BaseResDto
{
     private   static   final   long  serialVersionUID = 1L;
    
     /**
     * 内部主键
     */
     private  Long sysNo;
     /**
     * 打印机名称
     */
     private  String printerNo;
     /**
     * 打印机描述
     */
     private  String printerDesc;
    
     /**
     * 打印机ip 完整存取地址
     */
     private  String address;
    
     /**
     * 打印环节
     */
     private  Integer printNode;
    
     /**
     * 接入该打印机的客户端ip
     */
     private  String clientIp;
     /**
     * 仓库内部主键
     */
     private  String whSysNo;
     /**
     * 仓库编码
     */
     private  String whNo;
     /**
     * 仓库名称
     */
     private  String whName;
     /**
     * 备注
     */
     private  String comments;
     /**
     * 是否有效：1是 0否
     */
     private  Integer enable_flag;
     /**
     * 时间戳
     */
     private  Date ts;
     /**
     * 创建时间
     */
     private  Date createTime;
     private  Date updateTime;
     /**
     * 创建人
     */
     private  String createUser;
     private  String updateUser;
     public  Long getSysNo()
    {
         return  sysNo;
    }
     public   void  setSysNo(Long sysNo)
    {
         this .sysNo = sysNo;
    }
     public  String getPrinterNo()
    {
         return  printerNo;
    }
     public   void  setPrinterNo(String printerNo)
    {
         this .printerNo = printerNo;
    }
     public  String getPrinterDesc()
    {
         return  printerDesc;
    }
     public   void  setPrinterDesc(String printerDesc)
    {
         this .printerDesc = printerDesc;
    }
    
     public  String getAddress() {
         return  address;
    }
     public   void  setAddress(String address) {
         this .address = address;
    }
     public  Integer getPrintNode() {
         return  printNode;
    }
     public   void  setPrintNode(Integer printNode) {
         this .printNode = printNode;
    }
     public  String getClientIp() {
         return  clientIp;
    }
     public   void  setClientIp(String clientIp) {
         this .clientIp = clientIp;
    }
     public  String getWhSysNo() {
         return  whSysNo;
    }
     public   void  setWhSysNo(String whSysNo) {
         this .whSysNo = whSysNo;
    }
     public  Integer getEnable_flag() {
         return  enable_flag;
    }
     public   void  setEnable_flag(Integer enable_flag) {
         this .enable_flag = enable_flag;
    }
     public  Date getUpdateTime() {
         return  updateTime;
    }
     public   void  setUpdateTime(Date updateTime) {
         this .updateTime = updateTime;
    }
     public  String getCreateUser() {
         return  createUser;
    }
     public   void  setCreateUser(String createUser) {
         this .createUser = createUser;
    }
     public  String getUpdateUser() {
         return  updateUser;
    }
     public   void  setUpdateUser(String updateUser) {
         this .updateUser = updateUser;
    }
     public  String getWhNo()
    {
         return  whNo;
    }
     public   void  setWhNo(String whNo)
    {
         this .whNo = whNo;
    }
     public  String getWhName()
    {
         return  whName;
    }
     public   void  setWhName(String whName)
    {
         this .whName = whName;
    }
     public  String getComments()
    {
         return  comments;
    }
     public   void  setComments(String comments)
    {
         this .comments = comments;
    }
     public  Date getTs()
    {
         return  ts;
    }
     public   void  setTs(Date ts)
    {
         this .ts = ts;
    }
     public  Date getCreateTime()
    {
         return  createTime;
    }
     public   void  setCreateTime(Date createTime)
    {
         this .createTime = createTime;
    }
     public   static   long  getSerialversionuid() {
         return  serialVersionUID;
    }
/*    public List<MstAllocationStrategyDetail> getList()
    {
        return list;
    }
    public void setList(List<MstAllocationStrategyDetail> list)
    {
        this.list = list;
    }*/
    
    
}
```
MstPrinterManageDao.java

```
package  com.haiziwang.kwms.outbound.dao;
import  com.haiziwang.kwms.common.dao.base.BaseDao;
import  com.haiziwang.kwms.common.domain.bean.MstPrinterManage;
import  java.util.List;
/**
 *  @desc  打印机管理
 */
public   interface  MstPrinterManageDao  extends  BaseDao<MstPrinterManage, Long>
{
     /**
     *  @param  mstPrinterManage 请求参数对象
     *  @return
     *  @desc  根据查询条件查询列表
     */
     public  List<MstPrinterManage> selectMstPrinterManageByList(MstPrinterManage mstPrinterManage);
    
     /**
     *  @param  mstAllocationStrategy
     *  @desc  根据syn_no查询数据
     */
     public  MstPrinterManage queryOneBySysNo(MstPrinterManage mstPrinterManage);
     /**
     *  @param  mstPrinterManage 请求参数对象
     *  @return
     *  @desc  根据查询条件查询记录总数
     */
     public   int  selectMstPrinterManageDaoByCont(MstPrinterManage mstPrinterManage);
     /**
     *  @param  mstPrinterManage
     *  @desc  新增新纪录
     */
     public   void  addMstPrinterManage(MstPrinterManage mstPrinterManage);
     /**
     *  @param  mstPrinterManage
     *  @desc  根据syn_no更新数据
     */
     public   void  updateMstPrinterManageBySynNo(MstPrinterManage mstPrinterManage);
     /**
     *  @param  mstPrinterManage
     *  @desc  根据syn_no删除数据
     */
     public   void  deleteMstPrinterManageBySynNo(MstPrinterManage mstPrinterManage);
}
```
MstPrinterManageDaoImpl.java

```
package  com.haiziwang.kwms.outbound.dao.impl;
import  com.haiziwang.kwms.common.dao.base.BaseDaoSupport;
import  com.haiziwang.kwms.common.domain.bean.MstPrinterManage;
import  com.haiziwang.kwms.common.domain.enums.WMS3ExceptionCode;
import  com.haiziwang.kwms.common.domain.exception.WMS3UnCheckedException;
import  com.haiziwang.kwms.outbound.dao.MstPrinterManageDao;
import  org.springframework.stereotype.Repository;
import  java.util.List;
/**
 *  @desc  打印机管理
 */
@ Repository( "mstPrinterManageDao" )
public   class  MstPrinterManageDaoImpl  extends  BaseDaoSupport<MstPrinterManage, Long>  implements  MstPrinterManageDao {
     private   static   final  String NAMESPACE =  "mybatis.mapper.MstPrinterManageMapper" ;
     protected  MstPrinterManageDaoImpl() {
         super (NAMESPACE);
    }
     /**
     *  @param  mstPrinterManage 请求参数对象
     *  @return
     *  @desc  根据查询条件查询出库单列表
     */
     public  List<MstPrinterManage> selectMstPrinterManageByList(MstPrinterManage mstPrinterManage) {
        List<MstPrinterManage> list =  null ;
         try  {
            list =  this .getMySqlSessionTemplate().selectList(NAMESPACE +  ".selectMstPrinterManageByList" , mstPrinterManage);
        }  catch  (Exception e) {
            e.printStackTrace();
             throw   new  WMS3UnCheckedException(WMS3ExceptionCode.DB_OPERATOR_EXPT.getCode(), e);
        }
         return  list;
    }
     /**
     *  @param  mstPrinterManage 请求参数对象
     *  @return
     *  @desc  根据查询条件查询出库单记录总数
     */
     public   int  selectMstPrinterManageDaoByCont(MstPrinterManage mstPrinterManage) {
        Integer c = 0;
         try  {
            Object o =  this .getMySqlSessionTemplate().selectOne(NAMESPACE +  ".selectMstPrinterManageByCount" , mstPrinterManage);
            c = (Integer) o;
        }  catch  (Exception e) {
            e.printStackTrace();
             throw   new  WMS3UnCheckedException(WMS3ExceptionCode.DB_OPERATOR_EXPT.getCode(), e);
        }
         return  c;
    }
     /**
     *  @param  mstPrinterManage
     *  @desc  新增新纪录
     */
     public   void  addMstPrinterManage(MstPrinterManage mstPrinterManage) {
         try  {
             this .getMySqlSessionTemplate().insert(NAMESPACE +  ".insert" , mstPrinterManage);
        }  catch  (Exception e) {
            e.printStackTrace();
             throw   new  WMS3UnCheckedException(WMS3ExceptionCode.DB_OPERATOR_EXPT.getCode(), e);
        }
    }
     /**
     *  @param  putawayStrategy
     *  @desc  根据syn_no查询数据
     */
     public  MstPrinterManage queryOneBySysNo(MstPrinterManage Reqdto) {
        MstPrinterManage mstPrinterManage =  null ;
         try  {
            mstPrinterManage = (MstPrinterManage)  this .getMySqlSessionTemplate().selectOne(NAMESPACE +  ".selectByPrimaryKey" , Reqdto.getSysNo());
        }  catch  (Exception e) {
            e.printStackTrace();
             throw   new  WMS3UnCheckedException(WMS3ExceptionCode.DB_OPERATOR_EXPT.getCode(), e);
        }
         return  mstPrinterManage;
    }
    
     /**
     *  @param  mstPrinterManage
     *  @desc  根据syn_no更新数据
     */
     public   void  updateMstPrinterManageBySynNo(MstPrinterManage mstPrinterManage) {
         try  {
             this .getMySqlSessionTemplate().update(NAMESPACE +  ".updateByPrimaryKeySelective" , mstPrinterManage);
        }  catch  (Exception e) {
            e.printStackTrace();
             throw   new  WMS3UnCheckedException(WMS3ExceptionCode.DB_OPERATOR_EXPT.getCode(), e);
        }
    }
     @ Override
     public   void  deleteMstPrinterManageBySynNo(MstPrinterManage mstPrinterManage) {
         try  {
             this .getMySqlSessionTemplate().update(NAMESPACE +  ".deleteByPrimaryKey" , mstPrinterManage);
        }  catch  (Exception e) {
            e.printStackTrace();
             throw   new  WMS3UnCheckedException(WMS3ExceptionCode.DB_OPERATOR_EXPT.getCode(), e);
        }
    }
}
```
MstPrinterMangerMapper.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="mybatis.mapper.MstPrinterManageMapper" >
  <resultMap id="BaseResultMap" type="com.haiziwang.kwms.common.domain.bean.MstPrinterManage" >
    <id column="sys_no" property="sysNo" jdbcType="BIGINT" />
    <result column="printer_no" property="printerNo" jdbcType="VARCHAR" />
    <result column="printer_desc" property="printerDesc" jdbcType="VARCHAR" />
    <result column="address" property="address" jdbcType="VARCHAR" />
    <result column="print_node" property="printNode" jdbcType="INTEGER" />
    <result column="client_ip" property="clientIp" jdbcType="VARCHAR" />
    <result column="wh_sys_no" property="whSysNo" jdbcType="VARCHAR" />
    <result column="wh_no" property="whNo" jdbcType="VARCHAR" />
    <result column="wh_name" property="whName" jdbcType="VARCHAR" />
    <result column="comments" property="comments" jdbcType="VARCHAR" />
    <result column="yn" property="yn" jdbcType="INTEGER" />
    <result column="create_time" property="createTime"  jdbcType="TIMESTAMP" />
    <result column="create_user" property="createUser" jdbcType="VARCHAR" />
    <result column="update_user" property="updateUser" jdbcType="VARCHAR" />
    <result column="update_time" property="updateTime"  jdbcType="TIMESTAMP" />
  </resultMap>
  
  <sql id="Base_Column_List" >
    sys_no, printer_no, printer_desc, address, print_node, client_ip, wh_sys_no, wh_no, wh_name, 
    comments, yn, create_time, create_user, update_user, update_time
  </sql>
    <sql id="query_where">
        <if test="sysNo != null and sysNo != ''" >
            and sys_no = #{sysNo}
        </if>
        <if test="printerNo != null and printerNo != ''" >
            and printer_no=#{printerNo}
        </if>
        <if test="printerDesc != null and printerDesc != ''" >
            and printer_desc like concat('%', #{printerDesc}, '%')
        </if>
        <if test="printNode != null and printNode != ''" >
            and print_node=#{printNode}
        </if>
        <if test="clientIp != null and clientIp != ''" >
            and client_ip=#{clientIp}
        </if>
        <if test="whSysNo != null and whSysNo !=''" >
            and wh_sys_no=#{whSysNo}
        </if>
        <if test="whNo != null and whNo !=''" >
            and wh_no=#{whNo}
        </if>
        <if test="yn != null and yn != ''" >
            and yn=#{yn}
        </if>
    </sql>
  
  <select id="selectMstPrinterManageByList" resultMap="BaseResultMap" parameterType="com.haiziwang.kwms.common.domain.bean.MstPrinterManage" >
    select 
    <include refid="Base_Column_List" />
    from sys_printer
      where 1=1
      <include refid="query_where" />
    order by sys_no desc limit #{start}, #{rows}
  </select>
  
  <select id="selectMstPrinterManageByCount" resultType="java.lang.Integer" parameterType="com.haiziwang.kwms.common.domain.bean.MstPrinterManage" >
    select 
        count(1)
    from sys_printer
      where 1=1
      <include refid="query_where" />
  </select>  
  
  <select id="selectByPrimaryKey" resultMap="BaseResultMap" parameterType="java.lang.Long" >
    select 
    <include refid="Base_Column_List" />
    from sys_printer
    where sys_no = #{pk,jdbcType=BIGINT}
  </select>
  
  <delete id="deleteByPrimaryKey" parameterType="java.lang.Long" >
    delete from sys_printer
    where sys_no = #{pk,jdbcType=BIGINT}
  </delete>
  
  <insert id="insert" parameterType="com.haiziwang.kwms.common.domain.bean.MstPrinterManage" >
      INSERT INTO sys_printer
        (
           wh_sys_no,wh_no,wh_name,
           printer_no,printer_desc,address,print_node,client_ip,comments,yn,
           create_time,create_user,update_time,update_user
        )
        select t1.wh_sys_no,t1.wh_no,t1.wh_name,
        #{printerNo},#{printerDesc},#{address},#{printNode},#{clientIp},#{comments},#{yn},
        NOW(),#{createUser},NOW(),#{updateUser}
         
        FROM (SELECT sys_no AS wh_sys_no,wh_no,wh_name FROM mst_wh WHERE sys_no = #{whSysNo}) t1
        <selectKey keyProperty="sysNo" order="AFTER" resultType="Long">
            select last_insert_id() as sys_no
        </selectKey>
  </insert>
  <update id="updateByPrimaryKeySelective" parameterType="com.haiziwang.kwms.common.domain.bean.MstPrinterManage" >
    update sys_printer
    <set >
      <if test="whSysNo != null" >
          wh_sys_no = #{whSysNo,jdbcType=VARCHAR},
          wh_no = (select t1.wh_no from (SELECT sys_no AS wh_sys_no,wh_no,wh_name FROM mst_wh WHERE sys_no = #{whSysNo}) t1),
        wh_name = (select t1.wh_name from (SELECT sys_no AS wh_sys_no,wh_no,wh_name FROM mst_wh WHERE sys_no = #{whSysNo}) t1),
      </if>
      <if test="printerNo != null" >
        printer_no = #{printerNo,jdbcType=VARCHAR},
      </if>
      <if test="printerDesc != null" >
        printer_desc = #{printerDesc,jdbcType=VARCHAR},
      </if>
      <if test="address != null" >
        address = #{address,jdbcType=VARCHAR},
      </if>
      <if test="printNode != null" >
        print_Node = #{printNode,jdbcType=INTEGER},
      </if>
      <if test="clientIp != null" >
        client_ip = #{clientIp,jdbcType=VARCHAR},
      </if>   
      <if test="comments != null" >
        comments = #{comments,jdbcType=VARCHAR},
      </if>
      <if test="yn != null" >
        yn = #{yn,jdbcType=INTEGER},
      </if>
      <if test="createUser != null" >
        create_user = #{createUser,jdbcType=VARCHAR},
      </if>
      <if test="updateUser != null" >
        update_user = #{updateUser,jdbcType=VARCHAR},
      </if>
      update_time = now()
    </set>
    where sys_no = #{sysNo,jdbcType=BIGINT}
  </update> </mapper>   

```
PrinterManageBusinessService .java

```
package  com.haiziwang.kwms.outbound.business;
import  java.lang.reflect.InvocationTargetException;
import  org.springframework.transaction.annotation.Transactional;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageListResDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageReqDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageResDto;
public   interface  PrinterManageBusinessService
{
     /**
     *  @param  reqDto 请求参数对象
     *  @return
     *  @throws  NoSuchMethodException
     *  @throws  InvocationTargetException
     *  @throws  IllegalAccessException
     *  @desc  根据查询条件查询列表
     */
     @ Transactional
     public  MstPrinterManageListResDto selectMstPrinterManageByList(MstPrinterManageReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException;
     /**
     *  @param  reqDto
     *  @return
     *  @throws  NoSuchMethodException
     *  @throws  InvocationTargetException
     *  @throws  IllegalAccessException
     *  @desc  查询纪录
     */
     @ Transactional
     public  MstPrinterManageResDto selectMstPrinterManage(MstPrinterManageReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException;
     /**
     *  @param  reqDto
     *  @throws  NoSuchMethodException
     *  @throws  InvocationTargetException
     *  @throws  IllegalAccessException
     *  @desc  新增新纪录
     */
     @ Transactional
     public   void  addMstPrinterManage(MstPrinterManageReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException;
     /**
     *  @param  reqDto
     *  @return
     *  @throws  NoSuchMethodException
     *  @throws  InvocationTargetException
     *  @throws  IllegalAccessException
     *  @desc  新增新纪录
     */
     @ Transactional
     public   void  delMstPrinterManage(MstPrinterManageReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException;
     /**
     *  @param  reqDto
     *  @return
     *  @throws  NoSuchMethodException
     *  @throws  InvocationTargetException
     *  @throws  IllegalAccessException
     *  @desc  更新纪录
     */
     @ Transactional
     public   void  updateMstPrinterManage(MstPrinterManageReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException;
     @ Transactional
     public  Integer selectMstPrinterManageByCount(MstPrinterManageReqDto reqDto);
}
```
PrinterManageBusinessServiceImpl.java

```
package  com.haiziwang.kwms.outbound.business.impl;
import  java.lang.reflect.InvocationTargetException;
import  java.util.ArrayList;
import  java.util.List;
import  org.apache.commons.beanutils.PropertyUtils;
import  org.springframework.beans.factory.annotation.Autowired;
import  org.springframework.stereotype.Service;
import  org.springframework.transaction.annotation.Transactional;
import  com.haiziwang.kwms.common.domain.bean.MstCarrier;
import  com.haiziwang.kwms.common.domain.bean.MstPrinterManage;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageListResDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageReqDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageResDto;
import  com.haiziwang.kwms.common.domain.enums.CarrierStatusEnum;
import  com.haiziwang.kwms.common.domain.enums.PrintNodeEnum;
import  com.haiziwang.kwms.common.domain.enums.ResultStateEnum;
import  com.haiziwang.kwms.common.domain.exception.WMS3UnCheckedException;
import  com.haiziwang.kwms.outbound.business.PrinterManageBusinessService;
import  com.haiziwang.kwms.outbound.dao.MstPrinterManageDao;
@ Service( "PrinterManageBusinessService" )
public   class  PrinterManageBusinessServiceImpl  implements  PrinterManageBusinessService
{
     @ Autowired
     private  MstPrinterManageDao mstPrinterManageDao;
     @ Override
     public  MstPrinterManageListResDto selectMstPrinterManageByList(MstPrinterManageReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException
    {
        MstPrinterManageListResDto resDto =  new  MstPrinterManageListResDto();
        MstPrinterManage mstPrinterManage =  new  MstPrinterManage();
        PropertyUtils.copyProperties(mstPrinterManage, reqDto);
         int  cont = mstPrinterManageDao.selectMstPrinterManageDaoByCont(mstPrinterManage); //根据条件查询总记录数
        List<MstPrinterManage> list = mstPrinterManageDao.selectMstPrinterManageByList(mstPrinterManage);
        
         if  (list !=  null  && !list.isEmpty())
        {
             for  (MstPrinterManage printer : list)
            {
                printer.setPrintNodeName(PrintNodeEnum.getByCode(printer.getPrintNode()).getName());
            }
            resDto.setResultCode(String.valueOf(ResultStateEnum.SUCCESS.getCode()));
            resDto.setResultMsg(ResultStateEnum.SUCCESS.getDesc());
            resDto.setList(list);
            resDto.setTotalCont(cont);
        }
         else
        {
            resDto.setResultCode(String.valueOf(ResultStateEnum.NO_RECORD.getCode()));
            resDto.setResultMsg(ResultStateEnum.NO_RECORD.getDesc());
            resDto.setTotalCont(0);
            resDto.setTotalPage(0);
            resDto.setList( new  ArrayList<MstPrinterManage>());
        }
         return  resDto;
    }
     @ Override
     public  Integer selectMstPrinterManageByCount(MstPrinterManageReqDto reqDto)
    {
        MstPrinterManage mstPrinterManage =  new  MstPrinterManage();
        mstPrinterManage.setPrinterNo(reqDto.getPrinterNo());
         return  mstPrinterManageDao.selectMstPrinterManageDaoByCont(mstPrinterManage);
    }
     @ Override
     public  MstPrinterManageResDto selectMstPrinterManage(MstPrinterManageReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException {
        MstPrinterManageResDto resDto =  new  MstPrinterManageResDto();
        MstPrinterManage mstPrinterManage =  new  MstPrinterManage();
        PropertyUtils.copyProperties(mstPrinterManage, reqDto);
        mstPrinterManage = mstPrinterManageDao.queryOneBySysNo(mstPrinterManage);
        PropertyUtils.copyProperties(resDto, mstPrinterManage);
         return  resDto;
    }
    
     @ Override
     @ Transactional
     public   void  addMstPrinterManage(MstPrinterManageReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException, WMS3UnCheckedException
    {
        MstPrinterManage mstPrinterManage =  new  MstPrinterManage();
        
        mstPrinterManage.setPrinterNo(reqDto.getPrinterNo());
        mstPrinterManage.setAddress(reqDto.getAddress());
        mstPrinterManage.setWhSysNo(reqDto.getWhSysNo());
        
         int  n = mstPrinterManageDao.selectMstPrinterManageDaoByCont(mstPrinterManage);
         if  (n > 0)
        {
             throw   new  WMS3UnCheckedException(ResultStateEnum.FAIL.getCode().toString(),  "重复的编码" );
        }
        PropertyUtils.copyProperties(mstPrinterManage, reqDto);
        
        mstPrinterManageDao.addMstPrinterManage(mstPrinterManage);
    }
     @ Override
     @ Transactional
     public   void  delMstPrinterManage(MstPrinterManageReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException
    {
    }
     @ Override
     @ Transactional
     public   void  updateMstPrinterManage(MstPrinterManageReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException
    {
        MstPrinterManage mstPrinterManage =  new  MstPrinterManage();
        PropertyUtils.copyProperties(mstPrinterManage, reqDto);
        mstPrinterManageDao.updateMstPrinterManageBySynNo(mstPrinterManage);
    }
}
```
MstPrinterManageService.java

```
package  com.haiziwang.kwms.outbound.service;
import  org.springframework.transaction.annotation.Transactional;
import  com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyReqDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyResDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageListResDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageReqDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageResDto;
public   interface  MstPrinterManageService
{
     /**
     *  @param  reqDto 请求参数对象
     *  @return
     *  @desc  根据查询条件查询列表
     */
     @ Transactional
     public  MstPrinterManageListResDto selectMstPrinterManageByList(MstPrinterManageReqDto reqDto);
     /**
     *  @param  reqDto
     *  @return  
     *  @desc  查询纪录
     */
     @ Transactional
     public  MstPrinterManageResDto selectMstPrinterManage(MstPrinterManageReqDto reqDto);
    
     /**
     *  @param  reqDto
     *  @return  
     *  @desc  新增新纪录
     */
     @ Transactional
     public  MstPrinterManageResDto addMstPrinterManage(MstPrinterManageReqDto reqDto);
     /**
     *  @param  reqDto
     *  @return  
     *  @desc  删除纪录
     */
     @ Transactional
     public  MstPrinterManageResDto delMstPrinterManage(MstPrinterManageReqDto reqDto);
     /**
     *  @param  reqDto
     *  @return  
     *  @desc  更新纪录
     */
     @ Transactional
     public  MstPrinterManageResDto updateMstPrinterManage(MstPrinterManageReqDto reqDto);
}
```
MstPrinterManageServiceImpl.java

```
package  com.haiziwang.kwms.outbound.service.impl;
import  java.lang.reflect.InvocationTargetException;
import  org.springframework.beans.factory.annotation.Autowired;
import  org.springframework.stereotype.Service;
import  org.springframework.transaction.annotation.Transactional;
import  com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyResDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageListResDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageReqDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageResDto;
import  com.haiziwang.kwms.common.domain.enums.ResultStateEnum;
import  com.haiziwang.kwms.common.domain.enums.WMS3ExceptionCode;
import  com.haiziwang.kwms.common.domain.exception.WMS3UnCheckedException;
import  com.haiziwang.kwms.outbound.business.PrinterManageBusinessService;
import  com.haiziwang.kwms.outbound.service.MstPrinterManageService;
import  com.haiziwang.kwms.outbound.service.SysUserService;
@ Service( "mstPrinterManageService" )
public   class  MstPrinterManageServiceImpl  implements  MstPrinterManageService
{
     @ Autowired
     private  PrinterManageBusinessService printerManageBusinessService;
    
     @ Autowired
     private  SysUserService sysUserService;
    
     @ Override
     public  MstPrinterManageListResDto selectMstPrinterManageByList(MstPrinterManageReqDto reqDto)
    {
        MstPrinterManageListResDto resDto =  null ;
        
         try
        {
            resDto = printerManageBusinessService.selectMstPrinterManageByList(reqDto);
            
        }
         catch  (WMS3UnCheckedException e)
        {
            resDto =  new  MstPrinterManageListResDto();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + e.getCode() +  "]"  + e.getOutMsg());
            resDto.setTotalCont(0);
            resDto.setTotalPage(0);
        }
         catch  (IllegalAccessException e)
        {
            resDto =  new  MstPrinterManageListResDto();
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.LEGAL_ACCESS_EXPT.getCode() +  "]"  + WMS3ExceptionCode.LEGAL_ACCESS_EXPT.getDesout());
        }
         catch  (InvocationTargetException e)
        {
            resDto =  new  MstPrinterManageListResDto();
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getCode() +  "]"  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getDesout());
        }
         catch  (NoSuchMethodException e)
        {
            resDto =  new  MstPrinterManageListResDto();
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getCode() +  "]"  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getDesout());
        }
         return  resDto;
    }
    
     @ Override
     public  MstPrinterManageResDto selectMstPrinterManage(MstPrinterManageReqDto reqDto) {
        MstPrinterManageResDto resDto =  new  MstPrinterManageResDto();
         try
        {
            resDto = printerManageBusinessService.selectMstPrinterManage(reqDto);
            resDto.setResultCode(String.valueOf(ResultStateEnum.SUCCESS.getCode()));
            resDto.setResultMsg(ResultStateEnum.SUCCESS.getDesc());
        }
         catch  (WMS3UnCheckedException e)
        {
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + e.getCode() +  "]"  + e.getOutMsg());
        }
         catch  (IllegalAccessException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.LEGAL_ACCESS_EXPT.getCode() +  "]"  + WMS3ExceptionCode.LEGAL_ACCESS_EXPT.getDesout());
        }
         catch  (InvocationTargetException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getCode() +  "]"  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getDesout());
        }
         catch  (NoSuchMethodException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getCode() +  "]"  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getDesout());
        }
         return  resDto;
    
    }
     @ Override
     public  MstPrinterManageResDto addMstPrinterManage(MstPrinterManageReqDto reqDto)
    {
        MstPrinterManageResDto resDto =  new  MstPrinterManageResDto();
         try
        {
            reqDto.setCreateUser(sysUserService.getCurrentUser().getUsername());
            reqDto.setUpdateUser(sysUserService.getCurrentUser().getUsername());
            reqDto.setYn(1);
            printerManageBusinessService.addMstPrinterManage(reqDto);
            resDto.setResultCode(String.valueOf(ResultStateEnum.SUCCESS.getCode()));
            resDto.setResultMsg(ResultStateEnum.SUCCESS.getDesc());
        }
         catch  (WMS3UnCheckedException e)
        {
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + e.getCode() +  "]"  + e.getOutMsg());
        }
         catch  (IllegalAccessException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.LEGAL_ACCESS_EXPT.getCode() +  "]"  + WMS3ExceptionCode.LEGAL_ACCESS_EXPT.getDesout());
        }
         catch  (InvocationTargetException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getCode() +  "]"  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getDesout());
        }
         catch  (NoSuchMethodException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getCode() +  "]"  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getDesout());
        }
         return  resDto;
    }
     @ Override
     @ Transactional
     public  MstPrinterManageResDto delMstPrinterManage(MstPrinterManageReqDto reqDto)
    {
        MstPrinterManageResDto resDto =  new  MstPrinterManageResDto();
         try
        {
            printerManageBusinessService.delMstPrinterManage(reqDto);
            resDto.setResultCode(String.valueOf(ResultStateEnum.SUCCESS.getCode()));
            resDto.setResultMsg(ResultStateEnum.SUCCESS.getDesc());
        }
         catch  (WMS3UnCheckedException e)
        {
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + e.getCode() +  "]"  + e.getOutMsg());
        }
         catch  (IllegalAccessException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.LEGAL_ACCESS_EXPT.getCode() +  "]"  + WMS3ExceptionCode.LEGAL_ACCESS_EXPT.getDesout());
        }
         catch  (InvocationTargetException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getCode() +  "]"  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getDesout());
        }
         catch  (NoSuchMethodException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getCode() +  "]"  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getDesout());
        }
         return  resDto;
    }
     @ Override
     @ Transactional
     public  MstPrinterManageResDto updateMstPrinterManage(MstPrinterManageReqDto reqDto)
    {
        MstPrinterManageResDto resDto =  new  MstPrinterManageResDto();
         try
        {
            reqDto.setCreateUser(sysUserService.getCurrentUser().getUsername());
            reqDto.setUpdateUser(sysUserService.getCurrentUser().getUsername());
            printerManageBusinessService.updateMstPrinterManage(reqDto);
            resDto.setResultCode(String.valueOf(ResultStateEnum.SUCCESS.getCode()));
            resDto.setResultMsg(ResultStateEnum.SUCCESS.getDesc());
        }
         catch  (WMS3UnCheckedException e)
        {
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + e.getCode() +  "]"  + e.getOutMsg());
        }
         catch  (IllegalAccessException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.LEGAL_ACCESS_EXPT.getCode() +  "]"  + WMS3ExceptionCode.LEGAL_ACCESS_EXPT.getDesout());
        }
         catch  (InvocationTargetException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getCode() +  "]"  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getDesout());
        }
         catch  (NoSuchMethodException e)
        {
            e.printStackTrace();
            resDto.setResultCode(String.valueOf(ResultStateEnum.FAIL.getCode()));
            resDto.setResultMsg( "["  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getCode() +  "]"  + WMS3ExceptionCode.INVACTION_TARGET_EXPT.getDesout());
        }
         return  resDto;
    }
}
```
MstPrinterManageController.java

```
package  com.haiziwang.kwms.outbound.controller;
import  java.util.ArrayList;
import  java.util.List;
import  org.slf4j.Logger;
import  org.slf4j.LoggerFactory;
import  org.springframework.beans.factory.annotation.Autowired;
import  org.springframework.web.bind.annotation.RequestBody;
import  org.springframework.web.bind.annotation.RequestMapping;
import  org.springframework.web.bind.annotation.RequestMethod;
import  org.springframework.web.bind.annotation.RequestParam;
import  org.springframework.web.bind.annotation.RestController;
import  com.alibaba.fastjson.JSON;
import  com.haiziwang.kwms.common.domain.bean.EnumBean;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageListResDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageReqDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstPrinterManageResDto;
import  com.haiziwang.kwms.common.domain.enums.PrintNodeEnum;
import  com.haiziwang.kwms.common.domain.util.PageRespJson;
import  com.haiziwang.kwms.common.domain.util.RespJson;
import  com.haiziwang.kwms.common.domain.util.StringUtils;
import  com.haiziwang.kwms.outbound.service.MstPrinterManageService;
/**
 * Copyright: 2016 Haiziwang
 * *
 * Desc:    打印机管理 对外接口
 */
@ RestController
public   class  MstPrinterManageController {
    
     private   static   final  Logger logger = LoggerFactory.getLogger(MstAllocationStrategyController. class );
    
     @ Autowired
     private  MstPrinterManageService mstPrinterManageService;
    
     /**
     * 查看打印机
     *  @param  draw
     *  @param  start
     *  @param  rows
     *  @param  printerNo
     *  @param  whSysNo
     *  @param  printNode
     *  @param  printerDesc
     *  @param  clientIp
     *  @return
     */
     @ RequestMapping(value =  "/mst-printer-manage/list" , method = RequestMethod.GET)
     public  PageRespJson getPrinterList( @ RequestParam( "draw" )  int  draw,
                                                @ RequestParam( "start" )  int  start,
                                                @ RequestParam( "rows" )  int  rows,
                                                @ RequestParam(value =  "printerNo" , required =  false ) String printerNo,
                                                @ RequestParam(value =  "whSysNo" , required =  false ) String whSysNo,
                                                @ RequestParam(value =  "printNode" , required =  false ) Integer printNode,
                                                @ RequestParam(value =  "printerDesc" , required =  false ) String printerDesc,
                                                @ RequestParam(value =  "clientIp" , required =  false ) String clientIp) {
        logger.info( "查询打印机管理列表" );
    
        MstPrinterManageReqDto reqDto =  new  MstPrinterManageReqDto(draw, start, rows);
        reqDto.setPrinterNo(printerNo);
        reqDto.setWhSysNo(whSysNo);
        reqDto.setPrintNode(printNode);
        reqDto.setPrinterDesc(printerDesc);
        reqDto.setClientIp(clientIp);
        reqDto.setStart(start);
        reqDto.setRows(rows);
        
        MstPrinterManageListResDto mstPrinterManageListResDto =  null ;
        
         try  {
            mstPrinterManageListResDto = mstPrinterManageService.selectMstPrinterManageByList(reqDto);
        }  catch  (Exception e) {
             return  PageRespJson.buildFailureResponse(StringUtils.getValidString(e.getMessage()), draw);
        }
         if  (mstPrinterManageListResDto ==  null )
            mstPrinterManageListResDto =  new  MstPrinterManageListResDto();
         return  PageRespJson.buildSuccessResponse(mstPrinterManageListResDto.getList(), draw, mstPrinterManageListResDto.getTotalCont());
    }
    
     /**
     * 增加新的打印机
     *  @param  reqDto  增加新的打印机
     *  @return  创建成功/失败的结果
     */
     @ RequestMapping(value =  "/mst-printer-manage/add" , method = RequestMethod.POST, headers = {
             "Accept=application/json; charset=UTF-8" ,  "Content-Type=application/json" })
     public  RespJson addPrinter( @ RequestBody MstPrinterManageReqDto reqDto) {
        
        logger.info( "增加新的打印机："  +  JSON.toJSONString(reqDto));
        
         try  
        {
            MstPrinterManageResDto resDto = mstPrinterManageService.addMstPrinterManage(reqDto);
             return  RespJson.buildResponseFromResDto(resDto);
        }  catch  (Exception e) {
             return  RespJson.buildFailureResponse(StringUtils.getValidString(e.getMessage()));
        }
    }
    
     /**
     * 更新打印机信息
     *  @param  reqDto  更新打印机
     *  @return  更新成功/失败的结果
     */
     @ RequestMapping(value =  "/mst-printer-manage/update" , method = RequestMethod.POST, headers = {
             "Accept=application/json; charset=UTF-8" ,  "Content-Type=application/json" })
     public  RespJson updateCarrier( @ RequestBody MstPrinterManageReqDto reqDto) {
        logger.info( "修改打印机："  + JSON.toJSONString(reqDto));
         try  
        {
            MstPrinterManageResDto resDto = mstPrinterManageService.updateMstPrinterManage(reqDto);
             return  RespJson.buildResponseFromResDto(resDto);
        }  catch  (Exception e) {
             return  RespJson.buildFailureResponse(StringUtils.getValidString(e.getMessage()));
        }
    }
   
     /**
     * 查询单条打印机信息
     *  @param  sysNo  主键
     *  @return
     */
     @ RequestMapping(value =  "/mst-printer-manage/info" , method = RequestMethod.GET)
     public  RespJson getCarrierInfo( @ RequestParam( "id" )  long  sysNo) {
        logger.info( "查询单条 打印机：sysNo = "  + sysNo);
        MstPrinterManageReqDto reqDto =  new  MstPrinterManageReqDto();
        reqDto.setSysNo(sysNo);
         try  {
            MstPrinterManageResDto resDto = mstPrinterManageService.selectMstPrinterManage(reqDto);
             return  RespJson.buildSuccessResponse(resDto);
        }  catch  (Exception e) {
             return  RespJson.buildFailureResponse(StringUtils.getValidString(e.getMessage()));
        }
    }
    
     /**
     * 获取打印环节列表
     *
     *  @return  符合条件的信息集
     */
     @ RequestMapping(value =  "/mst-printer-manage/printNodeList" , method = RequestMethod.GET)
     public  RespJson selectPrintNodeList() {
        List<EnumBean> enumValueList =  new  ArrayList<EnumBean>();
         for  (PrintNodeEnum printNodeEnum : PrintNodeEnum.values()) {
            EnumBean enumBean =  new  EnumBean();
            enumBean.setKey(String.valueOf(printNodeEnum.getCode()));
            enumBean.setValue(printNodeEnum.getName());
            enumValueList.add(enumBean);
        }
         return  RespJson.buildSuccessResponse(enumValueList);
    }
}
```




PrintNodeEnum.java

```
package  com.haiziwang.kwms.common.domain.enums;
/**
 * Desc:    打印环节
 */
public   enum  PrintNodeEnum {
     // 1 发票；2 出库单； 3 交接单；4 收货单; 5 箱贴  ;6 预检单 ; 7 验收单 ;20 顺丰面贴 ;21 EMS面贴 ;22 妈妈团面贴
    INVOICE(1,  "发票" ),
    OUTBOUND_ORDER(2,  "出库单" ),
    TRANSFER_ORDER(3,  "交接单" ),    
    RECEIPT_ORDER(4,  "收货单" ),
    BOX_PASTE(5,  "箱贴" ),
    PREVIEW_ORDER(6,  "预检单" ),
    CHECK_ORDER(7,  "验收单" ),
    SF_SURFACEMOUNT(20,  "顺丰面贴" ),
    EMS_SURFACEMOUNT(21,  "EMS面贴" ),
    MMT_SURFACEMOUNT(22,  "妈妈团面贴" ),;
     /**
     * 状态编码
     */
     private  Integer code;
     /**
     * 状态名称
     */
     private  String name;
     /**
     * 构造函数
     */
    PrintNodeEnum(Integer code, String name) {
         this .code = code;
         this .name = name;
    }
     /**
     * 获得 code
     */
     public  Integer getCode() {
         return  code;
    }
     /**
     * 设置 code
     */
     public   void  setCode(Integer code) {
         this .code = code;
    }
     /**
     * 获得 name
     */
     public  String getName() {
         return  name;
    }
     /**
     * 设置 name
     */
     public   void  setName(String name) {
         this .name = name;
    }
    
    
     /**
     * 通过code获取枚举
     *
     *  @param  code
     */
     public   static  PrintNodeEnum getByCode(Integer code) {
         for  (PrintNodeEnum status : values()) {
             if  (status.getCode().intValue() == code) {
                 return  status;
            }
        }
         return   null ;
    }
}
```


















































##9.29以后的版本


printer-manager-service.js
新加：  getPrinterNo: function (params) {
                return restProxyService.sendHttpPost(WMS_API_PROVIDER, WMS_API_END_POINT + '/mst-printer-manage/printerNo', params);
  },


```
/**
              *   查询打印环节   
              */
            getPrintNodeList:  function  (params) {
                 return  restProxyService.sendHttpGet(WMS_API_PROVIDER, WMS_API_END_POINT +  '/mst-printer-manage/printNodeList' , params);             }  
```


 

```
/**
  *   Desc:      获取本机Ip
  */
wmsApp.factory( 'queryPrintInfoService' , [ 'restProxyService' ,  'WMS_API_PROVIDER' ,  'WMS_API_END_POINT' ,
     function  (restProxyService, WMS_API_PROVIDER, WMS_API_END_POINT) {
         /**
          *   函数列表
          */
         return  {
            queryHostIp:  function  () { 
                 return  restProxyService.sendHttpGet(WMS_API_PROVIDER, WMS_API_END_POINT +  '/sys-print-data/ip' );
            },
            queryWhNo:  function  () { 
                 return  restProxyService.sendHttpGet(WMS_API_PROVIDER, WMS_API_END_POINT +  '/sys-print-data/whNo' );
            }
        }
    } ]);   

```