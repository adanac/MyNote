##时间 比较大小
```
var beginInterval   = $( "#hourAroud1" ).val(); //开始时间段
var endInterval     = $( "#hourAroud2" ).val();
            
var  d1 =  new  Date(beginInterval.replace(/\-/g,  "\/" ));
var  d2 =  new  Date(endInterval.replace(/\-/g,  "\/" ));
if  (beginInterval !=  ""  && endInterval !=  ""  && d1 >= d2) {
     alert( "开始时间不能大于结束时间！" );
      return   false ; }   

``` ##数字 比较大小
```
var  uselimit  = +$( "input[name='uselimit']" ).val(); //商品购买总数量 var  buylimit  = +$( "input[name='buylimit']" ).val();   

如果不加 + ,则会出现 100 < 2 的bug
如果加上 + ,则会出现 如果uselimit 填写为0，则 以下判断也会弹出alert.
if ( uselimit != 0 && uselimit ==  "" ){
    alert( "请输入商品购买总数量限制" );
    return ; }   



解决方法： 不用+来处理数字，用parseFloat方法
js提供了parseInt()和parseFloat()两个转换函数。前者把值转换成整数，后者把值转换成浮点数。只有对String类型调用这些方法，这两个函数才能正确运行；对其他类型返回的都是NaN(Not a Number)。

parseInt("1234blue"); //returns 1234
parseInt("0xA"); //returns 10
parseInt("22.5"); //returns 22
parseInt("blue"); //returns NaN



parseInt()方法还有基模式，可以把二进制、八进制、十六进制或其他任何进制的字符串转换成整数。基是由parseInt()方法的第二个参数指定的，示例如下：

parseInt("AF", 16); //returns 175
parseInt("10", 2); //returns 2
parseInt("10", 8); //returns 8
parseInt("10", 10); //returns 10
如果十进制数包含前导0，那么最好采用基数10，这样才不会意外地得到八进制的值。例如：
parseInt("010"); //returns 8
parseInt("010", 8); //returns 8
parseInt("010", 10); //returns 10



parseFloat()方法与parseInt()方法的处理方式相似。
使用parseFloat()方法的另一不同之处在于，字符串必须以十进制形式表示浮点数，parseFloat()没有基模式。

下面是使用parseFloat()方法的示例：
parseFloat("1234blue"); //returns 1234.0
parseFloat("0xA"); //returns NaN
parseFloat("22.5"); //returns 22.5
parseFloat("22.34.5"); //returns 22.34
parseFloat("0908"); //returns 908
parseFloat("blue"); //returns NaN


来源：  http://blog.csdn.net/dxnn520/article/details/8267173
```