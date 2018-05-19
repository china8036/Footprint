- [Java 基础：字符串](#java)
  - [1. 子串](#1)
  - [2. 拼接](#2)
  - [3. 相等判断](#3)
  - [4. 字符串长度](#4)
  - [5. 空串和 Null 串](#5--null)
  - [6. 不可变字符串](#6)
  - [7. 字符串构建](#7)

# Java 基础：字符串

Java 字符串就是 Unicode 字符序列。例如：字符串"java/u2122"由 5 个 Unicode 字符组成，分别是 j、a、v、a、™。

Java 没有设计内置的字符串类型，而是在 Java 标准库中提供了一个预定义类 [String](https://docs.oracle.com/javase/9/docs/api/java/lang/String.html)。每个用双引号括起来的字符串都是 String 类的一个实例。

## 1. 子串

`String	substring​(int beginIndex, int endIndex)`：从一个较大的字符串中提取一个子串。子串从 beginIndex 开始，到 endIndex 为止（不包括 endIndex）。
```java
String sub = "hello".substring(1, 3); // "el"
```

## 2. 拼接

使用 + 拼接两个字符串，任何类型的值与字符串进行 "+" 运算时，非字符串的值总是被转换为字符串。

`static String	join​(CharSequence delimiter, CharSequence... elements)`：将多个字符串用分隔符进行连接。

## 3. 相等判断

使用`String.equals()`方法检测两个字符串的内容是否完全相同，使用`String.equalsIgnoreCase()`进行不区分大小写的判断。

NOTE：使用 `==` 判断的是两个是否指向同一个字符串，而不是判断内容是否相同。

## 4. 字符串长度

Java 字符串由 char 值序列组成。`String.length`方法返回采用 UTF-16 编码表示的给定字符串所需要的代码单元数量。

## 5. 空串和 Null 串

空串是长度为 0 的字符串对象，写为 `""`，有自己的长度（0）和内容（空）。

Null 串表示没有任何对象与该字符串变量关联，没有长度也没有内容。若对一个 Null 串调用 length 方法，会抛出异常。

一般使用以下条件检查字符串不为 null 也不为空串：
```java
if (str != null && str.length != 0) {//...}
```

## 6. 不可变字符串

Java 中将 String 类对象定义为不可变字符串，即**不能修改字符串中的某个字符**（但可以修改字符串变量 a 引用的字符串字面量）。

NOTE: 区分字符串拼接和字符串修改
```java
String str = "hello";
str = str + "world";// 字符串拼接
str += "world";// 字符串拼接
str[2] // error
str = str.substring(0, 2) + "p" + str.substring(3, 5);// "heplo" 字符串修改（实际上仍旧是字符串的拼接）
```

不可变字符串让编译器可以共享字符串，即在公共存储池中存放各种字符串常量，多个字符串变量可以指向同一个字符串常量（只有字符串常量是共享的，`+` 或 `substring()` 产生的临时字符串值并不是共享的）。虽然这样的设计导致**修改字符串时需要进行子串提取、拼接操作**，但共享带来的高效率远胜于多出的操作。因为实际上只有很少的情况需要修改字符串，而对于需要频繁拼接的字符串，Java 提供了 [StringBuilder](https://docs.oracle.com/javase/9/docs/api/java/lang/StringBuilder.html) 类。

P.S. 在 C++ 中字符串是可修改的，即可以修改字符串的单个字符。

## 7. 字符串构建

由于 Java 字符串不可修改，而每次拼接字符串时都会产生一个新的 String 临时对象。当需要对一个字符串变量进行频繁的拼接时，耗时与空间的浪费就显得非常严重，因此 Java 提供了 [StringBuilder](https://docs.oracle.com/javase/9/docs/api/java/lang/StringBuilder.html) 类，用于字符串的构建。

```java
// 创建一个空的字符串构建起
StringBuilder builder = new StringBuilder();
// 每次需要进行字符串拼接时，调用 append 方法
builder.append("xxx");
// 修改字符串中的字符，将第 i 个代码单圈设置为 (char)c
builder.setCharAt(i, 'c');
// 在 offset 位置插入一个字符串
builder.insert(offset, s);
// 构建完成，返回一个 String 对象
String str = builder.toString();
```

P.S. [StringBuffer](https://docs.oracle.com/javase/9/docs/api/java/lang/StringBuffer.html) 类是 StringBuilder 类的前身，两者 API 基本上完全相同，效率稍低，但允许采用多线程的方式执行添加或删除字符的操作。但若所有字符串在单线程中编译（通常情况都是这样），应使用 StringBuilder 进行字符串的构建。