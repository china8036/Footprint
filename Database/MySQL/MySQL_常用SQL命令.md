- [MySQL 常用 SQL 命令](#mysql-%E5%B8%B8%E7%94%A8-sql-%E5%91%BD%E4%BB%A4)
  - [1. 基本命令](#1-%E5%9F%BA%E6%9C%AC%E5%91%BD%E4%BB%A4)
  - [2. EXPLAIN 语句](#2-explain-%E8%AF%AD%E5%8F%A5)
    - [2.1. 使用方法](#21-%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95)
    - [2.2. 输出格式](#22-%E8%BE%93%E5%87%BA%E6%A0%BC%E5%BC%8F)
  - [3. Refer Links](#3-refer-links)

# MySQL 常用 SQL 命令

## 1. 基本命令

- `show databases;`: 显示所有数据库实例；
- `use database 数据库名；`: 进入指定数据库；
- `show tables;`: 显示当前数据库的所有表；
- `desc 表名；`: 显示当前数据库指定表的表结构；

## 2. EXPLAIN 语句

官方文档 https://dev.mysql.com/doc/refman/5.7/en/explain-output.html  

https://segmentfault.com/a/1190000008131735

http://overtrue.me/articles/2014/10/mysql-explain.html

MySQL 提供了一个 EXPLAIN 命令，它可以对 SELECT 语句进行分析，并输出 SELECT 执行的详细信息，以供开发人员针对性优化。

### 2.1. 使用方法

```sql
EXPLAIN SELECT ...;
```

### 2.2. 输出格式

- id

  SELECT 查询的标识符。每个 SELECT 都会自动分配一个唯一的标识符

- select_type

  select_type 表示了查询的类型，它的常用取值有：
  - SIMPLE, 表示此查询不包含 UNION 查询或子查询，是最常见的查询类别
  - PRIMARY, 表示此查询是最外层的查询
  - UNION, 表示此查询是 UNION 的第二或随后的查询
  - DEPENDENT UNION, UNION 中的第二个或后面的查询语句，取决于外面的查询
  - UNION RESULT, UNION 的结果
  - SUBQUERY, 子查询中的第一个 SELECT
  - DEPENDENT SUBQUERY: 子查询中的第一个 SELECT, 取决于外面的查询。即子查询依赖于外层查询的结果。

- table

  表示查询涉及的表或衍生表。

- partitions

  匹配的分区。

- type

  > The type column of EXPLAIN output describes how tables are joined. In JSON-formatted output, these are found as values of the access_type property.

  type 字段几乎可以说是查询优化中最重要的字段了，它提供了判断查询是否高效的重要依据。

  type 字段表示查询的 join 类型，我们根据此字段判断此次查询是全表扫描还是索引扫描等。

  type 值描述了该查询的效率高低，以下表格按从上往下效率渐差的顺序排序：
  - system: 数据表只有一行记录，是 const 类型的一个特殊情况
  - const: 针对主键或唯一索引的等值查询扫描，最多只返回一行数据。const 查询速度非常快，因为它仅仅读取一次即可。

    ```sql
    SELECT * FROM tbl_name WHERE primary_key=1;

    SELECT * FROM tbl_name
      WHERE primary_key_part1=1 AND primary_key_part2=2;
    ```
  
  - eq_ref: 此类型通常出现在多表的 join 查询，表示对于前表的每一个结果，都只能匹配到后表的一行结果。并且查询的比较操作通常是 =, 查询效率较高。

    ```sql
    SELECT * FROM ref_table,other_table
      WHERE ref_table.key_column=other_table.column;

    SELECT * FROM ref_table,other_table
      WHERE ref_table.key_column_part1=other_table.column
      AND ref_table.key_column_part2=1;
    ```

  - ref: 此类型通常出现在多表的 join 查询，针对于非唯一或非主键索引，或者是使用了最左前缀匹配规则索引的查询。

    ```sql
    SELECT * FROM ref_table WHERE key_column=expr;

    SELECT * FROM ref_table,other_table
      WHERE ref_table.key_column<=>other_table.column;

    SELECT * FROM ref_table,other_table
      WHERE ref_table.key_column_part1=other_table.column
      AND ref_table.key_column_part2=1;
    ```

  - fulltext: 使用了全文索引的查询。
  - ref_or_null: 同 ref，但是添加了 MySQL 可以专门搜索包含 NULL 值的行。在解决子查询中经常使用该联接类型的优化。

    ```sql
    SELECT * FROM ref_table
      WHERE key_column=expr OR key_column IS NULL;
    ```

  - index_merge: 表示使用了索引合并优化方法。在这种情况下，key 列包含了使用的索引的清单，key_len 包含了使用的索引的最长的关键元素。

    ```sql
    select * from t4 where id=3952602 or accountid=31754306 ;
    ```

  - unique_subquery: 该类型替换了下面形式的 IN 子查询的 ref：`value IN (SELECT primary_key FROM single_table WHERE some_expr)`，unique_subquery 是一个索引查找函数，可以完全替换子查询，效率更高。

  - index_subquery: 类似于 unique_subquery。可以替换 IN 子查询，但只适合下列形式的子查询中的非唯一索引：`value IN (SELECT key_column FROM single_table WHERE some_expr)`。

  - range: 表示使用索引范围查询，通过索引字段范围获取表中部分数据记录。这个类型通常出现在 =, <>, >, >=, <, <=, IS NULL, <=>, BETWEEN, IN() 操作中。当 type 是 range 时，那么 EXPLAIN 输出的 ref 字段为 NULL, 并且 key_len 字段是此次查询中使用到的索引的最长的那个。

    ```sql
    SELECT * FROM tbl_name
      WHERE key_column = 10;

    SELECT * FROM tbl_name
      WHERE key_column BETWEEN 10 and 20;

    SELECT * FROM tbl_name
      WHERE key_column IN (10,20,30);

    SELECT * FROM tbl_name
      WHERE key_part1 = 10 AND key_part2 IN (10,20,30);
    ```

  - index: 表示全索引扫描 (full index scan), 和 ALL 类型类似，只不过 ALL 类型是全表扫描，而 index 类型则仅仅扫描所有的索引，而不扫描数据。index 类型通常出现在：所要查询的数据直接在索引树中就可以获取到，而不需要扫描数据。当是这种情况时，Extra 字段 会显示 Using index。
  
  - ALL: 表示全表扫描，且完全没有用到索引，这个类型的查询是性能最差的查询之一。通常来说，我们的查询不应该出现 ALL 类型的查询，因为这样的查询在数据量大的情况下，对数据库的性能是巨大的灾难。如一个查询是 ALL 类型查询，那么一般来说可以对相应的字段添加索引来避免。

- possible_keys

  此次查询中可能选用的索引。

- key

  此次查询中真正使用到的索引。

- key_len

  表示查询优化器使用了索引的字节数。这个字段可以评估组合索引的各个字段是否完全被使用，或只有索引中最左部分的字段被使用到。计算规则如下：
  - 字符串
    - char(n): n 字节长度
    - varchar(n): 如果是 utf8 编码，则是 3 n + 2 字节；如果是 utf8mb4 编码，则是 4 n + 2 字节。
  - 数值类型：
    - TINYINT: 1 字节
    - SMALLINT: 2 字节
    - MEDIUMINT: 3 字节
    - INT: 4 字节
    - BIGINT: 8 字节
  - 时间类型
    - DATE: 3 字节
    - TIMESTAMP: 4 字节
    - DATETIME: 8 字节
  - 字段属性：NULL 属性，占用一个字节。如果一个字段是 NOT NULL 的，则没有此属性。

- ref

  哪个字段或常数与 key 一起被使用。

- rows

  rows 也是一个重要的字段。MySQL 查询优化器根据统计信息，估算 SQL 要查找到结果集需要扫描读取的数据行数。

  这个值非常直观的显示了 SQL 查询的效率好坏， 原则上 rows 越少效率越高。

- filtered

  表示此查询条件所过滤的数据的百分比。

- extra
  
  EXPLAIN 中的很多额外的信息会在 Extra 字段显示，常见的有以下几种内容：
  - Using filesort:	表示 MySQL 需额外的排序操作，不能通过索引顺序达到排序效果。一般有 Using filesort, 都建议优化去掉，因为这样的查询 CPU 资源消耗大。
  - Using index:	"覆盖索引扫描", 表示查询在索引树中就可查找所需数据，不用扫描表数据文件，往往说明性能不错。
  - Using temporary:	查询有使用临时表，一般出现于排序，分组和多表 join 的情况，查询效率不高，建议优化。

## 3. Refer Links
