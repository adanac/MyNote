[TOC]
##idea建立maven项目卡
<img src="images/maven_block.png" height="305" width="800">
```
另外可以使用国内的maven仓库, 速度快.
<mirror><id>alimaven</id><name>aliyun maven</name><url>http://maven.aliyun.com/nexus/content/groups/public/</url><mirrorOf>central</mirrorOf></mirror>
来源： https://segmentfault.com/q/1010000006945188
```
##解决IDEA建立web项目卡的问题
方法一:
在Properties中添加一个参数archetypeCatalog=internal。
New Module -> 输入 GroupId,ArtifactId,Version -> 
不加这个参数，在maven生成骨架的时候将会非常慢，有时候会直接卡住。

archetypeCatalog表示插件使用的archetype元数据，不加这个参数时默认为remote，local，即中央仓库archetype元数据，由于中央仓库的archetype太多了，所以导致很慢，指定internal来表示仅使用内部元数据。
<img src="images/maven_block2.png" height="589" width="743">

方法二:
用idea创建maven项目时 不要勾选 Create from archetype这个选项，其实archetype就是项目骨架模板，无非是帮你生成一个简单的maven工程。但是天朝的网络你懂的。。。想省这点时间还不如创建一个新的maven项目，然后自己根据需要建立包结构。10秒内保证可以创建好。

##IDEA的查询引用、调用关系图的功能
[参考1:推荐](http://www.cnblogs.com/ghj1976/p/5382455.html)
[参考2](http://blog.sina.com.cn/s/blog_72ef7bea0102vbai.html)
##展示
```
1.展示右侧窗口，
方法1.你点击一下你idea界面最左下角的那个小框，maven应该从里面找到
方法2.点击菜单栏View->Tool  Windows->Maven projects 
方法3.点击菜单栏Help->Find Action(Ctrl+Shift+A),输入Maven projects
```
##idea Error:java: Compilation failed: internal java compiler error
```
ctrl + alt + s -> Build,Execution,Deployment -> compile -> java compiler -> 设置jre
```
##idea java:-source 1.5中不支持 diamond 运算符
1.检查ide的默认编译环境 ，快捷键ctrl + alt +s
找Java Compiler ，发现设置是 Target bytencode version 是1.6 改成1.7 
Build -> Compiler -> Java Compiler 。
2.检查项目的SDK选择和项目语言level 都改成1.7 和7
Project Struct -> Project Settins -> Project
3.检查项目的 Modules 中的language level 改成 7 
Project Struct -> Project Settins -> Modules
##运行maven项目，程序包org..junit不存在
解决：
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.10</version>
      <scope>test</scope>
    </dependency>
  改成
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.10</version>
    </dependency>
##idea 创建项目 idea pom.xml already exists in vfs

##exploded: Error during artifact deployment. See server log for details.



