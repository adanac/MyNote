## mysql  如何让now()精确到毫秒？
```
SELECT CURRENT_TIMESTAMP(3);
SELECT CURRENT_TIMESTAMP(3),NOW(3);
```
##常用sql
```
一些重要的mysql语句用法
1.增加一个字段(一列)

alter table table_name add column column_name type default value;   type指该字段的类型,value指该字段的默认值

例如:alter table mybook add column publish_house varchar(10) default '';

2.更改一个字段名字(也可以改变类型和默认值)

alter table table_name change sorce_col_name dest_col_name type default value;   source_col_name指原来的字段名称,dest_col_name指改后的字段名称

例如:alter table Board_Info change IsMobile IsTelphone int(3) unsigned default 1;

3.改变一个字段的默认值

alter table table_name alter column_name set default value;

例如:alter table book alter flag set default '0';

4.改变一个字段的数据类型

alter table table_name change column column_name column_name type;

例如:alter table userinfo change column username username varchar(20);

5.向一个表中增加一个列做为主键

alter table table_name add column column_name type auto_increment PRIMARY KEY;

例如:alter table book add column id int(10) auto_increment PRIMARY KEY;

6.数据库某表的备份,在命令行中输入:

mysqldump -u root -p database_name table_name > bak_file_name

例如:mysqldump -u root -p f_info user_info > user_info.dat

7.导出数据

select_statment into outfile"dest_file";

例如:select cooperatecode,createtime from publish limit 10 into outfile"/home/mzc/temp/tempbad.txt";

8.导入数据

load data infile"file_name" into table table_name;

例如:load data infile"/home/mzc/temp/tempbad.txt" into table pad;

9.将两个表里的数据拼接后插入到另一个表里。下面的例子说明将t1表中的com2和t2表中的com1字段的值拼接后插入到tx表对应的字段里。

例如:insert into tx select t1.com1,concat(t1.com2,t2.com1) from t1,t2;

```
##mysql不支持select top n 语法
```
取前15条数据
select * from tablename order by orderfield desc/asc limit 0,15

``` ##数据库分库、分表
##修改表的字段的类型或长度
```
mysql> alter table 表名 modify column 字段名 类型;
例如
数据库中address表 city字段是varchar(30)
修改类型可以用（谨慎修改类型，可能会导致原有数据出错）
mysql> alter table address modify column city char(30);
修改长度可以用（修改长度，要保证不短与已有数据，以保证原有数据不出错）
mysql> alter table address modify column city varchar(50);
```

##修改主键类型(不会影响表中原来的数据)
```
修改表area_freight中的主键at_id int(11)为varchar(40)
/*首先去掉自增长属性*/
ALTER TABLE area_freight CHANGE at_id at_id INT;
/*去掉主键*/
ALTER TABLE area_freight DROP PRIMARY KEY;
/*修改主键类型为varchar*/
ALTER TABLE area_freight CHANGE at_id at_id VARCHAR(40);
/*最后重新添加主键*/
ALTER TABLE area_freight ADD PRIMARY KEY (at_id);
```
##查询订单表中的字段main_order_id不在主订单表（数据比订单表数据多）记录
```
SELECT COUNT(*) FROM main_order //36
SELECT COUNT(*) FROM sale_order //27
/**记录数多的表放在前面**/
SELECT mo.`MAIN_ORDER_ID` FROM main_order mo WHERE mo.`MAIN_ORDER_ID` NOT IN (SELECT main_order_id FROM sale_order)/**ok**/
SELECT mo.main_order_id FROM main_order mo WHERE mo.main_order_id NOT IN (SELECT main_order_id FROM sale_order)/**OK**/
SELECT mo.`MAIN_ORDER_ID` FROM main_order mo WHERE mo.`MAIN_ORDER_ID` NOT IN (SELECT MAIN_ORDER_ID FROM sale_order)
SELECT mo.`MAIN_ORDER_ID` FROM main_order mo LEFT JOIN sale_order so ON so.`MAIN_ORDER_ID` = mo.MAIN_ORDER_ID WHERE so.MAIN_ORDER_ID IS NULL
SELECT mo.main_order_id FROM  main_order mo WHERE (SELECT COUNT(1) AS num FROM sale_order so WHERE so.`MAIN_ORDER_ID` = mo.`MAIN_ORDER_ID`) = 0
```
##mysql 列转行
group_concat




