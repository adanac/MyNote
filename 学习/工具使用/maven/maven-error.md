[TOC]
## mvn deploy Failed to execute goal
```
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy (default-deploy) on project model-store-repo: Deployment failed: repository element was not specified in the POM inside distributionManagement element or in -DaltDeploymentRepository=id::layout::url parameter -> [Help 1]
[ERROR]

原因：没有在pom.xml中指定distributionManagement.

<distributionManagement>
    <repository>
        <id>adanac-nexus-releases</id>
        <name>adanac Release Repository</name>
        <url>http://localhost:8081/nexus/content/repositories/releases/</url>
    </repository>
    <snapshotRepository>
        <id>adanac-nexus-snapshots</id>
        <name>adanac Snapshot Repository</name>
        <url>http://localhost:8081/nexus/content/repositories/snapshots/</url>
    </snapshotRepository>
 </distributionManagement>

注意：如果要指定distributionManagement，前提要设置.settings.xml，添加对应的server.
<servers>
<!--mine-->
  <server>
    <id>adanac-nexus-releases</id>
    <username>admin</username>
    <password>admin123</password>
  </server>
  <server>
    <id>adanac-nexus-snapshots</id>
    <username>admin</username>
    <password>admin123</password>
  </server>
</servers>


```

## mvn package Failed to execute goal
```
Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2.10:test (default-test) on project model-store-repo: Unable to generate classpath: org.apache.maven.artifact.resolver.MultipleArtifactsNotFoundException: Missing:
[ERROR] ----------
[ERROR] 1) org.apache.maven.surefire:surefire-junit4:jar:2.10

原因是：本地的私服中没有surefire-junit4-2.10.jar

解决一：
这是idea自带的打包插件，通过 右键pom文件Maven->Show Effective Pom可以看到pom有引用这个插件，
如何改变idea自带的插件或插件的版本就可以解决这个问题。

解决二：
换用eclipse打包这个项目就可以了。eclipse与idea机制不同，没有自带这个插件。

```
##Could not resolve dependencies for project was cached in the local repository
```
本地仓库中的包是不完整的。

解决方法：到.m2/repository/的目录下把对应的包删除掉，再重新创建项目并配置pom.xml或是右键原项目-maven-update project
```
##Could not transfer artifact
```
Failure to transfer javax.persistence:persistence-api:jar:2.0.0.Final from http://localhost:8081/nexus/content/groups/public was cached in the local repository, resolution will not be reattempted until the update interval of adanac_maven_repo has elapsed or updates are forced. Original error: Could not transfer artifact javax.persistence:persistence-api:jar:2.0.0.Final from/to adanac_maven_repo (http://localhost:8081/nexus/content/groups/public): Connection refused: connect

org.eclipse.aether.transfer.ArtifactTransferException: Failure to transfer javax.persistence:persistence-api:jar:2.0.0.Final from http://localhost:8081/nexus/content/groups/public was cached in the local repository, resolution will not be reattempted until the update interval of adanac_maven_repo has elapsed or updates are forced. Original error: Could not transfer artifact javax.persistence:persistence-api:jar:2.0.0.Final from/to adanac_maven_repo (http://localhost:8081/nexus/content/groups/public): Connection refused: connect


解决方法：
对nexus 中Public Repositories 先执行“Rebuild Metadata”，再执行“Update Index".

```
##Maven乱码问题解决
```
1.编译乱码，设置编译的字符集编码和环境编码 
<plugin> 
        <groupId>org.apache.maven.plugins</groupId> 
        <artifactId>maven-compiler-plugin</artifactId> 
        <version>2.3.2</version> 
        <configuration> 
                <source>1.7</source> 
                <target>1.7</target> 
                <encoding>UTF-8</encoding> 
        </configuration> 
</plugin> 
设置环境变量MAVEN_OPTS=-Xms128m -Xmx512m -Dfile.encoding=UTF-8 

2.运行mvn test时乱码（IDE上运行TestCase时OK，但是运行maven 
test乱码,结果测试不通过）修改pom.xml增加如下内容即可 
<plugin> 
        <groupId>org.apache.maven.plugins</groupId> 
        <artifactId>maven-surefire-plugin</artifactId> 
        <version>2.7.2</version> 
        <configuration> 
                <forkMode>once</forkMode> 
                <argLine>-Dfile.encoding=UTF-8</argLine> 
                <systemProperties> 
                        <property> 
                                <name>net.sourceforge.cobertura.datafile</name> 
                                <value>target/cobertura/cobertura.ser</value> 
                        </property> 
                </systemProperties> 
        </configuration> 
</plugin> 

3.  除了上面的方法外，还有个更简单的方法
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <argLine>-Dfile.encoding=UTF-8</argLine>
  </properties>

```
##mvn install source-1.5 中不支持 diamond运算符
```
执行mvn 报错 source-1.5 中不支持 diamond运算符
原因：编译版本出现了问题

解决：将1.5版本换为1.7
//<plugins> 
//<plugin> 
//<groupId>org.apache.maven.plugins</groupId> 
//<artifactId>maven-compiler-plugin</artifactId> 
//<configuration> 
//<source>1.7</source> 
//<target>1.7</target> 
//</configuration> 
//</plugin> 
//</plugins>
```
##Failed to execute goal org.apache.maven.plugins
```
Failed to execute goal org.apache.maven.plugins:maven-javadoc-plugin:2.10.3:jar (attach-javadocs) on project MavenReportException: Error while generating Javadoc:
有可能是插件的版本不兼容
```
##No compiler is provided in this environment
```
No compiler is provided in this environment. Perhaps you are running on a JRE rather than a JDK?

下载java jdk，并安装java jdk。下载地址：http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html

在eclipse的菜单中，进入 Window > Preferences > Java
Installed JREs > Execution Environments，选择JavaSE-1.6, 在右侧选择jdk.

然后在maven菜单中使用 “update project ...”.

来源：http://blog.csdn.net/fox_lht/article/details/16369147
```
##failure to transfer org.apache.maven.plugins:maven-jar-plugin:pom:2.4 from 
```
Failure to transfer org.apache.maven.plugins:maven-surefire-plugin:pom:2.7.1 from http://repo1.maven.org/maven2 was cached in the local repository, resolution will not be reattempted until the update interval of central has elapsed or updates are forced

原因是maven的plugin并未下载到本地.

查看 
~\.m2\repository\org\apache\maven\plugins\maven-surefire-plugin\2.7.1

的话,会发现里面只有一个

maven-surefire-plugin-2.7.1.pom.lastUpdated 的文件.
而并没有maven-surefire-plugin-2.7.1.jar

解决办法:
1.删除所有以lastUpdated结尾的文件
2.右键点击project -> Maven - Update Dependencies （Update Project...）

如果点击更新后发现

~\.m2\repository\org\apache\maven\plugins\maven-surefire-plugin\2.7.1 下还是没有maven-surefire-plugin-2.7.1.jar
去http://repo1.maven.org/maven2/org/apache/maven/plugins/maven-surefire-plugin/2.7.1/
下载

maven-surefire-plugin-2.7.1.jar maven-surefire-plugin-2.7.1.jar.sha1 maven-surefire-plugin-2.7.1.pom maven-surefire-plugin-2.7.1.pom.sha1
包再重复步骤2.
```
##maven install 时，Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:2.3.2:compile
```
方案一：
 The  <plugin>  tag has  aspectj-maven-plugin  included when it tries to run the plugin the dependency is not found which is the aspectjrt.jar file. If you are using the aspectj-maven-plugin then you need to add the following dependency in your pom.xml,
<!-- AspectJ -->
<dependency>
<groupId>
org.aspectj
</groupId>
<artifactId>
aspectjrt
</artifactId>
<version>
${aspectj.version}
</version>
</dependency>

 where the  aspectj.version  is mentioned in the tag as,
<aspectj.version>
1.8.1
</aspectj.version>

 来源：  http://stackoverflow.com/questions/14433569/classpath-error-unable-to-find-org-aspectj-lang-joinpoint


 方案二：
 右键项目->MAVEN->Update Project Configuration
 然后clean相关项目
 再打包

 如果还不行   在你关联包的路径下  把所有文件删掉  在打包的时候会重新下载（有可能是冲突或没有下载到最新的）
```
##The declared package does not match the expected package
```
错误的原因是：
eclipse中包的定义一般是通过package包名产生，而不是通过文件的层次来定义。eclipse使用import导入源码时，导入的是文件结构而不是包形式，故报错。

解决方法：
点击> properties > java build path > source > add folder > select src/XXXX
```
##mvn install 程序包org.junit不存在
```
```
##maven update 工程项目时 to enable the nesting exclude "main/"
```
点击改工程的Properties属性选中Java Build Path项，找到source，发现有两个错误的包，将其选中remove掉；
重新在项目下创建一个source folder；folder name:src/main/java
```
##mvn install deploy时 maven编码 gbk 的不可映射字符
```
由于代码使用的UTF-8，而maven编译的时候使用的GBK的缘故。 通过修改pom文件，可以告诉maven这个项目使用UTF-8来编译。在pom的/project/build/plugins/下的编译插件声明 中加入下面的配置：

< encoding > utf8 </ encoding >
< plugin >   
     < groupId > org.apache.maven.plugins </ groupId >   
     < artifactId > maven-compiler-plugin </ artifactId >   
     < version > 2.3.1 </ version >   
     < configuration >   
         < source > 1.6 </ source >   
         < target > 1.6 </ target >   
         < encoding > utf8 </ encoding >     
     </ configuration >   
</ plugin >
```
##mvn update 项目时，cannot nest /src/main/resources inside
```
有可能是私服配置不正确，.setting文件中，<mirrorOf>external:*</mirrorOf>配置映射不能重复。
mvn update ---> mvn clean...即可。
```
##mvn deploy 时 401
```
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy (default-deploy) on project adanac-framework-common: Failed to deploy artifacts: Could not transfer artifact com.adanac.framework:adanac-framework-common:jar:1.0.0 from/to nexus-releases (http://localhost:8081/nexus/content/repositories/releases/): Access denied to http://localhost:8081/nexus/content/repositories/releases/com/adanac/framework/adanac-framework-common/1.0.0/adanac-framework-common-1.0.0.jar. Error code 401, Unauthorized ->
原因：有可能项目中的id与.settting.xml中的id不一致导致的。
1.项目中配置
<distributionManagement>
         <repository>
            <id>adanac-nexus-releases</id>
            <name>adanac Release Repository</name>
            <url>http://localhost:8081/nexus/content/repositories/releases/</url>
        </repository>
        <snapshotRepository>
            <id>adanac-nexus-snapshots</id>
            <name>adanac Snapshot Repository</name>
            <url>http://localhost:8081/nexus/content/repositories/snapshots/</url>
        </snapshotRepository>      </distributionManagement>
2..setting.xml中的配置
<server> <id>adanac-nexus-releases</id> <username>admin</username> <password>admin123</password> </server> <server> <id>adanac-nexus-snapshots</id> <username>admin</username> <password>admin123</password> </server> <server> <id>adanac-thirdparty</id> <username>admin</username> <password>admin123</password> </server>
```
##mvn install时 Could not find artifact org.apache.maven.surefire
```
Could not find artifact org.apache.maven.surefire:maven-surefire-common:jar:2.17 in adanac_maven_repo ( http://localhost:8081/nexus/content/groups/public)

I had similar issue, I was able to solve it using -U option along with mvn command as
mvn clean install -U
``` 
##mvn install 时Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:2.3.2:compile
```
原因：首先有可能项目的compliler与project faces和maven-compiler-plugin的版本不一致导致 ，其次mvn命令运行时，用的是jre的javac，而不是jdk的javac命令。
解决方法：把eclipse的jre目录改成jdk 的（*把maven运行的jre也改为jdk的就好了）【强烈推荐】
```


```
adanac-cache/pom.xml:
<plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <configuration>
                        <source>1.6</source>
                        <target>1.6</target>
                        <encoding>UTF-8</encoding>
                        <!-- 
                        <compilerArgs>
                            <arg>-XX:MaxPermSize=512M</arg>
                        </compilerArgs> -->
                    </configuration>                 </plugin>


adanac-parent/pom.xml:
<plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>${maven_compiler_plugin_version}</version>
                    <configuration>
                        <fork>true</fork>
                        <source>${java_source_version}</source>
                        <target>${java_target_version}</target>
                        <encoding>${file_encoding}</encoding>
                        <compilerArgs>
                            <arg>-XX:MaxPermSize=512M</arg>
                        </compilerArgs>
                    </configuration>                 </plugin>
<!-- for maven compiler plugin -->
        <maven_compiler_plugin_version>2.3.2</maven_compiler_plugin_version>
        <java_source_version>1.8</java_source_version>
        <java_target_version>1.8</java_target_version>         <file_encoding>UTF-8</file_encoding>
```


##eclipse中使用maven插件的时候，运行run as maven build的时候报错:
```
-Dmaven.multiModuleProjectDirectory system propery is not set. Check $M2_HOME environment variable and mvn script match.
直接的解决方法：使用低版本的maven
另一个方法是：可以设一个环境变量M2_HOME指向你的maven安装目录
然后在Window->Preference->Java->Installed JREs->Edit
在Default VM arguments中设置 -Dmaven.multiModuleProjectDirectory=$M2_HOME
```
##解决“Dynamic Web Module 3.0 requires Java 1.6 or newer.”错误：
``` 在项目的pom.xml的<build></build>标签中加入 <plugins> <plugin> <groupId>org.apache.maven.plugins</groupId> <artifactId>maven-compiler-plugin</artifactId> <version>2.3.2</version> <configuration> <source>1.6</source> <target>1.6</target> </configuration> </plugin> </plugins> ```

##web.xml is missing and <failOnMissingWebXml> is set to true	pom.xml
```
当jar变为war时，
Right click on Deployment Descriptor in Project Explorer. Choose Java EE Tool -> Select Generate Deployment Descriptor Stub. It will generate WEB_INF folder in src/main/webapp and an web.xml in it.

```
##mvn deploy时 400
```
 Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy (default-deploy) on project adanac-dac: Failed to deploy artifacts: Could not transfer artifact com.adanac.framework:adanac-dac:pom:1.0.0 from/to adanac-nexus-releases (http://192.168.1.162:8081/nexus/content/repositories/releases/): Failed to transfer file: http://192.168.1.162:8081/nexus/content/repositories/releases/com/adanac/framework/adanac-dac/1.0.0/adanac-dac-1.0.0.pom. Return code is: 400, ReasonPhrase: Bad Request 
原因是 release 默认库是不允许重复部署的,修改图中配置就可以重复部署了
Deployment Policy:Allow Redeploy 

```
##Plugin execution not covered by lifecycle configuration
```
比如：
Plugin execution not covered by lifecycle configuration: com.mycila.maven-license-plugin:maven-license-plugin:1.9.0:format (execution: default, phase: compile) pom.xml /latke-repository-redis line 17 Maven Project Build Lifecycle Mapping Problem

解决：在<plugins>标签外包上<pluginManagement>。
<build>
      <pluginManagement>
        <plugins>
            <plugin>
                <groupId>com.mycila.maven-license-plugin</groupId>
                <artifactId>maven-license-plugin</artifactId>
                <configuration>
                    <header>../src/main/resources/etc/header.txt</header>
                </configuration>
            </plugin>
        </plugins>
      </pluginManagement>
    </build>

```
##maven error 打不了包
```
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2.10:test (default-test) on project model-store-repo: Unable to generate classpath: org.apache.maven.artifact.resolver.ArtifactResolutionException: Unable to get dependency information for org.apache.maven.surefire:surefire-junit4:jar:2.10: Failed to retrieve POM for org.apache.maven.surefire:surefire-junit4:jar:2.10: Could not transfer artifact org.apache.maven.surefire:surefire-junit4:pom:2.10 from/to myRepository (http://maven.haiziwang.com:8081/nexus/content/groups/public): Connect to maven.haiziwang.com:8081 [maven.haiziwang.com/172.172.177.240] failed: Connection timed out: connect
[ERROR] org.apache.maven.surefire:surefire-junit4:jar:2.10

看来是由于连接不上maven私服导致找不到 surefire-junit4.jar导致的。
```