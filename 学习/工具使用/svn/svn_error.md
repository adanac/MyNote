## Synchronize operation failed.
svn: E200030: There are unfinished transactions detected 
```
First, try:


Right-click the project -> Team -> Cleanup.
If that didn't help:


Restart Eclipse -> Team -> Cleanup
```
##svn: E175002: connection refused by the server
```
Eclipse preferences. click on General>Network Connections -> change Active Provider value to Manual from the drop down. it worked for me too.
```
###svn commit fail out of date
```
1. 先备份out of date的新文件
2. 对out of date文件执行revert操作
3. 再执行update操作
4. 用刚才备份的新文件覆盖掉旧有的文件
5. 再次执行commit，ok.
``` ###svn locked 怎么解决
```
方法一.直接进行cleanup；对较小的文件比较管用，文件稍大些等待时间很长或不起作用；
方法二.选择文件，右键执行release lock；等待时间较长；
方法三.手动删除锁定文件：
 1.在运行中输入cmd进入命令行； 2.在命令提示符下cd 到svn项目出现问题的文件所在目录下； 3.执行命令del lock /q/s 4.等待删除lock文件成功，重新更新SVN。
注：最好关闭下eclipse并杀掉进程后，重启eclipse.
```