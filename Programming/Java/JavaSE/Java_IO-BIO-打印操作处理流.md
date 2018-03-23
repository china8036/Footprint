- [Java IO BIO 打印操作处理流](#java-io-bio-%E6%89%93%E5%8D%B0%E6%93%8D%E4%BD%9C%E5%A4%84%E7%90%86%E6%B5%81)
	- [1. PrintStream](#1-printstream)
		- [1.1. 基本概念](#11-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [1.2. 类定义](#12-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [1.3. 常用 API](#13-%E5%B8%B8%E7%94%A8-api)
	- [2. PrintWriter](#2-printwriter)
		- [2.1. 基本概念](#21-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
		- [2.2. 类定义](#22-%E7%B1%BB%E5%AE%9A%E4%B9%89)
		- [2.3. 常用 API](#23-%E5%B8%B8%E7%94%A8-api)
	- [3. Refer Links](#3-refer-links)

# Java IO BIO 打印操作处理流

<!-- TODO: 待补充完善 -->

## 1. PrintStream

### 1.1. 基本概念

标准输出 System.out 就是 PrintStream 的实例对象。

### 1.2. 类定义

### 1.3. 常用 API

例：
```java
private static void testOutputByPrintStream() {  
    try (
				PrintStream ps = new PrintStream(new FileOutputStream("G:/test/d.txt"));
    ) {
				// 输出字符串
				ps.println("hello world");
				// 输出对象
				ps.println(new Test());
        // 重定向标准输出
        System.setOut(ps);
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

## 2. PrintWriter

### 2.1. 基本概念

### 2.2. 类定义

### 2.3. 常用 API

## 3. Refer Links