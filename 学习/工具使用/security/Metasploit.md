##metasploit简介

```
    Metasploit是一款开源的安全漏洞检测工具，同时Metasploit是免费的工具，因此安全工作人员常用Metasploit工具来检测系统的安全性。Metasploit Framework (MSF) 在2003年以开放源码方式发布，是可以自由获取的开发框架。它是一个强大的开源平台，供开发，测试和使用恶意代码，这个环境为渗透测试、shellcode 编写和漏洞研究提供了一个可靠平台。
　　这种可以扩展的模型将负载控制(payload)、编码器(encode)、无操作生成器(nops)和漏洞整合在一起，使 Metasploit Framework 成为一种研究高危漏洞的途径。它集成了各平台上常见的溢出漏洞和流行的 shellcode ，并且不断更新。
目前的版本收集了数百个实用的溢出攻击程序及一些辅助工具，让人们使用简单的方法完成安全漏洞检测，即使一个不懂安全的人也可以轻松的使用它。当然，它并不只是一个简单的收集工具，提供了所有的类和方法，让开发人员使用这些代码方便快速的进行二次开发。
其核心中一小部分由汇编和C语言实现，其余由ruby实现。不建议修改汇编和C语言部分。
二、搭建metasploit环境


Windows环境下安装。
从官方网站http://www.metasploit.com/下载windows版本的安装版，直接安装即可。安装的版本是3.5.1。安装时需要注意以下两点：
在安装的时候要关闭杀毒软件。否则的话会导致杀毒软件和metasploit冲突，导致安装失败。
在控制面版——区域和语言选项——选择英文（美国）——高级选项卡中选择英文（美国）。因为在安装的时候，会进行检测，如果属于非英文地区会导致安装失败。


如果安装有杀毒软件，会经常提示在metasploit的安装目录下检测到病毒或木马。
Linux下环境下安装。
官方网站提供了两种Linux下的安装方式，一种是打包好的metasploit安装包，如framework-3.5.1-linux-i686.run，里面包含了安装所需要的各种包，下载后直接在电脑上安装即可。安装的时候需要具有root权限。如果装有杀毒软件，在安装的时候需要关闭杀毒软件。
另一种是源码包方式，下载到本机后自己安装。需要事先安装各种所信赖的包，安装后需要进行一定的配置，较为麻烦。本例使用了源码包安装方式，因为之前安装了postsql，在使用framework-3.5.1-linux-i686.run安装时会报错已经安装好了postsql数据库等。使用windows下的metasploit时，学习到一定阶段后，感觉有些东西搞不明白，就安装了Linux版本下的metasploit来学习。


三、metasploit的使用


Metasploit目前提供了三种用户使用接口，一个是GUI模式，另一个是console模式，第三种是CLI（命令行）模式。原来还提供一种WEB模式，目前已经不再支持。目前这三种模式各有优缺点，建议在MSF console模式中使用。在console中几乎可以使用MSF所提供的所有功能，还可以在console中执行一些其它的外部命令，如ping。
Windows下GUI启动方式。从开始菜单——Metasploit Framework——Metaspliit GUI即可。,如下图所示：
                                                        
图1：metasploit GUI启动方式
其GUI模式启动后界面如图2所示：


图2：metasploit GUI启动后界面
Windows下console模式的启动方式与GUI方式类似，启动后界面如图3所示：


图3：metasploit console启动后界面


http://www.metasploit.cn/thread-828-1-1.html


```