- [数据库：关系数据语言 - 标准 SQL](#sql)
  - [1. 概述](#1)
    - [1.1. 基本概念](#11)
    - [1.2. 发展历史](#12)
    - [1.3. SQL 特点](#13-sql)
    - [1.4. SQL 规范](#14-sql)
    - [1.5. 组成](#15)
  - [2. 数据定义 (DDL)](#2--ddl)
    - [2.1. 模式的定义与删除](#21)
      - [2.1.1. 模式的定义](#211)
      - [2.1.2. 模式的删除](#212)
    - [2.2. 表的定义、修改与删除](#22)
      - [2.2.1. 表的定义](#221)
      - [2.2.2. 表的修改](#222)
      - [2.2.3. 表的删除](#223)
    - [2.3. 索引的建立、修改与删除](#23)
      - [2.3.1. 建立索引](#231)
      - [2.3.2. 修改索引](#232)
      - [2.3.3. 删除索引](#233)
    - [2.4. 视图的建立与删除](#24)
      - [2.4.1. 定义视图](#241)
        - [2.4.1.1. 建立视图](#2411)
        - [2.4.1.2. 删除视图](#2412)
      - [2.4.2. 查询视图](#242)
      - [2.4.3. 更新视图](#243)
  - [3. 数据查询 (DQL)](#3--dql)
    - [3.1. 单表查询](#31)
      - [3.1.1. 选择若干列](#311)
      - [3.1.2. 选择若干元组](#312)
        - [3.1.2.1. 使用 DISTINCT 去重](#3121--distinct)
        - [3.1.2.2. 使用 WHERE 子句过滤元组](#3122--where)
      - [3.1.3. LIMIT 子句](#313-limit)
      - [3.1.4. ORDER BY 子句](#314-order-by)
      - [3.1.5. 聚集函数](#315)
      - [3.1.6. GROUP BY 子句](#316-group-by)
        - [3.1.6.1. 使用 HAVING 子句过滤分组](#3161--having)
        - [3.1.6.2. 使用 AS 取别名](#3162--as)
    - [3.2. 连接查询](#32)
      - [3.2.1. 内连接查询](#321)
        - [3.2.1.1. 等值 / 非等值连接查询](#3211)
        - [3.2.1.2. 自然连接查询](#3212)
        - [3.2.1.3. 自身连接查询](#3213)
      - [3.2.2. 外连接查询](#322)
        - [3.2.2.1. 左外连接查询](#3221)
        - [3.2.2.2. 右外连接查询](#3222)
        - [3.2.2.3. 全外连接查询](#3223)
      - [3.2.3. 交叉连接查询](#323)
      - [3.2.4. 连接查询的实现算法](#324)
      - [3.2.5. 连接查询的综合实例](#325)
    - [3.3. 嵌套查询](#33)
    - [3.4. 集合查询](#34)
      - [3.4.1. 并操作 UNION](#341--union)
      - [3.4.2. 交操作 INTERSECT](#342--intersect)
      - [3.4.3. 差操作 EXCEPT](#343--except)
  - [4. 数据操纵 (DML)](#4--dml)
    - [4.1. 插入数据 INSERT](#41--insert)
      - [4.1.1. 插入元组](#411)
      - [4.1.2. 插入子查询结果](#412)
      - [4.1.3. 使用 SELECT INTO 复制表](#413--select-into)
    - [4.2. 修改数据 UPDATE](#42--update)
      - [4.2.1. 单表数据更新](#421)
      - [4.2.2. 多表联合更新](#422)
        - [4.2.2.1. Theta 风格](#4221-theta)
        - [4.2.2.2. ANSI SQL 风格](#4222-ansi-sql)
    - [4.3. 删除数据 DELETE](#43--delete)
  - [5. 数据控制 (DCL)](#5--dcl)
    - [5.1. 权限控制](#51)
      - [5.1.1. GRANT](#511-grant)
      - [5.1.2. REVOKE](#512-revoke)
    - [5.2. 事务控制](#52)
  - [6. 空值处理](#6)

# 数据库：关系数据语言 - 标准 SQL

## 1. 概述

### 1.1. 基本概念

关系数据库的标准数据查询语言 SQL（Structured Query Language），是 1974 年由 Boyce 和 Chamberlin 提出的一种**介于关系代数与关系演算之间**的非过程化的结构化查询语言，是一个通用的、功能极强的关系性数据库语言。SQL 基于关系代数和元组关系演算，包括了数据定义语言和数据操纵语言，**其功能不仅仅是查询，而是包括了数据库模式创建、数据库数据插入与修改、数据库安全性完整性定义与控制等一系列功能**。

### 1.2. 发展历史

SQL 在 1986 年成为美国国家标准学会（ANSI）的一项标准（ANSI SQL 89），在 1987 年成为国际标准化组织（ISO）标准。**标准 SQL 由 ANSI 标准委员会管理，因此称为 ANSI SQL，所有 DBMS 都在 ANSI SQL 的基础上进行扩展**，如 T-SQL 等。

此后，各大数据库厂家纷纷推出各自的 SQL 软件或与 SQL 的接口软件，使得大多数数据库均用 SQL 作为共同的数据存取语言和标准接口，使得不同数据库系统之间的互操作有了共同的基础。同时，各个软件厂商对 SQL 基本命令集进行了不同程度的扩充和修改，可以支持标准外的一些功能特性。

目前 (2018/2/10)，SQL 的最新标准是 [ANSI SQL:2016](https://en.wikipedia.org/wiki/SQL:2016)，所有主要的关系数据库管理系统支持某些形式的 SQL，但**没有一个数据库系统能够支持 SQL 标准的所有概念和特性，大部分数据库至少遵守 ANSI SQL 92 标准的大部分功能**。

### 1.3. SQL 特点

- 综合统一

  SQL 集数据定义语言、数据操纵语言、数据控制语言的功能于一体，语言风格统一，可以独立完成数据库生命周期中的全部活动，为数据库应用系统的开发提供了良好的环境。

- 高度非过程化

  非关系数据模式的数据操纵语言是面向过程的语言，而 SQL 是高度非过程化的语言，只要提出“做什么”，而无须指明“怎么做”，SQL 的具体操作过程由系统自动完成，大大减轻了用户负担，且有利于提高数据独立性。

- 面向集合的操作方式

  非关系数据模型采用的是面向记录的操作方式，操作对象是一条记录。而 SQL 采用集合操作方式，不仅操作对象、查找结果可以是元组的集合，而且一次插入、删除、更新操作的对象也可以是元组的集合。

- 以同一种语法结构提供多种使用方式

  SQL 既可以是交互式的语言，又可以是嵌入式语言。用户可以在终端中直接使用 SQL 命令对数据库进行操作，也可以将 SQL 语句嵌入到高级编程语言中，供程序员设计程序时使用。两者语法结构基本相同，提供了极大的灵活性和便利性。

- 语言简洁，易学易用

  SQL 功能极强，但语言十分简洁，完成核心功能仅需要 9 个动词，易于学习和使用。

### 1.4. SQL 规范

- **SQL 不区分大小写，但一般将 SQL 关键词全部大写**，提高可读性。
- **多条 SQL 语句必须用分号 ; 分隔。**
- 尽量不使用通配符 “*”，影响性能，应直接写出所有所需字段。
- **处理 SQL 语句时，所有空格都将被忽略**，因此一般将一行 SQL 写成多行，提高可读性。

- 引号使用
  - 单引号：用于文本值和别名（为字段取别名一定要用单引号括起来，为表名取别名不用加单引号）。
  - 双引号：效果于单引号相同，但标准 SQL 不支持双引号。
  - 反引号：用于标识符（数据库名、表名、字段名），以区分与 SQL 保留字相同的标识符。
  例：
  ```sql
  SELECT * FROM `order` ‘a’, `desc` ‘b’ WHERE a.name = ‘Mike’ AND b.name = ‘Jack’;
  ```

### 1.5. 组成

SQL 包含四个部分：
- 数据定义语言 (Data Definition Language，**DDL**)：由 CREATE, DROP, ALTER, TRUNCATE 所组成。
- 数据查询语言 (Data Query Language, **DQL**)：核心指令是 SELECT。
- 数据操纵语言 (Data Manipulation Language, **DML**)：以 INSERT、UPDATE、DELETE 三种指令为核心。
- 数据控制语言 (Data Control Language, **DCL**)：由 GRANT, REVOKE, COMMIT, ROLLBACK, SAVEPOINT 组成。

## 2. 数据定义 (DDL)

关系数据库系统支持三级模式结构，其模式、外模式、内模式中的基本对象都有模式、表、视图、视图和索引等。因此，**SQL 的数据定义功能包括模式定义、表定义、视图定义和索引定义**。

**SQL 标准不提供修改模式定义和修改视图定义的操作**。因此，用户若希望修改这些对象，只能先将它们删除然后重建。

**SQL 标准也没有提供索引相关的语句，但为提高查询效率，商用关系数据库管理系统通常都提供了索引机制和相关的语句**。

一个关系数据库管理系统的实例中可以建立多个数据库，一个数据库中可以建立多个模式，一个模式下通常包含多个表、视图、索引等数据库对象。

> 数据库 > 模式 > 表 / 视图 / 索引

### 2.1. 模式的定义与删除

#### 2.1.1. 模式的定义

```sql
CREATE SCHEMA <schema_name> AUTHORIZATION <user_name>
```

若没有指定模式名，则模式名隐含为用户名。

模式的定义实际上定义了一个命名空间，在这个命名空间中可以进一步定义该模式包含的数据库对象，如基本表、视图、索引等。

#### 2.1.2. 模式的删除

```sql
DROP SCHEAM <schema_name> <CASCADE | RESTRICTT>
```

其中，CASCADE 和 RESTRICTT 两者必选其一：
- CASCADE：级联，表示在删除模式的同时把该模式中所有的数据库对象全部删除。
- RESTRICTT：限制，表示如果该模式中已经定义了下属的数据库对象（如表、视图等），则拒绝该删除语句的执行。只有当该模式中没有任何下属的对象时才能执行 `DROP SCHEMA` 语句。

### 2.2. 表的定义、修改与删除

#### 2.2.1. 表的定义

```sql
CREATE TABLE <table_name> （列名 1 数据类型 列级完整性约束条件，
                          ...，
                          列名 n 数据类型 列级完整性约束条件，
                          表级完整性约束条件)
```

#### 2.2.2. 表的修改

```sql
ALTER TABLE <table_name> 
ADD [COLUMN] 新列名 数据类型 完整性约束条件
ADD 表级完整性约束条件
DROP [COLUMN] [CASCADE | RESTRICTT]
DROP 表级完整性约束条件 [CASCADE | RESTRICTT]
ALTER COLUMN 列名 数据类型
```

#### 2.2.3. 表的删除

```sql
DROP TABLE <table_name> [CASCADE | RESTRICTT]
```
其中，CASCADE 和 RESTRICTT 两者必选其一：
- CASCADE：表示该表的删除没有限制条件。在删除基本表时，相关的依赖对象（如视图）都将被一起删除。
- RESTRICTT：表示该表的删除是有限制条件的。欲删除的表不能被其它表的约束所使用，不能有视图，不能有触发器，不能有存储过程或函数等。若存在这些依赖该表的对象，则此表不能被删除。

### 2.3. 索引的建立、修改与删除

当表的数据量比较大时，查询操作会比较耗时，这种情况下建立索引是加快查询速度的有效手段，用户可以根据应用环境的需要在基本表上建立一个或多个索引，以提供多种存取路径，加快查找速度。

数据库索引有多种类型，常见索引包括以下几种：
- 顺序文件上的索引：针对按指定属性值升序或降序存储的关系，在该属性上建立一个顺序索引文件，索引文件由属性值和相应的元组指针组成。
- B+ 树索引：将索引属性组织成 B+ 树形式，B+ 树的叶结点为属性值和相应的元组指针。B+ 树索引具有动态平衡的优点。
- 散列索引：建立若干个桶，将索引属性按照其散列函数值映射到相应桶中，桶中存放索引属性值和相应的元组指针。散列索引具有速度快的优点。
- 位图索引：用位向量记录索引属性中可能出现的值，每个位向量对应一个可能值。

索引虽能加速数据库查询，但需要占用一定的存储空间，当基本表更新时，索引要进行相应的维护，这些操作都会相应地增加数据库的负担，因此要根据实际应用的需求有选择地创建索引。

**关系数据库管理系统在执行查询时会自动选择合适的索引作为存取路径，用户不必也不能显式地选择索引。因此，索引是关系数据库管理系统的内部实现技术，属于内模式的范畴。**

#### 2.3.1. 建立索引

```sql
CREATE [UNIQUE] [CLUSTER] INDEX 索引名
ON 表名 ( 列名 次序，列名 次序 ... )
```
索引可建立在该表的一列或多列上，各列名之间用逗号分隔，每个列名后可以指定索引值的排列次序，可选 ASC 或 DESC。

UNIQUE 表示该索引的每一个索引值只对应唯一的数据记录。

CLUSTER 表示要建立的索引是聚簇索引。

#### 2.3.2. 修改索引

对于已建立的索引，若需要对其进行重命名，可以通过修改索引实现：
```sql
ALTER INDEX 旧索引名 RENAME TO 新索引名
```

#### 2.3.3. 删除索引

```sql
DROP INDEX 索引名
```
删除索引后，系统会同时从数据字典中删除有关该索引的描述。（数据字典是关系数据库管理系统内部的一组系统表，它记录了数据库中所有的定义信息。执行 DDL 时，实际上就是在更新数据字典中的相应信息）

### 2.4. 视图的建立与删除

视图是从一个或几个基本表（或视图）导出的表。与基本表不同，视图是一个虚表，数据库中只存放视图的定义，而不存放视图对应的数据，这些数据仍存放在原来的基本表中。

视图就像一个窗口，透过它可以看到数据库中自己感兴趣的数据及其变化。合理使用视图能有许多好处：
- 简化用户操作
- 使用户能以多种角度看待同一数据
- 对重构数据库提供了一定程度的逻辑独立性
- 对机密数据提供安全保护
- 合理利用视图可以更清晰地表达查询

视图定义后，可以和基本表一样被查询、删除，还可以在视图上再定义新的视图，但对视图的更新操作则有一定的限制。

#### 2.4.1. 定义视图

##### 2.4.1.1. 建立视图

```sql
CREATE VIEW 视图名 [( 列名，列名 ... )]
AS 子查询
[WITH CHECK OPTION];
```
其中，WITH CHECK OPTION 表示对视图进行更新、插入、删除操作时要保证相应的行满足视图定义中的谓词条件（即子查询中的条件表达式）。

##### 2.4.1.2. 删除视图

```sql
DROP VIEW 视图名 [CASCADE];`
```

#### 2.4.2. 查询视图

视图定义后，可以像对基本表一样对视图进行查询。

关系数据库管理系统执行对视图的查询时，首先进行有效性检查，检查查询中涉及的表、视图等是否存在。如果存在，则从数据字典中去除视图的定义，把定义中的子查询和用户的查询结合起来，转换成等价的对基本表的查询，然后再执行修正了的查询。这一过程称为视图消解。

#### 2.4.3. 更新视图

更新视图指的是通过视图来插入、删除、修改数据。

由于视图是不实际存储数据的虚表，因此对视图的更新最终要转换成对基本表的更新。与视图查询类型，视图更新也需要通过视图消解，转换为对基本表的更新操作。

在关系数据库中，并不是所有的视图都是可更新的，因为有些视图的更新不能唯一地有意义地转换成对相应基本表的更新。目前，各个关系数据库管理系统一般都只允许对行列子集视图进行更新，而且各个 DBMS 对视图的更新还有更进一步的规定。由于实现方法上的差异，这些规定都不尽相同。需要注意的是，不可更新的视图和不允许更新的视图是两个不同的概念。

## 3. 数据查询 (DQL)

数据查询是数据库的核心操作，SQL 提供了 SELECT 语句进行数据查询，该语句一般格式如下：
```sql
SELECT
	字段 1,
	字段 2,
	字段 3 ,...
FROM
	表名 [ IN 数据库名 ] 
[ WHERE 条件 ]
[ GROUP BY 条件 ]
[ HAVING 条件 ]
[ ORDER BY 字段 ] 
[ LIMIT 起始行，行数 ];
```
SELECT 语句中的**各种子句的相对顺序不可随意变换，否则会出现语法错误**。

### 3.1. 单表查询

单表查询指的是仅涉及一个表的查询。

#### 3.1.1. 选择若干列

- 查询指定列

  通过在 SELECT 子句的目标列表达式中指定要查询的属性列，查询指定列。

- 查询全部列

  通过在 SELECT 关键字后列出所有列名，或使用通配符`*`，查询全部列。

- 查询经过计算的值

  SELECT 子句的**目标列表达式不仅可以是表中的属性列，也可以是表达式**。也就是说，可在 SELECT 语句中，直接对查询结果进行计算。一般来说，在数据库服务器上完成这样的操作比在客户端中完成要快得多。

  - 字符串拼接
    ```sql
    SELECT `name`, 'year of birth:', `2018-age`, LOWER(dept) FROM `student`;
    ```
    在 MySQL 中，字符串拼接必须通过函数实现，如 `CONCAT(str1, str2,…)`。

  - 算术计算
    ```sql
    SELECT `name`, `age`, `price`*0.8 FROM `goods`; -- 每取出 price 字段的值都*0.8 后再显示
    ```

#### 3.1.2. 选择若干元组

##### 3.1.2.1. 使用 DISTINCT 去重

两个原本并不相同的元组在投影到指定的某些列上后，可能会变成相同的行，可以通过 DISTINCT 指示数据库只返回不同的值。

例：
```sql
SELECT DISTINCT var_name FROM tb_name
```
NOTE
- **DISTINCT 关键字只能作用于所有的列，无法仅对个别列起作用**。因此，若`SELECT DISTINCT ven_id, prod_price`，因为指定的两列不完全相同，会导致无法检索出所有的行。
- **DISTINCT 关键字必须用于列名，不可用于计算或表达式**，例如，不可用于`COUNT(*)`。

##### 3.1.2.2. 使用 WHERE 子句过滤元组

查询满足指定的条件的元组可以通过 WHERE 子句实现。

NOTE
- **若将数据过滤的任务移交到应用层，会极大影响应用的性能，并导致创建的应用完全不具备可伸缩性，另外会导致服务器不得不发送多余数据，浪费网络带宽。**

#### 3.1.3. LIMIT 子句

对于限制输出行数的操作，各个 DBMS 规定各不相同，如：
- SQL Server 使用 TOP。
- DB2 使用 FECTH。
- Oracle 使用 ROWNUM。
- **MySQL、MariaDB、PostgreSQL、SQLite 使用 LIMIT**。

以 MySQL 得 LIMIT 为例，用法有以下几种：
```sql
[LIMIT 行数』
[LIMIT 起始行，行数』
[LIMIT 行数 OFFSET 起始行』
```
NOTE
- **LIMIT 关键字从第 0 行开始计数，而不是第 1 行**。
- **LIMIT 后边不能接表达式，不能接用户自定义变量 (@xxx)，但可以接本地 / 局部变量 (xxx)**。

#### 3.1.4. ORDER BY 子句

用户可以用 ORDER BY 子句对查询结果按照一个或多个属性列的升序 (ASC) 或降序 (DESC) 排列，默认为升序排列。

- `ORDER BY` 子句可以使用非检索的列进行排序。

- `ORDER BY` 子句**支持层级排序，即指定多个列进行排序**，实际排序的顺序完全按照指定列的顺序进行，如：
  ```sql
  SELECT XXX FROM XXX ORDER BY var1, var2
  ```
  则会对查询结果先按照 var1 进行排序，若 var1 相同，再按照 var2 进行排序，若 var1 都各不相同，则不会使用 var2 进行排序；

- `ORDER BY` 子句**支持按照列的相对位置进行排序**，如：
  ```sql
  SELECT col1, col2 FROM XXX ORDER BY 1, 2
  ```
  则相当于 ORDER BY col1, col2

- **DESC 关键字只能作用到位于其前面的列名，若想对多个列都进行降序排序，必须对每一列都指定 DESC 关键字**；

- 对于空值，排序时显示的次序由具体的数据库系统实现来决定。因此，**若要保证数据库的可移植性，不应在属性值中存在 NULL 值**。

#### 3.1.5. 聚集函数

为方便用户使用，增强检索功能，SQL 提供了许多聚集函数：
- `COUNT(*)`: 统计元组个数。
- `COUNT( [DISTINCT|ALL] 列名 )`: 统计一列中值的个数。
- `SUM( [DISTINCT|ALL] 列名 )`: 计算一列值的总和（此列必须是数值型）。
- `AVG( [DISTINCT|ALL] 列名 )`: 计算一列值的平均值（此列必须是数值型）。
- `MAX( [DISTINCT|ALL] 列名 )`: 求一列值中的最大值。
- `MAX( [DISTINCT|ALL] 列名 )`: 求一列值中的最小值。

NOTE
- 若指定 DISTINCT，则表示计算时要取消指定列中的重复值。若不指定 DISTINCT 或指定 ALL（默认值），则表示不取消重复值。

- **当聚集函数遇到空值时，除 `COUNT(*)` 外，都跳过空值而只处理非空值**。

- **聚集函数只能用于 SELECT 子句和 GROUP BY 中的 HAVING 子句中，不可用于 WHERE 子句中**。

#### 3.1.6. GROUP BY 子句

通过 GROUP BY 子句可以将查询结果按某一列或多列的值进行分组，值相等为一组。

NOTE
- 对结果进行分组的目的是为了细化聚集函数的作用对象。如果未对查询结果分组，聚集函数将作用于整个查询结果。**分组后聚集函数将作用于每一个组，即每一组都有一个函数值**。

- 如果在 SELECT 语句中使用表达式，则必须在 GROUP BY 子句中指定相同的表达式，不可使用别名。

- 如果分组中有包含 NULL 值的行，则将 NULL 作为一个分组返回。

- 分组后 SELECT 操作的对象是每个组中的各个组员，最终显示出来的是每组占一行，该行记录是每个组被 SELECT 的对象。

例：
```sql
SELECT
	t.Request_at AS `Day`,
	ROUND(
		SUM(IF (t.Status = 'completed', 0, 1)) / COUNT(*), 2
	) AS `Cancellation Rate`
FROM
	Trips t
INNER JOIN Users u ON t.Client_Id = u.Users_Id
WHERE
	t.Request_at BETWEEN '2013-10-01' AND '2013-10-03'
		AND u.Banned = 'No'
		AND u.Role = 'client'
GROUP BY
	t.Request_at
ORDER BY
	t.Request_at;
```

##### 3.1.6.1. 使用 HAVING 子句过滤分组

HAVING 子句用于过滤掉不匹配的分组。

例：
```sql
SELECT cust_id, COUNT(*) AS orders
FROM orders
GROUP BY cust_id
HAVING COUNT(*) >= 2;
```

NOTE
- HAVING 子句与 WHERE 子句的异同：
  - 语法：HAVING 子句与 WHERE 子句在语法上基本相同，唯一差异在于 HAVING 子句中可以使用聚集函数作为条件表达式，但 WHERE 子句中不可使用聚集函数。
  - 效果：WHERE 在数据分组前进行过滤，从基本表或视图中选择满足条件的元组。HAVING 在分组后进行过滤，从分组中选择满足条件的组。

- 一般使用 GROUP BY 子句时，应该也给出 ORDER BY 子句。

##### 3.1.6.2. 使用 AS 取别名

可以通过 AS 使得 select 结果中显示的列名为所取的别名。
```sql
SELECT name AS ‘姓名’, age AS ‘年龄’ FROM user WHERE id<100;
```
为表取别名：多表联合操作时常用
<!-- todo: 不用为什么会报错？ -->
```sql
SELECT s.name AS ‘学生姓名’, t.name AS ‘教师姓名’ FROM students AS s, teacher AS t;
```
```sql
SELECT s.name ‘学生姓名’, t.name ‘教师姓名’ FROM students s, teacher t;//AS 关键字可用空格代替
```

NOTE
- 大多数 DBMS 中，AS 关键字可以省略，但一般不建议省略。
- 如果在 SELECT 语句中使用表达式，则必须在 GROUP BY 子句中指定相同的表达式，不可使用别名。

### 3.2. 连接查询

**若一个查询同时涉及两个以上的表，则称为连接查询。连接查询是关系数据库中最主要的查询**，包括等值连接查询、自然连接查询、非等值连接查询、自身连接查询、外连接查询和复合条件连接查询等。

**ANSI SQL 中提供了 JOIN 语句用于连接查询操作**，其中包括了五种 JOIN 方式：内连接 (INNER), 全外连接 (FULL OUTER), 左外连接 (LEFT OUTER), 右外连接 (RIGHT OUTER) 和交叉连接 (CROSS)。

**连接查询中，运行时连接的表越多，性能越差**。

#### 3.2.1. 内连接查询

内连接是一种最常用的连接类型。内连接查询实际上是一种任意条件的查询。使用内连接时，如果两个表的相关字段满足连接条件，就从这两个表中提取数据并组合成新的记录，也就是**在内连接查询中，只有满足条件的元组才能出现在结果关系中**。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/2/11/ffb88f60dd64ae7523a2f6795d4b4141.jpg)

内连接查询可分为：等值连接查询、非等值连接查询、自然连接查询和自身连接查询。

##### 3.2.1.1. 等值 / 非等值连接查询

连接查询中的 WHERE 子句用来连接两个表的条件称为连接条件，**连接条件中的各个连接字段必须是可比的，但名字不必相同**。当连接条件中的连接运算符为`=`时，称为等值连接，否则称为非等值连接。

例：

ANSI SQL 风格
```sql
SELECT vend_name, prod_name, prod_price
FROM Vendors INNER JOIN Products
ON Vendors.vend_id = Products.vend_id
-- INNER JOIN 中 INNER 关键字可省略，即 INNER JOIN 与 JOIN 完全等效；
```

Theta 风格
```sql
SELECT vend_name, prod_name, prod_price
FROM Vendors, Products
WHERE Vendors.vend_id = Products.vend_id
-- 若属性名在参加连接的各个表中是唯一的，则可以省略表名前缀。 
```

##### 3.2.1.2. 自然连接查询

**自然连接是特殊的等值连接**。若在等值连接中将目标列中重复的属性列去除，则称为自然连接查询。

例：
```sql
SELECT s.*, t.teacher_name FROM student_table s NATURAL JOIN teacher_table t; 
```

##### 3.2.1.3. 自身连接查询

如果在一个连接查询中，涉及到的两个表都是同一个表，这种查询就称为自连接查询。同一张表在 FROM 字句中多次出现，**为了区别该表的每一次出现，需要为表定义一个别名**。

自连接是一种特殊的内连接，它相互连接的表在物理上为同一张表，但可以在逻辑上分为两张表。

例：

ANSI SQL 风格
```sql
SELECT c1.cust_id, c1.cust_name
FROM Customers AS c1 INNER JOIN CUstomers AS c2
ON c1.cust_name = c2.cust_name
WHERE c2.cust_contact = ‘Jim’
```

Theta 风格
```sql
SELECT c1.cust_id, c1.cust_name
FROM Customers AS c1, CUstomers AS c2
WHERE c1.cust_name = c2.cust_name AND c2.cust_contact = ‘Jim’
```

NOTE: **若两个要关联表的字段名是一样的，可以使用 USING 语句减少 SQL 语句的长度**。
```sql
SELECT c1.cust_id, c1.cust_name
FROM Customers AS c1 INNER JOIN CUstomers AS c2
USING(cust_name)
WHERE c2.cust_contact = ‘Jim’
```
USING 语句现已被 MySQL，Oracle，PostgreSQL，SQLite，和 DB2/400 等 DBMS 支持。

自身连接查询经典题目：
- [leetcode human-traffic-of-stadium](https://leetcode.com/problems/human-traffic-of-stadium)
- [leetcode consecutive-numbers](https://leetcode.com/problems/consecutive-numbers/description/)

#### 3.2.2. 外连接查询

内连接的查询结果都是满足连接条件的元组，若希望将悬浮元组也保留在结果关系中，就需要使用外连接查询。外连接是只限制一张表中的数据必须满足连接条件，而另一张表中的数据可以不满足连接条件的连接方式。**也就是说，在外连接查询中，包含了不是多个表共有的记录，不是只有满足条件的元组才能出现在结果关系中。**

外连接根据全集端不同，又分为左外连接（简称左连接），右外连接（简称右连接）和全连接。

NOTE: **在 MySQL 中，关键字 RIGHT OUTER JOIN 等同于 RIGHT JOIN，LEFT OUTER JOIN 等同于 LEFT JOIN**。

##### 3.2.2.1. 左外连接查询

左连接查询会获取左表所有记录，即使右表没有对应匹配的记录。

`tb1 LEFT OUTER JOIN tb2` 表示左连接查询，tb1 为左表， tb2 为右表。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/2/11/b3891cbb63ce64ba7d47a37189369064.jpg)

例：
```sql
SELECT s.*, t.teacher_name 
FROM student_table s 
LEFT OUTER JOIN teacher_table t 
ON s.teacher > t.teacher_id; 
```

##### 3.2.2.2. 右外连接查询

与 LEFT JOIN 相反，右连接查询会获取右表所有记录，即使左表没有对应匹配的记录。

`tb1 RIGHT OUTER JOIN tb2` 表示右连接查询，tb1 为左表，tb2 为右表。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/2/11/2efc8e908de35c1c894c8ad41bc2920d.jpg)

例：
```sql
SELECT s.*, t.teacher_name
FROM student_table s 
RIGHT OUTER JOIN teacher_table t 
ON s.teacher > t.teacher_id; 
```

##### 3.2.2.3. 全外连接查询

全外连接会把两个表中所有不满足连接条件的记录取出。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/2/11/6ee885f7025010b2b6065d724e2a7c11.jpg)

例：
```sql
SELECT s.*, t.teacher_name 
FROM student_table s 
FULL OUTER JOIN teacher_table t 
ON s.teacher > t.teacher_id; 
```

NOTE: 一些数据库管理系统中（如 MySQL) 并不直接支持全外连接，但**可通过先左联结，再右联结，最后将两者使用 UNION 运算得到全外联结查询的结果**。
```sql
SELECT *
FROM employee 
  LEFT JOIN department 
    ON employee.DepartmentID = department.DepartmentID
UNION
SELECT *
FROM   employee
  RIGHT JOIN department
    ON employee.DepartmentID = department.DepartmentID
WHERE  employee.DepartmentID IS NULL
```

#### 3.2.3. 交叉连接查询

若连接查询时不指定 WHERE 条件或连接条件为"永真"，会返回两个表中所有记录的**笛卡儿积**，行数是表 1 行数与表 2 行数的乘积，此时也称为交叉连接查询。

交叉连接也称为笛卡尔连接。

例：

Theta 风格
```sql
SELECT vend_name, prod_name, prod_price
FROM Vendors, Products
```

ANSI 风格（在 MySQL 中定义了 CROSS JOIN 关键字，无需任何查询条件，效果与传统 Theta 风格相同）
```sql
SELECT vend_name, prod_name, prod_price
FROM Vendors CROSS JOIN Products
```

NOTE: **交叉连接不会应用任何谓词去过滤结果表中的记录，程序员可以用 WHERE 语句进一步过滤结果集**。

#### 3.2.4. 连接查询的实现算法

执行一个连接操作，存在三种基本的算法：

- 嵌套循环连接算法 (LOOP JOIN)

  类似于 C 语言编程时的双重循环。作为外层循环逐行扫描的表，称为外部输入表；针对外部输入表的每一行，要逐行扫描检查匹配的另一张表，称为内部输入表（相当于内层循环）。**适用于外部输入表的行数较少，内部输入表建立了索引的情形**。

- 合并连接算法 (MERGE JOIN)

  类似于两个有序数组的合并。两个输入表都在合并列上排序；然后依序对两张表逐行做连接或舍弃。**如果预先建好了索引，合并连接的计算复杂度是线性的**。

- 哈希连接算法 (HASH JOIN)

  哈希连接选择行数较小的输入表作为生成输入，对其连接列值应用哈希函数，把其行（的存储位置）放入哈希桶中。**适用于查询的中间结果，通常是无索引的临时表；以及中间结果的行数很大时**。

#### 3.2.5. 连接查询的综合实例

Person 表：

+----------+-----------+----------+
| PersonId | FirstName | LastName |
+----------+-----------+----------+
|        1 | Wang      | Allen    |
|        2 | Mary      | John     |
+----------+-----------+----------+

Address 表：

+-----------+----------+----------+-----------+
| AddressId | PersonId | City     | State     |
+-----------+----------+----------+-----------+
|         1 |        2 | BeiJing  | BeiJing   |
|         2 |        5 | ShaoGuan | GuangDong |
+-----------+----------+----------+-----------+

内连接
```sql
mysql> select * from Person join Address on Person.PersonId = Address.PersonId;
```
+----------+-----------+----------+-----------+----------+---------+---------+
| PersonId | FirstName | LastName | AddressId | PersonId | City    | State   |
+----------+-----------+----------+-----------+----------+---------+---------+
|        2 | Mary      | John     |         1 |        2 | BeiJing | BeiJing |
+----------+-----------+----------+-----------+----------+---------+---------+
可见内连接只输出满足连接条件的行。

左连接
```sql
mysql> select * from Person left join Address on Person.PersonId = Address.PersonId;
```
+----------+-----------+----------+-----------+----------+---------+---------+
| PersonId | FirstName | LastName | AddressId | PersonId | City    | State   |
+----------+-----------+----------+-----------+----------+---------+---------+
|        2 | Mary      | John     |         1 |        2 | BeiJing | BeiJing |
|        1 | Wang      | Allen    |      NULL |     NULL | NULL    | NULL    |
+----------+-----------+----------+-----------+----------+---------+---------+
可见，左连接输出左边表的全集，不满足条件的右表输出 NULL。

右连接
```sql
mysql> select * from Person right join Address on Person.PersonId = Address.PersonId;
```
+----------+-----------+----------+-----------+----------+----------+-----------+
| PersonId | FirstName | LastName | AddressId | PersonId | City     | State     |
+----------+-----------+----------+-----------+----------+----------+-----------+
|        2 | Mary      | John     |         1 |        2 | BeiJing  | BeiJing   |
|     NULL | NULL      | NULL     |         2 |        5 | ShaoGuan | GuangDong |
+----------+-----------+----------+-----------+----------+----------+-----------+
结果不言而喻。

全连接
```sql
mysql> select * from Person  left join Address on Person.PersonId = Address.PersonId
union (select * from Person right join Address on Person.PersonId = Address.PersonId);
```
+----------+-----------+----------+-----------+----------+----------+-----------+
| PersonId | FirstName | LastName | AddressId | PersonId | City     | State     |
+----------+-----------+----------+-----------+----------+----------+-----------+
|        2 | Mary      | John     |         1 |        2 | BeiJing  | BeiJing   |
|        1 | Wang      | Allen    |      NULL |     NULL | NULL     | NULL      |
|     NULL | NULL      | NULL     |         2 |        5 | ShaoGuan | GuangDong |
+----------+-----------+----------+-----------+----------+----------+-----------+

交叉连接
```sql
mysql> select * from Person  cross join Address;
```
+----------+-----------+----------+-----------+----------+----------+-----------+
| PersonId | FirstName | LastName | AddressId | PersonId | City     | State     |
+----------+-----------+----------+-----------+----------+----------+-----------+
|        1 | Wang      | Allen    |         1 |        2 | BeiJing  | BeiJing   |
|        2 | Mary      | John     |         1 |        2 | BeiJing  | BeiJing   |
|        1 | Wang      | Allen    |         2 |        5 | ShaoGuan | GuangDong |
|        2 | Mary      | John     |         2 |        5 | ShaoGuan | GuangDong |
+----------+-----------+----------+-----------+----------+----------+-----------+

### 3.3. 嵌套查询

在 SQL 中，一个 `SELECT-FROM-WHERE` 语句称为一个查询块，将一个查询块嵌套在另一个查询块的 WHERE 子句或 HAVING 短语的条件中的查询称为嵌套查询查询。

嵌套查询使得用户可以通过多个简单查询构成复杂的查询，从而增强 SQL 的查询能力，以层层嵌套的方式来构造程序正式 SQL 中“结构化”的含义所在。

NOTE: 外连接查询得到的结果也可以通过关联子查询得到。
```sql
SELECT employee.LastName, employee.DepartmentID, department.DepartmentName 
FROM employee LEFT OUTER JOIN department 
  ON employee.DepartmentID = department.DepartmentID
```
相当于
```sql
SELECT employee.LastName, employee.DepartmentID,
  (SELECT department.DepartmentName 
    FROM department
  WHERE employee.DepartmentID = department.DepartmentID)
FROM employee
```
但需要注意的是，数据库管理系统处理连接查询的性能远远优于处理嵌套查询。

### 3.4. 集合查询

SELECT 语句的查询结果是元组的集合，因此多个 SELECT 语句的结果可进行集合操作。集合操作主要包括并操作 UNION，交操作 INTERSECT 和差操作 EXCEPT。

**参加集合操作的各查询结果的列数必须相同，对应项的数据类型也必须相同。**

#### 3.4.1. 并操作 UNION

UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中。

MySQL UNION 操作符语法格式：
```SQL
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions]
UNION [ALL | DISTINCT]
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions];
```
参数
expression1, expression2, ... expression_n: 要检索的字段；
- tables: 要检索的数据表；
- WHERE conditions: 可选，设置检索条件；
- DISTINCT: 可选，删除结果集中重复的数据。默认情况下 UNION 操作符已经删除了重复数据，所以 DISTINCT 修饰符对结果没啥影响；
- ALL: 可选，返回所有结果集，包含重复数据；

例：
```sql
SELECT country FROM Websites
UNION
SELECT country FROM apps
ORDER BY country;
```

NOTE
- UNION 中的每个查询必须包含相同的列、表达式或聚集函数，但次序可不同。
- 列的数据类型必须兼容，即可被 DBMS 隐式转换。
- UNION 查询结果会自动去除重复的元组，若想返回所有匹配的行，应该使用 UNION ALL 操作符。
- UNION 查询中只能使用一条 ORDER BY 子句，必须位于最后一条 SELECT 语句之后，可对所有结果进行排序。

#### 3.4.2. 交操作 INTERSECT

例：查询计算机系的学生与年龄不大于 19 岁的学生的交集
```sql
SELECT *
FROM student
WHERE dept='CS'
INTERSECT
SELECT *
FROM student
WHERE age <= 19;
```
等同于
```sql
SELECT * 
FROM student
WHERE dept='CS' AND age <= 19;
```

#### 3.4.3. 差操作 EXCEPT

例：查询计算机系的学生与年龄不大于 19 岁学生的差集
```sql
SELECT *
FROM student
WHERE dept='CS'
EXCEPT
SELECT *
FROM student
WHERE age <= 19;
```
等同于
```sql
SELECT *
FROM student
WHERE dept='CS' AND age > 19;
```

## 4. 数据操纵 (DML)

### 4.1. 插入数据 INSERT

#### 4.1.1. 插入元组

```sql
INSERT INTO 表名 ( 字段 1, 字段 2, 字段 3, ... ) VALUES ( ‘值 1’, ‘值 2’, ‘值 3’, ... );
```

NOTE
- 若省略字段名列表，则默认将所有字段作为插入字段，这种方法并不安全，应尽量避免使用。
- 值的顺序必须与字段的顺序一一对应。
- VALUES 后边每个括号表示一条完整的记录值，可同时插入多条记录，即多个括号。

#### 4.1.2. 插入子查询结果

INSERT 还有一种使用形式，可将 SELECT 语句的查询结果插入表中，即 INSERT SELECT；

例：
```sql
INSERT INTO Customers(cust_id, cust_name) SELECT cust_id, cust_name FROM CustomersOther;
```

NOTE: 这种形式的 INSERT 不用加 VALUES。

#### 4.1.3. 使用 SELECT INTO 复制表

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

### 4.2. 修改数据 UPDATE

```sql
UPDATE 表名 SET 字段 1 = 值 1, 字段 2 = 值 2, ... [ WHERE 条件 ] [ ORDER BY 字段 ] [ LIMIT 行数 ];
```
若省略 WHERE 字句意味着 WHERE 字句的值总是为 true；

#### 4.2.1. 单表数据更新

例：
```sql
UPDATE `table_name` SET `col1` = xx, `col2` = yy where `col` = zz;
```

NOTE: 要删除某个列的值，可使用 UPDATE 将其设置为 NULL。

#### 4.2.2. 多表联合更新

##### 4.2.2.1. Theta 风格

例：
```sql
UPDATE `test_1`, `test_2` SET `test_1`.`name` = `test_2`.`name` WHERE `test_1`.`id`=100 AND `test_2`.`id`=200;
```
或
```sql
UPDATE `test_1` a, `test_2` b SET a.name = b.name WHERE a.id=100 AND b.id=200;
```

NOTE: **多表联合操作必须将所有表名在 UPDATE/SELECT/DELETE 关键字都列出，以下写法是错误的**：
```sql
UPDATE `test_1` SET `test_1`.`name` = `test_2`.`name` WHERE `test_1`.`id`=100 AND `test_2`.`id`=200;
```
会报错：
> [Err] 1054 - Unknown column 'test_2.id' in 'where clause'

##### 4.2.2.2. ANSI SQL 风格

多表联合 update 也可以使用 JOIN 子句连接多个表为一张表，再进行 update 操作。
```sql
UPDATE table_1 SET score = score + 5 WHERE uid IN (select uid from table_2 where sid = 10);
```
替换成 JOIN 方式： 
```sql
UPDATE (table_1 t1 INNER JOIN table_2 t2 ON t1.uid = t2.uid) SET score = score + 5 WHERE t2.sid = 10;
```

### 4.3. 删除数据 DELETE

```sql
DELETE  FROM 表名 [WHERE 条件』 [ORDER BY 字段』 [LIMIT 行数』;
```

NOTE: 若需要从表中删除所有行，可直接使用 TRUNCATE TABLE 语句，它比 DELETE 执行速度更快（因为不需要记录数据的变动）。

## 5. 数据控制 (DCL)

### 5.1. 权限控制

SQL 中使用 GRANT 和 REVOKE 语句向用户授予或收回对数据的操作权限。

#### 5.1.1. GRANT

GRANT 语句用于向用户授权：
```sql
GRANT 权限 ON 数据库 . 数据表 TO 用户名 @登录主机 INDENTIFIED BY “密码”;
```

例：
```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON *.* TO jq@localhost INDENTIFIED BY “jqjqjqjq”;
```

#### 5.1.2. REVOKE

REVOKE 语句用于收回授予用户的权限：
```sql
REVOKE 权限 ON 数据库 . 数据表 FROM 用户名 [CASCADE|RESTRICTT];
```

### 5.2. 事务控制

- COMMIT

- ROLLBACK

- SAVEPOINT

## 6. 空值处理

空值是指“不指定”、“不存在”、“无意义”的值。SQL 语言允许元组在以下情况取空值：
- **该属性应该有一个值，但目前还不知道它的具体值**。如，某学生的年龄属性由于登记时遗漏，暂时不知道该学生的年龄，因此取空值。
- **该属性不应该有值**。如，缺考学生的成绩为空。
- **由于某种原因不便于填写**。如，一个人的电话号码不想让其他人知道，则取空值。

空值的判断：SQL 中可以通过 IS NULL 和 IS NOT NULL 进行空值的判断。

空值的约束条件：SQL 中可以通过 NOT NULL 或 UNIQUE 限制属性不能取空值。码属性都不能取空值。

**空值与另一个任意值的算术运算结果为空值，与另一个任意值的比较运算结果为 UNKNOWN**。