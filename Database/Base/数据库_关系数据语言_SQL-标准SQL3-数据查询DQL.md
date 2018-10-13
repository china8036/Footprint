- [数据库：关系数据语言 - 标准 SQL - 数据查询(DQL)](#数据库关系数据语言---标准-sql---数据查询dql)
  - [1. 单表查询](#1-单表查询)
    - [1.1. 选择若干列](#11-选择若干列)
    - [1.2. 选择若干元组](#12-选择若干元组)
      - [1.2.1. 使用 DISTINCT 去重](#121-使用-distinct-去重)
      - [1.2.2. 使用 WHERE 子句过滤元组](#122-使用-where-子句过滤元组)
    - [1.3. LIMIT 子句](#13-limit-子句)
    - [1.4. ORDER BY 子句](#14-order-by-子句)
    - [1.5. 聚集函数](#15-聚集函数)
    - [1.6. GROUP BY 子句](#16-group-by-子句)
      - [1.6.1. 使用 HAVING 子句过滤分组](#161-使用-having-子句过滤分组)
      - [1.6.2. 使用 AS 取别名](#162-使用-as-取别名)
  - [2. 连接查询](#2-连接查询)
    - [2.1. 内连接查询](#21-内连接查询)
      - [2.1.1. 等值 / 非等值连接查询](#211-等值--非等值连接查询)
      - [2.1.2. 自然连接查询](#212-自然连接查询)
      - [2.1.3. 自身连接查询](#213-自身连接查询)
    - [2.2. 外连接查询](#22-外连接查询)
      - [2.2.1. 左外连接查询](#221-左外连接查询)
      - [2.2.2. 右外连接查询](#222-右外连接查询)
      - [2.2.3. 全外连接查询](#223-全外连接查询)
    - [2.3. 交叉连接查询](#23-交叉连接查询)
    - [2.4. 连接查询的实现算法](#24-连接查询的实现算法)
    - [2.5. 连接查询的综合实例](#25-连接查询的综合实例)
  - [3. 嵌套查询](#3-嵌套查询)
  - [4. 集合查询](#4-集合查询)
    - [4.1. 并操作 UNION](#41-并操作-union)
    - [4.2. 交操作 INTERSECT](#42-交操作-intersect)
    - [4.3. 差操作 EXCEPT](#43-差操作-except)
  - [5. Refer Links](#5-refer-links)

# 数据库：关系数据语言 - 标准 SQL - 数据查询(DQL)

数据查询是数据库的核心操作，SQL 提供了 `SELECT` 语句进行数据查询，该语句一般格式如下：
```sql
SELECT
	字段 1,
	字段 2,
	字段 3 ,...
FROM
	表名 [ IN database_name ] 
[ WHERE condition ]
[ GROUP BY condition ]
[ HAVING condition ]
[ ORDER BY field ] 
[ LIMIT begin_line, line_number ];
```
SELECT 语句中的**各种子句的相对顺序不可随意变换，否则会出现语法错误**。

## 1. 单表查询

单表查询指的是仅涉及一个表的查询。

### 1.1. 选择若干列

- 查询指定列

  通过在 SELECT 子句的目标列表达式中指定要查询的属性列，查询指定列。

- 查询全部列

  通过在 SELECT 关键字后列出所有列名，或使用通配符 `*`，查询全部列。

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

### 1.2. 选择若干元组

#### 1.2.1. 使用 DISTINCT 去重

两个原本并不相同的元组在投影到指定的某些列上后，可能会变成相同的行，可以通过 DISTINCT 指示数据库只返回不同的值。

例：
```sql
SELECT DISTINCT var_name FROM tb_name
```
NOTE
- **`DISTINCT` 关键字只能作用于所有的列，无法仅对个别列起作用**。因此，若 `SELECT DISTINCT ven_id, prod_price`，因为指定的两列不完全相同，会导致无法检索出所有的行。
- **`DISTINCT` 关键字必须用于列名，不可用于计算或表达式**，例如，不可用于`COUNT(*)`。

#### 1.2.2. 使用 WHERE 子句过滤元组

查询满足指定的条件的元组可以通过 WHERE 子句实现。

**若将数据过滤的任务移交到应用层，会极大影响应用的性能，并导致创建的应用完全不具备可伸缩性，另外会导致服务器不得不发送多余数据，浪费网络带宽。**

### 1.3. LIMIT 子句

对于限制输出行数的操作，各个 DBMS 规定各不相同，如：
- SQL Server 使用 TOP。
- DB2 使用 FECTH。
- Oracle 使用 ROWNUM。
- **MySQL、MariaDB、PostgreSQL、SQLite 使用 LIMIT**。

以 MySQL 的 LIMIT 为例，用法有以下几种：
```sql
[ LIMIT 行数 ]
[ LIMIT 起始行，行数 ]
[ LIMIT 行数 OFFSET 起始行 ]
```
NOTE
- **LIMIT 关键字从第 0 行开始计数，而不是第 1 行**。
- **LIMIT 后边不能接表达式，不能接用户自定义变量 (@xxx)，但可以接本地 / 局部变量 (xxx)**。

### 1.4. ORDER BY 子句

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

- **DESC 关键字只能作用到位于其前面的列名，若想对多个列都进行降序排序，必须对每一列都指定 DESC 关键字**。

- 对于空值，排序时显示的次序由具体的数据库系统实现来决定。因此，**若要保证数据库的可移植性，不应在属性值中存在 NULL 值**。

### 1.5. 聚集函数

为方便用户使用，增强检索功能，SQL 提供了许多聚集函数：
- `COUNT(*)`: 统计元组个数。
- `COUNT( [DISTINCT|ALL] 列名 )`: 统计一列中值的个数。
- `SUM( [DISTINCT|ALL] 列名 )`: 计算一列值的总和（此列必须是数值型）。
- `AVG( [DISTINCT|ALL] 列名 )`: 计算一列值的平均值（此列必须是数值型）。
- `MAX( [DISTINCT|ALL] 列名 )`: 求一列值中的最大值。
- `MIN( [DISTINCT|ALL] 列名 )`: 求一列值中的最小值。

NOTE
- 若指定 `DISTINCT`，则表示计算时要取消指定列中的重复值。若不指定 `DISTINCT` 或指定 `ALL`（默认值），则表示不取消重复值。

- **当聚集函数遇到空值时，除 `COUNT(*)` 外，都跳过空值而只处理非空值**。

- **聚集函数只能用于 SELECT 子句和 GROUP BY 中的 HAVING 子句中，不可用于 WHERE 子句中**。

### 1.6. GROUP BY 子句

通过 `GROUP BY` 子句可以将查询结果按某一列或多列的值进行分组，值相等为一组。

NOTE
- 对结果进行分组的目的是为了细化聚集函数的作用对象。如果未对查询结果分组，聚集函数将作用于整个查询结果。**分组后聚集函数将作用于每一个组，即每一组都有一个函数值**。

- **如果在 SELECT 语句中使用表达式，则必须在 GROUP BY 子句中指定相同的表达式，不可使用别名**。

- 如果分组中有包含 NULL 值的行，则将 NULL 作为一个分组返回。

- 分组后 SELECT 操作的对象是每个组中的各个组员，最终显示出来的是每组占一行，该行记录是每个组被 SELECT 的对象。

例：
```sql
SELECT
	t.Request_at AS `Day`,
	ROUND (
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

#### 1.6.1. 使用 HAVING 子句过滤分组

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
  - 语法：HAVING 子句与 WHERE 子句在语法上基本相同，**唯一差异在于 HAVING 子句中可以使用聚集函数作为条件表达式，但 WHERE 子句中不可使用聚集函数**。
  - 效果：**WHERE 在数据分组前进行过滤**，从基本表或视图中选择满足条件的元组。**HAVING 在分组后进行过滤**，从分组中选择满足条件的组。

- **一般使用 GROUP BY 子句时，应该也给出 ORDER BY 子句**。

#### 1.6.2. 使用 AS 取别名

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
- **如果在 SELECT 语句中使用表达式，则必须在 GROUP BY 子句中指定相同的表达式，不可使用别名**。

## 2. 连接查询

**若一个查询同时涉及两个以上的表，则称为连接查询。连接查询是关系数据库中最主要的查询**，包括等值连接查询、自然连接查询、非等值连接查询、自身连接查询、外连接查询和复合条件连接查询等。

**ANSI SQL 中提供了 JOIN 语句用于连接查询操作**，其中包括了五种 JOIN 方式：内连接 (INNER), 全外连接 (FULL OUTER), 左外连接 (LEFT OUTER), 右外连接 (RIGHT OUTER) 和交叉连接 (CROSS)。

**连接查询中，运行时连接的表越多，性能越差**。

### 2.1. 内连接查询

内连接是一种最常用的连接类型。内连接查询实际上是一种任意条件的查询。使用内连接时，如果两个表的相关字段满足连接条件，就从这两个表中提取数据并组合成新的记录，也就是**在内连接查询中，只有满足条件的元组才能出现在结果关系中**。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/2/11/ffb88f60dd64ae7523a2f6795d4b4141.jpg)

内连接查询可分为：等值连接查询、非等值连接查询、自然连接查询和自身连接查询。

#### 2.1.1. 等值 / 非等值连接查询

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

#### 2.1.2. 自然连接查询

**自然连接是特殊的等值连接**。若在等值连接中将目标列中重复的属性列去除，则称为自然连接查询。

例：
```sql
SELECT s.*, t.teacher_name FROM student_table s NATURAL JOIN teacher_table t; 
```

#### 2.1.3. 自身连接查询

如果在一个连接查询中，涉及到的两个表都是同一个表，这种查询就称为自连接查询。同一张表在 FROM 字句中多次出现，**为了区别该表的每一次出现，需要为表定义一个别名**。

自连接是一种特殊的内连接，它相互连接的表在物理上为同一张表，但可以在逻辑上分为两张表。

例：

ANSI SQL 风格
```sql
SELECT c1.cust_id, c1.cust_name
FROM Customers AS c1 INNER JOIN Customers AS c2
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
**USING 语句现已被 MySQL，Oracle，PostgreSQL，SQLite，和 DB2/400 等 DBMS 支持**。

自身连接查询经典题目：
- [leetcode human-traffic-of-stadium](https://leetcode.com/problems/human-traffic-of-stadium)
- [leetcode consecutive-numbers](https://leetcode.com/problems/consecutive-numbers/description/)

### 2.2. 外连接查询

内连接的查询结果都是满足连接条件的元组，若希望将悬浮元组也保留在结果关系中，就需要使用外连接查询。外连接是只限制一张表中的数据必须满足连接条件，而另一张表中的数据可以不满足连接条件的连接方式。**也就是说，在外连接查询中，包含了不是多个表共有的记录，不是只有满足条件的元组才能出现在结果关系中。**

外连接根据全集端不同，又分为左外连接（简称左连接），右外连接（简称右连接）和全连接。

NOTE: **在 MySQL 中，关键字 RIGHT OUTER JOIN 等同于 RIGHT JOIN，LEFT OUTER JOIN 等同于 LEFT JOIN**。

#### 2.2.1. 左外连接查询

左连接查询会获取左表所有记录，即使右表没有对应匹配的记录。

`tb1 LEFT OUTER JOIN tb2` 表示左连接查询，tb1 为左表， tb2 为右表。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/2/11/b3891cbb63ce64ba7d47a37189369064.jpg)

例：
```sql
SELECT s.*, t.teacher_name 
FROM student_table s LEFT OUTER JOIN teacher_table t 
ON s.teacher > t.teacher_id; 
```

#### 2.2.2. 右外连接查询

与 LEFT JOIN 相反，右连接查询会获取右表所有记录，即使左表没有对应匹配的记录。

`tb1 RIGHT OUTER JOIN tb2` 表示右连接查询，tb1 为左表，tb2 为右表。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/2/11/2efc8e908de35c1c894c8ad41bc2920d.jpg)

例：
```sql
SELECT s.*, t.teacher_name
FROM student_table s RIGHT OUTER JOIN teacher_table t 
ON s.teacher > t.teacher_id; 
```

#### 2.2.3. 全外连接查询

全外连接会把两个表中所有不满足连接条件的记录取出。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/2/11/6ee885f7025010b2b6065d724e2a7c11.jpg)

例：
```sql
SELECT s.*, t.teacher_name 
FROM student_table s FULL OUTER JOIN teacher_table t 
ON s.teacher > t.teacher_id; 
```

NOTE: 一些数据库管理系统中（如 MySQL) 并不直接支持全外连接，但**可通过先左联结，再右联结，最后将两者使用 UNION 运算得到全外联结查询的结果**。
```sql
SELECT *
	FROM employee LEFT JOIN department 
    ON employee.DepartmentID = department.DepartmentID
UNION
SELECT *
	FROM employee RIGHT JOIN department
    ON employee.DepartmentID = department.DepartmentID
	WHERE employee.DepartmentID IS NULL
```

### 2.3. 交叉连接查询

若连接查询时不指定 WHERE 条件或连接条件为"永真"，会返回两个表中所有记录的**笛卡儿积**，行数是表 1 行数与表 2 行数的乘积，此时也称为交叉连接查询。

**交叉连接也称为笛卡尔连接**。

例：

Theta 风格
```sql
SELECT vend_name, prod_name, prod_price
FROM Vendors, Products
```

ANSI 风格（在 MySQL 中定义了 `CROSS JOIN` 关键字，无需任何查询条件，效果与传统 Theta 风格相同）
```sql
SELECT vend_name, prod_name, prod_price
FROM Vendors CROSS JOIN Products
```

NOTE: **交叉连接不会应用任何谓词去过滤结果表中的记录，程序员可以用 WHERE 语句进一步过滤结果集**。

### 2.4. 连接查询的实现算法

执行一个连接操作，存在三种基本的算法：

- 嵌套循环连接算法 (LOOP JOIN)

  类似于 C 语言编程时的双重循环。作为外层循环逐行扫描的表，称为外部输入表；针对外部输入表的每一行，要逐行扫描检查匹配的另一张表，称为内部输入表（相当于内层循环）。**适用于外部输入表的行数较少，内部输入表建立了索引的情形**。

- 合并连接算法 (MERGE JOIN)

  类似于两个有序数组的合并。两个输入表都在合并列上排序；然后依序对两张表逐行做连接或舍弃。**如果预先建好了索引，合并连接的计算复杂度是线性的**。

- 哈希连接算法 (HASH JOIN)

  哈希连接选择行数较小的输入表作为生成输入，对其连接列值应用哈希函数，把其行（的存储位置）放入哈希桶中。**适用于查询的中间结果，通常是无索引的临时表；以及中间结果的行数很大时**。

### 2.5. 连接查询的综合实例

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

## 3. 嵌套查询

在 SQL 中，一个 `SELECT-FROM-WHERE` 语句称为一个查询块，**将一个查询块嵌套在另一个查询块的 WHERE 子句或 HAVING 短语的条件中的查询称为嵌套查询查询**。

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
但需要注意的是，**数据库管理系统处理连接查询的性能远远优于处理嵌套查询**。

## 4. 集合查询

SELECT 语句的查询结果是元组的集合，因此多个 SELECT 语句的结果可进行集合操作。集合操作主要包括并操作 UNION，交操作 INTERSECT 和差操作 EXCEPT。

**参加集合操作的各查询结果的列数必须相同，对应项的数据类型也必须相同。**

### 4.1. 并操作 UNION

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
- **UNION 中的每个查询必须包含相同的列、表达式或聚集函数，但次序可不同**。
- 列的数据类型必须兼容，即可被 DBMS 隐式转换。
- **UNION 查询结果会自动去除重复的元组，若想返回所有匹配的行，应该使用 UNION ALL 操作符**。
- **UNION 查询中只能使用一条 ORDER BY 子句，必须位于最后一条 SELECT 语句之后，可对所有结果进行排序**。

### 4.2. 交操作 INTERSECT

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

### 4.3. 差操作 EXCEPT

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

## 5. Refer Links