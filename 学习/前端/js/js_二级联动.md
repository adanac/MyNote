[TOC]
###简单的二级联动 
```
<!doctype html>
<html lang="en">
 <head>
  <meta charset="UTF-8">
  <title>二级联动</title>
 </head>
 <body>
  <script type="text/javascript">
    var city=[
        ["南京市","苏州市","南通市","常州市"],
        ["长沙市","株洲市","湘潭市","衡阳市"] 
    ];    
    function getCity(){    
        //获得省份下拉框的对象    
        var sltProvince=document.dialog_form.province;  
        //获得城市下拉框的对象    
        var sltCity=document.dialog_form.city;    
        //得到对应省份的城市数组    
        var provinceCity=city[sltProvince.selectedIndex - 1];    
        //清空城市下拉框，仅留提示选项 
        sltCity.length=1;    
        //将城市数组中的值填充到城市下拉框中    
        for(var i=0;i<provinceCity.length;i++){    
        sltCity[i+1]=new Option(provinceCity[i],provinceCity[i]);    
        }    
    }
 </script>
 <form  name="dialog_form">
    <select name="province" onChange="getCity()">
        <option value="0">请选择所在省份</option>
        <option value="1">江苏省</option>
        <option value="2">湖南省</option>
    </select>

    <select name="city">    
        <option VALUE="0">请选择所在城市</option>
    </select>
 </form>
 </body>
</html>
```