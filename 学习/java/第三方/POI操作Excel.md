[TOC]
##解决excel数据过长，展示 为E的形式
```
value = cell.getNumericCellValue();
if((value+"").contains("E")){
    value = new DecimalFormat("#").format(cell.getNumericCellValue());
}
```
##解决导出excel，如果数字过长，比如20930001113导到excel会显示2E.+11
```
输入流输出时加上一个"\t"即可。

private  String wirteFile(String  basePath , String  param , List<Map<String, Object>>  logList , String  typeName ,
            String  diffType ) {
        String  exportFilePath  =  "" ;
        File  csvFile  =  null ;
        BufferedWriter  csvFileOutputStream  =  null ;
         try  {
            String  title  =  "" ;  // 标题
            String  fileName  =  "" ;
            SimpleDateFormat  sdf  =  new  SimpleDateFormat( "yyyyMMddHHmmss" );
            String  timeStr  =  sdf .format( new  Date());
             basePath  =  basePath  +  "exportFile"  + File. separator ;
             // 如果文件夹不存在则创建
            File  file  =  new  File( basePath );
             if  (! file .exists()) {
                 file .mkdirs();
            }
             // 判断所属参数，执行对应的处理方法
             if  (ReportConstants. PARAM_DEPTSUPPDIFF .equals( param )) {  // 部门供应商差异报表
                 fileName  = ReportConstants. PARAM_DEPTSUPPDIFFTEMP ;
                 if  ( diffType .equals(OmcConstants.strNum. STR_0 )) {
                     fileName  = ReportConstants. PARAM_DEPTUNDEFINE  +  fileName ;
                }  else  {
                     fileName  = ReportConstants. PARAM_DEPTCANCEL  +  fileName ;
                }
                 title  =  "部门编码[deptCode],部门名称[deptName],供应商编码[suppCode],供应商名称[suppName],物流模式[tranMode],差异类型[diffType]" ;
            }  else   if  (ReportConstants. PARAM_DEPTGOODDIFF .equals( param )) {  // 部门商品差异报表
                 fileName  = ReportConstants. PARAM_DEPTGOODDIFFTEMP ;
                 if  ( diffType .equals(OmcConstants.strNum. STR_0 )) {
                     fileName  = ReportConstants. PARAM_DEPTUNDEFINE  +  fileName ;
                }  else  {
                     fileName  = ReportConstants. PARAM_DEPTCANCEL  +  fileName ;
                }
                 title  =  "部门编码[deptCode],部门名称[deptName],商品编码[goodCode],商品名称[goodName],差异类型[diffType],物流模式[tranMode],配送中心[dcCode],经营状态编码[operStaCode],经营状态名称[operStaName],供应商编码[suppCode],供应商名称[suppName],合同号[contractNo],建档日期[filDate]" ;
            }
             fileName  =  fileName  + ReportConstants. UNDERLINE  +  timeStr ;
             exportFilePath  =  basePath  +  fileName  + ReportConstants. DOT  +  typeName ;
             // 定义文件名格式并创建
             // csvFile = File.createTempFile(fileName, ".csv",new
             // File(basePath));
             csvFile  =  new  File( exportFilePath );
             csvFile .createNewFile();
             exportFilePath  =  csvFile .getPath();
             csvFileOutputStream  =  new  BufferedWriter( new  OutputStreamWriter( new  FileOutputStream( csvFile ),  "GB2312" ),
                    1024);
             // 写入文件头部
             csvFileOutputStream .write( title );
             csvFileOutputStream .newLine();
             // 拆分title
            String[]  titleArray  =  title .split( "," );
             // 写入文件内容
             for  ( int   i  = 0;  i  <  logList .size();  i ++) {
                Map<String, Object>  map  =  logList .get( i );
                 for  ( int   icount  = 0;  icount  <  titleArray . length ;  icount ++) {
                    String  enname  =  this .getEnname( titleArray [ icount ]);  // 获取对应的表头信息
                    String  val  =  map .get( enname ) ==  null  ?  ","  : ( map .get( enname ).toString().trim() +  "," );
                     // 判断是不是以"0"开头的，如果是则在前面添加一个"'"
                     val  =  this .startWith0( val );
                     if  ( icount  == 11) {
                         csvFileOutputStream .write( "\t"  +  val );
                    }  else  {
                         csvFileOutputStream .write( val );
                    }
                }
                 csvFileOutputStream .newLine();
            }
             csvFileOutputStream .flush();
        }  catch  (Exception  e ) {
             log .error( "ExportServiceImpl==>wirteCvs" ,  new  Object[] {  e .getMessage() });
        }  finally  {
             try  {
                 csvFileOutputStream .close();
            }  catch  (IOException  e ) {
                 log .error( "ExportServiceImpl==>wirteCvs" ,  new  Object[] {  e .getMessage() });
            }
        }
         return   exportFilePath ;     }

```

##遇到bug

```
对于操作Excel2007及以上的版本，要用到XSSFWorkbook要引入poi-ooxml-*.jar才可以。
提示 classnotfound: org.apache.xmlbeans.XmlObject
Caused by: java.lang.ClassNotFoundException: org.dom4j.DocumentException
导入 xmlbeans-2.3.0.jar org.dom4j
Caused by: org.apache.poi.openxml4j.exceptions.InvalidFormatException: Package should contain a content type part [M1.13]
{原因：要操作.xlsx的文件才可以}

```

##关于Excel2007以下的版本

``` 
cannot get a text value from a numeric cell
此异常常见于类似如下代码中：row.getCell(0).getStringCellValue()；
解决办法：先设置Cell的类型，然后就可以把纯数字作为String类型读进来了：
if(row.getCell(0)!=null){
      row.getCell(0).setCellType(Cell.CELL_TYPE_STRING);
      stuUser.setPhone(row.getCell(0).getStringCellValue());
}
```
##IE8上传文件时javascript读取文件的本地路径的问题（"C:/fakepath/"）的解决方案
```
因为IE8增加了安全选项，默认情况下不显示上传文件的真实路径，进入internet选项，修改下设置即可显示真实的文件路径。
一、浏览器选项设置方法
工具 -> Internet选项 -> 安全 -> 自定义级别 -> 找到“其他”中的“将本地文件上载至服务器时包含本地目录路径”，选中“启用”即可。
二、程序改进方法：
<script type="text/javascript" src="../js/jquery-1.8.3.js"></script>
<script type="text/javascript"> 
$(document).ready(function (){
                //按钮点击发送
                $("#loadbutton").click(function (){
                    // get path 
                    var filepath = load();
                    alert(filepath);
                    // 传值
                    /*$.post('http://localhost:8080/POITest/servlet/process.do',
                    {
                        name : "test",
                    },function(data){
                        alert(data);
                    });*/
                    $.ajax({
                        type:"post",
                        url:"http://localhost:8080/POITest/servlet/process.do",
                        data:"filepath="+filepath,
                        success:function(){
                            alert("数据处理成功!");
                        }
                    });
                });
            });
    function load(){
        alert("begin");
        var filepath = getPath(document.getElementById("selectfile1"));
        var selectfile1 = $("#selectfile1").val();
        return filepath;
    }
    function getPath(obj){
        if (obj){
            if (window.navigator.userAgent.indexOf("MSIE") >= 1){
                obj.select();
                return document.selection.createRange().text;
            }
            else if (window.navigator.userAgent.indexOf("Firefox") >= 1) {
                if (obj.files) {
                    return obj.files.item(0).getAsDataURL();
                }
                return obj.value;
            }
            return obj.value;
        }
    }
</script>
选择文件：<input type="file" name="myexcel" id="selectfile1" /><br/>
执行按钮：<input type="button" value="执行" id="loadbutton" /><br/>
```




