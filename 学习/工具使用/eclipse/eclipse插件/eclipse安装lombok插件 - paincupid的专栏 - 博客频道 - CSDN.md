1、下载Lombok.jar http://projectlombok.googlecode.com/files/lombok.jar
2、运行Lombok.jar: java -jar  D:\001_software\work\Java\libs\lombok.jar
        数秒后将弹出一框，以确认eclipse的安装路径
3、确认完eclipse的安装路径后，点击install/update按钮，即可安装完成
4、安装完成之后，请确认eclipse安装路径下是否多了一个lombok.jar包，并且其
     配置文件eclipse.ini中是否 添加了如下内容:
           -javaagent:lombok.jar
           -Xbootclasspath/a:lombok.jar
     如果上面的答案均为true，那么恭喜你已经安装成功，否则将缺少的部分添加到相应的位置即可
5、重启eclipse或myeclipse
备注：添加了这个插件有可能会导致 eclipse起不来，修改eclipse.ini将 新添加的内容去除即可。


6、创建一个 Java 工程，建立如下类：


[java] view plain copy
print ?
import  lombok.Data;  
import  lombok.Getter;  
import  lombok.Setter;  
  
@Data   
public   class  DataObject {  
      private  String id;     
      @Setter @Getter   
      private  String name;     
      private  String userId;     
      private  String password;    
}  

import lombok.Data;
import lombok.Getter;
import lombok.Setter;

@Data
public class DataObject {
	 private String id;   
	 @Setter@Getter
	 private String name;   
	 private String userId;   
	 private String password;  
}


在使用 lombok 注解的时候记得要导入 lombok.jar 包到工程，如果使用的是 Maven Project ，要在 pom.xml 中添加依赖，并设置 Maven 为自动导入，参见IntelliJ部分。

1

2

3

4

5

<
dependency
>

    
<
groupId
>
org.projectlombok
</
groupId
>

    
<
artifactId
>
lombok
</
artifactId
>

    
<
version
>
1.16.8
</
version
>

</
dependency
>

 来源：  http://codepub.cn/2015/07/30/Lombok-development-guidelines/



