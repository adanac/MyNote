## Git增加忽略文件
```
最简单的方法在项目根目录与.git目录同一位置创建一个文件: .gitignore
touch .gitignore
vi .gitignore
target
.settings
.classpath
.project

:wq

注:如果要忽略的文件已被git管理,需要先移除,命令如下:

e.g.:

git rm -r --cached  WebRoot/WEB-INF/classes/**/*

-r:递归

git commit

然后.gitignore中的忽略,起作用

from:
http://gitready.com/beginner/2009/01/19/ignoring-files.html

```



