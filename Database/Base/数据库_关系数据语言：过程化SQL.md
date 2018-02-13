- [数据库：关系数据语言 - 过程化 SQL](#%E6%95%B0%E6%8D%AE%E5%BA%93%EF%BC%9A%E5%85%B3%E7%B3%BB%E6%95%B0%E6%8D%AE%E8%AF%AD%E8%A8%80---%E8%BF%87%E7%A8%8B%E5%8C%96-sql)
  - [1. 嵌入式 SQL](#1-%E5%B5%8C%E5%85%A5%E5%BC%8F-sql)
  - [2. 过程化 SQL](#2-%E8%BF%87%E7%A8%8B%E5%8C%96-sql)
    - [2.1. 块的基本结构](#21-%E5%9D%97%E7%9A%84%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84)
    - [2.2. 注释](#22-%E6%B3%A8%E9%87%8A)
    - [2.3. 运算符](#23-%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [2.3.1. 比较运算符](#231-%E6%AF%94%E8%BE%83%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [2.3.2. 逻辑运算符](#232-%E9%80%BB%E8%BE%91%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [2.3.3. 赋值运算符](#233-%E8%B5%8B%E5%80%BC%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [2.3.4. 集合运算符](#234-%E9%9B%86%E5%90%88%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [2.4. 变量定义](#24-%E5%8F%98%E9%87%8F%E5%AE%9A%E4%B9%89)
      - [2.4.1. 本地 / 局部变量](#241-%E6%9C%AC%E5%9C%B0-%E5%B1%80%E9%83%A8%E5%8F%98%E9%87%8F)
      - [2.4.2. 用户自定义变量](#242-%E7%94%A8%E6%88%B7%E8%87%AA%E5%AE%9A%E4%B9%89%E5%8F%98%E9%87%8F)
      - [2.4.3. 系统变量](#243-%E7%B3%BB%E7%BB%9F%E5%8F%98%E9%87%8F)
        - [2.4.3.1. 全局变量](#2431-%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F)
        - [2.4.3.2. 会话变量](#2432-%E4%BC%9A%E8%AF%9D%E5%8F%98%E9%87%8F)
      - [2.4.4. 常量](#244-%E5%B8%B8%E9%87%8F)
    - [2.5. 流程控制](#25-%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6)
      - [2.5.1. 条件控制](#251-%E6%9D%A1%E4%BB%B6%E6%8E%A7%E5%88%B6)
        - [2.5.1.1. IF](#2511-if)
        - [2.5.1.2. IF-ELSE](#2512-if-else)
      - [2.5.2. 循环控制](#252-%E5%BE%AA%E7%8E%AF%E6%8E%A7%E5%88%B6)
        - [2.5.2.1. LOOP](#2521-loop)
        - [2.5.2.2. WHILE-LOOP](#2522-while-loop)
        - [2.5.2.3. FOR-LOOP](#2523-for-loop)
        - [2.5.2.4. iterate/leave](#2524-iterateleave)
      - [2.5.3. 错误处理](#253-%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86)
    - [2.6. 存储过程和函数](#26-%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B%E5%92%8C%E5%87%BD%E6%95%B0)
      - [2.6.1. 存储过程](#261-%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B)
        - [2.6.1.1. 创建存储过程](#2611-%E5%88%9B%E5%BB%BA%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B)
        - [2.6.1.2. 执行存储过程](#2612-%E6%89%A7%E8%A1%8C%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B)
        - [2.6.1.3. 修改存储过程](#2613-%E4%BF%AE%E6%94%B9%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B)
        - [2.6.1.4. 删除存储过程](#2614-%E5%88%A0%E9%99%A4%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B)
      - [2.6.2. 函数](#262-%E5%87%BD%E6%95%B0)
        - [2.6.2.1. 定义函数](#2621-%E5%AE%9A%E4%B9%89%E5%87%BD%E6%95%B0)
        - [2.6.2.2. 执行函数](#2622-%E6%89%A7%E8%A1%8C%E5%87%BD%E6%95%B0)
        - [2.6.2.3. 修改函数](#2623-%E4%BF%AE%E6%94%B9%E5%87%BD%E6%95%B0)
        - [2.6.2.4. 删除函数](#2624-%E5%88%A0%E9%99%A4%E5%87%BD%E6%95%B0)
        - [2.6.2.5. 查看函数](#2625-%E6%9F%A5%E7%9C%8B%E5%87%BD%E6%95%B0)
      - [2.6.3. ANSI SQL 缺省函数](#263-ansi-sql-%E7%BC%BA%E7%9C%81%E5%87%BD%E6%95%B0)
        - [2.6.3.1. 流程控制函数](#2631-%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6%E5%87%BD%E6%95%B0)
        - [2.6.3.2. 字符串操作函数](#2632-%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%93%8D%E4%BD%9C%E5%87%BD%E6%95%B0)
        - [2.6.3.3. 数学操作函数](#2633-%E6%95%B0%E5%AD%A6%E6%93%8D%E4%BD%9C%E5%87%BD%E6%95%B0)
        - [2.6.3.4. 聚集函数](#2634-%E8%81%9A%E9%9B%86%E5%87%BD%E6%95%B0)
  - [3. Refer Links](#3-refer-links)

# 数据库：关系数据语言 - 过程化 SQL

标准的 SQL 是高度非过程化的查询语言，具有操作统一、面向集合、功能丰富、使用简单的多项优点，但和程序设计语言相比，高度非过程化的优点也造成了它的缺点：缺少流程控制能力，难以实现应用业务中的逻辑控制。

因此，为克服 SQL 语言实现复杂应用方面的不足，可以在应用系统中使用 **SQL 编程技术**实现过程化 SQL，来访问和管理数据库，其主要方式包括：嵌入式 SQL 和过程化 SQL。

## 1. 嵌入式 SQL

SQL 主要的使用方式是交互式和嵌入式两种环境，两种方式的 SQL 语法结构基本相同。嵌入式 SQL 将 SQL 语句嵌入高级程序设计语言，借助高级语言的控制功能实现过程化。在程序设计环境下，SQL 语句要做某些必要的补充。

对于嵌入式 SQL，数据库管理系统一般采用预编译方法处理，即由数据库管理系统的预处理程序对源程序进行扫描，识别出嵌入式 SQL 语句，然后将它们转换为主语言调用语句，以让主语言编译程序能够识别它们，再由主语言的编译程序将纯的主语言程序编译成目标码。

在嵌入式 SQL 中，为快速区分 SQL 语句和主语言语句，所有 SQL 语句需要加上前缀。例如，Java 中的嵌入式 SQL 格式为：`# SQL { SQL 语句 }`。

## 2. 过程化 SQL

过程化 SQL 是 SQL 99 标准中对 SQL 的扩展，使其增加了过程化语句的功能支持，如函数和过程的概念。

过程化 SQL 的基本结构是块。所有的过程化 SQL 程序都是由块组成的。块之间可以相互嵌套，每个块完成一个逻辑操作。

### 2.1. 块的基本结构

- 定义部分
  - DECLARE: 定义的变量、常量等只能在该基本块中使用。
  - 变量、常量、游标、异常等：当基本块执行结束时，定义就不再存在。

- 执行部分
  - BEGIN: SQL 语句、过程化 SQL 的流程控制语句

  - EXCEPTION: 异常处理部分

  - END;

### 2.2. 注释

SQL 中可使用两种风格的注释：
- 双横杠：`--`，该风格一般用于单行注释。
- C 风格：`/*……*/`，该风格一般用于多行注释。

### 2.3. 运算符

#### 2.3.1. 比较运算符

- `>`: 大于，比较运算符不仅比较数值的大小，也可以比较字符串、日期之间的大小。

- `>=`: 大于等于。

- `<`: 小于。

- `<=`: 小于等于。	

- `!<`: 不小于。	

- `!>`: 不大于。	

- `=`: 相等，判断是否相等的比较运算符是“=”，而不是“==”。

- `<>`:  不相等	

- `between and`: 要求值在两个指定值之间（包括边界值）。

- `in`:	要求值为后边括号内的任意一个，使用 IN 操作符可代替大部分的 OR 连接，且 IN 的执行速度要优于 OR 语句，此外，还可用使用 IN 连接子查询，从而动态建立 WHERE 子句。

- `like`:	字符串匹配，主要用于模糊查询。可使用通配符：“_”（匹配一个任意字符）和“%”（匹配任意多个字符）。 		

  - 在大多数 DBMS 中，会使用空格填补字段内容，这导致使用 LIKE 模糊匹配时会出现一些问题，如：`WHERE var LIKE ‘F%y’` 无法匹配 varchar(50) 的字段值 ‘Fish bean bag toy’，因为 DBMS 会在 toy 后边补充 33 各空格以补全 50 个字符的长度，而空格就导致了匹配失败。
    - 解决方式一：改成 LIKE ‘F%y%’。
    - 解决方式二：使用函数 RTRIM 去掉空格再进行匹配，更推荐此方法。
  - ‘%’不会匹配值为 NULL 的字段。
  - 通配符搜索会比精确搜索耗费更长的处理时间，因此应该尽量避免使用模糊搜索。
  - 即便一定要使用通配符匹配，也不要将通配符置于匹配模式的开始处，这样的匹配是最慢的。

- `is null`:	要求值为 null，确定值是否未 NULL，不可使用 =NULL，应使用 IS NULL。

例：
```sql
SELECT * FROM user  WHERE id between 2 and 6;
SELECT * FROM user  WHERE id in(2, 3, 4, 5, 6);
SELECT * FROM user  WHERE id between 2 and 4;
SELECT * FROM user WHERE name LIKE ‘__’;-- 匹配名字为两个字符的记录
SELECT * FROM user WHERE name LIKE ‘\_’ escape ‘\’;-- 指定“\”为转义字符，匹配名字为下划线“_”的记录（标准 SQL 不支持“\”转义，需要显示指定）；
```
#### 2.3.2. 逻辑运算符

| and  | 逻辑与  |
| ---- | ---- |
| or   | 逻辑或  |
| not  | 逻辑非  |

优先级：NOT > AND > OR。

NOTE
- 任何时候使用具有 AND 和 OR 操作符的 WHERE 子句，应该使用圆括号明确分组操作符，以消除歧义且提高可读性；
- NOT 关键字可用在要过滤的列前，也可用在其后，大多数 DBMS 运行使用 NOT 否定任何条件；

例：
```sql
SELECT * FROM user  WHERE not id between 2 and 6;
SELECT * FROM user  WHERE id > 2 and id < 6;
```

#### 2.3.3. 赋值运算符

虽然“=”在 SET 和 UPDATE 语句中可用于赋值，但 SQL 中的专门的赋值运算符是冒号等号“:=”。

P.S. [MySQL 中 = 和 := 的区别](http://blog.csdn.net/wangjun5159/article/details/51378558)

- `=`

  在 SET 和 UPDATE 语句中，= 表示赋值；在其它语句中，= 表示相等；

- `:=`

  在所有语句中，:= 都表示赋值；

  P.S. 赋值符号 := 的优先级非常低，所以需要注意，赋值表达式应该使用明确的括号。

  例：`@num:=@num+1`,:= 是赋值的作用，所以，先执行 @num+1, 然后再赋值给 @num，所以能正确实现行号的作用。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/2/13/b54455810665ad20e1349f553fa4d5bd.jpg)

  @num=@num+1, 此时 = 是等于的作用，@num 不等于 @num+1，所以始终返回 0，如果改为 @num=@num, 始终返回 1 了。MySQL 数据库中，用 1 表示真，0 表示假。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/2/13/6009a6d57873e08a18f86fcd8dd703bd.jpg)

#### 2.3.4. 集合运算符

使用 SELECT 查询的结果可以视为一个集合，多个查询结果可进行集合的交 (intersect)、并 (union)、差 (minus) 运算，但只有并 (union) 运算是标准 SQL 中支持的。
```sql
SELECT 语句 1 UNION SELECT 语句 2
```

例：
```sql
SELECT * FROM table WHERE age < 10 OR age > 80;
-- 等价于
SELECT * FROM table WHERE age < 10 UNION SELECT * FROM table WHERE age > 80;// 效率更优
```

### 2.4. 变量定义

#### 2.4.1. 本地 / 局部变量

本地变量 / 局部变量 (Local Variables)，一般用于 SQL 的语句块中，比如存储过程中的 begin 和 end 语句块。其作用域仅限于该语句块内，生命周期也仅限于该存储过程的调用期间，即每次调用完毕后都会被置 NULL。

本地变量 / 局部变量在使用前必须先进行声明 (DECLARE)：
```sql
DECLARE local_var_name data_type [DEFAULT default_value]
```

本地变量 / 局部变量不要求必须初始化（缺省值为 NULL），赋值使用 SET/SELECT 语句完成：
```sql
SET variable_assignment [, variable_assignment] ...

variable_assignment:
      user_var_name = expr
    | param_name = expr
    | local_var_name = expr
    | [GLOBAL | SESSION]
        system_var_name = expr
    | [@@global. | @@session. | @@]
        system_var_name = expr
```

例：
```sql
DELIMITER //

CREATE PROCEDURE sp_test(var1 INT) 
BEGIN   
    DECLARE start  INT unsigned DEFAULT 1;  
    DECLARE finish INT unsigned DEFAULT 10;

    SELECT  var1, start, finish;

    SELECT * FROM places WHERE place BETWEEN start AND finish; 
END; //

DELIMITER ;

CALL sp_test(5);
```

#### 2.4.2. 用户自定义变量

用户自定义变量 (User-defined variables)，以"@"为前缀，形式为"@变量名"，其中变量名称由字母、数字、“.”、“_”和“$”组成。当然，在以字符串或者标识符引用时也可以包含其他字符（例如：@’my-var’，@”my-var”，或者 @`my-var`）

用户自定义变量是会话级别的变量。其变量的作用域仅限于声明其的客户端链接。在连接 MySQL 的整个过程中都存在，但当这个客户端断开时，其所有的会话变量将会被释放。

用户自定义变量的类型是一个动态类型，包括了：整形、浮点型、二进制与非二进制串和 NULL。在赋值浮点数时，系统不会保留精度。其他类型的值将会被转成相应的上述类型。比如：一个包含时间或者空间数据类型（temporal or spatial data type）的值将会转换成一个二进制串。

如果用户自定义变量的值以结果集形式返回，系统会将其转换成字符串形式。

User-defined variables 不要求在使用前先声明（因此永远不会出现 `DECLARE @xxx;` 的语句），也不要求必须初始化（缺省值为 NULL），可以在任何可以使用表达式的地方直接使用自定义变量。

User-defined variables 的赋值有两种方式：
- SET: `SET @start = 1, @finish = 10;`
- SELECT: 
  ```sql
  SELECT @start := 1, @finish := 10;
  SELECT * FROM places WHERE place BETWEEN @start AND @finish;
  ```

限制：
- 使用自定义变量的查询，无法使用查询缓存。
- 不能在显式使用常量的表达式中使用自定义变量，例如表名、列名和 SELECT 中的 LIMIT 子句、LOAD DATA 中的 IGNORE N LINES 的子句中。
- 用户自定义变量的生命周期是在一个连接中有效，所以不能用它们来做连接间的通信。
- 如果使用连接池或者持久化连接，自定义变量可能让看起来毫无关系的代码发生交互。
- MySQL 优化器在某些场景下可能会将这些变量优化掉，这可能导致代码不按预想的方式运行。
- 赋值的顺序和赋值的时间点并不总是固定的，这依赖于优化器的决定。
- 使用未定义变量不会产生任何语法错误，如果没有意识到这一点，非常容易犯错。
- 在 SELECT 语句中，在每一个 select 表达式被发送给客户端后，才会对表达式进行计算。这就意味着，在形如 HAVING，GROUP BY 和 ORDER BY 只句中有使用在当前 select 表达式定义的变量的情况下，该语句将不会得到如期的效果。

  例：
  ```sql
  mysql> SELECT (@aa:=id) AS a, (@aa+3) AS b FROM tbl_name HAVING b=5;
  ```
  上述在 HAVING 只句中使用了在当前的 select 列表中定义的别名 b，其使用了变量 @aa。这条语句并不会得到如期的效果：@aa 变量为上一次 SQL 语句执行的结果集中的 ID 值，并非当前的。

#### 2.4.3. 系统变量

MySQL 内置了许多有缺省值的 Server System Variables，与 MySQL 服务器绑定，对所有客户端的所有连接生效。

Server System Variables 的类型可分为 GLOBAL，SESSION or BOTH。

查看当前 MySQL 服务器的 Server System Variables 使用 SHOW VARIABLES 语句或 SELECT @@var_name 语句。

例：
```sql
SHOW VARIABLES LIKE '%wait_timeout%';

SELECT @@sort_buffer_size;
```

##### 2.4.3.1. 全局变量

定义时，以如下两种形式出现，set GLOBAL 变量名  或者  set @@global. 变量名。

全局变量影响服务器整体操作。当服务器启动时，它将所有全局变量初始化为默认值。这些默认值可以在选项文件中或在命令行中指定的选项进行更改。要想更改全局变量，必须具有 SUPER 权限。全局变量作用于 server 的整个生命周期，但是不能跨重启，即重启后所有设置的全局变量均失效。要想让全局变量重启后继续生效，需要更改相应的配置文件。
		
全局变量的赋值：
```sql
SET GLOBAL var_name = value;
SET @@global.var_name = value; 
```

全局变量的查询：
```sql
SELECT @@global.var_name;
SHOW GLOBAL VARIABLES LIKE "%var%";
```

##### 2.4.3.2. 会话变量

会话变量即为服务器为每个客户端连接维护的变量。在客户端连接时，使用相应全局变量的当前值对客户端的回话变量进行初始化。设置会话变量不需要特殊权限，但客户端只能更改自己的会话变量。其作用域与生命周期均限于当前客户端连接。

会话变量的赋值：
```sql
SET var_name = value;
SET SESSION var_name = value;
SET @@var_name = value;
SET @@session.var_name = value;
```

会话变量的查询：
```sql
SELECT @@var_name;
SELECT @@session.var_name;
SHOW SESSION VARIABLES LIKE "%var%";
```

NOTE
> LOCAL and @@local. are synonyms for SESSION and @@session. 
即 LOCAL 关键字与 SESSION 关键字同义。

#### 2.4.4. 常量

```sql
常量名 数据类型 CONSTANT := 常量表达式
```

### 2.5. 流程控制

过程化 SQL 提供了流程控制语句，主要有条件控制语句和循环控制语句。

#### 2.5.1. 条件控制

##### 2.5.1.1. IF

```sql
IF condition THEN
  ...
END IF;
```

##### 2.5.1.2. IF-ELSE

```sql
IF condition THEN
  ...
ELSE
  ...
END IF;
```

例：
```sql
delimiter $$
create function test_if(age int) returns varchar(100)
begin
    -- 判断传入值是否大于 20
    if age < 20 then
        return '劝君惜取少年时';
    else
        return '风华正茂';
    end if;
end
$$
```

#### 2.5.2. 循环控制

##### 2.5.2.1. LOOP

```sql
LOOP
  ...
END LOOP;
```

##### 2.5.2.2. WHILE-LOOP

```sql
WHILE condition LOOP
  ...
END LOOP;
```

例：MySQL 中
```sql
delimiter $$
-- 从 1 开始计算到指定数字的和
create function test_while(number int) returns int
begin
    -- 定义局部变量返回值，默认为 0
    declare res int default 0;
    -- 定义个局部变量，默认值为 1
    declare i int default 1;

    -- 循环操作
    sum_number:while i <= number do
            set res = res + i;
            set i =i + 1;
            end while;
    -- 返回值
    return res;
end
$$
```

##### 2.5.2.3. FOR-LOOP

```sql
FOR count IN [REVERSE] bound1 .. bound2 LOOP
  ...
END LOOP;
```

##### 2.5.2.4. iterate/leave

在 mysql 中也有类似 continue 和 break 的语句，iterate 对应 continue，leave 对应 break, 执行语法：
```sql
iterate/leave 循环名字；
```

#### 2.5.3. 错误处理

如果过程化 SQL 在执行时出现异常，则应该让程序在产生异常的语句处停下来，根据异常的类型去执行异常处理语句。

### 2.6. 存储过程和函数

过程化 SQL 块主要有两种类型，即命名块和匿名块。匿名块每次执行都要进行编译，它不能被存储到数据库中，也不能在其它过程化 SQL 块中调用。存储过程和函数是命名块，它们被编译后保存在数据库中，称为持久性存储模块，可以被反复调用，运行速度较快。

#### 2.6.1. 存储过程

存储过程是由过程化 SQL 语句书写的过程，这个过程经编译和优化后存储在数据库服务器中，使用时只要调用即可。

优点：
- 由于存储过程不像解释执行的 SQL 语句那样在提出操作请求时才进行语法分析和优化工作，因而运行效率高。
- 存储过程降低了客户机和服务器之间的通信量。
- 方便实施企业规则。

##### 2.6.1.1. 创建存储过程

```sql
CREATE OR REPLACE PROCEDURE 过程名 ([ 参数 1，参数 2，... ])
AS 过程化 SQL 块；
```
参数列表用名字来标识调用时给出的参数值，必须指定值的数据类型。可以定义输入参数、输出参数或输入 / 输出参数，默认为输入参数，也可以无参数。

例：
```sql
CREATE OR REPLACE PROCEDURE TEST(test1 INT, test2 INT, test3 FLOAT)
AS DECLARE
  test3 FLOAT;
  test4 FLOAT;
  test5 FLOAT;
BEGIN
  < 过程化 SQL 块 >
END;  
```

##### 2.6.1.2. 执行存储过程

```sql
CALL/PERFORM PROCEDURE 过程名 ([ 参数 1，参数 2，... ])
```
数据库服务器支持在过程体中调用其它存储过程。

##### 2.6.1.3. 修改存储过程

- 重命名
  ```sql
  ALTER PROCEDURE 原过程名 RENAME TO 新过程名；
  ```

- 重新编译
  ```sql
  ALTER PROCEDURE 过程名 COMPILE;
  ```

##### 2.6.1.4. 删除存储过程

```sql
DROP PROCEDURE 过程名 ();
```

#### 2.6.2. 函数

函数也称为自定义函数。函数和存储过程类似，都是持久性存储模块，不同之处在于函数必须指定返回的类型。

##### 2.6.2.1. 定义函数

```sql
create or replace function 函数名（参数列表) returns 数据类型
    begin
        // 函数体 
        // 返回值
    end
```

例：
```sql
delimiter $$ 
create function avg(first int) returns int
    begin 
        declare value ;
        set value = first;
        return value;
    end
$$
delimiter ;
```

##### 2.6.2.2. 执行函数

```sql
CALL/SELECT 函数名 ([ 参数 1，参数 2，... ])
```

##### 2.6.2.3. 修改函数

```sql
ALTER FUNCTION 原函数名 RENAME TO 新函数名；
```

```sql
ALTER FUNCTION 函数名 COMPILE;
```

##### 2.6.2.4. 删除函数

```sql
DROP FUNCTION 函数名；
```

##### 2.6.2.5. 查看函数

查看所有自定义函数：
```sql
show function status \G
```

#### 2.6.3. ANSI SQL 缺省函数

| 函数       | 描述               |
| -------- | ---------------- |
| AVG      | 平均值              |
| COUNT    | 计数（不含 Null）       |
| First    | 第一个记录的值          |
| MAX      | 最大值              |
| MIN      | 最小值              |
| STDEV    |                  |
| STDEVP   |                  |
| SUM      | 求和               |
| VAR      |                  |
| VARP     |                  |
| UCASE    | 转化为全大写字母         |
| LCASE    | 转化为全小写字母         |
| MID      | 取中值              |
| LEN      | 计算字符串长度          |
| INSTR    | 获得子字符串在母字符串的起始位置 |
| LEFT     | 取字符串左边子串         |
| RIGHT    | 取字符串右边子串         |
| ROUND    | 数值四舍五入取整         |
| MOD      | 取余               |
| NOW      | 获得当前时间的值         |
| FORMAT   | 字符串格式化           |
| DATEDIFF | 获得两个时间的差值        |

##### 2.6.3.1. 流程控制函数

- `CASE value WHEN [compare_value] THEN result [WHEN [compare_value] THEN result ...] [ELSE result] END`

  `CASE WHEN [condition] THEN result [WHEN [condition] THEN result ...] [ELSE result] END`
  
  The first CASE syntax returns the result for the first value=compare_value comparison that is true. The second syntax returns the result for the first condition that is true. If no comparison or condition is true, the result after ELSE is returned, or NULL if there is no ELSE part.

  例：
  ```sql
  mysql> SELECT CASE 1 WHEN 1 THEN 'one'
      ->     WHEN 2 THEN 'two' ELSE 'more' END;
          -> 'one'
  mysql> SELECT CASE WHEN 1>0 THEN 'true' ELSE 'false' END;
          -> 'true'
  mysql> SELECT CASE BINARY 'B'
      ->     WHEN 'a' THEN 1 WHEN 'b' THEN 2 END;
          -> NULL
  ```

- `IF(expr1,expr2,expr3)`

  If expr1 is TRUE (expr1 <> 0 and expr1 <> NULL), IF() returns expr2. Otherwise, it returns expr3.
  
  例：
  ```sql
  mysql> SELECT IF(1>2,2,3);
          -> 3
  mysql> SELECT IF(1<2,'yes','no');
          -> 'yes'
  mysql> SELECT IF(STRCMP('test','test1'),'no','yes');
          -> 'no'
  ```

- `IFNULL(expr1,expr2)`

  If expr1 is not NULL, IFNULL() returns expr1; otherwise it returns expr2.

  例：
  ```sql
  mysql> SELECT IFNULL(1,0);
          -> 1
  mysql> SELECT IFNULL(NULL,10);
          -> 10
  mysql> SELECT IFNULL(1/0,10);
          -> 10
  mysql> SELECT IFNULL(1/0,'yes');
          -> 'yes'
  ```

- `NULLIF(expr1,expr2)`

  Returns NULL if expr1 = expr2 is true, otherwise returns expr1. This is the same as CASE WHEN expr1 = expr2 THEN NULL ELSE expr1 END.

  例：
  ```sql
  mysql> SELECT NULLIF(1,1);
          -> NULL
  mysql> SELECT NULLIF(1,2);
          -> 1
  ```

##### 2.6.3.2. 字符串操作函数

- CONCAT(str1, str2, …)：拼接各个字符串；
- RTRIM(str)：去除 str 右边的空格；
- LTRIM(str)：去除 str 左边的空格；
- TRIM(str)：区别 str 两边的空格；

##### 2.6.3.3. 数学操作函数

在主要的 DBMS 中，数学操作函数是唯一所有 DBMS 统一支持且一致的函数。

| 函数   | 说明    |
| ---- | ----- |
| ABS  | 求绝对值  |
| COS  | 求余弦   |
| SIN  | 求正弦   |
| TAN  | 求正切   |
| PI   | 返回圆周率 |
| EXP  | 求指数值  |
| SQRT | 求平方根  |

##### 2.6.3.4. 聚集函数

汇总函数的参数为数据表某一列的列名，函数将对该列的所有行进行统计计算，从而得到汇总数据。

| 函数    | 说明       | 备注                                       |
| ----- | -------- | ---------------------------------------- |
| AVG   | 求列的平均值   | AVG 函数会忽略列值为 NULL 的行；                       |
| COUNT | 求列的行数    | 若参数指定列名，则 COUNT 函数会忽略列值为 NULL 的行；  若参数为*，则不会忽略 NULL 行； |
| MAX   | 求列的最大值   | MAX 函数会忽略列值为 NULL 的行  对于文本数据，MAX 函数返回排序后的最后一行； |
| MIN   | 求列的最小值   | MIN 函数会忽略列值为 NULL 的行；  对于文本数据，MIN 函数返回排序后的第一行； |
| SUM   | 求列所有行值的和 | SUM 函数会忽略列值为 NULL 的行；                       |

## 3. Refer Links

https://stackoverflow.com/questions/11754781/how-to-declare-a-variable-in-mysql 