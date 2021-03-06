[TOC]
##union合并两列且去除重复
```
select id1 from list
union
select id2 from list

并且可以过滤掉id1和id2联合起来的重复值
```
##union合并两列且不去除重复
```
select id1 from list 
union all 
select id2 from list
```
##两列字段，合并为一个字符串，中间加符号
```
数值型：
SELECT 
  CAST(a AS VARCHAR (10)) + '--' + CAST(b AS VARCHAR (10)) 
FROM
  tablename 

字符型(varchar)：
SELECT 
  a + '--' + b 
FROM
  tablename 

字符型(char):
SELECT 
  RTRIM(a) + '--' + RTRIM(b) 
FROM
  tablename 
```
##mysql/oracle复制一行数据（同一张表user[id,name,score]）
```

--无主键：
 mysql > INSERT INTO USER 
SELECT 
  * 
FROM
  USER 
WHERE id = 101 ;
 

--有主键：

mysql > INSERT INTO USER (NAME, score) 
SELECT 
  NAME,
  score 
FROM
  USER 
WHERE id = 101 ;
```


##mysql/oracle复制一行数据（两张表）
```
表user[id,name,score] 与 user_bak[id,name,score]
思路：先复制原来的表结构，再插入数据。

--复制表结构及其数据：
create table user_bak as select * from user

--只复制表结构：
create table user_bak as select * from user where 1=2;

或者：
create table user_bak like user 

--只复制表数据：

  
--如果两个表结构一样

insert into user_bak select * from table_name_old

  
--如果两个表结构不一样
INSERT INTO user_bak (col1, col2...) 
SELECT 
  col1,
  col2...
FROM
  USER 

--插入数据:
insert into user_bak select * from user
```
##oracle清空表数据
```
delete from t
truncate table t

区别：
--delete是dml操作；
truncate是ddl操作，ddl隐式提交不能回滚

--delete from t可以回滚，
truncate table t 不可以回滚

--truncate table t 执行效率更高，会回收表空间，
delete from t 执行效率慢，不会回收表空间

--truncate table t高水线下降，
delete from t高水线不降（这个不太明白...）

--增ID，TRUNCATE后从1开始，
DELETE后还是接着自增
```
##oracle恢复删除的数据
```
分为两种方法：
scn 和 时间戳两种方法恢复。

一、通过scn恢复删除且已提交的数据(假设表为emp)
1、获得当前数据库的scn号
select current_scn from v$database;
(切换到sys用户或system用户查询)
查询到的scn号为：1499223
　　
2、查询当前scn号之前的scn
select * FROM scott.emp AS of scn 1499220 ;
(确定删除的数据是否存在，如果存在，则恢复数据；如果不是，则继续缩小scn号)
　　
3、恢复删除且已提交的数据
flashback TABLE scott.emp TO scn 1499220 ;

二、通过时间恢复删除且已提交的数据
1、查询当前系统时间
SELECT 
  to_char (
    SYSDATE,
    'yyyy-mm-dd hh24:mi:ss'
  ) 
FROM
  DUAL ;
　　
2、查询删除数据的时间点的数据
SELECT 
  * 
FROM
  表名 AS of TIMESTAMP to_timestamp (
    '2013-05-29 15:29:00',
    'yyyy-mm-dd hh24:mi:ss'
  ) ;
(如果不是，则继续缩小范围)
　　
3、恢复删除且已提交的数据
flashback TABLE 表名 TO TIMESTAMP to_timestamp (
  '2013-05-29 15:29:00',
  'yyyy-mm-dd hh24:mi:ss'
) ;
　　　　
注意：如果在执行上面的语句，
出现错误ORA-08189:因为未启用行移动功能，不能闪回表。
         
可以尝试执行
alter table 表名 enable row movement;//允许更改时间
```
