##html变量、双引号拼接
```
用\"转义成“
实例一：
js代码中：
+ "<button id='addCategory_" +index+ "' type='button' class='btn green' onclick='addCategory(1,\"" +line+ "\")'>添加</button>"   

浏览器解析后：
<button id="addCategory_7" type="button" class="btn green" onclick="addCategory(1,&quot;on&quot;)">添加</button>

也就是<button id="addCategory_7" type="button" class="btn green" onclick="addCategory(1,“on”)">添加</button>



实例二：
check_btn =  "<button type='button' class='btn default' onclick='toInsertRule(\"" +prplist[i][ "id" ]+ "\")'>审核通过</button><button type='button' class='btn default' onclick='toCancelRule(\"" +prplist[i][ "id" ]+ "\")'>审核拒绝</button>" ;   

```