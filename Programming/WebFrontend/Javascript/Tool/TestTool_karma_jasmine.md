- [测试工具：karma 和 jasmine](#%E6%B5%8B%E8%AF%95%E5%B7%A5%E5%85%B7%EF%BC%9Akarma-%E5%92%8C-jasmine)
  - [1. 单元测试容器 karma](#1-%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95%E5%AE%B9%E5%99%A8-karma)
  - [2. 单元测试工具 jasmine](#2-%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95%E5%B7%A5%E5%85%B7-jasmine)

# 测试工具：karma 和 jasmine #

## 1. 单元测试容器 karma ##


- 安装
```
npm install karma
```



## 2. 单元测试工具 jasmine ##

jasmine 意为 “茉莉花”，类似 Java 中的 JUnit，jasmine 提供了一套语法，用来简洁地编写测试用例；

jasmine 有四个核心概念：**分组、用例、期望、匹配**，分别对应 jasmine 的四个函数：
- describe(string, function)：表示分组，即一组测试用例；
- it(string, function)：表示测试用例；
- expect(expression)：表示期望指定表示式具有某个值或者某种行为；
- to***(arg)：表示匹配；


- 安装
```
npm install jasmine
```