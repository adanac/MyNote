[TOC]
##关于map的有效期 
```
public class TestMap {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<String, String>();
        List<Map<String, String>> list = new ArrayList<Map<String, String>>();
        map.put("flag", "111");
        if (function1(map)) {
            list.add(map);
        }
        System.out.println(list.get(0).get("flag"));
    }
 
    public static boolean function1(Map<String, String> map) {
        map.put("flag", "222");
        return true;
    }
}
执行function1()方法，，取出map的内容，改变了map的内容。

测试Return
public static int Return(){
     int i = -1;
     boolean flag = true;
     if (flag){
         return 5;
     }
     return 2;
}
/// 最后返回的数据为5.
```

##有序map
```
最近的项目需要读取对照表放在MAP里面
开始用TreeMap发现顺序不对 换成 HashMap顺序也不对
最后查了一下
LinkedHashMap 才能保证迭代的时候取出的顺序和存入的顺序相同
Map<uint32_t, ConditionPo>  conditionInfoMap  =  new  LinkedHashMap<>();  
conditionInfoMap .put( ut ,  conditionPo );  

```
##TreeMap/HashMap进行排序
```
--对值按升序排序
private static void testTreeMap() {
List<Map.Entry<String, String>> mappingList = null;
Map<String, String> map = new TreeMap<String, String>();
map.put("aaaa", "month");
map.put("bbbb", "bread");
map.put("ccccc", "attack");
// 通过ArrayList构造函数把map.entrySet()转换成list
mappingList = new ArrayList<Map.Entry<String, String>>(map.entrySet());
// 通过比较器实现比较排序
Collections.sort(mappingList, new Comparator<Map.Entry<String, String>>() {
public int compare(Map.Entry<String, String> mapping1,
Map.Entry<String, String> mapping2) {
return mapping1.getValue().compareTo(mapping2.getValue());
}
});
for (Map.Entry<String, String> mapping : mappingList) {
System.out.println(mapping.getKey() + ":" + mapping.getValue());
}
}
--对键按升序排序
private static void testHashMap() {
List<Map.Entry<String, String>> mappingList = null;
Map<String, String> map = new HashMap<String, String>();
map.put("month", "month");
map.put("bread", "bread");
map.put("attack", "attack");
// 通过ArrayList构造函数把map.entrySet()转换成list
mappingList = new ArrayList<Map.Entry<String, String>>(map.entrySet());
// 通过比较器实现比较排序
Collections.sort(mappingList, new Comparator<Map.Entry<String, String>>() {
public int compare(Map.Entry<String, String> mapping1,
Map.Entry<String, String> mapping2) {
return mapping1.getKey().compareTo(mapping2.getKey());
}
});
for (Map.Entry<String, String> mapping : mappingList) {
System.out.println(mapping.getKey() + ":" + mapping.getValue());
}
}
```


##将Map中的key按插入顺序输出
```
HashMap并不能保证按插入的顺序，LinkedHashMap是在HashMap的基础上维护了一个链表，保存你的插入顺序。
Map<String, Integer> map = new LinkedHashMap<String, Integer>();
map.put("fileNum", new Integer(fileNum));
map.put("lineNum", new Integer(lineNum));
map.put("blankNum", new Integer(blankNum));
map.put("commentNum", new Integer(commentNum));
map.put("codeNum", new Integer(codeNum));
```