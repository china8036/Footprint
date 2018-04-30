- [MySQL 事务 (TRANSACTION)](#mysql--transaction)
  - [1. 基本概念](#1)
  - [2. MySQL API](#2-mysql-api)

# MySQL 事务 (TRANSACTION)

## 1. 基本概念

事务是由一组 SQL 语句组成的逻辑处理单元。

MySQL 事务主要用于处理操作量大，复杂度高的数据。比如说，在人员管理系统中，你删除一个人员，你即需要删除人员的基本资料，也要删除和该人员相关的信息，如信箱，文章等，这些 SQL 操作就可以构成一个事务。

## 2. MySQL API

事物控制语句：
- `BEGIN 或 START TRANSACTION；`显式地开启一个事务。
- `COMMIT；`也可以使用 COMMIT WORK，不过二者是等价的。COMMIT 会提交事务，并使已对数据库进行的所有修改称为永久性的。
- `ROLLBACK；`有可以使用 ROLLBACK WORK，不过二者是等价的。回滚会结束用户的事务，并撤销正在进行的所有未提交的修改。
- `SAVEPOINT identifier；`SAVEPOINT 允许在事务中创建一个保存点，一个事务中可以有多个 SAVEPOINT。
- `RELEASE SAVEPOINT identifier；`删除一个事务的保存点，当没有指定的保存点时，执行该语句会抛出一个异常。
- `ROLLBACK TO identifier；`把事务回滚到标记点。
- `SET TRANSACTION；`用来设置事务的隔离级别。InnoDB 存储引擎提供事务的隔离级别有 READ UNCOMMITTED、READ COMMITTED、REPEATABLE READ 和 SERIALIZABLE。

MYSQL 事务处理主要有两种方法：
- 用 BEGIN, ROLLBACK, COMMIT 来实现
  ```sql
  BEGIN 开始一个事务
  ROLLBACK 事务回滚
  COMMIT 事务确认
  ```
- 直接用 SET 来改变 MySQL 的自动提交模式：
  ```sql
  SET AUTOCOMMIT=0 禁止自动提交
  SET AUTOCOMMIT=1 开启自动提交
  ```

NOTE:

在 MySQL 命令行的默认设置下，事务都是自动提交的，即执行 SQL 语句后就会马上执行 COMMIT 操作。因此要显式地开启一个事务务须使用命令 BEGIN 或 START TRANSACTION，或者执行命令 SET AUTOCOMMIT=0，用来禁止使用当前会话的自动提交。
