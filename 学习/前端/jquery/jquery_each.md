## jquery $.each 和for 怎么跳出循环（终止本次循环）
```
  1、for循环中我们使用continue；终止本次循环计入下一个循环，使用break终止整个循环。


2、而在 jQuery 中 $.each则对应的使用return true  和return false。

实例：

$(function (){ 
 $("input[type='text']").each(function (i){  
  var _val=$(this).val();
  alert(_val);
  if(_val=='2'){   
   return false; //跳出循环
  }
 })
});



```
##实例一 ```
$("input[id^=giftNum]").each(function(i,obj){
giftNum += $(obj).val() + ",";
});
``` ## jquery的each()详细介绍
```
each()方法能使DOM循环结构简洁，不容易出错。each()函数封装了十分强大的遍历功能，使用也很方便，它可以遍历一维数组、多维数组、DOM, JSON 等等
在javaScript开发过程中使用$each可以大大的减轻我们的工作量。

下面提一下each的几种常用的用法

  

each处理一维数组

   var  arr1  =  [  " aaa " ,  " bbb " ,  " ccc "  ];      
  $.each(arr1,  function (i,val){      
      alert(i);   
      alert(val);
  });   
alert(i)将输出0，1，2
alert(val)将输出aaa，bbb，ccc

 

 
each处理二维数组  

　　 var  arr2  =  [[ ' a ' ,  ' aa ' ,  ' aaa ' ], [ ' b ' ,  ' bb ' ,  ' bbb ' ], [ ' c ' ,  ' cc ' ,  ' ccc ' ]]      
　　 $.each(arr,  function (i, item){      
      alert(i);   
      alert(item);      
　　 });  
arr2为一个二维数组，item相当于取这二维数组中的每一个数组。
item[0]相对于取每一个一维数组里的第一个值   
alert(i)将输出为0，1，2，因为这二维数组含有3个数组元素
alert(item)将输出为  ['a', 'aa', 'aaa']，['b', 'bb', 'bbb']，['c', 'cc', 'ccc']

 

对此二位数组的处理稍作变更之后



  var  arr  =  [[ ' a ' ,  ' aa ' ,  ' aaa ' ], [ ' b ' ,  ' bb ' ,  ' bbb ' ], [ ' c ' ,  ' cc ' ,  ' ccc ' ]]      
 　　$.each(arr,  function (i, item){      
      　　$.each(item, function (j,val){
       　　　　 alert(j);
        　　　　alert(val);
    　}); 
});    
 alert(j)将输出为0，1，2，0，1，2，0，1，2

 alert(val)将输出为a，aa，aaa，b，bb，bbb，c，cc，ccc

 

   each处理json数据，这个each就有更厉害了，能循环每一个属性     

 　　 var  obj  =  { one: 1 , two: 2 , three: 3 };      
  　　 each(obj,  function (key, val) {      
       　　 alert(key);   
       　　 alert(val);      
  　　 });   
这里alert(key)将输出one two three
alert(val)将输出one，1，two，2，three,3
这边为何key不是数字而是属性呢，因为json格式内是一组无序的属性-值，既然无序，又何来数字呢。
而这个val等同于obj[key]

 

ecah处理dom元素，此处以一个input表单元素作为例子。

如果你dom中有一段这样的代码
<input name="aaa" type="hidden" value="111" />
<input name="bbb" type="hidden" value="222" />
<input name="ccc" type="hidden" value="333" />
<input name="ddd" type="hidden"  value="444"/>
然后你使用each如下

 $.each($("input:hidden"), function(i,val){  
     alert(val);
     alert(i);
     alert(val.name);
     alert(val.value);   
 });   那么，alert(val)将输出[object HTMLInputElement]，因为它是一个表单元素。   

alert(i)将输出为0，1，2，3 


alert(val.name);将输出aaa,bbb,ccc,ddd，如果使用this.name将输出同样的结果
alert(val.value);  将输出111,222,333,444，如果使用this.value将输出同样的结果

 


如果将以上面一段代码改变成如下的形式  

$( " input:hidden " ).each( function (i,val){
    alert(i);
    alert(val .name);
    alert(val .value);       
});

 可以看到，输出的结果是一样的，至于两种写法究竟区别在哪，我也还不知。此改变运用到上面几段数组的操作也会输出同样的结果。

 

这样，几个例子的实际结果已经得到答案。接着再继续往下研究，总不能知其然不知其所以然。 
从以上的例子中可知 jQuery 和 jQuery 对象都实现了该方法，对于 jQuery 对象，只是把 each 方法简单的进行了委托：把 jQuery 对象作为第一个参数传递给 jQuery 的 each 方法。



看下jQuery中的each实现（网络摘抄） 

function  (object, callback, args) {
// 该方法有三个参数:进行操作的对象obj，进行操作的函数fn，函数的参数args
var  name, i  =   0 ,length  =  object.length;
if  (args) {
if  (length  ==  undefined) {
for  (name  in  object) {
if  (callback.apply(object[name], args)  ===   false ) {
break ;
}
}
}  else  {
for  (; i  <  length;) {
if  (callback.apply(object[i ++ ], args)  ===   false ) {
break ;
}
}
}
}  else  {
if  (length  ==  undefined) {
for  (name  in  object) {
if  (callback.call(object[name], name, object[name])  ===   false ) {
break ;
}
}
}  else  {
for  ( var  value  =  object[ 0 ]; i  <  length  &&  callback.call(value, i, value)  !==   false ; value  =  object[ ++ i]) {}
/* object[0]取得jQuery对象中的第一个DOM元素，通过for循环，
得到遍历整个jQuery对象中对应的每个DOM元素，通过 callback.call( value,i,value);
将callback的this对象指向value对象，并且传递两个参数,i表示索引值，value表示DOM元素；
其中callback是类似于 function(index, elem) { ... } 的方法。
所以就得到 $("...").each(function(index, elem){ ... });
*/
}
}
return  object;
}

  jquery会自动根据传入的元素进行判断，然后在根据判断结果采取apply还是call方法的处理。 在 fn 的实现中，可以直接采用 this 指针引用数组或是对象的子元素。





1.obj 对象是数组

each 方法会对数组中子元素的逐个进行 fn 函数调用，直至调用某个子元素返回的结果为 false 为止，也就是说，我们可以在提供的 fn 函数进行处理，使之满足一定条件后就退出 each 方法调用。当 each 方法提供了 arg 参数时， fn 函数调用传入的参数为 arg ，否则为：子元素索引，子元素本身

2.obj  对象不是数组 该方法同 1 的最大区别是： fn 方法会被逐次不考虑返回值的进行进行。换句话说， obj 对象的所有属性都会被 fn 方法进行调用，即使 fn 函数返回 false 。调用传入的参数同 1 类似。

来源：  http://www.cnblogs.com/xiaojinhe2/archive/2011/10/12/2208740.html
```