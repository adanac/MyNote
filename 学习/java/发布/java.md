##打包并发布源码
##将普通java工程代码打成jar包

- 在eclipse下，右键工程 -> export -> jar文件即可。
- 1.写个最简单的test.java，package都不需要，只有一个mian函数输出文字用。 2.创建一个MANIFEST.MF文件，里面加上内容：
Manifest-Version: 1.0
Main-Class: test
确保（java文件与.MF文件）这两个文件在同一目录。
3.javac test.java
4.jar cvfm test.jar MANIFEST.MF test.class
5.java -jar test.jar

##将maven工程代码打包并发布
```
方案一：用Eclipse自带的Export功能


步骤1：准备主清单文件 “MANIFEST.MF”，


由于是打包引用了第三方jar包的Java项目，故需要自定义配置文件MANIFEST.MF，在该项目根目录下建立文件MANIFEST.MF，内容如下：
Manifest-Version: 1.0
Class-Path: lib/hamcrest-core-1.1.jar lib/fastjson-1.2.3.jar
Main-Class: com.adanac.demo.queryCard.QueryCard


第一行是MAINIFEST的版本，第二行Class-Path就指定了外来jar包的位置，第三行指定我们要执行的MAIN java文件。


导出到一个文件夹下，比如：E:\learn\querycard
在此文件夹下建立java工程项目中所建立的source folder.如 conf、res、lib等文件夹,
建立好文件夹后，再将queryCard.jar导入到此文件夹下。


运行：E:\learn\querycard>java -jar querycard.jar


优化：新建一个批处理文件，如start.bat，内容为：java -jar queryCard.jar，放在jar文件同一目录下即可，以后点击自动运行即可，更加方便。
```