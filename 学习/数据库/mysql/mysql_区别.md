##change / modify
```
例如：要把一个INTEGER列的名称从a变更到b，您需要如下操作：
 mysql> ALTER TABLE t1 CHANGE a b INTEGER;
更改列的类型而不是名称， CHANGE语法仍然要求旧的和新的列名称，即使旧的和新的列名称是一样的。例如：
 mysql> ALTER TABLE t1 CHANGE b b BIGINT NOT NULL;
使用MODIFY来改变列的类型，此时不需要重命名：
mysql> ALTER TABLE t1 MODIFY b BIGINT NOT NULL;


alter table test MODIFY id INT UNSIGNED AUTO_INCREMENT;


当需要修改字段名称时使用change；当需要修改字段类型时使用modify，毕竟modify还是比change少写个字段名称的
```
##having / where
```
having 是对聚合后的结果进行条件的过滤，而 where是在聚合前就对记录进行过滤，如果逻辑允许，我们尽可能的用 where 先过滤记录，从而结果集减小，将对聚合的效率大大提高。
实例一：既要统计各部门人数，又要统计总人数
select deptno,count(1) from emp group by deptno with rollup;
```

##in / exist
```
select from A
where id in(select id from B)


以上查询使用了in语句,in()只执行一次,它查出B表中的所有id字段并缓存起来.之后,检查A表的id是否与B表中的id相等,如果相等则将A表的记录加入结果集中,直到遍历完A表的所有记录.
它的查询过程类似于以下过程


List resultSet=[];
Array A=(select from A);
Array B=(select id from B);
for(int i=0;i<A.length;i++) {
   for(int j=0;j<B.length;j++) {
      if(A[i].id==B[j].id) {
         resultSet.add(A[i]);
         break;
      }
   }
}
return resultSet;


可以看出,当B表数据较大时不适合使用in(),因为它会B表数据全部遍历一次.
如:A表有10000条记录,B表有1000000条记录,那么最多有可能遍历100001000000次,效率很差.
再如:A表有10000条记录,B表有100条记录,那么最多有可能遍历10000100次,遍历次数大大减少,效率大大提升.


结论:in()适合B表比A表数据小的情况


select a. from A a 
where exists(select 1 from B b where a.id=b.id)


以上查询使用了exists语句,exists()会执行A.length次,它并不缓存exists()结果集,因为exists()结果集的内容并不重要,重要的是结果集中是否有记录,如果有则返回true,没有则返回false.
它的查询过程类似于以下过程


List resultSet=[];
Array A=(select from A)
for(int i=0;i<A.length;i++) {
   if(exists(A[i].id) {    //执行select 1 from B b where b.id=a.id是否有记录返回
       resultSet.add(A[i]);
   }
}
return resultSet;


当B表比A表数据大时适合使用exists(),因为它没有那么遍历操作,只需要再执行一次查询就行.
如:A表有10000条记录,B表有1000000条记录,那么exists()会执行10000次去判断A表中的id是否与B表中的id相等.
如:A表有10000条记录,B表有100000000条记录,那么exists()还是执行10000次,因为它只执行A.length次,可见B表数据越多,越适合exists()发挥效果.
再如:A表有10000条记录,B表有100条记录,那么exists()还是执行10000次,还不如使用in()遍历10000100次,因为in()是在内存里遍历比较,而exists()需要查询数据库,我们都知道查询数据库所消耗的性能更高,而内存比较很快.


结论:exists()适合B表比A表数据大的情况


当A表数据与B表数据一样大时,in与exists效率差不多,可任选一个使用.

EXISTS与IN的使用效率的问题，通常情况下采用exists要比in效率高，因为IN不走索引，但要看实际情况具体使用：
IN适合于外表大而内表小的情况；EXISTS适合于外表小而内表大的情况。
```
##in 和exists

in是把外表和内表作hash 连接，而exists 是对外表作loop 循环，每次loop 循环再对内表进行查询。

一直以来认为exists 比in 效率高的说法是不准确的。如果查询的两个表大小相当，那么用in 和exists 差别不大。
如果两个表中一个较小，一个是大表，则子查询表大的用exists，子查询表小的用in：

例如：

表A（小表），表B（大表）1：
select * from A where cc in (select cc from B)
效率低，用到了A 表上cc 列的索引；
 

select * from A where exists(select cc from B where cc=A.cc)
效率高，用到了B 表上cc 列的索引。

相反的2：
select * from B where cc in (select cc from A)
效率高，用到了B 表上cc 列的索引；

select * from B where exists(select cc from A where cc=B.cc)
效率低，用到了A 表上cc 列的索引。

not in 和not exists
如果查询语句使用了not in 那么内外表都进行全表扫描，没有用到索引；

而not extsts 的子查询依然能用到表上的索引。所以无论那个表大，用not exists 都比not in 要快。





