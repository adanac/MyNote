[TOC]
##js数组中如何随机取出一个值，多种实现方法
```
有两种不同的方式实现：
一、随机取单个，二、让整个数组随机排序
注意：[ ] 符号在javascript中定义一个数组，{ } 则定义一个对象
随机取得数组里面的某一个：
< script type="text/javascript">
    //随机取得数组中的一个 var Arr = ["a", "b", "c", "d"]; var n = Math.floor(Math.random()
    * Arr.length + 1) - 1; alert(Arr[n]);
    < /script>
        随机排序整个数组Array：
        <script type="text/javascript ">
            //随机排序整个数组
            var Arr1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 22, 33, 55, 77, 88, 99];
            Arr1.sort(function() {
                return Math.random() > 0.5 ? -1 : 1;
            });
            alert(Arr1);
        </script>
        ========================================== PHP 里面有个非常方便的打乱数组的函数 shuffle()
        ，这个功能在许多情况下都会用到，但 javascript 的数组却没有这个方法，没有不要紧，可以扩展一个，自己动手，丰衣足食嘛。
        <script type="text / javascript ">
            //<![CDATA[ 
            var shuffle = function(v) {
                for (var j, x, i = v.length; i; j = parseInt(Math.random() * i), x = v[--i], v[i] = v[j], v[j] = x);
                return v;
            };
            var a = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
            document.write("A = ", a.join(",
"), " < br > <br > shuffle(A) = ", shuffle(a));
            if (!Array.prototype.shuffle) {
                Array.prototype.shuffle = function() {
                    for (var j, x, i = this.length; i; j = parseInt(Math.random() * i), x = this[--i], this[i] = this[j], this[j] = x);
                    return this;
                };
            }
            document.write(" < br > A.shuffle() = ", a.shuffle());
            //]]> 
            
        </script>
        "
```
##组建 list  , 获取第一个元素
```
var sellerList = [];//待添加商家
var seller = {};
seller.value = 11;
seller.text = 'hello';
sellerList.push(seller);


var firstEle = sellerList[0];//取得sellerList数组中的第一个元素
```
##js 将数组 变为 逗号分隔的字符串
```
将["10","50"] 变为 “10,50”
function  getList2Str(list){
     var  res =  "" ;
     if (list !=  undefined  && list !=  ""  && list.length > 0){
        res = list.toString();
    }
     return  res; }   

```
##js逗号分隔的字符串变数组
```
比如将“10，50” 转为 ["10","50"];
     function  getstr2List(source){
     var  res =  new  Array();
    res = source.split( "," );
     return  res;
}
```
##判断数组是否为空
```
var arr1 = [];
if(arr1.length === 0){
    console.log('arr1 is empty');

}

var arr2 = null;
if(arr2 === null){
    console.log('arr2 is null);

}
```
##jquery循环数组
```
var  arr1  =  [  " aaa " ,  " bbb " ,  " ccc "  ];      
  $.each(arr1,  function (i,val){      
      alert(i);   
      alert(val);
  });   
alert(i)将输出0，1，2
alert(val)将输出aaa，bbb，ccc
each处理json数据，这个each就有更厉害了，能循环每一个属性     

　　 var  obj  =  { one: 1 , two: 2 , three: 3 };      
　　 each(obj,  function (key, val) {      
    　　 alert(key);
    　　 alert(val);
　　 });
这里alert(key)将输出one two three
alert(val)将输出one，1，two，2，three,3

来源：  http://www.cnblogs.com/xiaojinhe2/archive/2011/10/12/2208740.html
``` 
##清空所有元素
```
方式一：
int[] ary = {1,2,3,4}; 
ary.length = 0;
方式二：【推荐，效率高】
var ary = [1,2,3,4];
ary = []; // 赋值为一个空数组以达到清空原数组
```

##删除指定元素
```
//扩展数组方法：查找指定元素的下标  
    Array.prototype.indexOf = function(val) {  
        for (var i = 0; i < this.length; i++) {  
            if (this[i] == val) return i;  
        }  
        return -1;  
    };  
  
    //扩展数组方法:删除指定元素  
    Array.prototype.remove = function(val) {  
        var index = this.indexOf(val);  
        while(index>-1){  
            this.splice(index, 1);  
            index = this.indexOf(val);  
        }  
    };  
```
## 删除数组元素
```
//删除数组元素需要扩展Array原型prototype.
Array.prototype.del=function(index){
        if(isNaN(index)||index>=this.length){
            return false;
        }
        for(var i=0,n=0;i
            if(this[i]!=this[index]){
                this[n++]=this[i];
            }
        }
        this.length-=1;
    };
```

##依据下标删除元素
```
1、创建数组
var array = new Array();
var array = new Array(size);//指定数组的长度
var array = new Array(item1,item2……itemN);//创建数组并赋值


2、取值、赋值
var item = array[index];//获取指定元素的值
array[index] = value;//为指定元素赋值


3、添加新元素
array.push(item1,item2……itemN);//将一个或多个元素加入数组，返回新数组的长度
array.unshift(item1,item2……itemN);//将一个或多个元素加入到数组的开始位置，原有元素位置自动后移，返回  新数组的长度
array.splice(start,delCount,item1,item2……itemN);//从start的位置开始向后删除delCount个元素，然后从start的位置开始插入一个或多个新元素


4、删除元素
array.pop();//删除最后一个元素，并返回该元素
array.shift();//删除第一个元素，数组元素位置自动前移，返回被删除的元素
array.splice(start,delCount);//从start的位置开始向后删除delCount个元素


5、数组的合并、截取
array.slice(start,end);//以数组的形式返回数组的一部分，注意不包括 end 对应的元素，如果省略 end 将复制 start 之后的所有元素
array.concat(array1,array2);//将多个数组拼接成一个数组


6、数组的排序
array.reverse();//数组反转
array.sort();//数组排序，返回数组地址


7、数组转字符串
array.join(separator);//将数组原因用separator连接起来
```
##数组的翻转（非reverse）
```
<script>
    var arr=[1,2,3,4];
    var arr2=[];
    while(arr.length) {
        var num=arr.pop();
        arr2.push(num);
    }
    alert(arr2);
</script>
```

##删除数组元素
```
var arr=[’a',’b',’c'];
若要删除其中的’b',有两种方法：


1.delete方法:delete arr[1]
这种方式数组长度不变,此时arr[1]变为undefined了,但是也有好处原来数组的索引也保持不变,此时要遍历数组元素可以才用
for(index in arr)
document.write(’arr[’+index+’]=’+arr[index]);
这种遍历方式跳过其中undefined的元素


* 该方式IE4.o以后都支持了

2.数组对象splice方法:arr.splice(1,1);
这种方式数组长度相应改变,但是原来的数组索引也相应改变
splice参数中第一个1,是删除的起始索引(从0算起),在此是数组第二个元素
第二个1,是删除元素的个数,在此只删除一个元素,即’b';
此时遍历数组元素可以用普通遍历数组的方式,比如for,因为删除的元素在
数组中并不保留

* 该方法IE5.5以后才支持

值得一提的是splice方法在删除数组元素的同时,还可以新增入数组元素
比如arr.splice(1,1,’d',’e'),d,e两个元素就被加入数组arr了
结果数组变成arr:’a',’d',’e',’c’
```
##js数组总结
<img src='images/数组.gif'>
##消除一个数组里面重复的元素
```
var arr1 =[1,2,2,2,3,3,3,4,5,6],
    arr2 = [];
for(var i = 0,len = arr1.length; i< len; i++){
    if(arr2.indexOf(arr1[i]) < 0){
        arr2.push(arr1[i]);
    }
}
document.write(arr2); // 1,2,3,4,5,6
```



