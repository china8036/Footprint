- [MySQL 存储引擎](#mysql-存储引擎)
  - [2. MyISAM](#2-myisam)
  - [1. InnoDB](#1-innodb)
  - [3. NDB](#3-ndb)
  - [4. Memery](#4-memery)
  - [5. Interview FAQ](#5-interview-faq)
    - [5.1. InnoDB 和 MyISAM 的区别？](#51-innodb-和-myisam-的区别)
    - [5.2. 如何选择使用 InnoDB 还是 MyISAM？](#52-如何选择使用-innodb-还是-myisam)
  - [6. Refer Links](#6-refer-links)

# MySQL 存储引擎

MySQL 有多种存储引擎，每种存储引擎有各自的优缺点，可以择优选择使用：MyISAM、InnoDB、MERGE、MEMORY(HEAP)、BDB(BerkeleyDB)、EXAMPLE、FEDERATED、ARCHIVE、CSV、BLACKHOLE。

## 2. MyISAM

https://dev.mysql.com/doc/refman/8.0/en/myisam-storage-engine.html

## 1. InnoDB

https://dev.mysql.com/doc/refman/8.0/en/innodb-storage-engine.html

## 3. NDB

## 4. Memery

## 5. Interview FAQ

### 5.1. InnoDB 和 MyISAM 的区别？

- 存储机制
  - 每个 MyISAM 表在磁盘上存储成 3 个文件，每一个文件的名字以表的名字开始，扩展名指出文件类型：
    - `.frm` 文件存储表定义。
    - `.MYD(MYData)` 文件为数据文件。 
    - `.MYI(MYIndex)` 文件为索引文件。
  - 每个 InnoDB 表在磁盘上存储成 2 个文件：
    - 数据文件
    - 日志文件
    其中，数据文件中包含了索引和数据记录，其大小只受限于操作系统文件的大小，一般为 2GB。

- 事务支持
  - **InnoDB 支持事务**。在 InnoDB 中，每一条 SQL 语言都会默认被封装成事务并自动提交，因此也影响了 SQL 语句的执行速度，所以最好把多条 SQL 语言放在 `BEGIN` 和 `COMMIT` 之间，组成一个事务再执行。
  - **MyISAM 不支持事务**。MyISAM 表更强调性能，不提供事务支持。

- 外键支持
  - **InnoDB 支持外键**。
  - **MyISAM 不支持外键**。
  对一个包含外键的 InnoDB 表转为 MYISAM 会失败。

- 行锁支持
  - **MyISAM 不支持行锁**，只支持表锁，因此并发效率很低。
  - **InnoDB 支持行锁**。但 InnoDB 表的行锁也不是绝对的，如果在执行一个 SQL 语句时 MySQL 不能确定要扫描的范围，InnoDB 表同样会锁全表，例如：`UPDATE TABLE SET num = 1 WHERE NAME LIKE "%aaa%"`。

- 全文索引 (FULLTEXT) 支持
  
  **Innodb 不支持全文索引，MyISAM 支持全文索引**，因此 MyISAM 的查询效率更高。

- 索引实现
  - **MyISAM 的索引实现是基于 B+Tree 的非聚集索引**，数据文件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的，因此主键索引和辅助索引的查询效率也是基本相同的。
  - **InnoDB 的索引实现是基于 B+Tree 的聚集索引**，数据文件是和索引绑在一起的，必须要有主键。通过主键索引效率很高，但辅助索引需要两次查询（先查询到主键，然后再通过主键查询到数据）。因此，主键不应该过大，因为主键太大，辅助索引的查询也都会消耗很大。

- 表行数查询
  - **InnoDB 不保存表的具体行数**，执行 `SELECT COUNT(*) FROM table` 时需要全表扫描，速度较慢。
  - **MyISAM 用一个变量保存了整个表的行数**，执行上述语句时只需要读出该变量即可，速度很快。
  NOTE: 当 `COUNT(*)` 语句包含 where 条件子句时，两种表的操作是一样的。

### 5.2. 如何选择使用 InnoDB 还是 MyISAM？

- 如果需要支持事务，选择 InnoDB 引擎。

- 如果表中绝大多数都只是读查询，可以考虑 MyISAM；如果写也挺频繁，选择 InnoDB 引擎。

- 系统奔溃后，MyISAM 恢复起来更困难，能否接受。

- **从 MySQL 5.5 版本开始，Innodb 取代了 MyISAM 成为 Mysql 的默认引擎**，说明其优势是有目共睹的，因此在大多数情况下，使用 InnoDB 是可以满足需求的。

## 6. Refer Links

[Mysql 中 MyISAM 和 InnoDB 的区别有哪些？](https://www.zhihu.com/question/20596402)