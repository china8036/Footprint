- [MySQL 索引](#mysql-索引)
  - [1. MySQL API](#1-mysql-api)
    - [1.1. 创建](#11-创建)
      - [1.1.1. NORMAL Index](#111-normal-index)
      - [1.1.2. UNIQUE Index](#112-unique-index)
      - [1.1.3. PRIMARY KEY Index](#113-primary-key-index)
    - [1.2. 删除](#12-删除)
    - [1.3. 查看](#13-查看)
  - [2. 索引选择](#2-索引选择)
  - [3. 索引优化](#3-索引优化)
  - [4. Refer Links](#4-refer-links)

# MySQL 索引

MySQL 主要使用 InnoDB 作为存储引擎，因此大部分使用的是 BTREE 索引。

## 1. MySQL API

### 1.1. 创建

#### 1.1.1. NORMAL Index

普通索引是最基本的索引类型，它没有任何限制，大多数情况下使用的都是 NORMAL Index。

- 直接创建索引
  ```
  CREATE INDEX 索引名 ON 表名（列名 1[（长度)]，列名 2[（长度)],...);
  ```
  如果是 CHAR，VARCHAR 类型，length 可以小于字段实际长度，就可以忽略字段长度；如果是 BLOB 和 TEXT 类型，必须指定 length。

- 修改表结构
  ```
  ALTER TABLE 表名 ADD INDEX 索引名 （列名 1[（长度)]，列名 2[（长度)],...);
  ```
- 创建表时指定索引
  ```
  CREATE TABLE 表名 (
  [...], 
  INDEX 索引名 （列名 1[（长度)]，列名 2[（长度)],...)
  );
  ```

#### 1.1.2. UNIQUE Index

唯一索引的列值必须唯一，不允许重复的索引，但允许有空值；如果是组合索引，则列值的组合必须唯一。

- 创建索引
  ```
  CREATE UNIQUE INDEX 索引名 ON 表名（列的列表);
  ```
- 修改表
  ```
  ALTER TABLE 表名 ADD UNIQUE 索引名 （列的列表);
  ```
- 创建表时指定索引
  ```
  CREATE TABLE 表名 (
  [...],
  UNIQUE 索引名 （列的列表)
  );
  ```

#### 1.1.3. PRIMARY KEY Index

主键索引是一种特殊的唯一索引，它必须指定为“PRIMARY KEY”，且不允许有空值。

- 主键索引一般在创建表的时候同时指定：
  ```
  CREATE TABLE 表名 (
  [...],
  PRIMARY KEY （列的列表)
  ); 
  ```
- 也可以通过修改表的方式
  ```
  ALTER TABLE 表名 ADD PRIMARY KEY （列的列表); 
  ```
- 不能用 CREATE INDEX 语句创建 PRIMARY KEY 索引；

NOTE:
- 每个表只能有一个主键索引。
- 主键相当于聚合索引，是查找最快的索引。

### 1.2. 删除

```
DROP INDEX 索引名 ON 表名；
```
ALTER TABLE table_name DROP INDEX index_name
```
ALTER TABLE table_name DROP PRIMARY KEY
```
只在删除 PRIMARY KEY 索引时使用，因为一个表只可能有一个 PRIMARY KEY 索引，因此不需要指定索引名。如果没有创建 PRIMARY KEY 索引，但表具有一个或多个 UNIQUE 索引，则 MySQL 将删除第一个 UNIQUE 索引。

NOTE:

如果从表中删除了某列，则索引会受到影响。对于多列组合的索引，如果删除其中的某列，则该列也会从索引中删除。如果删除组成索引的所有列，则整个索引将被删除。

### 1.3. 查看

```
mysql> SHOW INDEX FROM tblname;
mysql> SHOW KEYS FROM tblname;
```
- Table：表的名称
- Non_unique：如果索引不能包括重复词，则为 0。如果可以，则为 1
- Key_name：索引的名称
- Seq_in_index：索引中的列序列号，从 1 开始
- Column_name：列名称
- Collation：列以什么方式存储在索引中。在 MySQL 中，有值‘A’（升序）或 NULL（无分类）。
- Cardinality：索引中唯一值的数目的估计值。通过运行 ANALYZE TABLE 或 myisamchk -a 可以更新。基数根据被存储为整数的统计数据来计数，所以即使对于小型表，该值也没有必要是精确的。基数越大，当进行联合时，MySQL 使用该索引的机会就越大。
- Sub_part：如果列只是被部分地编入索引，则为被编入索引的字符的数目。如果整列被编入索引，则为 NULL。
- Packed：指示关键字如何被压缩。如果没有被压缩，则为 NULL。
- Null：如果列含有 NULL，则含有 YES。如果没有，则该列含有 NO。
- Index_type：用过的索引方法（BTREE, FULLTEXT, HASH, RTREE）。
- Comment：更多评注。

## 2. 索引选择

所谓索引选择，即选择哪个、哪些字段作为索引。

既然索引可以加快查询速度，那么是不是只要是查询语句需要的字段，就建上索引？答案是否定的。因为索引虽然加快了查询速度，索引也是有代价的：索引文件本身要消耗存储空间，同时索引会加重插入、删除和修改记录时的负担，另外，MySQL 在运行时也要消耗资源维护索引，因此索引并不是越多越好。

- 较频繁的作为查询条件的字段应该创建索引。

- 唯一性太差的字段不适合单独创建索引，即使频繁作为查询条件（会造成大量冲突而降低效率）。

- **更新非常频繁的字段不适合创建索引** -- **索引利于查询，但不利于更新**。

- 出现在 WHERE、JOIN 子句中的列应该建立索引，而不是在 SELECT 关键字后选择列表的列，不会出现在 WHERE 子句中的字段不应该创建索引。

- 不要选择包含 NULL 的列建立索引。
  
  **索引不会包含有 NULL 值的列，因此只要列中包含有 NULL 值都将不会被包含在索引中，复合索引中只要有一列含有 NULL 值，那么这一列对于此复合索引就是无效的。所以我们在数据库设计时不要让字段的默认值为 NULL**。

## 3. 索引优化

- 索引列的基数（字段的值域）越大，索引的效果越好。

- 使用短索引，如果对字符串列进行索引，应该指定一个前缀长度，可节省大量索引空间，提升查询速度。
  
  例：有一个 `CHAR(200)` 列，如果在前 10 个或 20 个字符内，多数值是唯一的，那么就不要对整个列进行索引。对前 10 个或者 20 个字符进行索引能够节省大量索引空间，也可能会使查询更快。较小的索引涉及的磁盘 I/O 较少，较短的值比较起来更快。更为重要的是，对于较短的键值，所以高速缓存中的快能容纳更多的键值，因此，MYSQL 也可以在内存中容纳更多的值。这样就增加了找到行而不用读取索引中较多快的可能性。

- 充分利用最左前缀原则。

  例：
  ```sql
  select
    count(*) 
  from
    task 
  where
    status=2 
    and operator_id=20839 
    and operate_time>1371169729 
    and operate_time<1371174603 
    and type=2;
  ```
  应当将 type 提到范围查询之前，才能充分利用索引，即
  ```sql
  where
    status=2 
    and operator_id=20839 
    and type=2
    and operate_time>1371169729 
    and operate_time<1371174603;
  ```

- 数据库默认排序可以符合要求的情况下，不要使用排序操作
  
  **MySQL 一次查询只能使用一个索引，也就是说如果 `WHERE` 子句中已经使用了索引的话，那么 `ORDER BY` 中的列是不会使用索引的。因此数据库默认排序可以符合要求的情况下，不要使用排序操作；且尽量不要包含多个列的排序，如果需要，最好给这些列创建复合索引**。

- 不要在列上进行任何运算

  例：
  ```sql
  SELECT * FROM users WHERE YEAR(adddate)<2007; 
  ```
  将在每个行上进行运算，这将导致索引失效而进行全表扫描，因此可以改成：
  ```SQL
  SELECT * FROM users WHERE adddate<‘2007-01-01’;  
  ```

- **尽量的扩展索引，不要新建索引**。比如表中已经有 a 的索引，现在要加 (a,b) 的索引，那么只需要修改原来的索引即可。

## 4. Refer Links

[查询容易，优化不易，且写且珍惜](https://tech.meituan.com/mysql-index.html )

https://segmentfault.com/a/1190000003072424 

http://database.51cto.com/art/200910/156685.htm 

[慢查询优化实例 - 美团技术团队](https://tech.meituan.com/mysql-index.html)

[MySQL 全文检索中 Like 索引的实现](http://database.51cto.com/art/200908/144024.htm)

[MySQL 索引背后的数据结构及算法原理](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)

[MySQL 索引与 Index Condition Pushdown](http://blog.codinglabs.org/articles/index-condition-pushdown.html)