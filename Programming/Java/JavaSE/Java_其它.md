- [Java 零碎知识点整理](#java-%E9%9B%B6%E7%A2%8E%E7%9F%A5%E8%AF%86%E7%82%B9%E6%95%B4%E7%90%86)
  - [Java 程序： Application 与 Applet 的区别](#java-%E7%A8%8B%E5%BA%8F%EF%BC%9A-application-%E4%B8%8E-applet-%E7%9A%84%E5%8C%BA%E5%88%AB)
    - [Application](#application)
    - [Applet](#applet)
  - [取余运算的实现以及奇偶判断的一个细节](#%E5%8F%96%E4%BD%99%E8%BF%90%E7%AE%97%E7%9A%84%E5%AE%9E%E7%8E%B0%E4%BB%A5%E5%8F%8A%E5%A5%87%E5%81%B6%E5%88%A4%E6%96%AD%E7%9A%84%E4%B8%80%E4%B8%AA%E7%BB%86%E8%8A%82)

# Java 零碎知识点整理

## Java 程序： Application 与 Applet 的区别

Java 语言是一种半编译半解释的语言，Java 的用户程序分为两类：Java Application 和 Java Applet。这两类程序在组成结构和执行机制上都有一定的差异：

### Application

- application 主要是桌面应用程序的开发，application 是不能用 Jsp 加载的 。

- Java Application 是完整的程序，可以独立运行。

- Java Application 程序被编译以后，用普通的 Java 解释器就可以使其边解释边执行。

- 每个 Java Application 程序必定含有一个并且只有一个 main 方法，程序执行时，首先寻找 main 方法，并以此为入口点开始运行。含有 main 方法的那个类，常被称为主类，也就是说，Java Application 程序都含有一个主类。

### Applet

- applet 一般用于 B/S 页面上作为插件式的开发。

- Java Applet 程序不能单独运行，它必须嵌入到用 HTML 语言编写的 Web 页面中，通过与 Java 兼容的浏览器来控制执行。

- Java Applet 必须通过网络浏览器或者 Applet 观察器才能执行。 

- Applet 程序则没有含 main 方法的主类，这也正是  Applet 程序不能独立运行的原因。


## 取余运算的实现以及奇偶判断的一个细节

```java
int ftcs = dealFtcs(ftcs) {
  if(ftcs % 2 == 1) {      //奇数
    /*
     * 处理.....
     */
  } else {        　　　  //偶数
    /*
     * 处理......
     */
  }
}
```
这个ftcs是需要经过一系列的运算得到的结果，然后再做奇偶判断，为奇数做相应处理，否则做偶数处理。对于正数可正常执行，但对于负数，所有负数都会被判定为偶像。

通过查找资料发现，java的取余的实现大致如下：
```java
/**
  * @desc 取余模拟算法
  * @param dividend 被除数
  * @param divisor 除数
  * @return
  * @return int
  */
public static int remainder(int dividend, int divisor) {
  return dividend - dividend / divisor * divisor;
}
//如：10 % 3 == 1，运算过程：10 – 10 / 3 * 3 = 1（注意都是int，没有小数部分）
```
当ftcs = -11时， -11 – (-11 / 2 * 2) = -1;（不等于1）

当ftcs = -10时， -10 – (-10 / 2 * 2) = 0;

……

因此，修正如下：
```java
int ftcs = dealFtcs(ftcs) {
  if(ftcs % 2 == 0) {       //偶数
    /*
     * 处理.....
     */
  } else {        　　　   //奇数
    /*
     * 处理......
     */
  }
}
```
因此：使用取余运算判断奇偶数时，推荐用偶数判断，不要用奇数判断。若用奇数判断，应对`1`加上绝对值。

