- [C 基础语法](#c-基础语法)
  - [1. 变量类型](#1-变量类型)
  - [2. sizeof 运算符](#2-sizeof-运算符)
    - [2.1. 指针类型](#21-指针类型)
    - [2.2. 结构类型](#22-结构类型)
  - [3. delete 操作符](#3-delete-操作符)
  - [4. char 类型](#4-char-类型)
  - [5. Refer Links](#5-refer-links)

# C 基础语法

C 语言作为一门应用途广泛、功能强大、使用灵活的面向过程式编程语言。既可用于编写应用软件，又能用于编写系统软件。

## 1. 变量类型

常用的基本数据类型 char、int 等在不同编译环境下就会占用不同大小的空间：

![image](http://img.cdn.firejq.com/jpg/2018/8/8/3f940c68f3b0ab91173e18626d0bb2f5.jpg)

## 2. sizeof 运算符

https://zh.cppreference.com/w/cpp/language/sizeof

http://blog.51cto.com/lemonmilk/107155

http://www.cppblog.com/w57w57w57/archive/2011/08/09/152845.html

sizeof 一般形式为：sizeof（object），也可以 sizeof var_char，通常习惯用 sizeof()。

sizeof 是单目运算符，其运算符的含义是：求出对象在计算机内存中所占用的字节数。对象可以是表达式或者数据类型名，当对象是表达式时，括号可省略。

### 2.1. 指针类型

对于指针，要特别区分，指针指向什么数据，它在内存占的字节数才是它的结果。如：指针指向一个字符串，就是字符串的长度，因为一个字符在内存中占一个字节。若指针指向一个数据结构，则结果应该是结构型数据的内存字节数。

### 2.2. 结构类型

```c
struct str{
    double d;
    char ch;
    int data;
}str_wu;
struct str1{
    char ch;
    double d;
    int data;
}str_wu1;
```

两个不同的结构，但是内部的元素是相同的，都是 double，int，char，只是顺序不一样，就结果不一样。why？

这时因为 VC 存储数据的时候要对其，具体的情况如下：

类型：对齐方式（变量存放的起始地址相对于结构的起始地址的偏移量）
- Char: 偏移量必须为 sizeof(char) 即 1 的倍数
- int: 偏移量必须为 sizeof(int) 即 4 的倍数
- float: 偏移量必须为 sizeof(float) 即 4 的倍数
- double: 偏移量必须为 sizeof(double) 即 8 的倍数
- Short: 偏移量必须为 sizeof(short) 即 2 的倍数

比如：str_wu，为上面的结构分配空间的时候，VC 根据成员变量出现的顺序和对齐方式，先为第一个成员 dda1 分配空间，其起始地址跟结构的起始地址相同（刚好偏移量 0 刚好为 sizeof(double) 的倍数），该成员变量占用 sizeof(double)=8 个字节；接下来为第二个成员 dda 分配空间，这时下一个可以分配的地址对于结构的起始地址的偏移量为 8，是 sizeof(char) 的倍数，所以把 dda 存放在偏移量为 8 的地方满足对齐方式，该成员变量占用 sizeof(char)=1 个字节；接下来为第三个成员 type 分配空间，这时下一个可以分配的地址对于结构的起始地址的偏移量为 9，不是 sizeof(int)=4 的倍数，为了满足对齐方式对偏移量的约束问题，VC 自动填充 3 个字节（这三个字节没有放什么东西），这时下一个可以分配的地址对于结构的起始地址的偏移量为 12，刚好是 sizeof(int)=4 的倍数，所以把 type 存放在偏移量为 12 的地方，该成员变量占用 sizeof(int)=4 个字节；这时整个结构的成员变量已经都分配了空间，总的占用的空间大小为：8+1+3+4=16，刚好为结构的字节边界数（即结构中占用最大空间的类型所占用的字节数 sizeof(double)=8）的倍数，所以没有空缺的字节需要填充。所以整个结构的大小为：sizeof(str_wu)=8+1+3+4=16，其中有 3 个字节是 VC 自动填充的，没有放任何有意义的东西。

而 str_wu1，同样的道理：如下：sizeof(char)=1, 而 1 不是 8 的倍数，因而增加到 8，sizeof（double）＝8，现在开始地址是 16，16 是 sizeof（int) 的倍数，可以存入。因而总的地址数：sizeof（char）＋7＋sizeof（double）＋sizeof（int）＝20，而 20 不是 8 的倍数（sizeof(double)=8), 所以需要在增加 4 个地址，即总共 24。

## 3. delete 操作符

https://www.nowcoder.com/questionTerminal/9fb652d48bee45bcb47771b2e3c6f690

## 4. char 类型

[浅析为什么 char 类型的范围是 —128~+127](https://blog.csdn.net/daiyutage/article/details/8575248)

## 5. Refer Links