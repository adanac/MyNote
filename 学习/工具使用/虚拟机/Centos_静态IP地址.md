
```



1.登陆连接centos系统，输入 ifconfig 可以查看到当前本机的IP地址信息

 

2.临时设置IP地址:

输入 ifconfig eth0 （默认是第一个网卡） 后面接IP地址， 网络掩码和 网关，如果不设置，就使用默认的掩码，如：

ifconfig eth0 192.168.187.16

注：注意这种方法修改只是临时修改，重启网卡或服务器后又会还原

 

3.永久生效修改静态IP

首先关闭VMware的DHCP：

Edit->Virtual Network Editor



```

 



```  

选择VMnet8，去掉使用本地DHCP服务将IP地址分配给虚拟机（Use local DHCP service to distribute IP address to VMs）

记下子网IP和子网掩码，点击NAT设置（NAT Settings）查看一下GATEWAY地址：

```  






```



设置CentOS静态IP：

 

涉及到三个配置文件，分别是：




<span style="font-family: 'Microsoft YaHei';">/etc/sysconfig/network                                        

/etc/sysconfig/network-scripts/ifcfg-eth0

/etc/resolv.conf

</span>

 

 

首先修改 vi /etc/sysconfig/network如下：




<span style="font-family: 'Microsoft YaHei';">NETWORKING=yes

HOSTNAME=localhost.localdomain

GATEWAY=192.168.187.2

</span>

 

　　

 

然后修改 vi  /etc/sysconfig/network-scripts/ifcfg-eth0：

 

DEVICE="eth0"

#BOOTPROTO="dhcp"

BOOTPROTO="static"

IPADDR=192.168.187.166<br>BROADCAST=192.168.187.255

NETMASK=255.255.255.0

HWADDR="06:1C:19:56:1C:AD"

IPV6INIT="no"

NM_CONTROLLED="yes"

ONBOOT="yes"

TYPE="Ethernet"

UUID="ck19a3c1-co3d-v305-19bc-239b01691c11"

DNS1=8.8.8.8

 

注意：这里DNS1是必须要设置的否则无法进行域名解析。

最后配置下 vi /etc/resolv.conf：




nameserver 8.8.8.8

其实这一步可以省掉，上面设置了DNS Server的地址后系统会自动修改这个配置文件。

 

网络服务重启一下， service network restart

静态IP设置成功！ 



```

 