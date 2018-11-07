- [数据库：关系数据语言 - 标准 SQL - 面试 FAQ](#数据库关系数据语言---标准-sql---面试-faq)
  - [1. 概念辨析](#1-概念辨析)
    - [1.1. 对于 DDL](#11-对于-ddl)
    - [1.2. 对于 DQL](#12-对于-dql)
    - [1.3. 对于 DML](#13-对于-dml)
    - [1.4. DELETE、DROP、TRUNCATE 区别](#14-deletedroptruncate-区别)
  - [2. MySQL JOIN 操作](#2-mysql-join-操作)
  - [3. 空值处理](#3-空值处理)
  - [4. Refer Links](#4-refer-links)

# 数据库：关系数据语言 - 标准 SQL - 面试 FAQ

## 1. 概念辨析

### 1.1. 对于 DDL

- 标准 SQL (ANSI SQL) 为模式 (SCHAME) 和视图 (VIEW) 提供了 `CREATE` 和 `DROP` 支持，但没有 `ALTER` 支持。
- 标准 SQL (ANSI SQL) 为数据表 (TABLE) 提供了 `CREATE`、`DROP` 和 `ALTER` 支持。
- 标准 SQL (ANSI SQL) 没有为索引 (INDEX) 提供任何支持，但在几乎所有 DBMS 中都为索引 (INDEX) 提供了 `CREATE`、`DROP` 和 `ALTER` 支持。

### 1.2. 对于 DQL

- DISTINCT 关键字
  - 只能作用于所有的列，无法仅对个别列起作用。
  - 必须用于列名，不可用于计算或表达式。
- LIMIT 关键字
  - 从第 0 行开始计数，而不是第 1 行。
  - 后边不能接表达式，不能接用户自定义变量 (@xxx)，但可以接本地 / 局部变量 (xxx)。
- DESC 关键字
  - 只能作用到位于其前面的列名，若想对多个列都进行降序排序，必须对每一列都指定 DESC 关键字。
- 聚集函数
  - 只能用于 SELECT 子句和 GROUP BY 中的 HAVING 子句中，不可用于 WHERE 子句中。
- 如果在 SELECT 语句中使用表达式，则必须在 GROUP BY 子句中指定相同的表达式，不可使用别名。
- 集合操作 (UNION/INTERSECT/EXCEPT) 
  - 参加集合操作的各查询结果的列数必须相同，对应项的数据类型也必须相同。
  - UNION 查询结果会自动去除重复的元组，若想返回所有匹配的行，应该使用 UNION ALL 操作符。
  - UNION 查询中只能使用一条 ORDER BY 子句，必须位于最后一条 SELECT 语句之后，可对所有结果进行排序。
- 运算符
  - **判断是否相等的比较运算符是 `=`，而不是 `==`**。在 SET 和 UPDATE 语句中，`=` 表示赋值，在其它语句中，`=` 表示相等。
  - 在所有语句中，`:=` 都表示赋值。
  - `BETWEEN AND` 运算符**包括了边界值**。
  - `LIKE` 表达式中匹配符 **`%` 不会匹配值为 NULL 的字段**。
- 函数和存储过程
  - 函数和存储过程都采取了预编译，不像解释执行的 SQL 语句那样在提出操作请求时才进行语法分析和优化工作，因而运行效率很高。
  - 函数和存储过程的区别在于函数必须指定返回的类型，而存储过程不用。

### 1.3. 对于 DML

- INSERT
  - VALUES 后边每个括号表示一条完整的记录值，可同时插入多条记录，即多个括号。
  - 插入子查询结果时，不用加 VALUES 关键字。
- DELETE
  - 若需要从表中删除所有行，可直接使用 `TRUNCATE TABLE` 语句，它比 DELETE 执行速度更快（因为不需要记录数据的变动）。
- 多表联合操作时必须将所有表名在 UPDATE/SELECT/DELETE 关键字后都列出。

### 1.4. DELETE、DROP、TRUNCATE 区别

- `DELETE` 用于删除表中的指定 row，`TRUNCATE` 用于删除表中的所有 row（清空表），`DROP` 用于删除表中的所有数据（包括表结构本身、约束 (constrain)、触发器 (trigger)、索引 (index)，但依赖于该表的存储过程 / 函数将被保留）。
- 事务支持
  - `TRUNCATE` 和 `DROP` 是数据库定义语言 (DDL)，操作立即生效，不放到 rollback segment 中，不能回滚；操作不能触发 trigger。
  - `DELETE` 语句是数据库操作语言 (DML)，这个操作会放到 rollback segement 中，事务提交之后才生效；如果有相应的 trigger，执行的时候将被触发。
- 高水线
  - `DELETE` 语句不影响表所占用的 extent，高水线 (high watermark) 保持原位置不动。
  - `DROP` 语句将表所占用的空间全部释放。
  - `TRUNCATE` 语句缺省情况下见空间释放到 minextents 个 extent，除非使用 reuse storage；truncate 会将高水线复位（回到最开始)。
- 对于由 FOREIGN KEY 约束引用的表，不能使用 `TRUNCATE TABLE`，而应使用不带 `WHERE` 子句的 `DELETE` 语句。
- 如果 `DELETE` 不加 WHERE 子句，那么它和 `TRUNCATE TABLE` 一样都能清空表的所有记录。
- `DELETE` 需要遍历所有记录，而 `TRUNCATE` 直接删除记录文件，因此效率更高，使用的系统和事务日志资源少。
- `DELETE` 可以返回被删除的记录数，而 `TRUNCATE TABLE` 返回的是 0。
- `DELETE` 不会改变自增字段的下一个值，而 `TRUNCATE TABLE` 会使自增字段恢复成初始值即 1。

## 2. MySQL JOIN 操作

在 MySQL 中，对连接查询的支持不完全与 ANSI SQL 相同：

- 内连接：`(INNER) JOIN ... ON`
  - 自然连接：`NATURAL JOIN`
  - 自身连接：`(INNER) JOIN ... USING`
- 左外连接：`LEFT (OUTER) JOIN`
- 右外连接：`RIGHT (OUTER) JOIN`
- 全外连接：不支持 `FULL OUTER JOIN`，需要通过 `UNION` 来实现
- 交叉连接：`CROSS JOIN`

## 3. 空值处理

空值是指“不指定”、“不存在”、“无意义”的值。SQL 语言允许元组在以下情况取空值：
- **该属性应该有一个值，但目前还不知道它的具体值**。如，某学生的年龄属性由于登记时遗漏，暂时不知道该学生的年龄，因此取空值。
- **该属性不应该有值**。如，缺考学生的成绩为空。
- **由于某种原因不便于填写**。如，一个人的电话号码不想让其他人知道，则取空值。

DBMS 中空值的处理：
- 空值的判断：SQL 中可以通过 `IS NULL `和 `IS NOT NULL` 进行空值的判断。

- 空值的约束条件：SQL 中可以通过 `NOT NULL` 或 `UNIQUE` 限制属性不能取空值。码属性都不能取空值。

- 空值的排序：对于空值，排序时显示的次序由具体的数据库系统实现来决定。因此，**若要保证数据库的可移植性，不应在属性值中存在 NULL 值**。

- 当聚集函数遇到空值时，除 `COUNT(*)` 外，都跳过空值而只处理非空值。

- **空值与另一个任意值的算术运算结果为空值，与另一个任意值的比较运算结果为 UNKNOWN**。

## 4. Refer Links

[SQL truncate 、delete 与 drop 区别](https://www.cnblogs.com/8765h/archive/2011/11/25/2374167.html)