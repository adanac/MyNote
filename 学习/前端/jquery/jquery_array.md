##获取页面table组装成list，传到后台
```
//参与实体列表信息
     function  getEntityList(){
         var  resList = [];
         var  tag = $( 'input[name="disType"]:checked' ).val();
         var  trs = $( "#skuTable" +tag+ " tbody tr" );
        console.log( 'trs:' +trs.length);
        
         //添加的参与实体
         for ( var  i=1; i<trs.length; i++){
             var  res = {};
             var  storeCode = $(trs[i]).children().first().find( 'input' ).first().val();
             var  saleWay = $(trs[i]).children().eq(1).find( 'select' ).first().val();
             var  suppName = $(trs[i]).children().eq(2).html();
             var  skuPrice = $(trs[i]).children().eq(3).html();
             var  setPrice = $(trs[i]).children().eq(4).find( 'input' ).first().val();
            
            res.storeCode = storeCode;
            res.saleWay = saleWay;
            res.suppName = suppName;
            res.skuPrice = skuPrice;
            res.setPrice = setPrice;
            resList.push(res);
        }
         //默认的参与实体
        
         return  resList;     }   

```
##jquery循环 each
```


输出每个 li 元素的文本：
$("button").click(function(){
  $("li").each(function(){
    alert($(this).text())
  });
});


jQuery的each方法的几种常用的用法

Js代码
 var arr = [ "one", "two", "three", "four"];     
 $.each(arr, function(){     
    alert(this);     
 });     
//上面这个each输出的结果分别为：one,two,three,four    
    
var arr1 = [[1, 4, 3], [4, 6, 6], [7, 20, 9]]     
$.each(arr1, function(i, item){     
   alert(item[0]);     
});     
//其实arr1为一个二维数组，item相当于取每一个一维数组，   
//item[0]相对于取每一个一维数组里的第一个值   
//所以上面这个each输出分别为：1   4   7     
  
  
var obj = { one:1, two:2, three:3, four:4};     
$.each(obj, function(key, val) {     
    alert(obj[key]);           
});   
//这个each就有更厉害了，能循环每一个属性     
//输出结果为：1   2  3  4

来源：  http://www.cnblogs.com/mabelstyle/archive/2013/02/19/2917260.html
```
##jquery为div绑定点击事件
```

<script type="text/javascript">

	$(function(){

		$("div").each(function(){

			$(this).click(function(){

				alert($(this).text());

			});

		});

	});

</script>
```
##jquery取数组中的最大值
var maxPrice = Math.max.apply(null,[silverPrice,goldPrice,platnumPrice,blackPrice]);
