##alimaven
```
建议别用maven的默认仓库
alimaven，谁用谁知道
<mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
```
##使用阿里云的MAVEN镜像
```
修改本地maven setting.xml文件，内容如下。

<mirrors>
   <mirror>
     <id>alimaven</id>
     <name>aliyun maven</name>
     <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
     <mirrorOf>central</mirrorOf>        
   </mirror>
</mirrors>
```
##easyreport的中央仓库地址
https://oss.sonatype.org/content/repositories/snapshots/com/easytoolsoft/