##function
```
   1、function是一个函数
   2、function是一个对象，可以利用该对象的constructor属性找到该对象的构造函数
   3、一个对象（这个对象必须有值，不能是null,undefined）可以动态的添加任何一个属性
   4、一个function同时又是一个构造器函数
   5、任何一个对象都有可能成为任何一个对象的属性
```
##prototype
```
   *  prototype是function中的一个属性，也是一个对象
   *  prototype是一个json格式的对象，可以动态的往json中添加一些内容
          Person.prototype.name = function(){
              alert("name");
          }
          Person.prototype["sex"] = "male";
   *  根据构造器可以创建对象，而创建出来的对象就拥有了prototype中的数据
   实例一：
   function Person(){


   }
   function Student(){


   }


   Person.prototype.name = function(){
    alert("name");
   }
   Person.prototype.age = 5;
   Person.prototype["sex"] = "male";
   Person.prototype["student"] = Student;


   window.onload = function(){
    Person.prototype;
   }
```
##继承的封装
```
//实例一：
    function createClass(jsonObj){
      //任意的类为F
      function F(){


      }


      //将json对象的key/value赋值给原型
      for(var name in jsonObj){
        F.prototype[name] = jsonObj[name]
      }
      return F;
    }


    var Person = createClass({
      getName:function(){
        alert("name");
      },
      getId:function(){
        alert("id");
      }
    });


    var p = new Person();
    p.getName();
    p.getId();


    //实例二：
    /*
     * 当该函数中有一个参数的情况下创建类
     * 当该函数中有两个参数的情况下，第一个参数是基类，第二个参数就是在基类的基础上添加的内容
     */
     function extend(obj,prop){
      function F(){}


      if(typeof obj ==  "object"){//obj是一个json格式的对象
        for(var name in obj){
          F.prototype[name] = obj.name;
        }
      }else{//obj是一个函数
        F.prototype = obj.prototype;//完成了基类的赋值
        for(var name in prop){
          F.prototype[name] = prop[name];
        }
      }
      return F;
     }


     var Person = extend({
      name:5,
      id:6
     });


     var superPerson = extend(Person,{
      sex:'male'
     });
     var sp = new superPerson();
     alert(sp.name);
     alert(sp.id);
     alert(sp.sex);


     alert(typeof Person);//function
     var a = {};
     alert(typeof a);//object
```
##自定义事件
```
//实例一
    $().ready(function(){
      //jquery的事件是可以叠加的
      for(var i = 0; i < 4; i++){
        //非专业写法，弹出来4次
        $("#aa").click(function(){
          alert("aa");
        });
        //先解绑再绑定，弹出来1次
        $("#aa").unbind("click");//解除click属性
        $("#aa").bind("click",function(){
          /**
           *事件的触发器
           */
           $(this).trigger("点击",['param1','param2'])
        })
      }
      /**
       * 1.声明事件，把事件绑定在一个dom对象上
       * 2.找一个事件的触发方式，触发事件
       * 3.传递参数
       */
       $("#aa").unbind("点击");
       $("#aa").bind("点击",function(event,a,b){
        alert("点击了aa");
        alert(a);
        alert(b);
       });


    });
    <span id="aa">content</span>
```
##回调函数
```
<script type="text/javascript" src="jquey.js"></script>
  <script type="text/javascript">
    //以面向对象的形式重构
    var ajaxObj = {
      xmlHttpRequest: '',
      getXMLHttpRequest: function(){
        var xmlHttp;
        try{
          xmlHttp = new xmlHttpRequest;
        }catch (e){
          try{
            xmlHttp = new ActiveXObject("Msxml2.XMLHTTP")
          }catch (e){
            try{
              xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
            }catch (e){


            }
          }
        }
        return xmlHttp;
      },


      post:function(ajaxJSON){
        ajaxObj.xmlHttpRequest = ajaxObj.getXMLHttpRequest();
        ajaxObj.xmlHttpRequest.onreadystatechange = function(){
          if (ajaxObj.xmlHttpRequest.readyState == 4) {//响应完毕
            if(ajaxObj.xmlHttpRequest.status == 200){//成功响应
              ajaxJSON.callback(ajaxObj.xmlHttpRequest.responseText);
            }
          };
        }
        ajaxObj.xmlHttpRequest.open(ajaxJSON.method,ajaxJSON.url,true);
        ajaxObj.xmlHttpRequest.send(ajaxJSON.data)
      }
    }
    
    //使用
    window.onload = function(){
      document.getElementById("ok").onclick = function(){
        ajaxObj.post({
          method: "post",
          url: '/oprom/getRuleList.do',
          data: 'id=4567',
          callback: function(data){
            alert(data);
          }
        });
      }
    }


    //实例一：获取下拉框改变的值
    $().ready(function(){
      $("select").unbind("me");
      $("select").bind("me",function(event,meJson){
        alert(meJson.value)
        meJson.callback();//调用回调函数
      });
      $("select").unbind("change");
      $("select").bind("change",function(){
        $(this).trigger("me",{
          value: $(this).val(),
          callback: function(){
            alert("我是回调函数")
          }
        })
      })
    });


    //举例一
    function ajax(ajaxJSON){
      $.post(url,parameter,function(data){
        callback(data);
      })
    }




  </script>
</head>
<body>
  <div id="ok">ok</div>
  <select>
    <option value="1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
  </select>
</body>
```
##this
```
//js中的this 具有多态性
    function Person(){
      alert(this)
    }
    Person();//window.Person()
    function Student(){


    }
    Student s = Person;
    Student.s();
    //call 与  apply 方法改变this的指向
    Person.call(Student);//等同于Student.Person()


    //回调函数中的this，调用者来确定
    function testCallback(callback){
      callback.call(this);//回调函数的this是window
      callback.apply(Person)//回调函数的this是Person
    }
    testCallback(function(){
      alert(this);//this为回调函数中的this
    })
```
##匿名函数或闭包
```
(function(){
      function Student(){//外部访问不到
        alert("student")
      }
    })();
    //Student();


    (function(a){
      alert(a)//5
    })(5);


    //闭包范式()()第二个括号是实参，第一个参数为一个函数，函数中的参数为形参,在匿名函数中的所有的方法都可以使用
    /*
     *下面的写法好处：
     * 1.可以让一些函数私有化
     * 2.可以让一些函数公开化
     * 3.在匿名函数中声明的属性，在外部访问不到
     */
    (function(window){


      function A(){//私有的
        B();
        return {//全部是公开的方法
          C:C,
          D:D
        }
      }
      function B(){//私有的
        alert("bb")
      }
      function C(){//公开的
        alert("cc")
      }
      function D(){//公开的
        alert("dd")
      }
      window.A = A;//通过此方式可以让一个函数变为一个公开的函数
    })(window);
    var json = window.A();
    json.C();
    json.D();
```
##权限/用户/角色
```
系统比较复杂时，直接抛弃了角色。
用户与权限的桥梁用字符串表示。
比如：权限 "a", "b", "c"; 权限交集的情况："e extend b,c";


```
##ajax异步过程
```
var ajax = {
      //创建xmlhttp对象
      createXMLHttpRequest: function(){
        var xmlHttp;
            try{// Firefox, Opera 8.0+, Safari
              xmlHttp = new xmlHttpRequest;
            }catch (e){
              try{// Internet Explorer
                xmlHttp = new ActiveXObject("Msxml2.XMLHTTP")
              }catch (e){
                try{
                  xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
                }catch (e){


                }
              }
            }
            return xmlHttp;
      },


      /**
       * alert("aaa")与 alert("bbb")谁先执行不一定
       * 如果js代码端用到服务器返回的数据，必写在成功响应这里
       */
      getData: function(){
        var xmlhttp = ajax.createXMLHttpRequest();


        /*譔属性是服务器触发的*/
        xmlhttp.onreadystatechange = function(){
          if(xmlhttp.readyState == 4){
            if(xmlhttp.status == 200){
              alert("aaa");
            }
          }
        };
        xmlhttp.open("post","../CheckUserNameServlet?aa="+new Date().getTime(),true);
        xmlhttp.send(null);
        alert("bbb");
      }
    };


    $().ready(function(){
      $("input[type='button']").unbind("click");
      $("input[type='button']").bind("click",function(){
        ajax.getData();
      });
    });


    <input type="button">ok</div>
```
##ajax项目 表的crud操作
```
<html>
<head>
  <title></title>
  <script type="text/javascript" src="jquey.js"></script>
  <script type="text/javascript">
    var classes = {
      /**curd**/
      getAllData: function(){
        $.post("classAction_showAllClasses.action", null, function(data){
          classes.createTR(data.classesList);
        });
      },
      createTR: function(classesList){
        for(var i=0; i<classesList.length; i++){
          var proObj = classesList[i];
          createTrMethod(proObj);
          
      },
      addData: function(){
        var cname = $("input[name='cname']").val();
        var description = $("input[name='description']").val();
        var parameter  = {
          cname: cname,
          description,description
        }
        $.post("classAction_addClasses.action".parameter,function(data){
          createTrMethod(data.classes);
        })
      }
    };
    $().ready(function(){
      classes.getAllData();
      $("input[type='button']").unbind("click");
      $("inupt[type='button']").bind("click",function(){
        classes.addData();
      })
    });


    createTrMethod(proObj){
      var $tdCID = $("<td/>");
      $tdCID.text(proObj.cid);


      var $tdCNAME = $("<td/>");
      //$tdCNAME.text(proObj.cname);


      var $cnameText = $("<input/>");
      $cnameText.attr("type","text");
      $cnameText.attr("value",proObj.cname);
      $cnameText.unbind("blur");
      $cnameText.bind("blur",function(){
        var cid = $(this).parent().siblings("td:first").text();
        var cname = $(this).val();
        var description = $(this).parent().next().children().val();
        var parameter = {
          cid:cid,
          cname:cname,
          description:description
        }
        $.post("classAction_updateClasses.action",parameter,function(){


        });
      });
      $tdCNAME.append($cnameText);




      $tdDescription = $("<td/>");
      var $descriptionText = $("<input/>");
      $descriptionText.attr("type","text");
      $descriptionText.attr("value",proObj.description);
      $tdDescription.append($descriptionText);


      var $aDel = $("<a/>");
      $aDel.text("删除");
      $aDel.css("cursor","pointer");
      $aDel.unbind("click");
      $aDel.bind("click",function(){
        var aHref = $(this);
        var cid = aHref.parent().siblings("td:first").text();
        var parameter = {
          cid: cid
        };
        $.post("classAction_deleteClasses.action",parameter,function(data){
          aHref.parent().parent().remove();
        })
      });
    
      var $aTD = $("<td/>");
      $aTD.append($aDel);


      var $tr = $("<tr/>");
      $tr.append($tdCID);
      $tr.append($tdCNAME);
      $tr.append($aTD);


      $("tbody").append($tr);
    }
    
  </script>
</head>
<body>
  <table>
    <tbody>
      <tr>
        <td>cid</td>
        <td>cname</td>
        <td>description</td>
        <td>操作</td>
      </tr>
    </tbody>
  </table>
</body>
</html>
```
##用户角色权限
```
1.三张表
user 用户
role 角色
privilege 权限
这 三张表都是多对多的关系
权限的操作就是页面上的CUD的按钮
权限中的值和页面上的cud是对应的


2.做一个拦截器：
获取当前登录用户：
select p.name from Role r inner join r.users u inner join r.privileges p where u.loginName = '"+user.getLoginName()+"'
根据譔hql语句就可以获取此用户所拥有的权限。将权限的集合存放在值栈中


3.在页面上
<td align="center">
 <s:iterator value="#privileges" var="privilege">
  <s:if test="#privilege == 'user_delete'">
    <a href="userAction_delete.action?userId=${userId}"></a>
  <s:if>
 <s:iterator>
</td>
```
##jquey内核
```
jQuery的内核;
  (function( window, undefined ) {
       //这就是jQuery的原型
       var jQuery = function( selector, context ) {
           return new jQuery.fn.init( selector, context );
       }
       //利用jQuery选择器产生的对象就是jQuery产生的对象，所以利用选择器产生的对象才拥有了jQuery中prototype中的内容
       jQuery.fn = jQuery.prototype = {
            ready:function(){},
            each:function(){},
            size:function(){}
       }
       //window.jQuery.prototype=window.jQuery.fn=window.$.fn=jQuery.prototype=$.prototype=$.fn
       window.jQuery = window.$ = jQuery;
       把加在jQuery对象上的方法或者加jQuery.prototype上的方法称为jQuery的插件开发
       jQuery.a = function(){


       }
       jQuery.prototype.b = function(){}
       总结：如果该方法与页面上的元素没有关系，该方法就为jQuery中全局的插件方法
             如果该方法与页面上的元素有关系，则方法就必须加在jQuery的prototype上
  })(window);


  插件开发实例：
  <html>
<head>
  <title></title>
  <script type="text/javascript" src="jquey.js"></script>
  <script type="text/javascript" src="plugin.js"></script>
  <script type="text/javascript">
    
    
  </script>
</head>
<body>
</body>
</html>


plugin.js:
(function($){
  $.a = function(){
    alert("aaa");
  }
  $.fn.b = function(){
    alert("bbb");
  }
})($);


$().ready(function(){
  $.a();
  $("body").b();
});
```
##jQuery中each和size方法实现
```
<html>
<head>
  <title></title>
  <script type="text/javascript" src="jquey.js"></script>
  <script type="text/javascript" src="each-plugin.js"></script>
  <script type="text/javascript">
    
    
  </script>
</head>
<body>
  <input type="button" name="aa" /> 
  <input type="button" name="bb" /> 
</body>
</html>


each-plugin.js:
(function($){
  $.fn.adanacEach = function(callback){
    var array = $(this);
    for (var i = array.size() - 1; i >= 0; i--) {
      callback.apply(array[i]);//要做什么不知道，就用回调函数
    }
  }
  $.fn.adanacSize = function(){
    return this.length;
  }
}) (jQuery);


$().ready(function(){
  $("input").adanacEach(function(){
    alert($(this).attr("name"));
  });
  alert($("input").adanacSize());
});
```
##jQuery的插件grid的解析-插件方法
```
实例一：合并settings 和  options ，修改并返回settings
var settings = {validate:false,limit:5,name:"foo"};
var options = {validate:true,limit:5,name:"bar"}
jQuery.extend(settings,options);
// 结果:settings == {validate:true,limit:5,name:"bar"}


实例二：合并settings 和  options ，不修改settings
var empty = {};
jQuery.extend(empty,settings,options);
//empty = {validate:true,limit:5,name:"bar"};


```
##ajax 数据传递的问题
```
$.post
$.ajax
1、页面上的js端要传递参数到后台
     json格式
        简单json格式
            {
                 key:value(基本类型)
            }
        复杂json格式
            {
                 key:value是一个对象
            }
2、$.post只能在成功的时候使用
   $.post只能传递简单格式的数据
3、$.ajax是ajax请求的企业级处理
     可以传递简单的json对象
     可以用$.ajax传递复杂格式的对象
        *  把struts2与json插件提供的拦截器导入到项目中来
              <interceptors>
                 <interceptor-stack name="ajaxStack">
                    <interceptor-ref name="defaultStack"></interceptor-ref>
                    <interceptor-ref name="json"></interceptor-ref>
                 </interceptor-stack>
              </interceptors>
              <default-interceptor-ref name="ajaxStack"></default-interceptor-ref>
        *  js端必须调用$.ajax提交请求，json格式必须是最标准的字符串的形式，可以利用$.toJSON来进行转化
        *  $.ajax本身注意的问题：
              contentType: "application/json"这项配置说是传递的是json，必须写


ajax:
    1、在js端的事情
       1、发出请求：
             $.post
                 1、写法比较简单，利于操作
                 2、在$.post方法的回调函数中，参数data
                                                    如果服务器端不出错，则返回的是一个json对象的形式
                                                    如果服务器端出错，则返回的是错误的模板页面(因为struts2内部没有设置状态码)
                 3、$.post请求本身就不处理错误请求
             $.ajax
                 1、写法比较复杂
                 2、能处理复杂格式的数据
                     1、必须把处理复杂格式的拦截器引入进来"json"
                     2、这个方法本身设置contentType: "application/json"，说明传递的是json格式
                     3、必须使用标准的json字符串来传递


    2、在后台的事情
         1、必须在action中写属性，属性并且有get方法，才能够回调到客户端
         2、如果在action中的方法中产生错误，在客户端使用的是$.ajax请求，在action中的方法中设置状态码，才能在$.ajax的error方法中输出
         3、统一的错误处理和拦截器冲突了，只能选择其一
         4、在js端传递数据的时候，要保证key值和action中的属性值一致


实例一：
$().ready(function(){
  $("input[type='button']").unbind("click");
  $("input[type='button']").bind("click",function(){
      $.ajax({
        url: 'ajaxAction_ajax.action',
        type:"post",
        contentType:"application/json",
        data:"{'aa':[5,6,7,8]}",
        success:function(){
          alert("aaa");
        },
        error:function(){
          alert("bbb");
        }
      });
  });
})


public class AjaxAction extends ActionSupport{
  private Long[] aa;
  public Long[] getAa(){
    return aa;
  }
  public void setAa(Long[] aa){
    this.aa = aa;
  }


  public String ajax(){
    return SUCCESS;
  }
}


实例二：
$().ready(function(){
  $("input[type='button']").unbind("click");
  $("input[type='button']").bind("click",function(){
      var parameter1 = {
        id:'1',
        uname:'adanac',
        arrayInt:[2,3,5,8],
        arrayList{20,21,22},
        teacher:{
          tname:'aaa',
          responseblitity:'ajax测试',
          strs:['Hello','Adanac']
        },
        teacherList:[
          {
            tname:'老师一',
            responseblitity:'ttt11',
            strs:['aa','bb']
          },{
            tname:'老师二',
            responseblitity:'ttt22',
            strs:['aa','bb']
          }
        ]
      }


      var para = $.toJSON(parameter1);
      $.ajax({
        url: 'ajaxAction_ajax.action',
        type:"post",
        contentType:"application/json",
        data:para,
        success:function(){
          alert("aaa");
        },
        error:function(){
          alert("bbb");
        }
      });
  });
})


public class AjaxAction extends ActionSupport{
  private Integer id;
  private String uname;
  private Integer[] arrayInt;
  private List<Integer> arrayList;
  private List<Teacher> teacherList;


  //getter and setter
  private Teacher teacher;
  public String ajax(){
    return SUCCESS;
  }


}
```
##js编码结构的说明，ztree树的数据库的搭建
```
树的加载
   加载树的两种情况
      一次性加载
      点击事件加载


zTree:
(function($){
    $.fn.zTree = function(){


    }
    function n1(){}
    function n2(){}
    …………
})($)
上述的写法结构性不强，比较混乱
(function($){
    {},{},{}
})($)
是以json格式组织的，这样结构比较好，维护性也比较好


实例一：
public class Tree implements Serializable{
  private Long tid;
  private Long pid;
  private String name;
  private Boolean isParent;//设置譔节点为父节点
  //getter and setter
}


public class TreeTest extends HibernateUtils{
  @Test
  public void addTree(){
    Session session = sessionFactory.getCurrentSession();
    Transaction transaction = session.beginTransaction();


    Tree treeItem1 = new Tree();
    treeItem1.setTid(1l);
    treeItem1.setName("办公自动化");
    treeItem1.setPid(0l);
    treeItem1.setIsParent(true);


    session.save(treeItem1);


    transaction.commit();
    session.close();
  }
}


public class HibernateUtils{
  public static sessionFactory sessionFactory;
  static{
    Configuration configuration = new Configuration();
    configuration.configure();
    sessionFactory = configuration.buildSessionFactory();
  }
}


public  class TreeDao{
  public List<Tree> getTree(){
    Session session = HibernateUtils.sessionFactory.getCurrentSession();
    Transaction transaction = session.beginTransaction();
    List<Tree> treeList = session.createQuery("from Tree").list();
    transaction.commit;
    return treeList;
  }
}


public class TreeAction extends ActionSupport{
  private List<Tree> treeList;


  public List<Tree> getTreeList(){
    return treeList;
  }


  public String showTree(){
    TreeDao treeDao = new TreeDao();
    this.treeList = treeDao.getTreeList();
    return SUCCESS;
  }
}


<html>
<head>
  <title></title>
  <script type="text/javascript" src="jquey.js"></script>
  <script type="text/javascript" src="tree.js"></script>
  <script type="text/javascript">
    
    
  </script>
</head>
<body>
  <ul id="tree" class="tree"></ul> 
</body>
</html>


tree.js:
/**
 * js的代码组织
 *   1.js代码中涉及到数据
 *   2.页面上需要触发很多事件
 *   3.把页面上的功能做一个分类
 */


var tree = {
  //用来封装数据
  data:{


  },
  //初始化页面所有的事件；初始化页面所有的数据
  init:{
    initEvent:function(){


    },
    initData:function(){


    }
  },
  //页面上所有的操作
  operation:{
    tree:{
      setting:{
        isSimpleData:true,
        treeNodeKey:"tid",//必须要与实体Tree中的属性保持一样
        treeNodeParentKey:"pid",//同样
        showLine:true,
        root:{
          isRoot:true,
          nodes:[]
        }
      },
      loadTree:function(){
        $.post("treeAction_showTree.action",null,function(data){
          $("#tree").zTree(tree.operation.tree.setting,data.treeList);
        });
      }
    },
    operation2:{


    },
    operation3:{


    }
  }
}


$().ready(function(){
  tree.operation.tree.loadTree();
})


```
##点击事件加载树上的节点
```
$().ready(function(){
  // tree.operation.tree.loadTree();
  /**
   * 1.当加载整个tree.html页面时，把树上的根节点加载
   * 2.当点击树上的节点的时候，触发事件，加载譔节点的子节点
   */
   tree.operation.tree.loadRoot();
})


public List<Tree> getSubTreeByPid(Long pid){
  Session session = HibernateUtils.sessionFactory.getCurrentSession();
  Transaction transaction = session.beginTransaction();
  List<Tree> trees = session.createQuery("from Tree where pid = "+pid).list();
  transaction.commit();
  return trees;
}


public String showNodeByPid(){
  TreeDao treeDao = new TreeDao();
  this.treeList = treeDao.getSubTreeByPid(this.pid);
  return SUCCESS;
}


//页面上所有的操作
  operation:{
    tree:{
      setting:{
        isSimpleData:true,
        treeNodeKey:"tid",//必须要与实体Tree中的属性保持一样
        treeNodeParentKey:"pid",//同样
        showLine:true,
        root:{
          isRoot:true,
          nodes:[]
        },
        callback:{
          // treeId:树的容器ID， treeNode:当前点击的节点
          expand:function(event,treeId,treeNode){
            $.post("treeAction_showNodeByPid.action",
              {
                pid:treeNode.tid
              },
            function(data){
              if(!tree.operation.tree.zTree.getNodeByParam("pid",treeNode,tid)){//当前点击的节点没有子节点
                tree.operation.tree.zTree.addNodes(treenode,data.treeList,true);
              }
            });
          }
        }
      },
      loadTree:function(){
        $.post("treeAction_showTree.action",null,function(data){
          tree.operation.tree.zTree = $("#tree").zTree(tree.operation.tree.setting,data.treeList);
        });
      },
      // 加载根节点
      loadRoot:function(){
        //方式一：
        $.post("treeAction_showNodeByPid.action",{pid:0},function(data){
          tree.operation.tree.zTree = $("#tree").zTree(tree.operation.tree.setting,data.treeList);
        }); 
        // 方式二：
        tree.operation.tree.ajaxMethod({
          url:"treeAction_showNodeByPid.action",
          data:{
            pid:0
          },
          callback: function(data){
            tree.operation.tree.zTree = $("#tree").zTree(tree.operation.tree.setting,data.treeList);
          }
        })
      },
      ajaxMethod:function(ajaxJson){
        $.post(ajaxJson.url, ajaxJson.data, function(data){
          ajaxJson.callback(data);
        });
      }


    },




```
##struts2与js的亮点
```
1、struts2的流程
      流程图画出来
2、struts2的统一错误处理
      原因总结出来
3、struts2的缺点(个人认为):
       核心流程写得不太灵活
       与ajax结合，状态码没有设置
4、struts2的有些设置过于复杂
5、js的面向对象开发
     对象
        {}
        function(){}
     prototype
     this
       具有多态性
     callback
     clouse(闭包)
        继承
        面向对象(可以公开一些函数，可以私有化函数)
     事件
     jQuery的内核
        jQuery,$,fn,jQuery.prototype,$.fn这些概念是什么
     jQuery的插件开发
        1、把整体的内容放在匿名函数中
        2、把一些函数弄成私有的函数
        3、用一定的方法把一些函数公开API
6、struts2与ajax整合的过程
    $.post与$.ajax
    在js端复杂格式的数据怎么样传递到后台
    前台需要什么样的数据，后台就返回什么样的数据
    异常的错误处理
    struts2中什么样的数据可以返回到客户端


```