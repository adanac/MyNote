##触发器语法
```


触发器(trigger)：监视某种情况，并触发某种操作。

触发器创建语法四要素：1.监视地点(table) 2.监视事件(insert/update/delete) 3.触发时间(after/before) 4.触发事件(insert/update/delete)

语法：

create trigger triggerName

after/before insert/update/delete on 表名

for each row   #这句话在mysql是固定的


begin

sql语句;

end;
```
##触发器优点
```
1，触发器的"自动性"

对程序员来说，触发器是看不到的，但是他的确做事情了，如果不用触发器的话，你更新了user表的name字段时，你还要写代码去更新其他表里面的冗余字段，我举例子，只是一张表，如果是几张表都有冗余字段呢，你的代码是不是要写很多呢，看上去是不是很不爽呢。
2，触发器的数据完整性
触发器有回滚性，举个例子，就是你要更新五张表的数据，不会出现更新了二个张表，而另外三张表没有更新。
但是如果是用php代码去写的话，就有可能出现这种情况的，比如你更新了二张表的数据，这个时候，数据库挂掉了。你就郁闷了，有的更新了，有的没更新。这样页面显示不一致了，变有bug了。
``` ##触发器实例1
```
#创建表1
DROP TABLE IF EXISTS tab1;
CREATE TABLE tab1(
    tab1_id VARCHAR(11)
);


#创建表2
DROP TABLE IF EXISTS tab2;
CREATE TABLE tab2(
    tab2_id VARCHAR(11)
);


#增加tab1表记录后自动将记录增加到tab2表中
DROP TRIGGER IF EXISTS t_afterinsert_on_tab1;
CREATE TRIGGER t_afterinsert_on_tab1
AFTER INSERT ON tab1 FOR EACH ROW
INSERT INTO tab2 VALUES(new.tab1_id)


#测试
INSERT INTO tab1(tab1_id) VALUES('0001');
SELECT * FROM tab1;
SELECT * FROM tab2;


#删除tab1表记录后自动将tab2表中对应的记录删去
DROP TRIGGER IF EXISTS t_afterdelete_on_tab1;
CREATE TRIGGER t_afterdelete_on_tab1
AFTER DELETE ON tab1
FOR EACH ROW
DELETE FROM tab2 WHERE tab2_id=old.tab1_id;


#测试
DELETE FROM tab1 WHERE tab1_id='0001';
SELECT * FROM tab1;
SELECT * FROM tab2;




#商品表
DROP TABLE IF EXISTS product;
CREATE TABLE product(
 id INT PRIMARY KEY AUTO_INCREMENT,
 pname VARCHAR(50),
 pcount INT
);
INSERT INTO product(pname,pcount) VALUES ('商品1',10),('商品2',10),('商品3',10);


#订单表
DROP TABLE IF EXISTS myorder;
CREATE TABLE myorder(
 oid INT PRIMARY KEY AUTO_INCREMENT,
 pid INT,
 pcount INT
);


#创建触发器:往订单表中插入一条记录时，同时更新商品表中的pcount字段
DROP TRIGGER IF EXISTS t_afterinsert_on_myorder;
CREATE TRIGGER t_afterinsert_on_myorder
AFTER INSERT ON myorder
FOR EACH ROW
UPDATE product SET pcount=pcount-new.pcount WHERE id = new.pid;


#测试
INSERT INTO myorder(pid,pcount) VALUES(3,4);


#情况1：当用户撤销一个订单的时候，直接删除一个订单，同时要把对应的商品数量再加回去
#分析
#监视地点：myorder表
#监视事件：delete
#触发时间：after
#触发事件：update
#对于delete而言：原本有一行,后来被删除，想引用被删除的这一行，用old来表示，old.列名可以引用被删除的行的值


#创建触发器
DROP TRIGGER IF EXISTS t_afterdelete_on_myorder;
CREATE TRIGGER t_afterdelete_on_myorder
AFTER DELETE ON myorder
FOR EACH ROW
UPDATE product SET pcount=pcount+old.pcount WHERE id=old.pid;


#测试
DELETE FROM myorder WHERE oid = 2;


#情况2：当用户修改一个订单的数量时，我们触发器修改怎么写
#分析
#监视地点：myorder表
#监视事件：update
#触发时间：after
#触发事件：update
#对于update而言：被修改的行，修改前的数据，用old来表示，old.列名引用被修改之前行中的值；
#修改的后的数据，用new来表示，new.列名引用被修改之后行中的值。


#创建触发器
DROP TRIGGER IF EXISTS t_afterupdate_on_myorder;
CREATE TRIGGER t_afterupdate_on_myorder
AFTER UPDATE ON myorder
FOR EACH ROW
UPDATE product SET pcount = pcount+old.pcount-new.pcount WHERE id = old.pid;


#测试
INSERT INTO myorder(pid,pcount) VALUES (4,5);#买5个商品2
UPDATE myorder SET pcount = 1 WHERE oid = 3;#修改商品2的购买数量为1




##############before 和 after 的区别#######################
#情景1：
/*假设：假设商品表有商品1，数量是10；
我们往订单表插入一条记录：
insert into o(gid,much) values(1,20);
会发现商品1的数量变为-10了。这就是问题的所在.
因为我们之前创建的触发器是after,也就是说触发的语句是在插入订单记录之后才执行的，
这样我们就无法判断新插入订单的购买数量。
*/
#当新增一条订单记录时，判断订单的商品数量，如果数量大于10，就默认改为10
#delimiter $(意思是告诉mysql语句的结尾换成以$结束)
DROP TRIGGER IF EXISTS t_beforeinsert_on_myorder;
CREATE TRIGGER `t_beforeinsert_on_myorder`
BEFORE INSERT ON myorder FOR EACH ROW 
BEGIN IF new.pcount>10 THEN SET new.pcount=10;
UPDATE product SET pcount = pcount-new.pcount WHERE id = new.pid;
END;
```
##old和new关键字
```
OLD和NEW不区分大小写
在INSERT触发程序中，仅能使用NEW.col_name，没有旧行。
在DELETE触发程序中，仅能使用OLD.col_name，没有新行。
在UPDATE触发程序中，可以使用OLD.col_name，也能使用NEW.col_name
用OLD命名的列是只读的。你可以引用它，但不能更改它。
对于用NEW命名的列，如果具有SELECT权限，可引用它。
在BEFORE触发程序中，如果你具有UPDATE权限，可使用“SET NEW.col_name = value”更改它的值。
这意味着，你可以使用触发程序来更改将要插入到新行中的值，或用于更新行的值。
```
##触发器的执行顺序
```
我们建立的数据库一般都是 InnoDB 数据库，其上建立的表是事务性表，也就是事务安全的。这时，若SQL语句或触发器执行失败，MySQL 会回滚事务，有：
①如果 BEFORE 触发器执行失败，SQL 无法正确执行。

②SQL 执行失败时，AFTER 型触发器不会触发。
③AFTER 类型的触发器执行失败，SQL 会回滚。
```


## MYSQL 触发器 设置变量 
```
需求：当触发器动作触发时，可以动态设置变量生成sql语句存储到数据库， 
如：a表发生触发器动作时，向b表（字段：id(int),content(varchar)）字段content插入内容为"insert into c(name) values(动态值)";


实现：
#创建表1
DROP TABLE IF EXISTS ta;
CREATE TABLE ta(
    aid INT PRIMARY KEY AUTO_INCREMENT,
    aname VARCHAR(50)
);
#创建表2
DROP TABLE IF EXISTS tb;
CREATE TABLE tb(
    bid INT PRIMARY KEY AUTO_INCREMENT,
    content VARCHAR(255)
);
#增加ta表记录后自动将记录增加到tb表中
DROP TRIGGER IF EXISTS t_afterinsert_on_ta;
CREATE TRIGGER t_afterinsert_on_ta
AFTER INSERT ON ta FOR EACH ROW
INSERT INTO tb(content) VALUES(CONCAT("insert into ta(aname) values('",new.aname,"')"));
#测试
INSERT INTO ta(aname) VALUES('lucy4')
SELECT * FROM ta;
SELECT * FROM tb;


#删除ta表记录
DROP TRIGGER IF EXISTS t_afterdelete_on_ta;
CREATE TRIGGER t_afterdelete_on_ta
AFTER DELETE ON ta FOR EACH ROW
INSERT INTO tb(content) VALUES(CONCAT("delete from ta where aid='",old.aid,"'"));
#测试
DELETE FROM ta WHERE aid='2';
SELECT * FROM ta;
SELECT * FROM tb;


#修改ta表中的记录
DROP TRIGGER IF EXISTS t_afterupdate_on_ta;
CREATE TRIGGER t_afterupdate_on_ta
AFTER UPDATE ON ta
FOR EACH ROW
INSERT INTO tb(content) VALUES(CONCAT("update ta set aname='",new.aname,"' where aid='",new.aid,"'"));
#测试
UPDATE ta SET aname='jessy1' WHERE aid='6'


```