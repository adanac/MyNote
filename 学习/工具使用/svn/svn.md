##svn 提交代码  比对 branch 与 trunk 分支
1.将项目对应的branch 、trunk分支 导出两个 项目到eclipse中
2.项目右击---> find/check out as ....
3.右击两个项目-->Compare With --> Each other即可。
```
##svn修改用户名、密码
```
办法一：在TortoiseSVN的设置对话框中，选择“已保存数据”，在“认证数据”那一行点击“清除”按钮，清楚保存的认证数据，再检出的时候就会重新跳出用户名密码输入框。

如果方法一不起作用，则可以采用方法二：

Tortoise的用户名密码等认证信息都是缓存在客户端文件系统的这个目录：

C:/Documents and Settings/Administrator/Application Data/Subversion/auth

删除auth下面的所有文件夹，重新连接远程服务器进行检出，对话框就会出现！
```
##svn查看某人某段时间所有修改的文件
```
svn log -v -r '{2015-07-03}:{2015-07-10}'|sed -n '1p; 2,/^-/d; /zhy/,/^-/p' > 1.txt 


svn log -v -r '{2015-10-01}:{2015-11-20}'|sed -n '1p; /guotong/,/^-/p'|sed -n '/M/,1p;/A/,1p'|awk '{print $2}'|sort|uniq > 3.txt
```
## 获取 SVN 一段时间内文件改动列表
```
通过 svn 命令行(TortoiseSVN不行, 需要先安装 svn 命令行工具)是可以获得这个列表的.
命令格式如下:
    svn diff -r REVNO:HEAD --summarize http://svn-url

例如
想检查从 724版本 开始到目前所有改动文件的列表
    svn diff -r 724:HEAD --summarize https://192.168.198.2/svn > changedfiles.txt
可以简写成这样
    svn diff -r 724 --summarize https://192.168.198.2/svn > changedfiles.txt

或者你只知道需要检查版本的日期, 这就相当于检查从 2015-05-06(上次封版日期) 开始到目前(此次发版日期)所有的文件改动
    svn diff -r {2015-05-06} --summarize https://192.168.198.2/svn > changedfiles.txt
或者日期区间
    svn diff -r {2015-05-04}:{2015-05-05} --summarize https://192.168.198.2/svn > changedfiles.txt

这样我们就能够实现自动化发布了...
注意： 需要先安装 svn 命令行工具
https://www.visualsvn.com/downloads/ （ Apache Subversion command line tools ）

来源：  https://www.douban.com/note/497853339/
``` ##回退到之前的版本
选中项目 --> totorsvn --> update to revision - show log -->选中要回退的版本即可。

