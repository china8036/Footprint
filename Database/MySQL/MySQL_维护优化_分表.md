- [MySQL 维护优化：分表](#mysql)
  - [1. 垂直分表](#1)
  - [2. 水平分表](#2)
  - [3. Refer Links](#3-refer-links)

# MySQL 维护优化：分表

## 1. 垂直分表

垂直拆分是指数据表列的拆分，把一张列比较多的表拆分为多张表：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/30/c3c4f40b11896abc54e4f7329fa949b8.jpg)

通常我们按以下原则进行垂直拆分：
- 把不常用的字段单独放在一张表。
- 把 text，blob 等大字段拆分出来放在附表中。
- 经常组合查询的列放在一张表中。

NOTE: 垂直拆分更多时候应该是在数据表设计之初就执行的步骤，然后查询的时候用 jion 关键起来即可。

## 2. 水平分表

水平拆分是指数据表行的拆分，表的行数超过 200 万行时，就会变慢，这时可以把一张的表的数据拆成多张表来存放。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/30/34b106eadf1e626e20ed4f9755f3f1c0.jpg)

通常情况下，我们使用取模的方式来进行表的拆分；比如一张有 400W 的用户表 users，为提高其查询效率我们把其分成 4 张表 users1，users2，users3，users4 通过用 ID 取模的方法把数据分散到四张表内 Id % 4 + 1 = [1,2,3,4] 然后查询，更新，删除也是通过取模的方法来查询：
```sql
$_GET['id'] = 17,
17%4 + 1 = 2,  
$tableName = 'users'.'2'
Select * from users2 where id = 17;
```
在 insert 时还需要一张临时表 uid_temp 来提供自增的 ID, 该表的唯一用处就是提供自增的 ID：
```sql
insert into uid_temp values(null);
```
得到自增的 ID 后，再通过取模法进行分表插入。

NOTE: 进行水平拆分后的表，字段的列和类型和原表应该是相同的，但是要记得去掉 auto_increment 自增长。

## 3. Refer Links

[慕课网：数据库设计那些事](http://www.imooc.com/learn/117)

[Mysql 设计与优化专题：表的垂直拆分和水平拆分](https://www.kancloud.cn/thinkphp/mysql-design-optimalize/39326 )