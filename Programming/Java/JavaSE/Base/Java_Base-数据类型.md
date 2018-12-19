- [Java 基础：数据类型](#java)
  - [1. 基本数据类型](#1)
    - [1.1. 整型](#11)
    - [1.2. 浮点型](#12)
    - [1.3. char](#13-char)
    - [1.4. boolean](#14-boolean)
  - [2. 引用类型](#2)
  - [3. 内存存储结构](#3)
  - [4. 大数值类型](#4)
  - [5. 类型转换](#5)
    - [5.1. 基本数据类型变量 & 直接量的类型转换](#51)
      - [5.1.1. 自动类型转换](#511)
      - [5.1.2. 强制类型转换](#512)
    - [5.2. 引用类型变量的类型转换](#52)
    - [5.3. 表达式中的类型转换——类型自动提升](#53)
      - [5.3.1. 类型提升规则](#531)
      - [5.3.2. 实例](#532)

# Java 基础：数据类型

Java 是一种强类型的语言，这意味着必须为每一个变量都声明一种类型。

## 1. 基本数据类型

在 Java 中，一共有 8 种基本数据类型，包裹 4 种整型、2 种浮点型、1 种用于标识 Unicode 编码的字符单元的字符类型和 1 种用于表示真值的 boolean 类型。

### 1.1. 整型

整型用于表示没有小数部分的数值，允许为负数。Java 提供了 4 种整型：
- int

  占 4 字节，-2147483648(-2^31) ~ 2147483647(2^31 - 1)（正好超过 30 亿），默认值是 0。

  没有使用后缀的整型数值都为 int 型。

- short

  占 2 字节，-32768(-2^15) ~ 32767(2^15 - 1)，默认值是 0。

  Short 数据类型也可以像 byte 那样节省空间。一个 short 变量是 int 型变量所占空间的二分之一。

- byte

  占 1 字节，-128(-2^7) ~ 127(2^7 - 1)，默认值是 0。

  byte 类型用在大型数组中节约空间，主要代替整数，因为 byte 变量占用的空间只有 int 类型的四分之一。

- long

  占 8 字节，-9,223,372,036,854,775,808(-2^63) ~ 9,223,372,036,854,775,807(2^63 -1)，默认值是 0L。

  长整型数值有一个后缀"L"，"L"理论上不分大小写，但是若写成"l"容易与数字"1"混淆，不容易分辩，所以最好大写。

  long 类型主要使用在需要比较大整数的系统上。

Java 中整型数值默认为 int 类型。

在 Java 中，整型的范围与运行 Java 代码的机器无关，这解决了代码的移植性问题。

使用非十进制表示数时，需要使用不同的前缀：
- 二进制：0b 或 0B
- 八进制：0
- 十六进制：0x 或 0X

从 Java7 开始，支持使用下划线作为数字字面量的分隔符，以增强可读性。例如：10000000 可写作 10_000_000，让人更容易读。

在 Java 中，没有任何无符号形式的 int、long、short、byte 类型。

### 1.2. 浮点型

浮点型用于表示有小数部分的数值。Java 提供了 2 种浮点型：
- float

  单精度浮点型，占 4 字节，有效位数为 6~7 位，默认值是 0.0f。

  float 型数值有一个后缀 F 或 f，**没有后缀的浮点数默认为 double 类型**。

- double

  双精度浮点型，占 8 字节，有效位数为 15 为，默认值是 0.0d。

  double 型数值可使用后缀 D 或 d，也可不使用。

Java 中的浮点数值默认为 double 类型。

实际上绝大部分应用程序都使用 double 类型，很少情况适合使用 float 类型（如：在储存大型浮点数组时使用 float 以节省空间）。

Java 中的浮点数都遵循 IEEE 754 标准。

可以使用十六进制表示浮点数值。例如，0.125 也就是 2^-3 可以表示为 0x1.0p-3（十六进制的浮点数表示法中，p 表示基数为 2 的指数，且尾数采用十六进制，但指数采用十进制表示）。

Java 中提供了三个特殊的浮点数值：
- 正无穷大：用常量 Double.POSITIVE_INFINITY 表示。一个正整数除以 0 的结果为正无穷大。

- 负无穷大：用常量 Double.NEGATIVE_INFINITY 表示。一个负整数除以 0 的结果为正无穷大。

- 不合法的数：用常量 Double.NaN 表示。0/0 或负数的平方根结果为 NaN。

  NOTE: 不可通过 == 检测一个特定值是否为 NaN，因为 NaN 不等于任何数，包括它本身。但可使用 Double.isNaN(x) 方法来判断。

P.S. 浮点数的误差问题：
- 对于浮点数的等值判断，由于 IEEE 754 标准的浮点数表示，当二进制不能精确表示的数进行运算时就可能会出现浮点误差，导致使用`==`可能会出现不符合常理的情况。

  因此：
  - 对于一般情况，应使用两个浮点数之差是否小于某个极小数（一般小于 0.000000001）来判断两个数是否相等。我们并不能保证比较一定得出正确的结果，但只要在 99.99999....% 的情况下能够正确就够了。
  - 对于需要精确的进行浮点数运算（比较）的情况，应该用 BigDecimal 类来解决，它帮你解决了所有必要的精度问题，当然，代价就是性能。
  <!-- todo: -->

  https://www.zhihu.com/question/21175703

  https://bbs.csdn.net/topics/330141975

  https://www.zhihu.com/question/29064056

### 1.3. char

- char 类型占 2 字节，用于表示 UTF-16 编码中的一个代码单元。

  **一个 char 类型的值不一定能表示单个 Unicode 字符**。由于 16 位的长度无法表示所有 Unicode 字符（大于 65536 个），因此 UTF-16 编码采用不同长度的编码表示所有的 Unicode 码点：在基本的多语言级别中，每个字符用 16 位表示，称为代码单元；而辅助字符采用连续 2 个代码单元进行编码。通过这样的设计，才能使用 16 位的长度表示所有 Unicode 字符。但这就导致了**有些 Unicode 字符可以用一个 char 值描述，另外一些 Unicode 字符需要 2 个 char 值才能表示**。

  core Java 10th:
  > 我们强烈简易不要在程序中使用 char 类型，除非确实需要处理 UTF-16 代码单元。最好将字符串作为抽象数据类型处理。

- char 类型的字面量要使用单引号括起来。

  'A'与"A"表示不同的值。'A'是编码值位 65 所对应的字符常量，而"A"是包含一个字符 A 的字符串。

- char 类型的值也可以表示为十六进制值，其范围为、u0000~\uffff。

- 在 Java 中，Unicode 转义序列会在解析代码之前得到处理，而其它转义序列不会。因此，转义序列、u 可以出现在加引号的常量之外，而其它转义序列不可以。

  例如：

  `public static void main(String \u005B \u005D args)`是完全合法的。

  `// \u00A0 is a newline`会导致语法错误，因为、u00A0 会被替换为一个换行符。

  `// look inside c:\users`会导致语法错误，因为、u 后没有跟着 4 个十六进制数。

- 在表示用户密码时，使用 `char []` 代替 `String` 是更安全的做法。

  https://stackoverflow.com/questions/8881291/why-is-char-preferred-over-string-for-passwords?rq=1

  **Strings are immutable. That means once you've created the String, if another process can dump memory, there's no way (aside from reflection) you can get rid of the data before garbage collection kicks in.**

  **With an array, you can explicitly wipe the data after you're done with it. You can overwrite the array with anything you like, and the password won't be present anywhere in the system, even before garbage collection.**

  So yes, this is a security concern - **but even using char[] only reduces the window of opportunity for an attacker**, and it's only for this specific type of attack.

  It's possible that arrays being moved by the garbage collector will leave stray copies of the data in memory. I believe this is implementation-specific - the garbage collector may clear all memory as it goes, to avoid this sort of thing. Even if it does, there's still the time during which the char[] contains the actual characters as an attack window.

### 1.4. boolean

boolean 类型占 1 字节，有两个取值：false 和 true（默认值为 false），用于进行逻辑判断。

整型值与布尔值之间不能进行相互转换，因此，0 不等价于 false，1 不等价于 true。

## 2. 引用类型

在 Java 中，引用类型的变量非常类似于 C/C++ 的指针。引用类型指向一个对象，指向对象的变量是引用变量。这些变量在声明时被指定为一个特定的类型，变量一旦声明后，类型就不能被改变了。

对象、数组都是引用数据类型。

所有引用类型的默认值都是 null。

一个引用变量可以用来引用任何与之兼容的类型。

## 3. 内存存储结构

对于基本类型和引用类型，在内存中的存储结构如下：

- 定义
  ```java
  int num = 10;
  String str = "hello";
  ```
  ![image](http://img.cdn.firejq.com/jpg/2018/5/11/43dd8c214a38cf62387886cbdb778972.jpg)

  - 基本类型：值就直接保存在变量中。
  - 引用类型：变量中保存的只是实际对象的地址。一般称这种变量为"引用"，引用指向实际对象，实际对象中保存着内容。

- 赋值
  ```java
  num = 20;
  str = "java";
  ```
  ![image](http://img.cdn.firejq.com/jpg/2018/5/11/3171f746c645585b49474d0ab78499eb.jpg)

  - 基本类型：赋值运算符会直接改变变量的值，原来的值被覆盖掉。
  - 引用类型：赋值运算符会改变引用中所保存的地址，原来的地址被覆盖掉。**原来的对象不会被改变**，但没有被任何引用所指向的对象将会被 GC 回收。

## 4. 大数值类型

若基本的数值类型不能满足需求，可使用 BigInteger 和 BigDecimal。

这两个类可以处理包含任意长度的数字序列的数值，实现了任意精度的整数运算和浮点运算。

## 5. 类型转换

### 5.1. 基本数据类型变量 & 直接量的类型转换

#### 5.1.1. 自动类型转换

下图给出数值类型之间的合法转换规则：

![image](http://img.cdn.firejq.com/jpg/2018/1/25/c22c9e291a897e9967753c3caaca04f4.jpg)

（**转换原则：往表数范围更大的方向转换**）

其中，实心箭头表示无信息丢失的转换；虚箭头表示可能会有精度损失的转换。

**当对两个不同类型的操作数进行运算时，会按照合法转换规则发生类型的自动转换，使得转换后的两个操作数类型相同后再进行运算**。例：若两个操作数中有一个 double，则另一个操作数无论是什么类型都会自动转换为 double。

- 变量与变量之间的转换
  ```java
  int a = 6;
  float b = a;//a 会自动转换为 float 型，再赋值给 b

  byte a = 9;
  char b =a;// 错误，因为 byte 不能自动转换为 char，要进行强制转换才可

  char a = ‘a’;
  int b = a;//b 值为 97，a 会自动转换为 int 型

  // 任何类型的变量与 String 做“+“运算时，都会转换为 String
  String str = “a” + 5 + 5;//a55
  String str = 5 + 5 + “a”;//10a（将表达式从左到右理解)
  String str = “a” + (5 + 5);//a10
  String str = ‘a’ + 5 + 5;//97+5+5=107, 因此特别注意双引号和单引号的区别
  ```

- 直接量与直接量之间的转换

  ```java
  5 == 5.0// 表达式值为 true，在进行比较运算之前会先转换为 double
  97 == ‘a’// 表达式值为 true，在进行比较运算之前会先转换为 int
  ```

- 变量与直接量之间的转换

  ```java
  char a = 97;//a 值为‘a’，会自动将 97 视为 char
  ```

#### 5.1.2. 强制类型转换

- 转换方式
  - `(type)variable；`

    NOTE：
    - 使用（type）操作的基本数据类型强制转换只能在数值（整型，浮点型，char 型）之间转换，不可在数值型和布尔型之间转换。
    - 强制转换又称“缩小转换”，若没有明确数据范围，很容易造成数据的溢出。

    适用于：无法发生自动类型转换的时候，如从表数范围大的类型转换为表数范围小的类型。

  - 使用函数等其他特殊方法。

    适用于：第一种方式无法满足需求时。

- 整型和浮点型的转换

  浮点转换为整型：直接舍弃浮点型的小数部分，不会进行四舍五入。
  ```java
  float a = 6.865;
  int b = (int)a;//b 值为 6
  ```
- 字符、字符串与数值类型的转换
  - 字符串转换为数值 int/double/float/long
    ```java
    String string="123";
      int x=Integer.parseInt(string);// 方法一
      int x = Integer.valueOf(string).intValue();// 方法二
    ```
  - 字符转换为数值 int/double/float/long
    ```java
      char c='5';
      int x1=c-'0';
    ```
  - 数值转换为字符串 / 字符
    ```java
    int v=123;
    String s1=String.valueOf(v);// 方法一（更高效）
    String s2=Integer.toString(v);// 方法二
    String s3=” + v;// 方法三（更方便，适用于任何类型转换为字符串）
    ```
  实例：
  ```java
  String str="abc123";
  StringBuffer buf=new StringBuffer();
  char[] ch=str.toCharArray();
  for(int i=0;i<ch.length;i++){
    if(ch[i]>'0' && ch[i]<'9'){
      buf.append(ch[i]);
    }
  }
  int b=Integer.valueOf(buf.toString());
  int b2=Integer.parseInt(buf.toString());
  System.out.println("提取的 int 值为 "+b+" "+b2);

  int index=buf.indexOf("2");
  System.out.println("字符串、"2\"在串中的位置"+index);
  int index1=str.indexOf("123");
  System.out.println("字符串、"123\"在串中的位置"+index1);
  ```
  输出：
  ```
   1:	字符串转数值 123
   2:	字符转数值 5
   3:	数值转字符串 / 字符 123 123
   4: 提取的 int 值为 123 123
     	字符串"2"在串中的位置 1
    	字符串"123"在串中的位置 3
  ```

- 字符与字符串的转换
  ```java
  // 字符转换为字符串：
  char ch = ‘b’;
  String res = ch + “”;

  // 字符串转化为字符：
  String str = “b”;
  Char ch = str.charAt(0);
  ```

P.S.`(int)` 强制类型转换 与 Integer.parseInt(String) 的区别：
```java
System.out.println((int)'A');// 输出 65, ’A’的 ASCⅡ码值，即：（int）强制转换可输出字符的 ASCⅡ码
System.out.println((char)65);// 输出 A，即：（char）强制转换可输出 ASCⅡ码为某个值的字符
// 总结：（int）强制类型转换可实现 ASCⅡ码与 char 的转换
```
```java
System.out.println(Integer.parseInt(“A”));// 报错
System.out.println(Integer.parseInt(“32”));// 输出 32
// 总结：parseInt 方法只能将表示数字的字符串转换为 int 值
// 其他包装类的 parseXxx(string) 方法也是类似，如 parseBoolean 方法。
```

### 5.2. 引用类型变量的类型转换

原则：转换的类型必须是相容的，即：有继承关系的。

方式：
- 将子类对象转换为其父类类型——自动完成

  又称为“向上转型”，由系统自动完成，（实际上没有发生任何类型转换，因为子类是一种特殊的父类）
  ```java
  base a = new sub();// 合法！Java 允许将子类对象直接赋给父类类型的引用变量，因为系统会自动完成向上转型
  sub a = new base();// 出错！不允许将父类对象直接赋给子类引用变量，因为无法将父类自动转换为子类类型
  sub a = (sub) new base();// 合法！将父类对象强制转换为子类类型后，才可赋值给子类引用变量
  ```
  NOTE：向上转型由系统自动完成，若 base a = (base) new sub();，依旧合法，但完全多此一举。

- 将父类对象转换为其子类类型——需要强制转换

  前提：该对象编译时为父类类型，运行时为子类类型，否则会在运行时引发异常 ClassCastException；

  应用：使得编译类型为父类，运行类型为子类的对象能调用子类的特有方法；
  ```java
  base a = new sub();
  (sub)a.subMethod();// 将父类类型的引用变量强制转换为子类类型（即：运行时的类型）后，才可调用子类中定义的父类没有的方法
  ```
  强制转换前一般要使用 instanceof 运算判断是否可成功转换，可使程序更健壮。

### 5.3. 表达式中的类型转换——类型自动提升

在表达式中，对中间值的精确要求有时超过任何一个操作数的范围。例如，考虑下面的表达式：
```java
byte a = 40;
byte b = 50;
byte c = 100;
int d = a * b / c;
```
中间项结果 a*b 很容易超过它的任何一个 byte 型操作数的范围。

**为处理这种问题，避免表达式的中间项的值溢出，当分析处理表达式时，Java 对表达式有类型的自动提升机制**。
针对上述问题，在处理该表达式 a * b / c 时，Java 会自动提升各个 byte 型的操作数到 int 型。这意味着子表达式`a*b`使用整数而不是字节型来执行。这样，尽管变量 a 和 b 都被指定为 byte 型，50*40 中间表达式的结果 2000 是合法的。

#### 5.3.1. 类型提升规则

Java 定义了若干适用于表达式的类型提升规则（type promotion rules）：
- byte、short 和 char 类型的值都被提升为 int 类型；
- 如果有一个操作数是 long 类型，就将整个表达式提升为 long 类型；
- 如果有一个操作数是 float 类型，就将整个表达式提升为 float 类型；
- 如果任何一个操作数为 double 类型，结果将为 double 类型。
- 任何与 String 做 + 运算的，都会成为 String。

e.g.
```java
byte b=1;
char c='a';
short s=1024;
int i=50;
float f=2.0f;
double d=.123;
double result = (f * b) + (i / c) - (d * s);
//  f*b 中，b 被自动提升为 float 类型，该表达式结果是 float 类型；
//  i/c 中，c 被自动提升为 int 类型，该表达式结果是 int 类型；
//  d*s 中，s 被自动提升为 doubl 类型，该表达式结果是 double 类型；
//  最后，float+int-double，结果会被提升为 double 类型，double 类型为最后结果 result 的类型。
```

#### 5.3.2. 实例

自动类型提升有好处，但它也会引起令人疑惑的编译错误。以下是几个具体的例子：

- eg1
  ```java
  short a = 5;
  a = a + 1;
  ```
  报错：`“incompatible types: possible lossy conversion from int to short”.`

  解析：对于第一步 short a = 5，5 为 int 型，但系统为结合赋值操作，将足够小的值即 5（在 short 的范围内）当作 short 处理，因此这一步没有错误。
  而对于表达式 a+1, a 是 short，1 是 int，执行“+“操作时系统发生表达式的类型自动提升，整个表达式变为 int 型。将 int 型的 a+1 赋值给 short 型的 a，肯定会发生错误（表达式中含有变量，因此系统不知道该表达式的值是否足够小，无法确定是否在 short 范围内，因而即便足够小，也不会将其视为 short 处理）。

  修改方法：
  ```java
  short a = 5;
  a = (short) (a +1);
  ```

- eg2
  ```java
  short a = 5;
  a += 1;
  ```
  不会出错。因为表达式中使用了 += 复合赋值运算符，复合赋值运算符包含了一个隐式的类型转换，即：复合赋值运算符会自动将他计算的结果值强制类型转换为其左边变量的类型。

  `a = a + 1`和`a += 1`实际上并不等价。`a += 1`实际上等于`a = (a 的类型）(s1+1);`。

- eg3

  ```java
  short a = 5 + 1;
  ```
  不会出错。不含变量的表达式，不会发生表达式的自动提升，直接计算出结果 6 后执行赋值操作，6 为 int 型，但系统为结合赋值操作，将足够小的值即 6（在 short 的范围内）当作 short 处理，因此没有错误。

- eg4
  ```java
  short a = 5, b = 1;
  short c = a + b;
  ```
  报错：“incompatible types: possible lossy conversion from int to short”。对于含变量的表达式 a+b， 系统自动提升为 int 型，因此出错。

- eg5

  ```java
  class Promote {
    public static void main(String args[]) {
      byte b = 42;
      char c = 'a';
      short s = 1024;
      int i = 50000;
      float f = 5.67f;
      double d = .1234;
      double result = (f * b) + (i / c) - (d * s);// 发生变量提升
      System.out.println((f * b) + " + " + (i / c) + " - " + (d * s));
      System.out.println("result = " + result);
    }
  }
  ```
  在第一个子表达式`f*b` 中，变量 b 被提升为 float 类型，该子表达式的结果当然是 float 类型。接下来，在子表达式 i/c，中，变量 c 被提升为 int 类型，该子表达式的结果当然是 int 类型。然后，子表达式 d*s 中的变量 s 被提升为 double 类型，该子表达式的结果当然也是 double 类型。最后，考虑三个中间值，float 类型，int 类型，和 double 类型。float 类型加 int 类型的结果是 float 类型。然后 float 类型减去提升为 double 类型的 double 类型，该表达式的最后结果是 double 类型。
