##reset命令
```
reset命令有3种方式：
    git reset --mixed：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
    git reset --soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
    git reset --hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容


以下是一些reset的示例：

(1) 回退所有内容到上一个版本  
git reset HEAD^  
(2) 回退a.py这个文件的版本到上一个版本  
git reset HEAD^ a.py  
(3) 向前回退到第3个版本  
git reset –soft HEAD~3  
(4) 将本地的状态回退到和远程的一样  
git reset –hard origin/master  
(5) 回退到某个版本  
git reset 057d  
(7) 回退到上一次提交的状态，按照某一次的commit完全反向的进行一次commit  
Git revert HEAD   


在本地开发了一个版本，然后加入某些代码， git commit 之后再 git push 与远程版本库同步，这时出现一个问题，在这次 git commit 之前的版本内容如何找回？
首先git log显示提交的历史
[plain] view plain copy print?
commit ee50348120302b19318ab6a564d4092dd87a85ef  
Author: ShichaoXu <gudujianjsk@gmail.com>  
Date:   Mon Jun 3 15:18:16 2013 +0800  
  
    support for printf  
  
commit e7a5e492c742a7b68c07f124edd4b713122c0666  
Author: ShichaoXu <gudujianjsk@gmail.com>  
Date:   Tue May 7 15:44:11 2013 +0800  
  
    del file lib/2440slib.s init/2440init.s  
  
commit 5a05f002ef1dfbbf118b2ffd7b829159b727ce16  
Author: ShichaoXu <gudujianjsk@gmail.com>  
Date:   Tue May 7 15:26:30 2013 +0800  
  
    change file name lib/2440slib.s init/2440init.s to lib/2440slib.S init/2440init.S  
  
commit a948e62c9eabd54bfc4de3c4dfd14b4fc2bb48dd  
Author: ShichaoXu <gudujianjsk@gmail.com>  
Date:   Tue May 7 15:06:17 2013 +0800  
  
    add code for this project  
  
commit 59a902efdef8fb3dd47264df8a666de7026d1595  
Author: ShichaoXu <gudujianjsk@gmail.com>  
Date:   Mon May 6 23:15:01 2013 -0700  
  
    Initial commit  


然后用 
[plain] view plain copy print?
~/gun-ucos$$git reset --hard e7a5e492c742a7b68c07f124edd4b713122c0666  

显示如下
[plain] view plain copy print?
HEAD is now at e7a5e49 del file lib/2440slib.s init/2440init.s  

此时正常回到git commit    "support for printf" 之前的状态！

参考链接：
http://blog.163.com/jianlizhao@126/blog/static/173251163201183045856216/
http://liuhui998.com/2010/10/22/recover_lost_commits_with_git/
```
##push失败问题
```
先说HEAD

HEAD是一个头指针，通常情况下指向不同的分支，每个分支对应一个commit（准确的说，每个分支对应多个commit，但是只有一个顶层的commit，而commit之间是简单的线性关系。）

git checkout 其实是修改HEAD文件的内容，让它指向不同的分支。

下面是一个一般的情况：

HEAD (refers to branch 'master')

                       |

                       v

           a---b---c  branch 'master' (refers to commit 'c')

既然checkout是修改HEAD，所以可以出现下面的情况：

HEAD (refers to commit 'b')

                 |

                 v

           a---b---c  branch 'master' (refers to commit 'c')

HEAD指向b，用git branch看看有几个分支：

×HEAD detached from b

master

发现有两个分支，可是我们没有创建除了master以外的任何分支啊～

可以把HEAD detached from b理解为一个临时的分支，并且这时候HEAD指针是游离普通分支之外的。

在这个临时分支上可以进行git的一切操作：add commit 等等，像这样：

  HEAD (refers to commit 'f')

                      |

                      v

                 e---f

                /

           a---b---c  branch 'master' (refers to commit 'c')

假如远程库中有一个master分支，一个用来开发的develop分支，这时候如果我们要向远程库推送，会发现无法推送。

因为HEAD不知道要把内容推送到哪个远程分支上去。

那么问题来了，怎么把修改的内容提交到远程库呢？

由于本地没有develop分支，所以需要先这样：

git fetch origin develop:develop

在本地创建一个develop分支，并且把它和远程develop关联起来。现在再看看本地有哪些分支：

git branch

×develop

master

刚才的HEAD detached from b分支消失了！ 在这个分支下修改的内容也不见了！

没关系，进行下一步。

git reflog show HEAD@{now} -10

这个命令会把HEAD指针所有的动作显示出来。从中可以清楚的看到，在指针中提交对应的commit id

找到需要恢复的commit ，记下前面的commit id

git branch temp efa64f5 

新建一个名字叫temp的分支，用这个分支代替之前的临时分支并且拥有想要恢复的commit，现在切换到temp下会发现一切都回来了

但是还是不能推送啊。原因是temp是我们本地的分支，远程库中并没有这个分支。

git checkout develop

切换到从远程库拉取到的develop分支

git merge temp

将temp分支合并到develop分支上，有冲突就解决冲突。

最后：

git push origin develop

OK，推送到远程库。

```
##idea 在导入github项目时，do you want to add this host to know hosts database
```
ssh -T git@github.com

Permission denied (publickey).
$ ssh-keygen -t rsa -C "adanac@sina.com"
路径选择 : 使用该命令之后, 会出现提示选择ssh-key生成路径, 这里直接点回车默认即可, 生成的ssh-key在默认路径中;
密码确认 : 这里我们不使用密码进行登录, 用密码太麻烦;就一路回车下去
将ssh配置到GitHub中
在mac os X 下前往文件夹，/Users/自己电脑用户名/.ssh。
windows应该是（C:\Documents and Settings\Administrator\.ssh （或者 C:\Users\自己电脑用户名\.ssh）中）。
然后用记事本打开id_rsa.pub，将里面的全部代码复制到github的SSH中。
登陆github网站，点击Settings——SSH keys——点击右侧的Add SSH key
验证是否配置成功 :
$ ssh -T git@github.com
Hi adanac! You've successfully authenticated, but GitHub does not provide shell access.
``` 
## Automatic merge failed
```

$ git pull
Auto-merging pom.xml
CONFLICT (content): Merge conflict in pom.xml
Automatic merge failed; fix conflicts and then commit the result.
```

##Git Error
```

Q ： $ hexo deploy
ERROR  Deployer  not found :  git
A : npm install hexo - deployer - git  -- save

Q: Updates were rejected because the tip of your current branch is behind
A:  原因：自己分支版本低于主版本  解决：$ git push -u  joe prod- 2295 - 1  -f  


Q: hexo deploy 没反应
A:  你的配置文件写的有问题，type和repo后面都要有一个空格
```
##git hint: Updates were rejected because the remote contains work that you do
git remote 
git remote -v
git remote rm [主机名]
git init
git add .
git commit -m""
git push [新的主机名] master

##Could not open a connection to your authentication agent.
You might need to start ssh-agent before you run the ssh-add command:
eval `ssh-agent -s`
ssh-add

##Error: ERROR: Permission to zhongxin007/zhongxin007.github.io.git denied to adanac.

