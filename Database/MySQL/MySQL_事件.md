- [MySQL 事件](#mysql)
  - [1. 基本概念](#1)
  - [2. MySQL API](#2-mysql-api)
    - [2.1. 查看 event 是否开启](#21--event)
    - [2.2. 开启 event](#22--event)
    - [2.3. 查看 event](#23--event)
    - [2.4. 创建 event](#24--event)
    - [2.5. 关闭 event](#25--event)
    - [2.6. 启动 event](#26--event)
    - [2.7. 删除 event](#27--event)
    - [2.8. 修改 event](#28--event)

# MySQL 事件

## 1. 基本概念

事件（event）是 MySQL 在相应的时刻调用的过程式数据库对象。一个事件可调用一次，也可周期性的启动，它由一个特定的线程来管理的，也就是所谓的“事件调度器”。

事件和触发器类似，都是在某些事情发生的时候启动。当数据库上启动一条语句的时候，触发器就启动了，而事件是根据调度事件来启动的。由于他们彼此相似，所以事件也称为临时性触发器。

事件取代了原先只能由操作系统的计划任务来执行的工作，而且 mysql 的事件调度器可以精确到每秒钟执行一个任务，而操作系统的计划任务（如：Linux 下的 CRON 或 Windows 下的任务计划）只能精确到每分钟执行一次。

优点
- 一些对数据定时性操作不再依赖外部程序，而直接使用数据库本身提供的功能。
- 可以实现每秒钟执行一个任务，这在一些对实时性要求较高的环境下非常实用。

缺点
- 定时触发，不可以调用。

## 2. MySQL API

### 2.1. 查看 event 是否开启

MySQL evevt 功能默认是关闭的，可以使用下面的语句来看 evevt 的状态：
```sql
SHOW VARIABLES LIKE '%event%';
```

### 2.2. 开启 event

```sql
SET @@global.event_scheduler = ON;
```
或
```sql
SET GLOBAL event_scheduler = 1;
```
注意：
- 如果是设定事件计划为 0 或 OFF，即关闭事件计划进程的时候，不会有新的事件执行，但现有的正在运行的事件会执行到完毕。
- 对于线上环境来说，使用 even 时，注意在主库上开启定时器，从库上关闭定时器，event 触发所有操作均会记录 binlog 进行主从同步，从库上开启定时器很可能造成卡库。切换主库后之后记得将新主库上的定时器打开。

### 2.3. 查看 event

```sql
select * from  mysql.event;
```
查看 event 执行情况
```
select * from information_schema.EVENTS;
```
```sql
show events;
```

### 2.4. 创建 event

https://www.cnblogs.com/geaozhang/p/6821692.html 

http://blog.sina.com.cn/s/blog_6d39ac7e01017sd6.html 

格式：
```sql
CREATE
　　[DEFINER = { user | CURRENT_USER }]
EVENT
　　[IF NOT EXISTS]
<event_name>
ON SCHEDULE <schedule>
　　[ON COMPLETION [NOT] PRESERVE]
　　[ENABLE | DISABLE | DISABLE ON SLAVE]
　　[COMMENT 'comment']
DO 
<event_body>;
```

NOTE:
- DEFINER：指明该 event 的用户，服务器在执行该事件时，使用该用户来检查权限。
  
  默认用户为当前用户，即 definer = current_user；
  
  如果明确指明了 definer，则必须遵循如下规则：
  - 如果没有 super 权限，唯一允许的值就是自己当前用户，而不能设置为其他用户。
  - 如果具有 super 权限，则可以指定任意存在的用户；如果指定的用户不存在，则事件在执行时会报错。

- IF NOT EXISTS：如果事件已经存在，则不会创建，也不会报错。

- ON SCHEDULE 子句：指定何时执行该事件，以及如何执行该事件。
  
  - AT TIMESTAMP 形式：
    - 用于创建单次执行的事件；
    - AT 后加一个时间点（如果指定的时间是过去的时间，则会产生一个 warning)，时间可以是具体的时间字符串或者是一个 datetime 类型的表达式（如 current_timestamp)；
    - 如果要指定将来某个时间，直接使用 at timestamp，例：AT '2017-08-08 08:08:08'；
    - 如果要指定将来某个时间间隔，可利用 interval 关键字 (interval 关键字可以进行组合，at timestamp + INTERVAL 2 HOUR、 + INTERVAL 30 MINUTE)；
    - 示例：创建一个 10 分钟后清空 test 表数据的事件
      ```sql
      CREATE EVENT IF NOT EXISTS event_truncate_test2
      ON SCHEDULE
      AT CURRENT_TIMESTAMP + INTERVAL 10 MINUTE
      DO TRUNCATE TABLE test2;
      ```
  
  - EVERY 形式：
    - 用于创建重复执行的事件；
    - EVERY 后加一个时间间隔，可以为 1 second，3 minute，5 hour，9 day，1 month，1 quarter（季度），1 year；
    - 可以指定一个开始事件和结束时间，通过 STARTS 和 ENDS 关键字来表示。
    - 示例：
      
      从 30 分钟后每 12 个小时执行一次，4 周后截止
      ```sql
      EVERY 12 HOUR STARTS CURRENT_TIMESTAMP + INTERVAL 30 MINUTE ENDS CURRENT_TIMESTAMP + INTERVAL 4 WEEK;
      ```
      从 2013 年 1 月 13 号 0 点开始，每天运行一次
      ```sql
      ON SCHEDULE EVERY 1 DAY STARTS '2013-01-13 00:00:00'
      ```
      从现在开始每隔九天定时执行
      ```sql
      ON SCHEDULE EVERY 9 DAY STARTS NOW()
      ```
      每个月的一号凌晨 1 点执行
      ```sql
      on schedule every 1 month starts date_add(date_add(date_sub(curdate(),interval day(curdate())-1 day),interval 1 month),interval 1 hour)
      ```
      每个季度一号的凌晨 1 点执行
      ```sql
      on schedule every 1 quarter starts date_add(date_add(date(concat(year(curdate()),'',elt(quarter(curdate()),1,4,7,10),'',1)),interval 1 quarter),interval 1 hour)
      ```
      每年 1 月 1 号凌晨 1 点执行
      ```sql
      on schedule every 1 quarter starts date_add(date_add(date(concat(year(curdate()),'-',elt(quarter(curdate()),1,4,7,10),'-',1)),interval 1 quarter),interval 1 hour)
      ```

- 通常情况下，如果一个事件过期已过期，则会被立即删除。但是，可通过 ON COMPLETION [NOT] PRESERVE 子句保留已过期的事件；默认：ON COMPLETION NOT PRESERVE，也就是不保存。

- 默认情况下，enable on slave，事件一旦创建后就立即开始执行；可以通过 disable 关键字来禁用该事件。

- COMMENT 子句用于给事件添加注释。

- DO 子句用于指示事件需要执行的操作，可以是一条 SQL 语句，也可以是被 begin...end 包括的语句块，也可以在语句块中调用存储过程。

例：每分钟往表 t2 中添加数据
```sql
DELIMITER $$
CREATE EVENT e_daily
　　ON SCHEDULE
　　EVERY 1 MINUTE
　　　　COMMENT 'Saves total number of sessions then clears the table each day'
　　DO
　　　　BEGIN
　　　　　　INSERT INTO t2 values (null,current_timestamp);
　　　　END $$
DELIMITER ;
```

### 2.5. 关闭 event

```sql
ALTER EVENT `e_test` ON COMPLETION PRESERVE DISABLE;
```

### 2.6. 启动 event

```sql
ALTER EVENT `e_test` ON COMPLETION PRESERVE ENABLE;
```

### 2.7. 删除 event

```sql
DROP EVENT [IF EXISTS] event_name;
```

### 2.8. 修改 event

ALTER EVENT 语句可以修改事件的一个或多个属性。

格式：
```sql
ALTER 
    [DEFINER = { user | CURRENT_USER }] 
EVENT event_name 
    [ON SCHEDULE schedule] 
    [ON COMPLETION [NOT] PRESERVE] 
    [RENAME TO new_event_name] 
    [ENABLE | DISABLE | DISABLE ON SLAVE] 
    [COMMENT 'comment'] 
    [DO event_body]
```

ALTER EVENT 语句语法与 CREATE EVENT 语句完全相同，唯一不同的是可以对事件重命名，使用 RENAME TO 子句。
```sql
ALTER EVENT OLDDB.MYEVENT RENAME TO NEWDB.MYEVENT;
```
