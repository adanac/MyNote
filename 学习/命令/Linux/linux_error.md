##The remote system refused the connection
```
有可能是22端口没有开
service sshd status
openssh-daemon (pid  1873) is running...看ssh服务有没有开启，没有的话开启service sshd start。

解决：开启sshd服务
service sshd start.

想让他开机自动启动，就chkconfig sshd on。
另外看iptables -L看ssh服务有没有被禁用，iptable服务可以用iptables -F进行关闭。
``` 
##A protocol error occurred. Change of username or service not allowed: (root,ssh-connection) -> (roota,ssh-connection) 
```
当通过secrt登录时出现的，原因是用户名不存在。
```
##bash: cmake: command not found
```
检查本机有没有安装cmake
`which cmake`
/usr/bin/which: no cmake in (/usr/lib/qt-3.3/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/jdk1.7.0_55/bin:/root/bin)
- 安装cmake
`yum install cmake` (*本机为centos6.5*)
```
##[Errno 256] No more mirrors to try.
```
`yum clean all && yum clean metadata && yum clean dbcache && yum makecache && yum update -y`
```