[TOC]
##命令行查看创建表信息
show create table emp \G;
##改变表 change 命令
```
将age 改名为 age1 ,同时修改字段类型为int(4):
alter table emp change age age1 int(4);
```
##having  和 where 的区别
##左连接
```
如查询出emp表中所有的记录，如果 右表dept没有对应记录则显示为"";
select ename , deptname from emp e left join dept d on e.deptno = d.deptno;//展示出emp表中所有的记录
等同于
select ename , deptname from dept  d right join emp e on .d.d eptno = e.deptno;
```
##修改表
```
将新增的字段birth date 加在 ename后
alter table emp add birth date after ename;
修改字段age ，将它放到最前面
alter table emp modify age int(3) first;
##zerofill的使用
create table t1(id1 int,id2 int(5));
SELECT IF(1, 'true', 'false');//TRUE
ALTER TABLE tab3 MODIFY age INT ZEROFILL;
CREATE TABLE t1(id1 INT,id2 INT(5));
INSERT INTO t1 VALUES(1,1);
ALTER TABLE t1 MODIFY id1 INT ZEROFILL;
ALTER TABLE t1 MODIFY id2 INT(5) ZEROFILL;
/**
       id1     id2  
----------  --------
0000000001     00001
0000000001     00001
**/


INSERT INTO t1 VALUES(1,22222222);//id2显示了正确的值
```
##小数的表示
```
CREATE TABLE `t1`(
id1 FLOAT(5,2) DEFAULT NULL,
id2 DOUBLE(5,2) DEFAULT NULL,
id3 DECIMAL(5,2) DEFAULT NULL
)
INSERT t1 VALUES(1.234,1.234,1.23);//只有id3可以正常显示 
/**将字段的精度和标度全部去掉**/
ALTER TABLE t1 MODIFY id1 FLOAT;
ALTER TABLE t1 MODIFY id2 DOUBLE;
ALTER TABLE t1 MODIFY id3 DECIMAL;


FIELD   TYPE           COLLATION  NULL    
------  -------------  ---------  ------  
id1     FLOAT          (NULL)     YES           
id2     DOUBLE         (NULL)     YES             
id3     DECIMAL(10,0)  (NULL)     YES                  
```
##位与时间类型
```
CREATE TABLE t2 (
id1 BIT(1) DEFAULT NULL
)
INSERT INTO t2 VALUES(1);
id1     
--------
b'1'  
SELECT BIN(id1),HEX(id1) FROM t2;
BIN(id1)  HEX(id1)  
--------  ----------
1         1    
INSERT INTO t2 VALUES(2);
错误代码： 1406
DATA too LONG FOR COLUMN 'id1' AT ROW 1
ALTER TABLE T2 MODIFY ID1 BIT(2);
INSERT INTO t2 VALUES(2);
ID1     
--------
b'1'    
b'10' 


CREATE TABLE t (d DATE ,t TIME ,dt DATETIME);
INSERT INTO t VALUES(NOW(),NOW(),NOW());
d           t         dt                   
----------  --------  ---------------------
2016-10-15  17:43:42  2016-10-15 17:43:42  
DROP TABLE t;
CREATE TABLE t(id1 TIMESTAMP);
INSERT INTO t VALUES (NULL);
```

##timestamp 和 datetime的区别


##算术运算符


##between


##逻辑运算符


##round()四舍五入和truncate()截断
 
##日期和时间函数


##流程函数
create table salary (userid int,salary decimal(9,2));
select if(salary>2000,'high','low') from salary;


select ifnull(salary,0) from salary;
##存储引擎


##两种方法查询当前数据库支持的数据引擎
```
/**方式一**/
SHOW ENGINES;
/**方式二**/
SHOW VARIABLES LIKE 'have%'


```


##各种存储引擎的特性





##merge 表


##选择合适的存储引擎


##合成索引




##浮点数与定点数的区别
```
DROP TABLE IF EXISTS `t` ;
CREATE TABLE t(f FLOAT(8,1));
DESC t;
INSERT INTO t VALUES(1.23456);
     f  
--------
     1.2




CREATE TABLE t(c1 FLOAT(10,2),c2 DECIMAL(10,2));
DESC t;
INSERT INTO t VALUES(131072.18,131072.18);
       c1  c2         
---------  -----------
131072.19  131072.18  
   
//131072.18   变成了 131072.19，这是浮点数特有的问题。因此在精度要求比较高的情况应用中（比如货币）要使用定点数而不是浮点数来保存数据。 
```
##日期类型


##字符集的修改




