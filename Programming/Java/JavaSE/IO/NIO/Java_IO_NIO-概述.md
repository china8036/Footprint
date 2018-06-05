- [Java IO NIO: 概述](#java-io-nio)
  - [1. 基本特性](#1)
  - [2. 核心内容](#2)
  - [3. Refer Links](#3-refer-links)

# Java IO NIO: 概述

## 1. 基本特性

在传统的 BIO 中，存在以下问题：
- 当从输入 / 输出流中读取 / 写入数据时，如果资源还未就绪，程序将在此处阻塞该线程的执行。
- 输入 / 输出操作面向流进行，即通过字节的移动来处理数据，一次只能处理一个字节，因此通常效率不高。

从 JDK1.4 开始，Java 提供了一些列改进的用于提高 I/O 效率的输入 / 输出处理新功能，这些功能被封装在 java.nio 包及其子包下，统称为 New IO (NIO)。针对 BIO 存在的问题，NIO 主要做了以下改进：

- 面向块的 IO 处理
  
  NIO 模拟了操作系统中虚拟内存的概念，采用**内存映射文件**的方式来处理输入 / 输出，即将文件或文件的一部分映射到内存中，从而可以像访问内存一样来访问文件了，大大提高了 IO 处理的速度。

  NIO 使用 Channel 模拟传统的输入 / 输出系统，所有的数据都需要通过 Channel 进行传输。Channel 与 BIO 中的 InputStream/OutputStream 最大的区别在于它提供了一个 map 方法，通过该方法可以直接将“一块数据”映射到内存中。每一块数据以 Buffer 对象形式存在，Buffer 本质上是一个数组，它相当于一个容器，与 Channel 进行交互的所有对象都必须存放在 Buffer 对象中。

- 同步非阻塞式 IO

  NIO 提供了多路复用器 Selector，用以实现基于 IO 多路复用的同步非阻塞式 IO。

## 2. 核心内容

Java NIO 的相关接口和类封装在 java.nio 包下：
- java.nio: 主要包含各种与 Buffer 相关的类。
- java.nio.channels: 主要包含与 Channel 和 Selector 相关的类。
- java.nio.channels.spi: 主要包含与 Channel 相关的服务提供者编程接口。
- java.nio.charset: 主要包含与字符集相关的类。
- java.nio.charset.spi: 主要包含了字符集相关的服务提供者编程接口。

Java NIO 的核心内容包括以下 4 项：
- Buffer 
- Channel
- Charset
- Seletor

## 3. Refer Links
