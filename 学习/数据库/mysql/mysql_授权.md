##java.sql.SQLException: Access denied for user 'root'@'allen1521' (using password: YES)
```

 GRANT ALL ON *.* TO 'root'@'allen1521' IDENTIFIED BY 'root';
 FLUSH PRIVILEGES;
```
##mysql授权用户以指定IP登录
```
1、指定特定IP
GRANT ALL on *.* to '登陆名'@'你的ip地址' identified by '你的密码';
 
2、把HOST字段改成 % ，表示任何（本地联网）地址的都可以用此帐号登录
GRANT ALL PRIVILEGES ON *.* TO '用户名'@'特定IP' IDENTIFIED BY '密码' WITH GRANT OPTION;


3.使权限生效：
FLUSH   PRIVILEGES;


```