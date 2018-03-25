- [Java IO BIO 数组操作节点流](#java-io-bio-%E6%95%B0%E7%BB%84%E6%93%8D%E4%BD%9C%E8%8A%82%E7%82%B9%E6%B5%81)
    - [1. ByteArrayInputStream](#1-bytearrayinputstream)
        - [1.1. 基本概念](#11-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
        - [1.2. 类定义](#12-%E7%B1%BB%E5%AE%9A%E4%B9%89)
        - [1.3. 常用 API](#13-%E5%B8%B8%E7%94%A8-api)
    - [2. ByteArrayOutputStream](#2-bytearrayoutputstream)
        - [2.1. 基本概念](#21-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
        - [2.2. 类定义](#22-%E7%B1%BB%E5%AE%9A%E4%B9%89)
        - [2.3. 常用 API](#23-%E5%B8%B8%E7%94%A8-api)
    - [3. CharArrayReader](#3-chararrayreader)
        - [3.1. 基本概念](#31-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
        - [3.2. 类定义](#32-%E7%B1%BB%E5%AE%9A%E4%B9%89)
        - [3.3. 常用 API](#33-%E5%B8%B8%E7%94%A8-api)
    - [4. CharArrayWriter](#4-chararraywriter)
        - [4.1. 基本概念](#41-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
        - [4.2. 类定义](#42-%E7%B1%BB%E5%AE%9A%E4%B9%89)
        - [4.3. 常用 API](#43-%E5%B8%B8%E7%94%A8-api)
    - [5. Refer Links](#5-refer-links)

# Java IO BIO 数组操作节点流

<!-- TODO: 待补充完善 -->

## 1. ByteArrayInputStream

### 1.1. 基本概念

### 1.2. 类定义

### 1.3. 常用 API

## 2. ByteArrayOutputStream

### 2.1. 基本概念

### 2.2. 类定义

### 2.3. 常用 API

例：将内容写入到 ByteArrayOutputStream 中并打印出来，不需要关闭流
```java
private static void testByByteArrayStream() {  
    ByteArrayOutputStream byteOut = new ByteArrayOutputStream(8);
    String str = "Hello World!";
    try {
        byteOut.write(str.getBytes());
    } catch (IOException e) {
        log.error("写入字节数据出错！", e);
    }

    byte[] buf = byteOut.toByteArray();
    for (byte b : buf) {
        log.info("{}", (char) b);
    }
}
```

## 3. CharArrayReader

### 3.1. 基本概念

### 3.2. 类定义

### 3.3. 常用 API

## 4. CharArrayWriter

### 4.1. 基本概念

### 4.2. 类定义

### 4.3. 常用 API

## 5. Refer Links