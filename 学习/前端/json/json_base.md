## org.json. jsonObject 获取属性 而不会抛异常
```
org.json. JSONObject jsonObj = Obj.optJSONObject(Name);这个如果找不到会返回null，不会抛出异常

JSONObject有很多optXXX方法，比如optBoolean, optString, optInt...

他们的意思是，如果这个jsonObject有这个属性，则返回这个属性，否则返回一个默认值

例如
JSONObject json =  new  JSONObject(content);  
int  x = json.optInt( "length" ,  0 );  

String response = HttpUtil.doGet(url);
                JSONObject jsonObj = JSONObject.fromObject(response);
                wsTradeDto = new WsTradeDto();
                wsTradeDto.setCompanyId(companyId);
                wsTradeDto.setOccurDate(date);
                wsTradeDto.setPayCount(jsonObj.optJSONObject("response").optDouble("numFound"));
                wsTradeDto.setPayMoney(jsonObj.optJSONObject("stats").optJSONObject("stats_fields").optJSONObject("mo_totalMoney").optDouble("sum"));
                result.add(wsTradeDto);
```
##jsonObject 转 java对象
```
方法一：
ResourcesDto resourcesDto = com.alibaba.fastjson.JSON.toJavaObject((JSONObject) br.getContent(), ResourcesDto.class);

方法二：
String result = HttpUtil.doGet(url.toString());
br = com.alibaba.fastjson.JSONObject.parseObject(result, BaseResult.class);


```
##jsonOject 转 java数组
```
BaseResult = baseResult = new BaseResult();
Pager<ResourcesDto> result = imgTextDataService.queryResourcesList(resourcesDto, param);
baseResult.setContent(result);
baseResult.setStatus(MmcAppErrorCode.SUCCESS);


String result = HttpUtil.doGet(url.toString());
br = JSONObject.parseObject(result, BaseResult.class);


com.alibaba.fastjson.JSONObject re = (com.alibaba.fastjson.JSONObject) br.getContent();
pager = new Pager<ResourcesDto>();
JSONArray datas = re.getJSONArray("datas");
List<ResourcesDto> navs = com.alibaba.fastjson.JSONArray.parseArray(datas.toJSONString(), ResourcesDto.class);
```
##将对象转为json的两种方法
```
return net.sf.json.JSONObject.fromObject(data).toString();
return import com.alibaba.fastjson.JSONObject.toJSONString(data);


856701290 3264
865281318 wwg764


856 281 318 w333jr

```
http://my.oschina.net/heweipo/blog/386808

##数组转对象
```
String paramListStr = map.get("paramList");
paramListStr = StringUtil.proListStr(paramListStr);
JSONObject jobj = JSONObject.fromObject(paramListStr);
JSONArray jb = JSONArray.fromObject(paramListStr);
if (jb.size() > 0) {
for (int i = 0; i < jb.size(); i++) {
Map<String, Object> mapRes = new HashMap<>();
JSONObject job = jb.getJSONObject(i); // 遍历 jsonarray
// 数组，把每一个对象转成 json 对象
// System.out.println(job.get("name")+"=") ; // 得到 每个对象中的属性值
mapRes.put(i + "", job);
paramList.add(mapRes);
}
}
```
## json中key所对应的value值
```
getJson('age');  
  
function getJson(key){
    var jsonObj={"name":"傅红雪","age":"24","profession":"刺客"};  
    //1、使用eval方法      
    var eValue=eval('jsonObj.'+key);  
    alert(eValue);  
    //2、遍历Json串获取其属性  
    for(var item in jsonObj){  
        if(item==key){  //item 表示Json串中的属性，如'name'  
            var jValue=jsonObj[item];//key所对应的value  
            alert(jValue);  
        }  
    }  
    //3、直接获取  
    alert(jsonObj[''+key+'']);  
}
```
##json字符串转为对象
```
//通过eval() 函数可以将JSON字符串转化为对象 var  t3 = "[['<a href=# onclick=openLink(14113295100,社旗县国税局桥头税务所,14113295100,d6d223892dc94f5bb501d4408a68333d,swjg_dm);>14113295100</a>','社旗县国税局桥头税务所','社旗县城郊乡长江路西段']]" ;   //通过eval() 函数可以将JSON字符串转化为对象  
实例一：
var  obj  =  eval (t3); 
  for(var  i = 0 ;i < obj.length ;i++){ 
      for(var  j = 0 ;j < obj [i].length;j++){  
          alert(obj[i][j]);     
      }   
 }  


实例二：
var  temp = $( "#oldGoodsList" ).val();
var  strlen0 = temp.length;
var  strlen1 = eval(strlen0+ "-" +5);
var  temp0 = temp.substring(9,strlen1);
var  ods = eval(temp0);
console.log(ods)
for ( var  i=0;i<ods.length;i++){
    goodsList.push(ods[i]); }   

```
##json数组转为List<Object>
```
使用的是json-lib.jar包

将json格式的字符数组转为List对象

import  java.util.Iterator;  
import  java.util.List;  
  
import  org.junit.Test;  
  
import  net.sf.json.JSONArray;  
import  net.sf.json.JsonConfig;  
  
public   class  JsonToList {  
  
     public   static   void  main(String[] args) {  
        String json= "[{'name':'huangbiao','age':15},{'name':'liumei','age':14}]" ;  
        JSONArray jsonarray = JSONArray.fromObject(json);  
        System.out.println(jsonarray);  
        List list = (List)JSONArray.toCollection(jsonarray, Person. class );  
        Iterator it = list.iterator();  
         while (it.hasNext()){  
            Person p = (Person)it.next();  
            System.out.println(p.getAge());  
        }  
    }  
      
     @Test   
     public   void  jsonToList1(){  
        String json= "[{'name':'huangbiao','age':15},{'name':'liumei','age':14}]" ;  
        JSONArray jsonarray = JSONArray.fromObject(json);  
        System.out.println(jsonarray);  
        List list = (List)JSONArray.toList(jsonarray, Person. class );  
        Iterator it = list.iterator();  
         while (it.hasNext()){  
            Person p = (Person)it.next();  
            System.out.println(p.getAge());  
        }  
          
    }  
      
     @Test   
     public   void  jsonToList2(){  
        String json= "[{'name':'huangbiao','age':15},{'name':'liumei','age':14}]" ;  
        JSONArray jsonarray = JSONArray.fromObject(json);  
        System.out.println(jsonarray);  
        System.out.println( "------------" );  
        List list = (List)JSONArray.toList(jsonarray,  new  Person(),  new  JsonConfig());  
        Iterator it = list.iterator();  
         while (it.hasNext()){  
            Person p = (Person)it.next();  
            System.out.println(p.getAge());  
        }  
          
    }  
}  

来源：  http://hbiao68.iteye.com/blog/1771155

将list对象转为JSON字符串数组

import  java.util.LinkedList;  
import  java.util.List;  
  
import  net.sf.json.JSONArray;  
  
public   class  ListToJson {  
  
     public   static   void  main(String[] args) {  
        List list =  new  LinkedList();  
         for ( int  i= 0 ;i< 3 ;i++){  
            Person p =  new  Person();  
            p.setAge(i);  
            p.setName( "name" +i);  
            list.add(p);  
        }  
        JSONArray jsonarray = JSONArray.fromObject(list);  
        System.out.println(jsonarray);  
    }  
  
}

 打印结果

[{ "age" : 0 , "birthday" : null , "id" : "" , "name" : "name0" },{ "age" : 1 , "birthday" : null , "id" : "" , "name" : "name1" },{ "age" : 2 , "birthday" : null , "id" : "" , "name" : "name2" }]  
```
##json格式
```
var json='[{"CityId":18,"CityName":"西安","ProvinceId":27,"CityOrder":1},{"CityId":53,"CityName":"广州","ProvinceId":27,"CityOrder":1}]';

```
##java从json字符串获取值
```
import  net . sf . json .JSONObject;
import  com . alibaba . dubbo . common . json .JSON;
1.JSONObject jsonObject = JSONObject.fromObject(paramJson);//string转为jsonObject
2.activity.setDiscountType(Integer.parseInt(jsonObject.getString("discountType")));//从jsonObject根据key获取值


@RequestMapping(value = "add")
    public String add(HttpServletRequest request, HttpServletResponse response, @RequestParam String paramJson) {
        log.info("add====>activity:" + paramJson.toString());
        try {
            paramJson = java.net.URLDecoder.decode(paramJson, "utf-8");
            XSActivityDto activity = JSON.parse(paramJson, XSActivityDto.class);//将string转为java对象
            // activity.setStatus(1);
            HttpSession session = request.getSession();
            String suppId = session.getAttribute(MmcConstants.SESSION_SUPP_ID).toString();
            activity.setSuppId(suppId);
            JSONObject jsonObject = JSONObject.fromObject(paramJson);
            activity.setDiscountType(Integer.parseInt(jsonObject.getString("discountType")));
            BaseResult br = disCountService.addActivity(activity);
            return ajaxJson(response, JSONObject.fromObject(br).toString());
        } catch (Exception e) {
            log.error("新增活动失败：", e.toString());
            return "";
        }     } 
 

``` ##json合并数组
```
var edit_form = $('#wepay_edit_form'); var param1 = $.serializeObject(edit_form); 
var   param2  = {"name":"allen","age":"18"};
var param0 = {"param1":param1,"param2":param2}
 
``` ##后台返回json对象，前台取出属性
```
response.setContentType("text/html;charset=UTF-8"); 
Map map = new HashMap();
map.put("name", "allen");

String json = JSONObject.fromObject(map).toString();
response.getWriter().write(json.toString());
response.getWriter().flush();
response.getWriter().close();
return null;
```
上面是后台返回的json对象，在前台取出name的值

```
$.post("../../strtgcObjctvs.do","ope=init",function(data){
var json = eval("("+data+")"); // data的值是json字符串，这行把字符串转成object
        alert(json.name);// 取出属性值allen
}
```
##js数组转json
```
普通的数组格式是：['a','b','c']
JSON的格式是：{'1':'a','2':'b','3':'c'}
所以把数组循环一下就可以了；
var a = ['a','b','c'];
var json = {};
for(var i=0;i<a.length;i++){
    json[i]=a[i];
}
JSON.stringify(json);  //结果：{'1':'a','2':'b','3':'c'}
```
##关于JSON.stringify()方法
```
JSON.stringify 接受三个参数，很多人都知道第三个参数可以设置空白字符来美化输出，但是你可能不知道第二个参数的作用，它为 {Array|Function} 类型，如果为 Array 则用于过滤 key，如果为 Function 则可以对 value 做处理
```

