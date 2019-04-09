- [Matlab Script：基本语法](#matlab-script基本语法)
  - [1. 标识符](#1-标识符)
  - [2. 数据类型](#2-数据类型)
  - [3. 常量 / 预定义变量](#3-常量--预定义变量)
  - [4. 变量](#4-变量)
    - [4.1. 向量](#41-向量)
    - [4.2. 矩阵](#42-矩阵)
  - [5. 运算符](#5-运算符)
    - [5.1. 算术运算符](#51-算术运算符)
    - [5.2. 关系运算符](#52-关系运算符)
    - [5.3. 逻辑运算符](#53-逻辑运算符)
    - [5.4. 集合运算符](#54-集合运算符)
  - [6. 输入 / 输出](#6-输入--输出)
  - [7. 流程控制](#7-流程控制)
    - [7.1. 决策控制](#71-决策控制)
    - [7.2. 循环控制](#72-循环控制)
  - [8. 其它](#8-其它)
  - [9. Refer Links](#9-refer-links)

# Matlab Script：基本语法

MATLAB Script 是一种解释型语言。

## 1. 标识符

MATLAB 标识符是由一个字母后由任意数量的字母、数字或下划线，区分大小写。

## 2. 数据类型

http://www.yiibai.com/matlab/matlab_data_types.html

## 3. 常量 / 预定义变量

- `ans`	Most recent answer.
- `eps`	Accuracy of floating-yiibai precision.
- `i,j`	The imaginary unit √-1.
- `Inf`	Infinity.
- `NaN`	Undefined numerical result (not a number).
- `pi` The number π.

## 4. 变量

### 4.1. 向量

- 向量声明
  - 行向量
    ```
    x = [1, 2, 3, 4] 或 x = [1 2 3 4]
    ```
  - 列向量
    ```
    y = [1; 5; 9]
    ```

- 向量加法
  ```
  a = [1 2 3];
  b = [1 2 3];
  c = a + b; % c = [2 4 6]
  ```
- 向量减法
  ```
  a = [1 2 3];
  b = [1 2 3];
  c = a - b; % c = [0 0 0]
  ```
- 向量标量乘法
  ```
  v = [ 12 34 10 8];
  m = 5 * v % m = [60 170 50 40]
  ```
- 向量转置

  NOTE: 向量转置的操作符为后单引号 `’`
  ```
  r = [ 1 2 3 4 ];
  tr = r’;
  ```
  或
  ```
  v = [1;2;3;4];
  tv = v’;
  ```
- 向量追加

  - e.g.1
    ```
    r1 = [ 1 2 3 4 ];
    r2 = [5 6 7 8 ];
    r = [r1,r2];
    rMat = [r1;r2];
    ```
    结果：
    ```
    disp(r);
        1     2     3     4     5     6     7     8
    disp(rMat);
        1     2     3     4
        5     6     7     8

    ```
  - e.g.2
    ```
    c1 = [ 1; 2; 3; 4 ];
    c2 = [5; 6; 7; 8 ];
    c = [c1; c2];
    cMat = [c1,c2];
    ```
    结果：
    ```
    disp(c);
        1
        2
        3
        4
        5
        6
        7
        8

    disp(cMat);
        1     5
        2     6
        3     7
        4     8
    ```

- 向量求模

  需要采取以下步骤来计算向量的模：
  1. 采取的矢量及自身的积，使用数组相乘（*）。这将产生一个向量 SV，其元素是向量的元素的平方和 V。
      ```
      sv = v.*v;
      ```
  1. 使用求和函数得到 v。这也被称为矢量的点积向量的元素的平方的总和 V。
      ```
      dp= sum(sv);
      ```
  1. 使用 sqrt 函数得到的总和的平方根，这也是该矢量的大小 V。
      ```
      mag = sqrt(s);
      ```

  e.g.
  ```
  v = [1 2 3 1 1];
  sv = v.* v;      %the vector with elements as square of v's elements
  dp = sum(sv);    % sum of squares -- the dot product
  mag = sqrt(dp);  % magnitude
  disp(mag);  		 % 4
  ```

- 向量点积
  ```
  v1 = [2 3 4];
  v2 = [1 2 3];
  dp = dot(v1, v2);
  disp(dp); % 20
  ```

### 4.2. 矩阵

3x4 矩阵：
```
z = [1, 2, 3, 4; 5, 6, 7, 8; 9, 10, 11, 12] 或 z= [1 2 3 4; 5 6 7 8; 9 10 11 12]
```

## 5. 运算符

### 5.1. 算术运算符

| 运算符  | 目的                                                         |
| ------- | ------------------------------------------------------------ |
| +       | Plus; addition operator.                                     |
| -       | Minus; subtraction operator.                                 |
| *       | Scalar and matrix multiplication operator.                   |
| **.\*** | **Array   multiplication operator.**                         |
| ^       | Scalar and matrix exponentiation   operator.                 |
| **.^**  | **Array   exponentiation operator.**                         |
| \       | Left-division operator.                                      |
| /       | Right-division operator.                                     |
| .       | Array left-division operator.                                |
| **./**  | **Array   right-division operator.**                         |
| :       | Colon; generates regularly spaced   elements and represents an entire row or column. |
| ( )     | Parentheses; encloses function arguments   and array indices; overrides precedence. |
| [ ]     | Brackets; enclosures array elements.                         |
| .       | Decimal yiibai.                                              |
| …       | Ellipsis; line-continuation operator                         |
| ,       | Comma; separates statements and elements   in a row          |
| ;       | Semicolon; separates columns and   suppresses display.       |
| %       | Percent sign; designates a comment and   specifies formatting. |
| _       | Quote sign and transpose operator.                           |
| ._      | Nonconjugated transpose operator.                            |
| =       | Assignment operator.                                         |

NOTE:
- 小括号代表运算级别，中括号用于生成矩阵，大括号用于构成单元数组。
- 分号；的作用：不显示运算结果，但对图形窗口不起作用。分号也用于区分行。
- 逗号，的作用：函数参数分隔符，也用于区分行，显示运算结果，当然不加标点也显示运算结果。
- 冒号：最有用的运算符在 MATLAB 之一。它可用来创建矢量，下标数组和指定的迭代。
  - 如果想创建一个行向量，包含从 1 到 10 的整数，如下：1:10
  - 如果想指定递减步数的一个增量值，例如：100: -5: 50
- 续行号。.. 不能放在等号后面使用，不能放在变量名中间使用，起作用时默认显蓝色。
- 双引号'string'是字符串的标识符。
- 感叹号！用于调用操作系统运算。
- 百分号 % 是注释号，百分号后面直到行末的语句 matlab 跳过执行。另外还有一个块注释，即对多行一次注释，会使用到“%{   %}”，格式为（注意 %{ 和 %}都要单独成行）。
- 乘号*总是不能省略的，除了表示复数，比如 2+3i 时可以省略。
- 除号 / 或、，它两个的关系是：a 除以 b 表示为 a/b，或 b\a。
- 等号 = 用于赋值。
- 双等号 == 表示数学意义上的等号。

### 5.2. 关系运算符

| 运算符 | 描述                     |
| ------ | ------------------------ |
| <      | Less than                |
| <=     | Less than or equal to    |
| >      | Greater than             |
| >=     | Greater than or equal to |
| ==     | Equal to                 |
| ~=     | Not equal to             |

### 5.3. 逻辑运算符

MATLAB 提供了两种类型的逻辑运算符和函数：
- Element-wise - 这些运算符的逻辑阵列上运行相应的元素。
- Short-circuit - 这些运算上的标量，逻辑表达式。

Element-wise 的逻辑运算符操作元素元素逻辑阵列。符号＆，|和〜逻辑数组运算符 AND，OR，NOT。

允许短路短路逻辑运算符，逻辑运算。符号 && 和 | | 是短路逻辑符 AND 和 OR。

### 5.4. 集合运算符

| 函数                  | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| intersect(A,B)        | Set intersection of two arrays; returns   the values common to both A and B. The values returned are in sorted order. |
| intersect(A,B,'rows') | Treats each row of A and each row of B as   single entities and returns the rows common to both A and B. The rows of the   returned matrix are in sorted order. |
| ismember(A,B)         | Returns an array the same size as A,   containing 1 (true) where the elements of A are found in B. Elsewhere, it   returns 0 (false). |
| ismember(A,B,'rows')  | Treats each row of A and each row of B as   single entities and returns a vector containing 1 (true) where the rows of   matrix A are also rows of B. Elsewhere, it returns 0 (false). |
| issorted(A)           | Returns logical 1 (true) if the elements   of A are in sorted order and logical 0 (false) otherwise. Input A can be a   vector or an N-by-1 or 1-by-N cell array of strings. A is considered to be   sorted if A and the output of sort(A) are equal. |
| issorted(A, 'rows')   | Returns logical 1 (true) if the rows of   two-dimensional matrix A are in sorted order, and logical 0 (false)   otherwise. Matrix A is considered to be sorted if A and the output of   sortrows(A) are equal. |
| setdiff(A,B)          | Set difference of two arrays; returns the   values in A that are not in B. The values in the returned array are in sorted   order. |
| setdiff(A,B,'rows')   | Treats each row of A and each row of B as   single entities and returns the rows from A that are not in B. The rows of   the returned matrix are in sorted order.    The 'rows' option does not support cell   arrays. |
| setxor                | Set exclusive OR of two arrays                               |
| union                 | Set union of two arrays                                      |
| unique                | Unique values in array                                       |

e.g.
```
a = [7 23 14 15 9 12 8 24 35]
b = [ 2 5 7 8 14 16 25 35 27]
u = union(a, b) % 并集
i = intersect(a, b) % 交集
s = setdiff(a, b) % 差集
```

## 6. 输入 / 输出

- `disp`: 显示一个数组或字符串的内容。

- `fscanf`: 阅读从文件格式的数据。

- `format`: 控制屏幕显示的格式。

- `fprintf`: 执行格式化写入到屏幕或文件。

- `input`: 显示提示并等待输入。

- `;`: 禁止显示网版印刷。

- `format + 格式`: 设置输出格式。
  - `format long`: 设置长（6 位小数）格式输出。
  - `format short`: 设置短（3 位小数）格式输出。
  - `format bank`: 设置 2 位小数输出。
  - `format short e`: 以指数的形式显示小数点后四位，加上指数。
  - `format long e`: 以指数的形式显示小数点后十六位，加上指数。
  - `format rat`: 给出最接近的有理表达式，从计算所得。

## 7. 流程控制

### 7.1. 决策控制

- `if ... end `

  An if ... end statement consists of a boolean expression followed by one or more statements.

- `if...else...end`

  An if statement can be followed by an optional else statement, which executes when the boolean expression is false.

- `if... elseif...elseif...else...end `

  An if statement can be followed by an (or more) optional elseif...and an else statement, which is very useful to test various condition.

- `switch`

  A switch statement allows a variable to be tested for equality against a list of values.
  ```
  switch <switch_expression>
    case <case_expression>
      <statements>
    case <case_expression>
      <statements>
    otherwise
        <statements>
  end
  ```

### 7.2. 循环控制

- while 循环

  一个给定的条件为真时重复语句或语句组。测试条件才执行循环体。
  ```
  while <expression>
    <statements>
  end
  ```

- for 循环

  执行的语句序列多次缩写管理循环变量的代码。
  ①
  ```
  for index = initval:endval
    <program statements>
  end
  ```
  ②
  ```
  for index = initval:step:endval
    <program statements>
  end
  ```
  ③
  ```
  for index = valArray
    <program statements>
  end
  ```

## 8. 其它

- m 文件名的命名尽量不要是简单的英文单词，最好是由大小写英文 / 数字 / 下划线等组成。原因是简单的单词命名容易与 matlab 内部函数名同名，结果会出现一些莫名其妙的错误。例如，写个 m 文件，命名为 spy，运行时就弹出一个奇怪的 figure。

## 9. Refer Links

[易百教程：Matlab 教程](https://www.yiibai.com/matlab/)