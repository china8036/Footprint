- [Java 基础：类型转换](#java-%E5%9F%BA%E7%A1%80%EF%BC%9A%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
  - [基本数据类型变量 & 直接量的类型转换](#%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E5%8F%98%E9%87%8F-%E7%9B%B4%E6%8E%A5%E9%87%8F%E7%9A%84%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
    - [自动类型转换](#%E8%87%AA%E5%8A%A8%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
    - [强制类型转换](#%E5%BC%BA%E5%88%B6%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
  - [引用类型变量的类型转换](#%E5%BC%95%E7%94%A8%E7%B1%BB%E5%9E%8B%E5%8F%98%E9%87%8F%E7%9A%84%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
  - [表达式中的类型转换——类型自动提升](#%E8%A1%A8%E8%BE%BE%E5%BC%8F%E4%B8%AD%E7%9A%84%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2%E2%80%94%E2%80%94%E7%B1%BB%E5%9E%8B%E8%87%AA%E5%8A%A8%E6%8F%90%E5%8D%87)
    - [类型提升规则](#%E7%B1%BB%E5%9E%8B%E6%8F%90%E5%8D%87%E8%A7%84%E5%88%99)
    - [实例](#%E5%AE%9E%E4%BE%8B)

# Java 基础：类型转换

## 基本数据类型变量 & 直接量的类型转换

### 自动类型转换

下图给出数值类型之间的合法转换规则：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/25/c22c9e291a897e9967753c3caaca04f4.jpg)

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

### 强制类型转换

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

## 引用类型变量的类型转换

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

## 表达式中的类型转换——类型自动提升

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

### 类型提升规则

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

### 实例

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
