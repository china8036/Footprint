- [Java 面向对象：封装](#java)
	- [1. 构造器](#1)
	- [2. 属性](#2)
	- [3. 方法](#3)
		- [3.1. 参数传递](#31)
		- [3.2. 静态方法](#32)
		- [3.3. 静态域](#33)
		- [3.4. 静态常量](#34)
		- [3.5. 静态方法](#35)
		- [3.6. 工厂方法](#36)
		- [3.7. main 方法](#37-main)
	- [4. 访问控制](#4)
	- [5. this](#5-this)
	- [6. 内部类](#6)
	- [7. 包](#7)

# Java 面向对象：封装

## 1. 构造器

## 2. 属性

## 3. 方法

### 3.1. 参数传递

[Java 到底是值传递还是引用传递？ - Intopass 的回答 - 知乎](https://www.zhihu.com/question/31203609/answer/50992895)

[为什么 Java 只有值传递，但 C# 既有值传递，又有引用传递，这种语言设计有哪些好处？ - Hugo Gu 的回答 - 知乎](https://www.zhihu.com/question/20628016/answer/28970414)

在 Java 语言中，函数参数的传递分为两种类型：
- 对于基本类型变量：参数传递为值传递，即将实参的副本赋值给形参。因此，函数内部的改变与外部参数无关。
- 对于引用类型变量（包括 String 和数组）: 参数传递为引用传递，即将实参的引用的副本赋值给形参。因此，在函数内部对该引用的操作可以影响到外部参数，但是无法将该引用赋值对象的改变传递到函数体外。

eg:
- 基本类型
	```java
	void foo(int value) {
			value = 100;
	}
	foo(num); // num 没有被改变
	```
- 没有提供改变自身方法的引用类型
	```java
	void foo(String text) {
			text = "windows";
	}
	foo(str); // str 也没有被改变
	```
- 提供了改变自身方法的引用类型
	```java
	StringBuilder sb = new StringBuilder("iphone");
	void foo(StringBuilder builder) {
			builder.append("4");
	}
	foo(sb); // sb 被改变了，变成了"iphone4"。
	```
	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/5/11/808284be05008851d3bea1dc9e061a9a.jpg)

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/5/11/cdffce0a9c92fa18cfb9b73f6cbe9508.jpg)

- 提供了改变自身方法的引用类型，但是不使用，而是使用赋值运算符
	```java
	StringBuilder sb = new StringBuilder("iphone");
	void foo(StringBuilder builder) {
			builder = new StringBuilder("ipad");
	}
	foo(sb); // sb 没有被改变，还是 "iphone"。
	```
	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/5/11/808284be05008851d3bea1dc9e061a9a.jpg)

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/5/11/86242c7bf0f630043bddcd62ce642b0f.jpg)

### 3.2. 静态方法

### 3.3. 静态域

### 3.4. 静态常量

### 3.5. 静态方法

### 3.6. 工厂方法

### 3.7. main 方法

## 4. 访问控制

## 5. this

## 6. 内部类

## 7. 包