[TOC]
##Properties文件的写入、读取、遍历
```
在Java开发中通常我们会存储配置参数信息到属性文件，这样的属性文件可以是拥有键值对的属性文件，向属性文件写入键值对，如何读取属性文件中的键值对，如何遍历属性文件。
1、向属性文件中写入键值对


特别注意：
Properties类调用setProperty方法将键值对保存到内存中，此时可以通过getProperty方法读取，propertyNames()方法进行遍历，但是并没有将键值对持久化到属性文件中，故需要调用store()方法持久化键值对到属性文件中，这里的store方法类似于Android SharedPreferences的commit()方法。
[java] view plain copy print?
package com.andieguo.propertiesdemo;  
  
import java.io.FileInputStream;  
import java.io.FileOutputStream;  
import java.io.IOException;  
import java.io.InputStream;  
import java.io.OutputStream;  
import java.util.Date;  
import java.util.Enumeration;  
import java.util.Properties;  
  
import junit.framework.TestCase;  
  
public class PropertiesTester extends TestCase {  
  
    public void writeProperties() {  
        Properties properties = new Properties();  
        OutputStream output = null;  
        try {  
            output = new FileOutputStream("config.properties");  
            properties.setProperty("url", "jdbc:mysql://localhost:3306/");  
            properties.setProperty("username", "root");  
            properties.setProperty("password", "root");  
            properties.setProperty("database", "bbs");//保存键值对到内存  
            properties.store(output, "andieguo modify" + new Date().toString());// 保存键值对到文件中  
        } catch (IOException io) {  
            io.printStackTrace();  
        } finally {  
            if (output != null) {  
                try {  
                    output.close();  
                } catch (IOException e) {  
                    e.printStackTrace();  
                }  
            }  
        }  
    }  
}  


执行单元测试后，属性文件内容如下：


2、读取属性文件中的键值对

public class PropertiesTester extends TestCase {  
  
    public void loadProperties() {  
        Properties properties = new Properties();  
        InputStream input = null;  
        try {  
            input = new FileInputStream("config.properties");//加载Java项目根路径下的配置文件  
            properties.load(input);// 加载属性文件  
            System.out.println("url:" + properties.getProperty("url"));  
            System.out.println("username:" + properties.getProperty("username"));  
            System.out.println("password:" + properties.getProperty("password"));  
            System.out.println("database:" + properties.getProperty("database"));  
        } catch (IOException io) {  
        } finally {  
            if (input != null) {  
                try {  
                    input.close();  
                } catch (IOException e) {  
                    e.printStackTrace();  
                }  
            }  
        }  
    }  
}  


执行单元测试方法，console输出的output如下：

3、遍历属性文件中的键值对

package com.andieguo.propertiesdemo;  
  
import java.io.IOException;  
import java.io.InputStream;  
import java.util.Enumeration;  
import java.util.Map.Entry;  
import java.util.Properties;  
import java.util.Set;  
  
import junit.framework.TestCase;  
  
public class PropertiesTester extends TestCase {  
  
    public void printAll() {  
        Properties prop = new Properties();  
        InputStream input = null;  
        try {  
            String filename = "config.properties";  
            input = getClass().getClassLoader().getResourceAsStream(filename);  
            if (input == null) {  
                System.out.println("Sorry, unable to find " + filename);  
                return;  
            }  
            prop.load(input);  
            //方法一：  
            Set<Object> keys = prop.keySet();//返回属性key的集合  
            for(Object key:keys){  
                System.out.println("key:"+key.toString()+",value:"+prop.get(key));  
            }  
            //方法二：  
            Set<Entry<Object, Object>> entrys = prop.entrySet();//返回的属性键值对实体  
            for(Entry<Object, Object> entry:entrys){  
                System.out.println("key:"+entry.getKey()+",value:"+entry.getValue());  
            }  
            //方法三：  
            Enumeration<?> e = prop.propertyNames();  
            while (e.hasMoreElements()) {  
                String key = (String) e.nextElement();  
                String value = prop.getProperty(key);  
                System.out.println("Key:" + key + ",Value:" + value);  
            }  
        } catch (IOException ex) {  
            ex.printStackTrace();  
        } finally {  
            if (input != null) {  
                try {  
                    input.close();  
                } catch (IOException e) {  
                    e.printStackTrace();  
                }  
            }  
        }  
    }  
      
}  


4、其他方法

public void list(PrintStream out)
将属性列表输出到指定的输出流。此方法对调试很有用。
public void storeToXML(OutputStream os,Stringcomment) throws IOException
发出一个表示此表中包含的所有属性的 XML 文档。
```


##遍历属性文件中的键值对
```
实例一：
1.  configcenter.properties
#ServerList--trunk
PromotionSerIp= 172.172.178.19
PromotionSerPort= 53101
ProductNaviSerIp= 172.172.178.20
ProductNaviSerPort= 53101
StockSerIp= 172.172.178.19
StockSerPort= 53101
AddressSerIp= 172.172.178.19
AddressSerPort= 53101
groupbuySerIp= 172.172.178.19
groupbuySerPort= 53101
couponSerIp= 172.172.178.19
couponSerPort= 53101
hotOrderIp= 172.172.178.19
hotOrderPort= 53101
getUserInfoIp= 172.172.178.24 getUserInfoPort= 53101   

2.model.self.properties
1
2
3
4
5  
3.com.haiziwang.jplugin.study.Util.PropertiesUtil
public   class  PropertiesUtil {
     private   static  Properties  properties  =  new  Properties();
     private   static  Properties  modelSelfProperties  =  new  Properties();
     private  PropertiesUtil() {
    }
     static  {
         try  {
            InputStream  propertiesIn  = PropertiesUtil. class .getResourceAsStream( "/config/configcenter.properties" );
             properties .load( propertiesIn );
           
            InputStream  isModel  = PropertiesUtil. class .getResourceAsStream( "/config/model.self.properties" );
            BufferedReader  bfModel  =  new  BufferedReader( new  InputStreamReader( isModel ,  "UTF-8" ));
             modelSelfProperties .load( bfModel );
        }  catch  (Exception  e ) {
             e .printStackTrace();
        }
    }
     public   static  String get(String  key ) {
        String  value  =  "" ;
         if  ( properties .containsKey( key )) {
             value  =  properties .getProperty( key );
        }
         return   value ;
    }
     public   static  String get(String  key , String  defaultValue ) {
         return   properties .getProperty( key ,  defaultValue );
    }
     public   static   void  reomve(String  key ) {
         properties .remove( key );
    }
     public   static  List<String> getAllModelSelf() {
        List<String>  res  =  new  LinkedList<String>();
        Set<Object>  keySet  =  modelSelfProperties .keySet();
         for  (Object  key  :  keySet ) {
             res .add( key .toString());
        }
         return   res ;
    }
     public   static   boolean  modelSelfPropertiesContainsKey(String  key ) {
         return   modelSelfProperties .containsKey( key );
     }
}   

4.调用
List<String>  modelSelfList  = PropertiesUtil. getAllModelSelf () ;
             for  ( int   i  = 0;  i  <  modelSelfList .size();  i ++) {
                Long  seller  = Long. parseLong ( modelSelfList .get( i ));
                 sellerVector .add( new  uint64_t( seller ));             }  
 

实例二：
package com.andieguo.propertiesdemo;  
  
import java.io.IOException;  
import java.io.InputStream;  
import java.util.Enumeration;  
import java.util.Map.Entry;  
import java.util.Properties;  
import java.util.Set;  
  
import junit.framework.TestCase;  
  
public class PropertiesTester extends TestCase {  
  
    public void printAll() {  
        Properties prop = new Properties();  
        InputStream input = null;  
        try {  
            String filename = "config.properties";  
            input = getClass().getClassLoader().getResourceAsStream(filename);  
            if (input == null) {  
                System.out.println("Sorry, unable to find " + filename);  
                return;  
            }  
            prop.load(input);  
            //方法一：  
            Set<Object> keys = prop.keySet();//返回属性key的集合  
            for(Object key:keys){  
                System.out.println("key:"+key.toString()+",value:"+prop.get(key));  
            }  
            //方法二：  
            Set<Entry<Object, Object>> entrys = prop.entrySet();//返回的属性键值对实体  
            for(Entry<Object, Object> entry:entrys){  
                System.out.println("key:"+entry.getKey()+",value:"+entry.getValue());  
            }  
            //方法三：  
            Enumeration<?> e = prop.propertyNames();  
            while (e.hasMoreElements()) {  
                String key = (String) e.nextElement();  
                String value = prop.getProperty(key);  
                System.out.println("Key:" + key + ",Value:" + value);  
            }  
        } catch (IOException ex) {  
            ex.printStackTrace();  
        } finally {  
            if (input != null) {  
                try {  
                    input.close();  
                } catch (IOException e) {  
                    e.printStackTrace();  
                }  
            }  
        }  
    }       
} 
```
##读取properties文件
```
1 文件系统加载 
InputStream in = new FileInputStream("config.properties");        
Properties p = new Properties();  
p.load(in);  
2 类加载方式 
A 与类同级目录 
InputStream in = Main.class.getResourceAsStream("config.properties");  
B 在类的下一级目录 
InputStream in =   
Main.class.getResourceAsStream("resource/config.properties");  
C 指定加载资源配置文件的classes相对路径 
InputStream in =   
Main.class.getResourceAsStream("/test/resource/config.properties");  
注意事项：如上以/开头的是指从根目录开始加载。 
D 使用类加载器的方式 
InputStream in = Main.class.getClassLoader().  
getResourceAsStream("test/resource/config.properties");  
E 资源配置文件在classes下 
InputStream in =   
Main.class.getClassLoader().getResourceAsStream("config.properties");  
```
##读取Src下的文件
```
public class TestClassLoader{
      private TestClassLoader loader = new TestClassLoader();
      private TestClassLoader(){
           BufferedReader reader = new BufferedReader(new InputStreamReader(this.getClass().getClassLoader().getResourceAsStream("hs_eraror.h")));
           String line = null;
           int index = -1;
           while ((line = reader.readLine()) != null){
                 index = line.indexOf("=");  // 某个指定的字符串值在字符串中首次出现的位置
                 // String str = "Hello world!";
                 // document.write(str.indexOf("Hello"); // 0
                 // document.write(str.indexOf("world")); // 6
           }
           System.out.println(index);
      }
}
```