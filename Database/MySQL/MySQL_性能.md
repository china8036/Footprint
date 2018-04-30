- [MySQL 性能](#mysql)
  - [1. 性能优化](#1)
    - [1.1. 设计原则](#11)
      - [1.1.1. 数据库设计原则](#111)
      - [1.1.2. 数据表设计原则](#112)
      - [1.1.3. 索引类设计原则](#113)
      - [1.1.4. SQL 语句设计原则](#114-sql)
  - [2. 性能指标](#2)
  - [3. 常见问题](#3)
    - [3.1. 分页问题](#31)
  - [4. 性能测试](#4)
    - [4.1. Mysqlslap](#41-mysqlslap)
      - [4.1.1. 参数说明](#411)
      - [4.1.2. 运行过程](#412)
      - [4.1.3. 实例](#413)
  - [5. Refer Links](#5-refer-links)

# MySQL 性能

## 1. 性能优化

### 1.1. 设计原则

#### 1.1.1. 数据库设计原则

- 不在数据库做运算：cpu 计算务必移至业务层。
- 控制单库表数量在 300 内。
- 控制单表数据量：单表记录 int 型不超过 1000w，含 char 则不超过 500w，若超过，请合理分表。
- 控制列数量：字段数控制在 20 以内。
- 平衡范式与冗余：为提高效率牺牲范式设计，冗余数据。
- 拒绝 3B：拒绝大 sql，大事务，大批量。
- 表字符集使用 UTF8。
- 使用 INNODB 存储引擎。

#### 1.1.2. 数据表设计原则

- 尽可能地使用最有效（最小) 的数据类型
  ```
  tinyint(1Byte)
  smallint(2Byte)
  mediumint(3Byte)
  int(4Byte)
  bigint(8Byte)
  bad case：int(1)/int(11)
  ```

- 不要将数字存储为字符串，字符转化为数字，用 int 存储 ip 而非 char(15)

- 优先使用 enum 或 set，sex enum (‘F’, ‘M’)

- 避免使用 NULL 字段
  
  - NULL 字段很难查询优化
  - NULL 字段的索引需要额外空间
  - NULL 字段的复合索引无效
  ```
  bad case：`name` char(32) default null`age` int not null
  good case：`age` int not null default 0
  ```

- 少用 text/blob，varchar 的性能会比 text 高很多；实在避免不了 blob，请拆表

- 不在数据库里存图片

- 对于 MyISAM 表，如果没有任何变长列 (VARCHAR、TEXT 或 BLOB 列)，使用固定尺寸的记录格式。这比较快但是不幸地可能会浪费一些空间。即使你已经用 CREATE 选项让 VARCHAR 列 ROW_FORMAT=fixed，也可以提示想使用固定长度的行

- 使用 sample character set，例如 latin1。尽量少使用 utf-8，因为 utf-8 占用的空间是 latin1 的 3 倍。可以在不需要使用 utf-8 的字段上面使用 latin1，例如 mail，url 等

- 精确度与空间的转换。在存储相同数值范围的数据时，浮点数类型通常都会比 DECIMAL 类型使用更少的空间。FLOAT 字段使用 4 字节存储 数据。DOUBLE 类型需要 8 个字节并拥有更高的精确度和更大的数值范围，DECIMAL 类型的数据将会转换成 DOUBLE 类型

- 库名表名字段名必须有固定的命名长度，12 个字符以内；库名、表名、字段名禁⽌止超过 32 个字符。须见名之意；库名、表名、字段名禁⽌止使⽤用 MySQL 保留字；临时库、表名必须以 tmp 为前缀，并以⽇日期为后缀； 备份库、表必须以 bak 为前缀，并以日期为后缀

- InnoDB 表行记录物理长度不超过 8KB，InnoDB 的 data page 默认是 16KB，基于 B+Tree 的特点，一个 data page 中需要至少存储 2 条记录。因此，当实际存储长度超过 8KB（尤其是 TEXT/BLOB 列）的大列（large column）时会引起“page-overflow 存储”，类似 ORACLE 中的“行迁移”，因此，如果必须使用大列（尤其是 TEXT/BLOB 类型）且读写频繁的话，则最好把这些列拆分到子表中，不要和主表放在一起存储，如果不太频繁，可以考虑继续保留在主表中，如果将 innodbpagesize 选项修改成 8KB，那么行记录物理长度建议不超过 4KB

#### 1.1.3. 索引类设计原则

- 使用索引导致读用时缩短，写用时增长。因此，如果能提升读性能时再添加索引。

- 索引一定不是越多越好（能不加就不加，要加的一定得加）

- 覆盖记录条数过多不适合建索引，例如“性别”

- 字符字段必须建前缀索引

- 不在索引做列运算，bad case：select id where age +1 = 10;

- innodb 主键推荐使用自增列

- 主键建立聚簇索引

- 主键不应该被修改

- 字符串不应该做主键

- 如果不指定主键，innodb 会使用唯一且非空值索引代替

- 不用外键，请由程序保证约束

- 避免在已有索引的前缀上建立索引。例如：如果存在 index（a，b）则去掉 index（a）

- 控制单个索引的长度。使用 key（name（8））在数据的前面几个字符建立索引

- 要选择性的使用索引。在变化很少的列上使用索引并不是很好，例如性别列

- Optimize table 可以压缩和排序 index，注意不要频繁运行

- Analyze table 可以更新数据

- 索引选择性是不重复的索引值也叫基数（cardinality）表中数据行数的比值，索引选择性 = 基数 / 数据行，count(distinct(username))/count(*) 就是索引选择性，高索引选择性的好处就是 mysql 查找匹配的时候可以过滤更多的行，唯一索引的选择性最佳，值为 1

- 不要用重复或多余索引，对于 INNODB 引擎的索引来说，每次修改数据都要把主键索引，辅助索引中相应索引值修改，这可能会出现大量数 据迁移，分页，以及碎片的出现

- 超过 20 个长度的字符串列，最好创建前缀索引而非整列索引（例如：ALTER TABLE t1 ADD INDEX(user(20))），可以有效提高索引利用率，不过它的缺点是对这个列排序时用不到前缀索引。前缀索引的长度可以基于对该字段的统计得出， 一般略大于平均长度一点就可以了

- 定期用 pt-duplicate-key-checker 工具检查并删除重复的索引。比如 index idx1(a, b) 索引已经涵盖了 index idx2(a)，就可以删除 idx2 索引了

#### 1.1.4. SQL 语句设计原则

- sql 语句尽可能简单，一条 sql 只能在一个 cpu 运算，大语句拆小语句，减少锁时间，一条大 sql 可以堵死整个库（充分利用 QUERY CACHE 和充分利用多核 CPU)

- 简单的事务，事务时间尽可能短，bad case：上传图片事务

- 避免使用 trig/func, 触发器、函数不用，客户端程序取而代之

- 不用 select *, 消耗 cpu，io，内存，带宽，这种程序不具有扩展性

- OR 改写为 IN()
  
  OR 的效率是 O(n)，IN 的效率是 O(log(n))。in 的个数建议控制在 200 以内。
  ```sql
  select id from t where phone=’159′ or phone=’136′ =>select id from t where phone in (’159′, ’136′);
  ```

- OR 改写为 UNION
  
  mysql 的索引合并很弱智
  ```sql
  select id from t where phone = '159' or name = 'john';
  ```
  =>
  ```sql
  select id from t where phone='159' union  select id from t where name='jonh';
  ```

- 避免负向 %，如 not in/like

- 慎用 count(*)

- limit 高效分页

  limit 越大，效率越低
  ```sql
  select id from t limit 10000, 10;
  ```
  =>
  ```sql
  select id from t where id > 10000 limit 10;
  ```

- 使用 union all 替代 union，union 有去重开销

- 少用连接 join

- 使用 group by，分组、自动排序

- 请使用同类型比较

- 使用 load data 导数据，load data 比 insert 快约 20 倍

- 对数据的更新要打散后批量更新，不要一次更新太多数据

- 使用性能分析工具
  ```
  explain / showprofile / mysqlsla
  ```

- 使用 --log-slow-queries –long-query-time=2 查看查询比较慢的语句。然后使用 explain 分析查询，做出优化
  ```
  show profile;
  mysqlsla;
  mysqldumpslow;
  explain;
  show slow log;
  show processlist;
  show query_response_time(percona)
  ```
  optimize 数据在插入，更新，删除的时候难免一些数据迁移，分页，之后就出现一些碎片，久而久之碎片积累起来影响性能， 这就需要 DBA 定期的优化数据库减少碎片，这就通过 optimize 命令。如对 MyISAM 表操作：`optimize table 表名`。

- 禁止在数据库中跑大查询

- 使⽤预编译语句，只传参数，比传递 SQL 语句更高效；一次解析，多次使用；降低 SQL 注入概率

- 禁止使⽤order by rand()

- 禁⽌单条 SQL 语句同时更新多个表

- 避免在数据库中进⾏数学运算 (MySQL 不擅长数学运算和逻辑判断)

- SQL 语句要求所有研发，SQL 关键字全部是大写，每个词只允许有一个空格

- 能不用 NOT IN 就不用 NOTIN，坑太多了。会把空和 NULL 给查出来

## 2. 性能指标

QPS，Queries Per Second：每秒查询数，一台数据库每秒能够处理的查询次数 TPS，Transactions Per Second：每秒处理事务数

NOTE:
- 哪怕是基于索引的条件过滤，如果优化器意识到总共需要扫描的数据量超过 30% 时（ORACLE 里貌似是 20%，MySQL 目前是 30%，没准以后会调整），就会直接改变执行计划为全表扫描，不再使用索引。
- 多表 JOIN 时，要把过滤性最大（不一定是数据量最小哦，而是只加了 WHERE 条件后过滤性最大的那个）的表选为驱动表。此外，如果 JOIN 之后有排序，排序字段一定要属于驱动表，才能利用驱动表上的索引完成排序。
- 绝大多数情况下，排序的代价通常要来的更高，因此如果看到执行计划中有 Using filesort，优先创建排序索引。
- 利用 pt-query-digest 定期分析 slow query log，并结合 Box Anemometer 构建 slow query log 分析及优化系统。

## 3. 常见问题

### 3.1. 分页问题

https://www.cnblogs.com/billyxp/p/3535579.html 

```sql
select * from table where x＝‘？’ limit 0，20；
```
和
```sql
select * from table where x='?' limit 100000,20;
```
执行效率差了非常多：由于当 limit m,n 的时候，虽然都会扫描所有记录，但是当 m 越大的时候需要返回的数据就越多（从磁盘上返回的数据实际是 m+n 行），故消耗的 IO 也就越多，执行时间也就越慢。

总结
- 针对大分页带 order by 的查询 SQL 的优化方法是，添加等值限定字段 +order by 字段的联合索引，不要去管范围查询的字段。（status_id+cmt_id）不要管 flag。
- 增加定位查询，通过对上一页的定位来查找下一页，缩小 mysql 需要 sending data 的量。

## 4. 性能测试

### 4.1. Mysqlslap

https://my.oschina.net/moooofly/blog/152547 

mysqlslap 可以用于模拟服务器的负载，并输出计时信息。其被包含在 MySQL 的发行包中，MySQL\MySQL Server 5.7\bin\mysqlslap.exe；

测试时，可以指定并发连接数，可以指定 SQL 语句。如果没有指定 SQL 语句，mysqlslap 会自动生成查询 schema 的 SELECT 语句。

#### 4.1.1. 参数说明

- `-c，--concurrency` 指定并发数，也就是模拟多少个客户端同时执行 select。可以多个值，逗号分开。
- `-i，--iterations` 测试执行的迭代次数，代表要在不同并发环境下，各自运行测试多少次。
- `-a，--auto-generate-sql` 自动生成测试表和数据，表示用 mysqlslap 工具自己生成的 SQL 脚本来测试并发压力而不用自己写 SQL 语句。
- `--auto-generate-sql-load-type=type` 测试语句的类型。代表要测试的环境是读操作还是写操作还是两者混合的。取值包括：read，key，write，update 和 mixed（多种混合，默认模式)。
- `-q，--query=name` 使用自定义脚本执行测试，例如可以调用自定义的一个存储过程或者 sql 语句来执行测试。
- `--debug-info, -T` 打印内存和 CPU 的相关信息。

- `--auto-generate-sql-add-auto-increment` 代表对生成的表自动添加 auto_increment。
- `--number-char-cols=N, -x N` 自动生成的测试表中包含多少个字符类型的列，默认。
- `--number-int-cols=N, -y N` 自动生成的测试表中包含多少个数字类型的列，默认。
- `--number-of-queries=N` 总的测试查询次数（并发客户数×每客户查询次数。
- `--create-schema` 代表自定义的测试库名称，测试的 schema，MySQL 中 schema 也就是 database。
- `--commint=N` 多少条 DML 后提交一次。
- `--compress, -C` 如果服务器和客户端支持都压缩，则压缩信息传递。
- `--engine=engine_name, -e engine_name` 代表要测试的引擎，可以有多个，用分隔符隔开。例如：--engines=myisam,innodb。
- `--only-print` 只打印测试语句而不实际执行。
- `--detach=N` 执行 N 条语句后断开重连。

#### 4.1.2. 运行过程

mysqlslap 运行分三个阶段：
1. 创建 schema，table 和任何用来测试的已经存储了的程序和数据。这个阶段使用单客户端连接。
1. 进行负载测试。这个阶段使用多客户端连接。
1. 清除（断开连接，删除指定表）。这个阶段使用单客户端连接。

#### 4.1.3. 实例

- 多线程测试。使用–c 来模拟并发连接
  ```
  mysqlslap -a -c 100 -uroot -p123456
  ```
- 迭代测试。用于需要多次执行测试得到平均值
  ```
  mysqlslap -a -i 10 -uroot -p123456
  ```
- 提供自己的创建 SQL 语句和查询 SQL 语句，有 50 个客户端查询，每个查询 200 次
  ```
  mysqlslap --delimiter=";" --create="CREATE TABLE a (b int);INSERT INTO a VALUES (23)"  --query="SELECT * FROM a" --concurrency=50 --iterations=200 -uroot -p
  //–create ="用于生成一个表"	
  //–query 用这个表做测试，这里一般是 insert , 或 select 或 update 语句
  //–create 可以是几条 sql 语句 ，也可以是一个文件名，里面写满 sql 语句
  ```
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/30/b4c7d2f3dbd5ebbcf293e520134b3f7c.jpg)

  ```
  mysqlslap -a --concurrency=50,100 --number-of-queries 1000 --debug-info -uroot -proot
  // --number-of-queries 1000：指定查询总次数，总共进行 1000 次访问
  //-a, --auto-generate-sql 自动生成一条测试用的语句， 不用你写 sql 进行测试
  //--concurrency=50,100 分别测试并发为 50, 和 100
  // 这句的结果是分别测试了并发为 50, 和 100 时，总共进行 1000 次访问花费的时间
  ```
- 以自动生成测试表和数据的形式，分别模拟 50 和 100 个客户端并发连接处理 1000 个 query 的情况：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/30/1ea49b2814d245cb904d1b559860cd1d.jpg)

## 5. Refer Links

[Mysql 数据库 32 条军规](http://www.jianshu.com/p/b7849d3f8698)

[MySQL 数据库优化有哪些方式？](https://segmentfault.com/q/1010000000265631)

[谨慎合理使用索引](http://www.imooc.com/article/7253)

[适合使用索引的实例](http://blog.jobbole.com/111129/)