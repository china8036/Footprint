- [数据库：关系数据语言 - 标准 SQL - 数据操纵(DML)](#数据库关系数据语言---标准-sql---数据操纵dml)
  - [1. 插入数据 INSERT](#1-插入数据-insert)
    - [1.1. 插入元组](#11-插入元组)
    - [1.2. 插入子查询结果](#12-插入子查询结果)
    - [1.3. 使用 SELECT INTO 复制表](#13-使用-select-into-复制表)
  - [2. 修改数据 UPDATE](#2-修改数据-update)
    - [2.1. 单表数据更新](#21-单表数据更新)
    - [2.2. 多表联合更新](#22-多表联合更新)
      - [2.2.1. Theta 风格](#221-theta-风格)
      - [2.2.2. ANSI SQL 风格](#222-ansi-sql-风格)
  - [3. 删除数据 DELETE](#3-删除数据-delete)
  - [4. Refer Links](#4-refer-links)

# 数据库：关系数据语言 - 标准 SQL - 数据操纵(DML)

## 1. 插入数据 INSERT

### 1.1. 插入元组

```sql
INSERT INTO 表名 ( 字段 1, 字段 2, 字段 3, ... ) VALUES ( ‘值 1’, ‘值 2’, ‘值 3’, ... );
```

NOTE
- 若省略字段名列表，则默认将所有字段作为插入字段，这种方法并不安全，应尽量避免使用。
- 值的顺序必须与字段的顺序一一对应。
- **VALUES 后边每个括号表示一条完整的记录值，可同时插入多条记录，即多个括号**。

### 1.2. 插入子查询结果

INSERT 还有一种使用形式，可将 SELECT 语句的查询结果插入表中，即 INSERT SELECT；

例：
```sql
INSERT INTO Customers(cust_id, cust_name) SELECT cust_id, cust_name FROM CustomersOther;
```

NOTE: **这种形式的 INSERT 不用加 VALUES**。

### 1.3. 使用 SELECT INTO 复制表

除 INSERT 外，利用 SELECT INTO 语句也可进行数据插入。事实上，SELECT INTO 创建一个新的表并将数据完整复制到该表，实现导出数据的功能。

例：

SQL Server 语法：
```sql
SELECT *
INTO CustCopy
FROM Customers;
```
MySQL、Oracle、SQLite、PostgreSQL 语法：
```sql
CREATE TABLE CustCopy AS
SELECT * FROM Customers;
```

NOTE: 不论从多少个表中检索数据，数据只能插入到一个表中。

## 2. 修改数据 UPDATE

```sql
UPDATE 表名 SET 字段 1 = 值 1, 字段 2 = 值 2, ... [ WHERE 条件 ] [ ORDER BY 字段 ] [ LIMIT 行数 ];
```
若省略 WHERE 字句意味着 WHERE 字句的值总是为 true；

### 2.1. 单表数据更新

例：
```sql
UPDATE `table_name` SET `col1` = xx, `col2` = yy where `col` = zz;
```

NOTE: 要删除某个列的值，可使用 UPDATE 将其设置为 NULL。

### 2.2. 多表联合更新

#### 2.2.1. Theta 风格

例：
```sql
UPDATE `test_1`, `test_2` SET `test_1`.`name` = `test_2`.`name` WHERE `test_1`.`id`=100 AND `test_2`.`id`=200;
```
或
```sql
UPDATE `test_1` a, `test_2` b SET a.name = b.name WHERE a.id=100 AND b.id=200;
```

NOTE: **多表联合操作必须将所有表名在 UPDATE/SELECT/DELETE 关键字后都列出，以下写法是错误的**：
```sql
UPDATE `test_1` SET `test_1`.`name` = `test_2`.`name` WHERE `test_1`.`id`=100 AND `test_2`.`id`=200;
```
会报错：
> [Err] 1054 - Unknown column 'test_2.id' in 'where clause'

#### 2.2.2. ANSI SQL 风格

多表联合 update 也可以使用 JOIN 子句连接多个表为一张表，再进行 update 操作。
```sql
UPDATE table_1 SET score = score + 5 WHERE uid IN (select uid from table_2 where sid = 10);
```
替换成 JOIN 方式： 
```sql
UPDATE (table_1 t1 INNER JOIN table_2 t2 ON t1.uid = t2.uid) SET score = score + 5 WHERE t2.sid = 10;
```

## 3. 删除数据 DELETE

```sql
DELETE FROM 表名 [WHERE 条件 ] [ORDER BY 字段 ] [LIMIT 行数 ];
```

NOTE: **若需要从表中删除所有行，可直接使用 TRUNCATE TABLE 语句，它比 DELETE 执行速度更快（因为不需要记录数据的变动）**。

## 4. Refer Links