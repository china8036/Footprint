- [Protocol Buffers](#protocol-buffers)
  - [1. 数据类型](#1-数据类型)
  - [2. 使用步骤](#2-使用步骤)
    - [2.1. 定义 proto 文件](#21-定义-proto-文件)
    - [2.2. 编译 proto 文件](#22-编译-proto-文件)
    - [2.3. 使用 Protocol Buffer API](#23-使用-protocol-buffer-api)
  - [3. 高级使用](#3-高级使用)
    - [3.1. 反射](#31-反射)
    - [4. 优化](#4-优化)
  - [5. Refer Links](#5-refer-links)

# Protocol Buffers

protobuf 是 google 的中立于语言，平台，可扩展的用于序列化结构化数据的解决方案。

protobuf 支持目前主流的开发语言，包括 C++、Java、Python、Objective-C、C#、JavaNano、JavaScript、Ruby、Go、PHP 等。只要你使用以上语言，都可以用 protobuf 来序列化和反序列化你的数据。

Google Protocol Buffers 效率较高，但是数据对象必须预先定义，并使用 protoc 编译，适合要求效率，允许自定义类型的内部场合使用。

## 1. 数据类型

| proto 文件消息类型 | C++ 类型 | 说明                                                         |
| ----------------- | -------- | ------------------------------------------------------------ |
| double            | double   |                                                              |
| float             | float    |                                                              |
| int32             | int32    | 使用可变长编码方式，负数时不够高效，应该使用 sint32           |
| int64             | int64    | 同上                                                         |
| uint32            | uint32   | 使用可变长编码方式                                           |
| uint64            | uint64   | 同上                                                         |
| sint32            | int32    | 使用可变长编码方式，有符号的整型值，负数编码时比通常的 int32 高效 |
| sint64            | sint64   | 同上                                                         |
| fixed32           | uint32   | 总是 4 个字节，如果数值总是比 2^28 大的话，这个类型会比 uint32 高效 |
| fixed64           | uint64   | 总是 8 个字节，如果数值总是比 2^56 大的话，这个类型会比 uint64 高效 |
| sfixed32          | int32    | 总是 4 个字节                                                  |
| sfixed64          | int64    | 总是 8 个字节                                                  |
| bool              | bool     |                                                              |
| string            | string   | 一个字符串必须是 utf-8 编码或者 7-bit 的 ascii 编码的文本          |
| bytes             | string   | 可能包含任意顺序的字节数据                                   |

## 2. 使用步骤

1. 定义 proto 文件，文件的内容就是定义我们需要存储或者传输的数据结构，也就是定义我们自己的数据存储或者传输的协议。

1. 编译安装 protocol buffer 编译器来编译自定义的。proto 文件，用于生成。pb.h 文件（proto 文件中自定义类的头文件）和 .pb.cc（proto 文件中自定义类的实现文件）。

1. 使用 protoco buffer 的 C++ API 来读写消息。

todo: http://km.oa.com/group/11800/articles/show/272397?kmref=search&from_page=1&no=1&is_from_iso=1

### 2.1. 定义 proto 文件

可以在 [Language Guide (proto3)](https://developers.google.com/protocol-buffers/docs/proto3) 一文中找到编写。proto 文件的完整指南。但是，不要想在里面找到与类继承相似的特性，因为 protocol buffers 不是拿来做这个的。

### 2.2. 编译 proto 文件

1. 编译安装 [Protocol Buffers](https://developers.google.com/protocol-buffers/docs/downloads) 
1. 编译 proto 文件
    
    eg:
    ```
    protoc -I=$SRC_DIR --cpp_out=$DST_DIR $SRC_DIR/addressbook.proto
    ```

### 2.3. 使用 Protocol Buffer API

[C++ API reference](https://developers.google.com/protocol-buffers/docs/reference/cpp/)

[complete API documentation for Message](https://developers.google.com/protocol-buffers/docs/reference/cpp/google.protobuf.message#Message)

[C++ generated code reference](https://developers.google.com/protocol-buffers/docs/reference/cpp-generated)

[Message API reference](https://developers.google.com/protocol-buffers/docs/reference/cpp/google.protobuf.message#Message)

## 3. 高级使用

### 3.1. 反射

protocol 消息类所提供的一个关键特性就是反射。你不需要编写针对一个特殊的消息（message）类型的代码，就可以遍历一个消息的字段，并操纵它们的值，就像 XML 和 JSON 一样。“反射”的一个更高级的用法可能就是可以找出两个相同类型的消息之间的区别，或者开发某种“协议消息的正则表达式”，利用正则表达式，你可以对某种消息内容进行匹配。只要你发挥你的想像力，就有可能将 Protocol Buffers 应用到一个更广泛的、你可能一开始就期望解决的问题范围上。

“反射”是由 Message::Reflection interface 提供的。

### 4. 优化

Protocol Buffer 的 C++ 库已经做了极度优化。但是，正确的使用方法仍然会提高很多性能。下面是一些小技巧，用来提升 protocol buffer 库的最后一丝速度能力：

- 如果有可能，重复利用消息（message）对象。即使被清除掉，消息（message）对象也会尽量保存所有被分配来重用的内存。这样的话，如果你正在处理很多类型相同的消息以及一系列相似的结构，有一个好办法就是重复使用同一个消息（message）对象，从而使内存分配的压力减小一些。然而，随着时间的流逝，对象占用的内存也有可能变得越来越大，尤其是当你的消息尺寸（译者注：各消息内容不同，有些消息内容多一些，有些消息内容少一些）不同的时候，或者你偶尔创建了一个比平常大很多的消息（message）的时候。你应该自己监测消息（message）对象的大小——通过调用 SpaceUsed 函数——并在它太大的时候删除它。

- 在多线程中分配大量小对象的内存的时候，你的操作系统的内存分配器可能优化得不够好。在这种情况下，你可以尝试用一下 Google’s tcmalloc。

## 5. Refer Links

[Protocol Buffers Developer Guide](https://developers.google.com/protocol-buffers/docs/overview)