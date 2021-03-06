[TOC]
##Maven将项目发布到私服
```
1 . 修改私服中仓库的部署策略
Release版本的项目应该发布到Releases仓库中，对应的，Snapshot版本应该发布到Snapshots仓库中。Maven根据pom.xml文件中版本号<version>节点的属性是否包含-SNAPSHOT，来判断该项目是否是snapshot版本。如果是snapshot版本，在执行mvn  deploy部署命令时，maven会自动将项目发布到Snapshots仓库。要发布项目，首先需要将Releases仓库和Snapshots仓库的“Deployment Policy”设置为“Allow Redeploy”：

2 . 配置项目的部署仓库
在pom.xml中分别对Release版本和Snapshot版本配置部署仓库，其中id唯一，url分别对应私服中Releases和Snapshots仓库的Repository Path：
<url>http://localhost:8081/nexus/content/repositories/releases/</url>  
<url> http://localhost:8081/nexus/content/repositories/snapshots/</url>  
<uniqueVersion>表示是否为Snapshot版本分配一个包含时间戳的构建号，效果如下：

ssm-pom.xml
<!-- 部署到私服  -->
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

在settings.xml中分别为上面配置的部署仓库配置server，
其中id需要分别对应上面的部署仓库id：
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
<mirror>
      <id>myRepository</id>
 <!-- 匹配所有远程仓库 -->
      <!--<mirrorOf>*</mirrorOf>-->
 <!-- 匹配所有远程仓库，使用localhost的除外:external:* -->
 <mirrorOf>external:*</mirrorOf>
      <name>hzw maven server.</name>
      <url>http://maven.haiziwang.com:8081/nexus/content/groups/public</url>
    </mirror>

<!-- mine-->
   <mirror>
      <id>adanac_maven_repo</id>
 <!--<mirrorOf>*,!myRepository</mirrorOf>-->
 <mirrorOf>*</mirrorOf>
      <name>adanac maven server.</name>
      <url>http://localhost:8081/nexus/content/groups/public</url>
    </mirror>
  </mirrors>

<profiles>
    <profile>
        <id>myProfile</id>
        <repositories>
            <repository>
                <id>myRepository</id>
                <name>Repository for me</name>
                <url>http://maven.haiziwang.com:8081/nexus/content/groups/public</url>
            </repository>
        </repositories>
    </profile>
<profile>
<id>adanac_dev</id>
<repositories>
<repository>
<id>adanac_maven_repo</id>
<url>http://localhost:8081/nexus/content/groups/public</url>
<releases>
<enabled>true</enabled>
</releases>
<snapshots>
<enabled>true</enabled>
</snapshots>
</repository>
</repositories>
<pluginRepositories>
                <pluginRepository>
                    <id>adanac_maven_repo</id>
                    <url>http://localhost:8081/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                   </snapshots>
                </pluginRepository>
            </pluginRepositories>
</profile>
  </profiles>
<activeProfiles>
    <activeProfile>myProfile</activeProfile>
<activeProfile>adanac_dev</activeProfile>
  </activeProfiles>

4 . 发布项目
　　右键pom.xml - Run As - 2 Maven build...
clean deploy..
发布成功后，在私服的仓库中就能看到了：

5 . 在Nexus中手动上传项目构件
　　在Nexus仓库的Artifact Upload选项卡中，填写相关信息，可以手动的方式上传项目构件
```