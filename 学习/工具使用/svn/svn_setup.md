##想重新在windows里部署svn服务
```
1.删除svn服务：
管理员权限下运行cmd：
“sc delete svnserver”

2.再部署svn的服务：
“sc create svnserver binpath= "C:\Program Files\TortoiseSVN\bin\svnserve.exe --service" displayname= "SVN Server" depend= Tcpip start= auto”

3.此时报错 “指定的服务已标记为删除”。
一般这种问题是由于在命令行中删除时，服务管理窗口处于打开的状态，或者被删除的服务正处于开启状态。
前一种情况的解决：
确保服务已经停止，然后关闭服务管理窗口，重新执行删除命令即可
第二种情况的解决：
打开服务管理窗口，停用当前正在运行的服务，之后这个服务会自动删除。

```