1.   概述

ZooKeeper是 Hadoop 的正式子项目，它是一个针对大型分布式系统的可靠协调系统，提供的功能包括：配置维护、名字服务、分布式同步、组服务等。ZooKeeper的目标就是封装好复杂易出错的关键服务，将简单易用的接口和性能高效、功能稳定的系统提供给用户。

 

  2.   安装&配置

在apache的官方网站提供了好多镜像下载地址，然后找到对应的版本，目前最新的是3.3.6

下载地址：

http://mirrors.cnnic.cn/apache/zookeeper/zookeeper-3.3.6/zookeeper-3.3.6.tar.gz




Windows下安装






把下载的zookeeper的文件解压到指定目录

D:\machine\zookeeper-3.3.6>




修改conf下增加一个zoo.cfg

内容如下：

# The number of milliseconds of each tick  心跳间隔 毫秒每次

tickTime=2000

# The number of ticks that the initial

# synchronization phase can take

initLimit=10

# The number of ticks that can pass between

# sending a request and getting anacknowledgement

syncLimit=5

# the directory where the snapshot isstored.  //镜像数据位置

dataDir=D:\\data\\zookeeper

#日志位置

dataLogDir=D:\\logs\\zookeeper

# the port at which the clients willconnect  客户端连接的端口

clientPort=2181

注：如果启动有报错提示cfg文件有错误，可以用zoo_sample.cfg内内容替代也是可以的






进入到bin目录，并且启动zkServer.cmd，这个脚本中会启动一个 Java 进程

D:\machine\zookeeper-3.3.6>cd bin

D:\machine\zookeeper-3.3.6\bin>

D:\machine\zookeeper-3.3.6\bin >zkServer.cmd



启动后jps可以看到QuorumPeerMain的进程

D:\machine\zookeeper-3.3.6\bin >jps






启动客户端运行查看一下

D:\machine\zookeeper-3.3.6\bin>zkCli.cmd  -server 127.0.0.1:2181







这个时候zookeeper已经安装成功了，

参考官方文档：

http://zookeeper.apache.org/doc/trunk/zookeeperStarted.html

 参考 单机模式、 集群和伪集群的帖子 http://sqcjy111.iteye.com/blog/1741320





在 一台机器上通过伪集群运行时可以修改 zkServer.cmd 文件在里面加入

set ZOOCFG=..\conf\zoo1.cfg  这行，另存为  zkServer-1.cmd








如果有多个可以以此类推







 











 


 


还需要 在对应的

/tmp/zookeeper/1，

/tmp/zookeeper/2，

/tmp/zookeeper/3

 建立一个文本文件命名为myid，内容就为对应的zoo.cfg里server.后数字

  

  




