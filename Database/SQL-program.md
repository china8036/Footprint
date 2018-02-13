- [SQL 编程](#sql-%E7%BC%96%E7%A8%8B)
  - [1. SQL 基础](#1-sql-%E5%9F%BA%E7%A1%80)
    - [1.1. 数据库语言种类](#11-%E6%95%B0%E6%8D%AE%E5%BA%93%E8%AF%AD%E8%A8%80%E7%A7%8D%E7%B1%BB)
    - [1.2. 关系数据库标准语言：SQL](#12-%E5%85%B3%E7%B3%BB%E6%95%B0%E6%8D%AE%E5%BA%93%E6%A0%87%E5%87%86%E8%AF%AD%E8%A8%80%EF%BC%9Asql)
    - [1.3. 规范](#13-%E8%A7%84%E8%8C%83)
    - [1.4. 引号](#14-%E5%BC%95%E5%8F%B7)
    - [1.5. 运算符](#15-%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [1.5.1. 比较运算符](#151-%E6%AF%94%E8%BE%83%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [1.5.2. 逻辑运算符](#152-%E9%80%BB%E8%BE%91%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [1.5.3. 赋值运算符](#153-%E8%B5%8B%E5%80%BC%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [1.5.4. 集合运算符](#154-%E9%9B%86%E5%90%88%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [1.6. 数据类型](#16-%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
    - [1.7. 注释](#17-%E6%B3%A8%E9%87%8A)
    - [1.8. 变量](#18-%E5%8F%98%E9%87%8F)
      - [1.8.1. 本地 / 局部变量](#181-%E6%9C%AC%E5%9C%B0-%E5%B1%80%E9%83%A8%E5%8F%98%E9%87%8F)
      - [1.8.2. 用户自定义变量](#182-%E7%94%A8%E6%88%B7%E8%87%AA%E5%AE%9A%E4%B9%89%E5%8F%98%E9%87%8F)
      - [1.8.3. 系统变量](#183-%E7%B3%BB%E7%BB%9F%E5%8F%98%E9%87%8F)
        - [1.8.3.1. 全局变量](#1831-%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F)
        - [1.8.3.2. 会话变量](#1832-%E4%BC%9A%E8%AF%9D%E5%8F%98%E9%87%8F)
    - [1.9. 函数](#19-%E5%87%BD%E6%95%B0)

# SQL 编程

## 1. SQL 基础

### 1.1. 数据库语言种类

数据库语言分为：
- 关系代数语言：用对关系的运算来表达查询要求，代表：ISBL
- 关系演算语言：用谓词来表达查询要求 
  - 元组关系演算语言
    - 谓词变元的基本对象是元组变量
    - 代表：APLHA,QUEL
  - 域关系演算语言
    - 谓词变元的基本对象是域变量 
    - 代表：QBE 
- 具有关系代数和关系演算双重特点的语言，代表：SQL（Structured Query Language）。

### 1.2. 关系数据库标准语言：SQL

结构化查询语言 (SQL, Structured Query Language)，是关系数据库的标准语言，也是一个通用的、功能极强的关系数据库语言。其功能不仅仅是查询，而是包括数据库模式创建、数据库数据的插入与修改、数据库安全性完整性定义与控制等一系列功能。

标准 SQL 由 ANSI 标准委员会管理，因此称为 ANSI SQL，所有 DBMS 都在 ANSI SQL 的基础上进行扩展，如 T-SQL 等；

### 1.3. 规范

- SQL 不区分大小写，但一般将 SQL 关键词全部大写，提高可读性；

- 多条 SQL 语句必须用分号 ; 分隔；

- 数据库名、表名、字段名用反引号“``”包含；

- 数据值用单引号包含；

- 尽量不使用通配符 “*”，影响性能，应直接写出所有所需字段；

- 处理 SQL 语句时，所有空格都将被忽略，因此一般将一行 SQL 写成多行，提高可读性；

### 1.4. 引号

单引号：用于文本值和别名（为字段取别名一定要用单引号括起来，为表名取别名不用加单引号）；

双引号：效果于单引号相同，但标准 SQL 不支持双引号；

反引号：用于标识符，以区分与 SQL 保留字相同的标识符；

例：
```sql
SELECT * FROM `order` ‘a’, `desc` ‘b’ WHERE a.name = ‘Mike’ AND b.name = ‘Jack’;
```

### 1.5. 运算符

#### 1.5.1. 比较运算符

| 运算符         | 名称                                       | 备注                                       |
| ----------- | ---------------------------------------- | ---------------------------------------- |
| >           | 大于                                       | 比较运算符不仅比较数值的大小，也可以比较字符串、日期之间的大小；         |
| >=          | 大于等于                                     |                                          |
| <           | 小于                                       |                                          |
| <=          | 小于等于                                     |                                          |
| !<          | 不小于                                      |                                          |
| !>          | 不大于                                      |                                          |
| =           | 相等                                       | 判断是否相等的比较运算符是“=”，而不是“==”；                |
| <>          | 不相等                                      |                                          |
| between and | 要求值在两个指定值之间（**包括边界值**）                   |                                          |
| in          | 要求值为后边括号内的任意一个                           | 使用 IN 操作符可代替大部分的 OR 连接，且 IN 的执行速度要优于 OR 语句，此外，还可用使用 IN 连接子查询，从而动态建立 WHERE 子句； |
| like        | 字符串匹配，主要用于模糊查询；  可使用通配符：“_”（匹配一个任意字符）和“%”（匹配任意多个字符） | 1)    在大多数 DBMS 中，会使用空格填补字段内容，这导致使用 LIKE 模糊匹配时会出现一些问题，如：  WHERE  var LIKE ‘F%y’ 无法匹配 varchar(50) 的字段值 ‘Fish bean bag toy’，因为 DBMS 会在 toy 后边补充 33 各空格以补全 50 个字符的长度，而空格就导致了匹配失败。  解决方式一：改成 LIKE  ‘F%y%’；  解决方式二：使用函数 RTRIM 去掉空格再进行匹配，更推荐此方法；  2)    ‘%’不会匹配值为 NULL 的字段；  3)    通配符搜索会比精确搜索耗费更长的处理时间，因此应该尽量避免使用模糊搜索；  4)    即便一定要使用通配符匹配，也不要将通配符置于匹配模式的开始处，这样的匹配是最慢的； |
| is null     | 要求值为 null                                 | 确定值是否未 NULL，不可使用 =NULL，应使用 IS  NULL；        |

例：
```sql
SELECT * FROM user  WHERE id between 2 and 6;
SELECT * FROM user  WHERE id in(2, 3, 4, 5, 6);
SELECT * FROM user  WHERE id between 2 and 4;
SELECT * FROM user WHERE name LIKE ‘__’;  -- 匹配名字为两个字符的记录
SELECT * FROM user WHERE name LIKE ‘\_’ escape ‘\’;  -- 指定“\”为转义字符，匹配名字为下划线“_”的记录（标准 SQL 不支持“\”转义，需要显示指定）；
```

#### 1.5.2. 逻辑运算符

| 运算符 | 说明 |
| ---- | ---- |
| and  | 逻辑与  |
| or   | 逻辑或  |
| not  | 逻辑非  |

优先级：NOT > AND > OR；

注：
- 任何时候使用具有 AND 和 OR 操作符的 WHERE 子句，应该使用圆括号明确分组操作符，以消除歧义且提高可读性；
- NOT 关键字可用在要过滤的列前，也可用在其后，大多数 DBMS 运行使用 NOT 否定任何条件；

例：
```sql
SELECT * FROM user  WHERE not id between 2 and 6;
SELECT * FROM user  WHERE id > 2 and id < 6;
```

#### 1.5.3. 赋值运算符

虽然“=”在 SET 和 UPDATE 语句中可用于赋值，但 SQL 中的专门的赋值运算符是冒号等号“:=”；

- [MySQL 中 = 和 := 的区别](http://blog.csdn.net/wangjun5159/article/details/51378558 )

  - =
    
    在 SET 和 UPDATE 语句中，= 表示赋值；在其它语句中，= 表示相等；
  - :=
    
    在所有语句中，:= 都表示赋值；

- 赋值符号 := 的优先级非常低，所以需要注意，赋值表达式应该使用明确的括号。
  
  例：
  
  @num:=@num+1,:= 是赋值的作用，所以，先执行 @num+1, 然后再赋值给 @num，所以能正确实现行号的作用。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/12/24/d8e62f8d5b3d77c7f2ab3c8cd639fc1c.jpg)

  @num=@num+1, 此时 = 是等于的作用，@num 不等于 @num+1，所以始终返回 0，如果改为 @num=@num, 始终返回 1 了。MySQL 数据库中，用 1 表示真，0 表示假。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/12/24/0af38fcf76cd6cdd3338c100bfdc12ba.jpg)

#### 1.5.4. 集合运算符

使用 SELECT 查询的结果可以视为一个集合，多个查询结果可进行集合的交 (intersect)、并 (union)、差 (minus) 运算，但只有并 (union) 运算是标准 SQL 中支持的：
```sql
SELECT 语句 1 UNION SELECT 语句 2
```
例：
```sql
SELECT * FROM table WHERE age < 10 OR age > 80;
-- 等价于
SELECT * FROM table WHERE age < 10 UNION SELECT * FROM table WHERE age > 80; -- 效率更优
```

### 1.6. 数据类型

TODO:

- 实际开发中常用整型列类型存储 UNIX 时间戳，代替日期和时间列类型；

- 自动类型转换

### 1.7. 注释

SQL 中可使用两种风格的注释

双横杠：“--“，该风格一般用于单行注释；

c 风格：“/*……*/”，该风格一般用于多行注释；

### 1.8. 变量

https://stackoverflow.com/questions/11754781/how-to-declare-a-variable-in-mysql 

#### 1.8.1. 本地 / 局部变量

本地变量 / 局部变量 (Local Variables)，一般用于 SQL 的语句块中，比如存储过程中的 begin 和 end 语句块。其作用域仅限于该语句块内，生命周期也仅限于该存储过程的调用期间，即每次调用完毕后都会被置 NULL。

- 使用：
  - 本地变量 / 局部变量在使用前必须先进行声明 (DECLARE)：
    
    DECLARE 语法：
    ```sql
    DECLARE local_var_name data_type [DEFAULT default_value]
    ```
    注意：

    用户自定义变量不需要声明，因此永远不会出现 `DECLARE @xxx;` 的语句；

  - 本地变量 / 局部变量不要求必须初始化（缺省值为 NULL），赋值使用 SET/SELECT 语句完成：

    SET 语法：
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

#### 1.8.2. 用户自定义变量

用户自定义变量 (User-defined variables)，以"@"为前缀，形式为"@变量名"，其中变量名称由字母、数字、“.”、“_”和“$”组成。当然，在以字符串或者标识符引用时也可以包含其他字符（例如：@’my-var’，@”my-var”，或者 @`my-var`）
用户自定义变量是会话级别的变量。其变量的作用域仅限于声明其的客户端链接。在连接 MySQL 的整个过程中都存在，但当这个客户端断开时，其所有的会话变量将会被释放。

用户自定义变量的类型是一个动态类型，包括了：整形、浮点型、二进制与非二进制串和 NULL。在赋值浮点数时，系统不会保留精度。其他类型的值将会被转成相应的上述类型。比如：一个包含时间或者空间数据类型（temporal or spatial data type）的值将会转换成一个二进制串。

如果用户自定义变量的值以结果集形式返回，系统会将其转换成字符串形式。

- 使用：

  User-defined variables 不要求在使用前先声明，也不要求必须初始化（缺省值为 NULL），可以在任何可以使用表达式的地方直接使用自定义变量；

  User-defined variables 的赋值有两种方式：SET 和 SELECT：
  ```sql
  SET @start = 1, @finish = 10;
  ```
  ```sql
  SELECT @start := 1, @finish := 10;
  SELECT * FROM places WHERE place BETWEEN @start AND @finish;
  ```

- 限制：
  
  - 使用自定义变量的查询，无法使用查询缓存
  
  - 不能在显式使用常量的表达式中使用自定义变量，例如表名、列名和 SELECT 中的**LIMIT 子句、LOAD DATA 中的 IGNORE N LINES 的子句**中。
  
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

#### 1.8.3. 系统变量

MySQL 内置了许多有缺省值的 Server System Variables，与 MySQL 服务器绑定，对所有客户端的所有连接生效；
Server System Variables 的类型可分为 GLOBAL，SESSION or BOTH；

查看当前 MySQL 服务器的 Server System Variables 使用 SHOW VARIABLES 语句或 SELECT @@var_name 语句：

例：
```sql
SHOW VARIABLES LIKE '%wait_timeout%';

SELECT @@sort_buffer_size;
```

##### 1.8.3.1. 全局变量

定义时，以如下两种形式出现，set GLOBAL 变量名  或者  set @@global. 变量名；

全局变量影响服务器整体操作。当服务器启动时，它将所有全局变量初始化为默认值。这些默认值可以在选项文件中或在命令行中指定的选项进行更改。要想更改全局变量，必须具有 SUPER 权限。全局变量作用于 server 的整个生命周期，但是不能跨重启，即重启后所有设置的全局变量均失效。要想让全局变量重启后继续生效，需要更改相应的配置文件。
		
- 全局变量的赋值：
  ```sql
  SET GLOBAL var_name = value;
  SET @@global.var_name = value; 
  ```

- 全局变量的查询：
  ```sql
  SELECT @@global.var_name;
  SHOW GLOBAL VARIABLES LIKE "%var%";
  ```

##### 1.8.3.2. 会话变量

会话变量即为服务器为每个客户端连接维护的变量。在客户端连接时，使用相应全局变量的当前值对客户端的回话变量进行初始化。设置会话变量不需要特殊权限，但客户端只能更改自己的会话变量。其作用域与生命周期均限于当前客户端连接。

- 会话变量的赋值：
  ```sql
  SET var_name = value;
  SET SESSION var_name = value;
  SET @@var_name = value;
  SET @@session.var_name = value;
  ```

- 会话变量的查询：
  ```sql
  SELECT @@var_name;
  SELECT @@session.var_name;
  SHOW SESSION VARIABLES LIKE "%var%";
  ```

注意：
> LOCAL and @@local. are synonyms for SESSION and @@session

即 LOCAL 关键字与 SESSION 关键字同义；

### 1.9. 函数

TODO: