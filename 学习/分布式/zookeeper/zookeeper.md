##linux 防火墙是否关闭
```
Linux下查看、关闭及开启防火墙命令

1)永久性生效，重启后不会复原 
开启： chkconfig iptables on 关闭： chkconfig iptables off 
2)即时生效，重启后复原 
开启： service iptables start 关闭： service iptables stop 
需要说明的是对于Linux下的其它服务都可以用以上命令执行开启和关闭操作。 
在开启了防火墙时，做如下设置，开启相关端口， 修改/etc/sysconfig/iptables 文件，添加以下内容： 
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT 
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
3)查看防火墙状态
chkconfig iptables --list


```
http://blog.csdn.net/congcong68/article/details/41113239
##查看linux中是否安装tomcat
```
rpm -qa|grep tomcat
1.查看Tomcat进程
执行命令$ps -ef|grep tomcat 你就能找出tomcat占据的进程号

2.查看Tomcat占据的端口
执行命令$netstat -nat能列出tomcat占据的端口，8080及其它类似的端口是需要注意的。

3.查看tomcat所在目录
执行命令#find / -name tomcat,系统将列出所有tomcat为名的目录，进入目录后就能查清楚了。同理可以用find / -name startup.sh去找tomcat启动文件。
```
## 某个目录下所有文件拷贝到别的目录
```
把/home/a下的所有文件拷贝到/usr/local/src下： 
cp /home/a/* /usr/local/src/a

```
## Linux下Tomcat重新启动
```
首先，进入Tomcat下的bin目录
cd /usr/local/tomcat/bin
使用Tomcat关闭命令
./shutdown.sh

查看Tomcat是否以关闭
ps -ef|grep java
如果显示以下相似信息，说明Tomcat还没有关闭
root      
7010
     1  0 Apr19 ?        00:30:13 /usr/local/java/bin/java -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties -Djava.awt.headless=
true
 -Dfile.encoding=UTF-8 -server -Xms1024m -Xmx1024m -XX:NewSize=256m -XX:MaxNewSize=256m -XX:PermSize=256m -XX:MaxPermSize=256m -XX:+DisableExplicitGC -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djava.endorsed.dirs=/usr/local/tomcat/endorsed -classpath /usr/local/tomcat/bin/bootstrap.jar -Dcatalina.base=/usr/local/tomcat -Dcatalina.home=/usr/local/tomcat -Djava.io.tmpdir=/usr/local/tomcat/temp org.apache.catalina.startup.Bootstrap start

*如果你想直接干掉Tomcat，你可以使用kill命令，直接杀死Tomcat进程

 kill -9 
7010

然后继续查看Tomcat是否关闭

 ps -ef|grep java

如果出现以下信息，则表示Tomcat已经关闭
root      7010     1  0 Apr19 ?        00:30:30 [java] <defunct>
最后，启动Tomcat
 ./startup.sh 

注意：使用root用户登录Linux系统；正确进入Tomcat目录；在确定Tomcat关闭之后再启动Tomcat，否则会报端口被占用异常。
```
## Linux查看程序端口占用情况
```
今天发现服务器上Tomcat 8080端口起不来，老提示端口已经被占用。

使用命令：

ps -aux | grep tomcat

发现并没有8080端口的Tomcat进程。

使用命令：netstat –apn

查看所有的进程和端口使用情况。发现下面的进程列表，其中最后一栏是PID/Program name 

发现8080端口被PID为9658的Java进程占用。

进一步使用命令：ps -aux | grep java，或者直接：ps -aux | grep pid 查看

就可以明确知道8080端口是被哪个程序占用了！然后判断是否使用KILL命令干掉！

方法二：直接使用 netstat   -anp   |   grep  portno
即：netstat –apn | grep 8080
```
## linux重命名文件
```
例子：将目录A重命名为B

mv A B

例子：将/a目录移动到/b下，并重命名为c

mv /a /b/c

 

其实在文本模式中要重命名文件或目录的话也是很简单的，我们只需要使用mv命令就可以了，比如说我们要将一个名为abc的文件重命名为1234就可以这样来写：mv abc 1234，但是要注意的是，如果当前目录下也有个1234的文件的话，我们的这个文件是会将它覆盖的
```
##linux查找当前文件中的字符串
```
方法一：
 可以使用vim打开文件，然后通过 vim编辑 中的 /（向后查找）或者 ?（向前查找）来查找相应的字符串。
示例：用vim打开/etc/passwd查找admin用户名
vim /etc/passwd
打开文件后，直接输入 /admin 回车即可

方法二：
grep 字符串 文件名

```
##linux查看某个端口是否打开
```
netstat -nupl //n表示用数字形式显示端口号，u，表示UDP协议类型，p是程序PID，l表示处于监听状态的；

# netstat -nupl|grep 8080  udp协议类型

# netstat -ntpl|grep 8080  tcp协议类型

```
##linux拷贝文件到当前目录
```
cp /home/xy/abc.txt .
.   当前目录
..  上级目录
~  用户家目录
-   前一个目录
```


##Linux下安装Zookeeper
```
一、Zookeeper下载
[root@localhost 下载]# wget http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.3.6/zookeeper-3.3.6.tar.gz    
2016-01-15 23:17:07 (170 KB/s) - 已保存 “zookeeper-3.3.6.tar.gz” [11833706/11833706])  


二、解压
[root@localhost deploy]# tar -zxvf /home/lk/下载/zookeeper-3.3.6.tar.gz   
解压完之后，会在deploy文件夹下面得到一个zookeeper-3.3.6的文件夹


三、进入到conf目录
[root@localhost deploy]# cd /opt/deploy/zookeeper-3.3.6/conf  


四、拷贝zoo_samle.cfg为zoo.cfg
[root@localhost conf]# cp zoo_sample.cfg zoo.cfg  


五、编辑zoo.cfg文件
[root@localhost conf]# vi zoo.cfg   
修改为：
# The number of milliseconds of each tick  
tickTime=2000  
# The number of ticks that the initial  
# synchronization phase can take  
initLimit=10  
# The number of ticks that can pass between  
# sending a request and getting an acknowledgement  
syncLimit=5  
# the directory where the snapshot is stored.  
dataDir=/usr/zookeeper  
dataLogDir=/usr/zookeeper/log  
# the port at which the clients will connect  
clientPort=2181  
server.1=192.168.32.129:2888:3888  




六、设置环境变量
vi /etc/profile
[root@localhost conf]# export ZOOKEEPER_INSTALL=/opt/deploy/zookeeper-3.3.6  
[root@localhost conf]# export PATH=$PATH:$ZOOKEEPER_INSTALL/bin  


七、启动
[root@localhost bin]# ./zkServer.sh start  
JMX enabled by default  
Using config: /opt/deploy/zookeeper-3.3.6/bin/../conf/zoo.cfg  
Starting zookeeper ... STARTED  
[root@localhost bin]#   


八、测试zookeeper
[root@localhost bin]# ./zkCli.sh -server 192.168.32.129:2181  
如果是本地连接，那么不需要 -server 192.168.32.129:2181,默认是本地


注意：如果出现拒绝连接，请检查如下：
1、防火墙是否关闭  systemctl stop firewalld
2、需要将192.168.32.129 映射到本地 /etc/hosts文件中，否则无法连接
```
##zookeeper的使用
```
[zk: 127.0.0.1:2181(CONNECTED) 1] ls /
[zookeeper]
[zk: 127.0.0.1:2181(CONNECTED) 2] create /xiaobai hello
Created /xiaobai
[zk: 127.0.0.1:2181(CONNECTED) 3] get /xiaobai
hello
cZxid = 0x2
ctime = Wed May 29 16:13:14 CST 2013
mZxid = 0x2
mtime = Wed May 29 16:13:14 CST 2013
pZxid = 0x2
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 5
numChildren = 0
[zk: 127.0.0.1:2181(CONNECTED) 4] set /xiaobai world
cZxid = 0x2
ctime = Wed May 29 16:13:14 CST 2013
mZxid = 0x3
mtime = Wed May 29 16:13:33 CST 2013
pZxid = 0x2
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 5
numChildren = 0
[zk: 127.0.0.1:2181(CONNECTED) 5] get /xiaobai
world
cZxid = 0x2
ctime = Wed May 29 16:13:14 CST 2013
mZxid = 0x3
mtime = Wed May 29 16:13:33 CST 2013
pZxid = 0x2
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 5
numChildren = 0
[zk: 127.0.0.1:2181(CONNECTED) 6] delete /xiaobai
[zk: 127.0.0.1:2181(CONNECTED) 7] get /xiaobai
Node does not exist: /xiaobai
```
##ZooKeeper 典型的应用场景
###统一命名服务（Name Service）
Zookeeper 的 Name Service 与 JNDI 能够完成的功能是差不多的，它们都是将有层次的目录结构关联到一定资源上，你并不需要将名称关联到特定资源上，你可能只需要一个不会重复名称，就像数据库中产生一个唯一的数字主键一样。
调用 create 接口就可以很容易创建一个目录节点
###配置管理（Configuration Management）
同一个应用系统需要多台 PC Server 运行,将配置信息保存在 Zookeeper 的某个目录节点中，然后将所有需要修改的应用机器监控配置信息的状态，一旦配置信息发生变化，每台应用机器就会收到 Zookeeper 的通知，然后从 Zookeeper 获取新的配置信息应用到系统中。
###集群管理（Group Membership）
要一个“总管”知道当前集群中每台机器的服务状态，一旦有机器不能提供服务，集群中其它集群必须知道，从而做出调整重新分配服务策略。
###共享锁（Locks）
需要获得锁的 Server 创建一个 EPHEMERAL(临时)_SEQUENTIAL(连续) 目录节点，然后调用 getChildren方法获取当前的目录节点列表中最小的目录节点是不是就是自己创建的目录节点，如果正是自己创建的，那么它就获得了这个锁，如果不是那么它就调用exists(String path, boolean watch) 方法并监控 Zookeeper 上目录节点列表的变化，一直到自己创建的节点是列表中最小编号的目录节点，从而获得锁，释放锁很简单，只要删除前面它自己所创建的目录节点就行了。
###队列管理
Zookeeper 可以处理两种类型的队列：
1.当一个队列的成员都聚齐时，这个队列才可用，否则一直等待所有成员到达，这种是同步队列。
2.队列按照 FIFO 方式进行入队和出队操作，例如实现生产者和消费者模型。

同步队列用 Zookeeper 实现的实现思路如下：
创建一个父目录 /synchronizing，每个成员都监控标志（Set Watch）位目录 /synchronizing/start 是否存在，然后每个成员都加入这个队列，加入队列的方式就是创建 /synchronizing/member_i 的临时目录节点，然后每个成员获取 / synchronizing 目录的所有目录节点，也就是 member_i。判断 i 的值是否已经是成员的个数，如果小于成员个数等待 /synchronizing/start 的出现，如果已经相等就创建 /synchronizing/start。