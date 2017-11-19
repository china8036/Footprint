- [Python 语法](#python-%E8%AF%AD%E6%B3%95)
  - [变量](#%E5%8F%98%E9%87%8F)
    - [变量命名规范](#%E5%8F%98%E9%87%8F%E5%91%BD%E5%90%8D%E8%A7%84%E8%8C%83)
    - [全局、局部变量](#%E5%85%A8%E5%B1%80%E3%80%81%E5%B1%80%E9%83%A8%E5%8F%98%E9%87%8F)
  - [数据类型](#%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
    - [基本数据类型](#%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
      - [布尔值](#%E5%B8%83%E5%B0%94%E5%80%BC)
      - [数字](#%E6%95%B0%E5%AD%97)
        - [int](#int)
          - [大小](#%E5%A4%A7%E5%B0%8F)
          - [基数](#%E5%9F%BA%E6%95%B0)
        - [float](#float)
      - [字符串](#%E5%AD%97%E7%AC%A6%E4%B8%B2)
        - [创建](#%E5%88%9B%E5%BB%BA)
        - [输出](#%E8%BE%93%E5%87%BA)
        - [使用`+`进行拼接](#%E4%BD%BF%E7%94%A8%E8%BF%9B%E8%A1%8C%E6%8B%BC%E6%8E%A5)
        - [使用`*`进行复制](#%E4%BD%BF%E7%94%A8%E8%BF%9B%E8%A1%8C%E5%A4%8D%E5%88%B6)
        - [使用`[]`提取字符](#%E4%BD%BF%E7%94%A8%E6%8F%90%E5%8F%96%E5%AD%97%E7%AC%A6)
        - [使用`[start:end:step]`分片](#%E4%BD%BF%E7%94%A8startendstep%E5%88%86%E7%89%87)
        - [使用 len() 获取长度](#%E4%BD%BF%E7%94%A8-len-%E8%8E%B7%E5%8F%96%E9%95%BF%E5%BA%A6)
        - [使用字符串方法](#%E4%BD%BF%E7%94%A8%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%96%B9%E6%B3%95)
          - [使用 split() 分割](#%E4%BD%BF%E7%94%A8-split-%E5%88%86%E5%89%B2)
          - [使用 join() 合并](#%E4%BD%BF%E7%94%A8-join-%E5%90%88%E5%B9%B6)
          - [replace()](#replace)
          - [strip()](#strip)
          - [大小写转换](#%E5%A4%A7%E5%B0%8F%E5%86%99%E8%BD%AC%E6%8D%A2)
          - [格式排版](#%E6%A0%BC%E5%BC%8F%E6%8E%92%E7%89%88)
    - [容器类型](#%E5%AE%B9%E5%99%A8%E7%B1%BB%E5%9E%8B)
      - [列表](#%E5%88%97%E8%A1%A8)
        - [特性](#%E7%89%B9%E6%80%A7)
        - [创建](#%E5%88%9B%E5%BB%BA)
        - [使用 [offset] 获取、修改元素](#%E4%BD%BF%E7%94%A8-offset-%E8%8E%B7%E5%8F%96%E3%80%81%E4%BF%AE%E6%94%B9%E5%85%83%E7%B4%A0)
        - [使用切片提取元素](#%E4%BD%BF%E7%94%A8%E5%88%87%E7%89%87%E6%8F%90%E5%8F%96%E5%85%83%E7%B4%A0)
        - [使用 len() 获取长度](#%E4%BD%BF%E7%94%A8-len-%E8%8E%B7%E5%8F%96%E9%95%BF%E5%BA%A6)
        - [使用 in 判断值是否存在](#%E4%BD%BF%E7%94%A8-in-%E5%88%A4%E6%96%AD%E5%80%BC%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8)
        - [使用 del 删除指定位置的元素](#%E4%BD%BF%E7%94%A8-del-%E5%88%A0%E9%99%A4%E6%8C%87%E5%AE%9A%E4%BD%8D%E7%BD%AE%E7%9A%84%E5%85%83%E7%B4%A0)
        - [使用 list 方法](#%E4%BD%BF%E7%94%A8-list-%E6%96%B9%E6%B3%95)
          - [使用 append() 添加元素至尾部](#%E4%BD%BF%E7%94%A8-append-%E6%B7%BB%E5%8A%A0%E5%85%83%E7%B4%A0%E8%87%B3%E5%B0%BE%E9%83%A8)
          - [使用 extend() 或 += 合并列表](#%E4%BD%BF%E7%94%A8-extend-%E6%88%96-%E5%90%88%E5%B9%B6%E5%88%97%E8%A1%A8)
          - [使用 insert() 在指定位置插入元素](#%E4%BD%BF%E7%94%A8-insert-%E5%9C%A8%E6%8C%87%E5%AE%9A%E4%BD%8D%E7%BD%AE%E6%8F%92%E5%85%A5%E5%85%83%E7%B4%A0)
          - [使用 remove() 删除具有指定值的元素](#%E4%BD%BF%E7%94%A8-remove-%E5%88%A0%E9%99%A4%E5%85%B7%E6%9C%89%E6%8C%87%E5%AE%9A%E5%80%BC%E7%9A%84%E5%85%83%E7%B4%A0)
          - [使用 pop() 获取并删除指定位置的元素](#%E4%BD%BF%E7%94%A8-pop-%E8%8E%B7%E5%8F%96%E5%B9%B6%E5%88%A0%E9%99%A4%E6%8C%87%E5%AE%9A%E4%BD%8D%E7%BD%AE%E7%9A%84%E5%85%83%E7%B4%A0)
          - [使用 index() 查询具有特定值的元素位](#%E4%BD%BF%E7%94%A8-index-%E6%9F%A5%E8%AF%A2%E5%85%B7%E6%9C%89%E7%89%B9%E5%AE%9A%E5%80%BC%E7%9A%84%E5%85%83%E7%B4%A0%E4%BD%8D)
          - [使用 count() 记录特定值出现的次数](#%E4%BD%BF%E7%94%A8-count-%E8%AE%B0%E5%BD%95%E7%89%B9%E5%AE%9A%E5%80%BC%E5%87%BA%E7%8E%B0%E7%9A%84%E6%AC%A1%E6%95%B0)
          - [使用 sort() 重新排列元素](#%E4%BD%BF%E7%94%A8-sort-%E9%87%8D%E6%96%B0%E6%8E%92%E5%88%97%E5%85%83%E7%B4%A0)
          - [使用 = 赋值， 使用 copy() 复制](#%E4%BD%BF%E7%94%A8-%E8%B5%8B%E5%80%BC%EF%BC%8C-%E4%BD%BF%E7%94%A8-copy-%E5%A4%8D%E5%88%B6)
      - [元组](#%E5%85%83%E7%BB%84)
        - [特性](#%E7%89%B9%E6%80%A7)
        - [创建](#%E5%88%9B%E5%BB%BA)
        - [元组解包](#%E5%85%83%E7%BB%84%E8%A7%A3%E5%8C%85)
        - [元组与列表的比较](#%E5%85%83%E7%BB%84%E4%B8%8E%E5%88%97%E8%A1%A8%E7%9A%84%E6%AF%94%E8%BE%83)
      - [字典](#%E5%AD%97%E5%85%B8)
        - [特性](#%E7%89%B9%E6%80%A7)
        - [创建](#%E5%88%9B%E5%BB%BA)
        - [使用 [key] 添加或修改元素](#%E4%BD%BF%E7%94%A8-key-%E6%B7%BB%E5%8A%A0%E6%88%96%E4%BF%AE%E6%94%B9%E5%85%83%E7%B4%A0)
        - [使用 del 删除具有指定键的元素](#%E4%BD%BF%E7%94%A8-del-%E5%88%A0%E9%99%A4%E5%85%B7%E6%9C%89%E6%8C%87%E5%AE%9A%E9%94%AE%E7%9A%84%E5%85%83%E7%B4%A0)
        - [使用 in 判断是否存在](#%E4%BD%BF%E7%94%A8-in-%E5%88%A4%E6%96%AD%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8)
        - [使用 dict 的方法](#%E4%BD%BF%E7%94%A8-dict-%E7%9A%84%E6%96%B9%E6%B3%95)
          - [使用 clear() 删除所有元素](#%E4%BD%BF%E7%94%A8-clear-%E5%88%A0%E9%99%A4%E6%89%80%E6%9C%89%E5%85%83%E7%B4%A0)
          - [使用 update() 合并字典](#%E4%BD%BF%E7%94%A8-update-%E5%90%88%E5%B9%B6%E5%AD%97%E5%85%B8)
          - [使用 get() 获取键值](#%E4%BD%BF%E7%94%A8-get-%E8%8E%B7%E5%8F%96%E9%94%AE%E5%80%BC)
          - [使用 keys() 获取所有键](#%E4%BD%BF%E7%94%A8-keys-%E8%8E%B7%E5%8F%96%E6%89%80%E6%9C%89%E9%94%AE)
          - [使用 values() 获取所有值](#%E4%BD%BF%E7%94%A8-values-%E8%8E%B7%E5%8F%96%E6%89%80%E6%9C%89%E5%80%BC)
          - [使用 items() 获取所有键值对](#%E4%BD%BF%E7%94%A8-items-%E8%8E%B7%E5%8F%96%E6%89%80%E6%9C%89%E9%94%AE%E5%80%BC%E5%AF%B9)
          - [使用 = 赋值， 使用 copy() 复制](#%E4%BD%BF%E7%94%A8-%E8%B5%8B%E5%80%BC%EF%BC%8C-%E4%BD%BF%E7%94%A8-copy-%E5%A4%8D%E5%88%B6)
      - [集合](#%E9%9B%86%E5%90%88)
        - [特性](#%E7%89%B9%E6%80%A7)
        - [创建](#%E5%88%9B%E5%BB%BA)
        - [使用 in 测试值是否存在](#%E4%BD%BF%E7%94%A8-in-%E6%B5%8B%E8%AF%95%E5%80%BC%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8)
        - [集合运算操作](#%E9%9B%86%E5%90%88%E8%BF%90%E7%AE%97%E6%93%8D%E4%BD%9C)
          - [交集运算](#%E4%BA%A4%E9%9B%86%E8%BF%90%E7%AE%97)
          - [并集运算](#%E5%B9%B6%E9%9B%86%E8%BF%90%E7%AE%97)
          - [差集运算](#%E5%B7%AE%E9%9B%86%E8%BF%90%E7%AE%97)
          - [异或集运算](#%E5%BC%82%E6%88%96%E9%9B%86%E8%BF%90%E7%AE%97)
          - [子集](#%E5%AD%90%E9%9B%86)
          - [超集](#%E8%B6%85%E9%9B%86)
    - [类型转换](#%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
      - [自动类型转换](#%E8%87%AA%E5%8A%A8%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
      - [强制类型转换](#%E5%BC%BA%E5%88%B6%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
        - [int()](#int)
        - [float()](#float)
        - [str()](#str)
        - [list()](#list)
        - [tuple()](#tuple)
        - [dict()](#dict)
        - [set()](#set)
    - [类型判断](#%E7%B1%BB%E5%9E%8B%E5%88%A4%E6%96%AD)
      - [基本类型](#%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B)
      - [引用类型](#%E5%BC%95%E7%94%A8%E7%B1%BB%E5%9E%8B)
  - [符号](#%E7%AC%A6%E5%8F%B7)
    - [运算符](#%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [算术运算符](#%E7%AE%97%E6%9C%AF%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [布尔 / 关系运算符](#%E5%B8%83%E5%B0%94-%E5%85%B3%E7%B3%BB%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [位运算符](#%E4%BD%8D%E8%BF%90%E7%AE%97%E7%AC%A6)
      - [逻辑运算符](#%E9%80%BB%E8%BE%91%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [其它符号](#%E5%85%B6%E5%AE%83%E7%AC%A6%E5%8F%B7)
  - [代码结构](#%E4%BB%A3%E7%A0%81%E7%BB%93%E6%9E%84)
    - [流程控制](#%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6)
      - [if、 elif 和 else](#if%E3%80%81-elif-%E5%92%8C-else)
      - [while、else](#while%E3%80%81else)
      - [for、else](#for%E3%80%81else)
        - [使用 zip() 并行迭代](#%E4%BD%BF%E7%94%A8-zip-%E5%B9%B6%E8%A1%8C%E8%BF%AD%E4%BB%A3)
        - [使用 range() 生成自然数序列](#%E4%BD%BF%E7%94%A8-range-%E7%94%9F%E6%88%90%E8%87%AA%E7%84%B6%E6%95%B0%E5%BA%8F%E5%88%97)
    - [推导式](#%E6%8E%A8%E5%AF%BC%E5%BC%8F)
      - [列表推导式](#%E5%88%97%E8%A1%A8%E6%8E%A8%E5%AF%BC%E5%BC%8F)
      - [字典推导式](#%E5%AD%97%E5%85%B8%E6%8E%A8%E5%AF%BC%E5%BC%8F)
      - [集合推导式](#%E9%9B%86%E5%90%88%E6%8E%A8%E5%AF%BC%E5%BC%8F)
      - [生成器推导式](#%E7%94%9F%E6%88%90%E5%99%A8%E6%8E%A8%E5%AF%BC%E5%BC%8F)
    - [函数](#%E5%87%BD%E6%95%B0)
      - [参数](#%E5%8F%82%E6%95%B0)
        - [位置参数](#%E4%BD%8D%E7%BD%AE%E5%8F%82%E6%95%B0)
        - [关键字参数](#%E5%85%B3%E9%94%AE%E5%AD%97%E5%8F%82%E6%95%B0)
        - [参数默认值](#%E5%8F%82%E6%95%B0%E9%BB%98%E8%AE%A4%E5%80%BC)
          - [使用函数参数的默认值来实现函数静态变量的功能](#%E4%BD%BF%E7%94%A8%E5%87%BD%E6%95%B0%E5%8F%82%E6%95%B0%E7%9A%84%E9%BB%98%E8%AE%A4%E5%80%BC%E6%9D%A5%E5%AE%9E%E7%8E%B0%E5%87%BD%E6%95%B0%E9%9D%99%E6%80%81%E5%8F%98%E9%87%8F%E7%9A%84%E5%8A%9F%E8%83%BD)
        - [使用*和**收集参数](#%E4%BD%BF%E7%94%A8%E5%92%8C%E6%94%B6%E9%9B%86%E5%8F%82%E6%95%B0)
          - [使用*收集位置参数](#%E4%BD%BF%E7%94%A8%E6%94%B6%E9%9B%86%E4%BD%8D%E7%BD%AE%E5%8F%82%E6%95%B0)
          - [使用**收集关键字参数](#%E4%BD%BF%E7%94%A8%E6%94%B6%E9%9B%86%E5%85%B3%E9%94%AE%E5%AD%97%E5%8F%82%E6%95%B0)
      - [函数文档字符串](#%E5%87%BD%E6%95%B0%E6%96%87%E6%A1%A3%E5%AD%97%E7%AC%A6%E4%B8%B2)
        - [定义](#%E5%AE%9A%E4%B9%89)
        - [输出](#%E8%BE%93%E5%87%BA)
      - [内部函数](#%E5%86%85%E9%83%A8%E5%87%BD%E6%95%B0)
      - [闭包](#%E9%97%AD%E5%8C%85)
      - [匿名函数：lambda()](#%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0%EF%BC%9Alambda)
    - [生成器](#%E7%94%9F%E6%88%90%E5%99%A8)
      - [可迭代对象](#%E5%8F%AF%E8%BF%AD%E4%BB%A3%E5%AF%B9%E8%B1%A1)
      - [生成器函数](#%E7%94%9F%E6%88%90%E5%99%A8%E5%87%BD%E6%95%B0)
    - [装饰器](#%E8%A3%85%E9%A5%B0%E5%99%A8)
    - [模块、包](#%E6%A8%A1%E5%9D%97%E3%80%81%E5%8C%85)
      - [命令行参数](#%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0)
      - [import 语句](#import-%E8%AF%AD%E5%8F%A5)
      - [模块](#%E6%A8%A1%E5%9D%97)
        - [导入整个模块](#%E5%AF%BC%E5%85%A5%E6%95%B4%E4%B8%AA%E6%A8%A1%E5%9D%97)
        - [导入模块的某一部分](#%E5%AF%BC%E5%85%A5%E6%A8%A1%E5%9D%97%E7%9A%84%E6%9F%90%E4%B8%80%E9%83%A8%E5%88%86)
        - [导入后使用别名](#%E5%AF%BC%E5%85%A5%E5%90%8E%E4%BD%BF%E7%94%A8%E5%88%AB%E5%90%8D)
      - [包](#%E5%8C%85)
        - [包结构](#%E5%8C%85%E7%BB%93%E6%9E%84)
        - [`__init__.py`](#initpy)
      - [异常控制](#%E5%BC%82%E5%B8%B8%E6%8E%A7%E5%88%B6)
        - [try、except](#try%E3%80%81except)
        - [自定义异常](#%E8%87%AA%E5%AE%9A%E4%B9%89%E5%BC%82%E5%B8%B8)
  - [面向对象](#%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1)
    - [数据封装](#%E6%95%B0%E6%8D%AE%E5%B0%81%E8%A3%85)
      - [类定义](#%E7%B1%BB%E5%AE%9A%E4%B9%89)
      - [属性](#%E5%B1%9E%E6%80%A7)
        - [实例属性](#%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7)
        - [类属性](#%E7%B1%BB%E5%B1%9E%E6%80%A7)
          - [内置类属性](#%E5%86%85%E7%BD%AE%E7%B1%BB%E5%B1%9E%E6%80%A7)
        - [实例属性和类属性的关系](#%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7%E5%92%8C%E7%B1%BB%E5%B1%9E%E6%80%A7%E7%9A%84%E5%85%B3%E7%B3%BB)
        - [@property](#property)
      - [方法](#%E6%96%B9%E6%B3%95)
        - [实例方法](#%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95)
        - [类方法](#%E7%B1%BB%E6%96%B9%E6%B3%95)
          - [魔法方法](#%E9%AD%94%E6%B3%95%E6%96%B9%E6%B3%95)
        - [静态方法](#%E9%9D%99%E6%80%81%E6%96%B9%E6%B3%95)
        - [实例、静态、类方法辨析](#%E5%AE%9E%E4%BE%8B%E3%80%81%E9%9D%99%E6%80%81%E3%80%81%E7%B1%BB%E6%96%B9%E6%B3%95%E8%BE%A8%E6%9E%90)
      - [访问控制](#%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)
      - [获取类的信息](#%E8%8E%B7%E5%8F%96%E7%B1%BB%E7%9A%84%E4%BF%A1%E6%81%AF)
        - [type()](#type)
        - [isinstance()](#isinstance)
        - [dir()](#dir)
        - [getattr()、setattr()、hasattr()](#getattr%E3%80%81setattr%E3%80%81hasattr)
      - [枚举类](#%E6%9E%9A%E4%B8%BE%E7%B1%BB)
      - [元类](#%E5%85%83%E7%B1%BB)
    - [继承](#%E7%BB%A7%E6%89%BF)
      - [方法重写](#%E6%96%B9%E6%B3%95%E9%87%8D%E5%86%99)
        - [基础重写方法](#%E5%9F%BA%E7%A1%80%E9%87%8D%E5%86%99%E6%96%B9%E6%B3%95)
        - [运算符重载](#%E8%BF%90%E7%AE%97%E7%AC%A6%E9%87%8D%E8%BD%BD)
      - [多重继承](#%E5%A4%9A%E9%87%8D%E7%BB%A7%E6%89%BF)
    - [多态](#%E5%A4%9A%E6%80%81)
      - [“开闭”原则](#%E2%80%9C%E5%BC%80%E9%97%AD%E2%80%9D%E5%8E%9F%E5%88%99)
      - [鸭子类型](#%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B)


# Python 语法

<!--TODO: https://www.zhihu.com/question/29995881/answer/172961766  -->

## 变量

Python 里所有数据——布尔值、整数、浮点数、字符串，甚至大型数据结构、函数以及程序——都是以对象（object） 的形式存在的；

### 变量命名规范

- 变量名只能包含大小写字母、数字、下划线；

- 变量名不允许以数字开头；

- 变量名中以两个下划线开头的变量有特殊意义，是 python 的保留用法，自定义变量不可使用这样的命名；
  例：

  函数名：function.__name__；
  
  函数文档字符串：function.__doc__；
  
  主程序名称：__main__

### 全局、局部变量

为了读取全局变量而不是函数中的局部变量， 需要在变量前面显式地加关键字 global；

- Python 提供了两个获取命名空间内容的函数：
  - locals() 返回一个局部命名空间内容的字典；
  - globals() 返回一个全局命名空间内容的字典。

## 数据类型

### 基本数据类型

Python 是强类型的（strongly typed）（用户无法修改一个已有对象的类型，即使它包含的值是可变的）的动态语言；

#### 布尔值

Python 里最简单的数据类型是布尔型，它只有两个可选值： True 和 False。当转换为整数时，它们分别代表 1 和 0。

下面的情况都会被 python 解释器认为是 False：

| 类型       | 取值     |
| -------- | ------------- |
| 布尔       | 布尔  False     |
| null 类型  | null 类型  None |
| 整型       | 整型  0         |
| 浮点型      | 浮点型  0.0      |
| 空字符串     | 空字符串 ''       |
| 空列表      | 空列表  []       |
| 空元组      | 空元组  ()       |
| 空字典      | 空字典  {}       |
| 空集合  set | 空集合  set()    |

#### 数字

##### int

###### 大小

在 Python 3 中， long 类型已不复存在，int 类型变为可以存储任意大小的整数，甚至超过 64 位；

###### 基数

在 Python 中，整数默认使用十进制数（以 10 为底），除非你在数字前添加前缀，显式地指定使用其他基数（base）。

在 Python 中，除十进制外你还可以使用其他三种进制的数字：
- 0b 或 0B 代表二进制（以 2 为底）
- 0o 或 0O 代表八进制（以 8 为底）
- 0x 或 0X 代表十六进制（以 16 为底）

Python 解释器会打印出它们对应的十进制整数。

##### float

整数全部由数字组成，而浮点数（在 Python 里称为 float）包含非数字的小数点。

#### 字符串

对 Unicode 的支持使得 Python 3 可以包含世界上任何书面语言以及许多特殊符号；字符串的本质是字符序列；

**Python 字符串是不可变的。你无法对原字符串进行修改，但可以将字符串的一部分复制到新字符串，来达到相同的修改效果；**

##### 创建

将一系列字符包裹在一对单引号或一对双引号中即可创建字符串，无论使用哪种引号， Python 对字符串的处理方式都是一样的，没有任何区别；

三元引号在创建短字符串时没有什么特殊用处。它多用于创建多行字符串；

##### 输出

`print()` 会**把包裹字符串的引号截去**，仅输出其实际内容，易于阅读。它还会**自动地在各个输出部分之间添加空格**，并**在所有输出的最后添加换行符**：
```python
>>> print(99, 'bottles', 'would be enough.')
99 bottles would be enough.
```

##### 使用`+`进行拼接

在 Python 中，你可以使用 `+` 将多个字符串或字符串变量拼接起来
```python
>>> 'Release the kraken! ' + 'At once!'
'Release the kraken! At once!‘
```
也可以直接将一个字面字符串（非字符串变量）放到另一个的后面直接实现拼接：
```python
>>> "My word! " "A gentleman caller!"
'My word! A gentleman caller!'
```
进行字符串拼接时， Python 并不会自动为你添加空格，需要显示定义（调用 print() 进行打印时， Python 会在各个参数之间自动添加空格并在结尾添加换行符）。

##### 使用`*`进行复制

```python
>>> start = 'Na ' * 4 + '\n'
>>> middle = 'Hey ' * 3 + '\n'
>>> end = 'Goodbye.'
>>> print(start + start + middle + end)
Na Na Na Na
Hey Hey Hey
Goodbye.
```
##### 使用`[]`提取字符

在字符串名后面添加 []，并在括号里指定偏移量可以提取该位置的单个字符。 

- 第一个字符（最左侧）的偏移量为 0，下一个是 1，以此类推。 

- 最后一个字符（最右侧）的偏移量也可以用 -1 表示，这样就不必从头数到尾。偏移量从右到左紧接着为 -2、 -3，以此类推。

```python
>>> letters = 'abcdefghijklmnopqrstuvwxyz'
>>> letters[0]
'a'
>>> letters[1]
'b
>>> letters[-1]
'z'
>>> letters[-2]
'y'
```
由于字符串是不可变的， 因此你无法直接插入字符或改变指定位置的字符，也就是说所有提取的字符都是只读的：
```python
>>> name = 'Henny'
>>> naume[0] = 'P'
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

为了达到改变字符串的效果，需要组合使用一些字符串函数，例如 replace()，以及分片操作
```python
>>> name = 'Henny'
>>> name.replace('H', 'P')
'Penny'
>>> 'P' + name[1:]
'Penny'
```

##### 使用`[start:end:step]`分片

分片操作（slice） 可以从一个字符串中抽取子字符串（字符串的一部分）。我们使用一对方括号、起始偏移量 start、终止偏移量 end 以及可选的步长 step 来定义一个分片。其中一些可以省略。分片得到的子串包含从 start 开始到 end 之前的全部字符。

- `[:]` 提取从开头到结尾的整个字符串

- `[start:]` 从 start 提取到结尾

- `[:end]` 从开头提取到 end - 1

- `[start:end]` 从 start 提取到 end - 1

- `[start:end:step]` 从 start 提取到 end - 1，每 step 个字符提取一个

##### 使用 len() 获取长度

len() 函数可用于计算字符串（以及其它序列类型）包含的字符数：
```python
>>> len(letters)
26
>>> empty = ""
>>> len(empty)
0
```

如果你调用 len() 函数试图获取一个对象的长度，实际上，在 len() 函数内部，它自动去调用该对象的__len__() 方法，因此，对于自己写的类，如果也想用 len(myObj) 的话，就实现一个__len__() 方法即可，例：
```python
>>> class MyDog(object):
...     def __len__(self):
...         return 100
...
>>> dog = MyDog()
>>> len(dog)
100
```

##### 使用字符串方法

**字符串方法需要通过字符串调用。**

###### 使用 split() 分割

与广义函数 len() 不同，split 函数只适用于字符串类型。

使用 split() 可以基于分隔符将字符串分割成由若干子串组成的列表。
```python
>>> todos = 'get gloves,get mask,give cat vitamins,call ambulance'
>>> todos.split(',')
['get gloves', 'get mask', 'give cat vitamins', 'call ambulance']
```
如果不指定分隔符，那么 split() 将默认使用空白字符——换行符、空格、制表符。

###### 使用 join() 合并

join() 函数与 split() 函数正好相反：它将包含若干子串的列表分
```python
>>> crypto_list = ['Yeti', 'Bigfoot', 'Loch Ness Monster']
>>> crypto_string = ', '.join(crypto_list) #join 函数是字符串方法，需要通过字符串调用
>>> print('Found and signing book deals:', crypto_string)
Found and signing book deals: Yeti, Bigfoot, Loch Ness Monster 解，并将这些子串合成一个完整的大的字符串。
```

###### replace()

使用 replace() 函数可以进行简单的子串替换。你需要传入的参数包括：需要被替换的子串，用于替换的新子串， 以及需要替换多少处。最后一个参数如果省略则默认只替换第一次出现的位置：
```python
>>> setup.replace('duck', 'marmoset')
'a marmoset goes into a bar...'
```
修改最多 100 处：
```python
>>> setup.replace('a ', 'a famous ', 100)
'a famous duck goes into a famous bar...'
```
也可使用正则表达式匹配。

###### strip()

```python
>>> setup = 'a duck goes into a bar...'
>>> setup.strip('.')
'a duck goes into a bar'
```
由于字符串是不可变的，上述方法实际上没有一个对 setup 真正做了修改。它们都仅仅是获取了 setup 的值，进行某些操作后将操作结果赋值给了另一个新的字符串而已。

###### 大小写转换

让字符串首字母变成大写：
```python
>>> setup.capitalize()
'A duck goes into a bar...'
```

让所有单词的开头字母变成大写：
```python
>>> setup.title()
'A Duck Goes Into A Bar...'
```

让所有字母都变成大写：
```python
>>> setup.upper()
'A DUCK GOES INTO A BAR...'
```

将所有字母转换成小写：
```python
>>> setup.lower()
'a duck goes into a bar...'
```

将所有字母的大小写转换：
```python
>>> setup.swapcase()
'a DUCK GOES INTO A BAR...'
```

###### 格式排版

在 30 个字符位居中：
```python
>>> setup.center(30)
' a duck goes into a bar... '
```
左对齐：
```python
>>> setup.ljust(30)
'a duck goes into a bar... '
```
右对齐：
```python
>>> setup.rjust(30)
' a duck goes into a bar...'
```

### 容器类型

#### 列表

字符串、列表、元组是 python 的三大序列结构；

##### 特性

1.	可变；

2.	有序；

3.	允许重复；

4.	元素类型可不同；

##### 创建

列表可以由零个或多个元素组成，元素之间用逗号分开，整个列表被方括号所包裹：
```python
>>> empty_list = [ ]
>>> weekdays = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
>>> big_birds = ['emu', 'ostrich', 'cassowary']
>>> first_names = ['Graham', 'John', 'Terry', 'Terry', 'Michael']
```
也可以使用 list() 函数来创建一个空列表：
```python
>>> another_empty_list = list()
>>> another_empty_list
[]
```

##### 使用 [offset] 获取、修改元素

和字符串一样，通过偏移量可以从列表中提取对应位置的元素：
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo']
>>> marxes[0]
'Groucho'
>>> marxes[2] = 'Wanda'
>>> marxes
['Groucho', 'Chico', 'Wanda']
```

##### 使用切片提取元素

列表的切片仍然是一个列表。
```python
>>> marxes = ['Groucho', 'Chico,' 'Harpo']
>>> marxes[0:2]
['Groucho', 'Chico']
```
从列表的开头开始每 2 个提取一个元素：
```python
>>> marxes[::2]
['Groucho', 'Harpo']
```
从尾部开始提取，步长仍为 2：
```python
>>> marxes[::-2]
['Harpo', 'Groucho']
```
实现列表逆序：
```python
>>> marxes[::-1]
['Harpo', 'Chico', 'Groucho']
```

##### 使用 len() 获取长度

##### 使用 in 判断值是否存在

判断一个值是否存在于给定的列表中有许多方式，其中最具有 Python 风格的是使用 in：
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
>>> 'Groucho' in marxes
True
>>> 'Bob' in marxes
False
```

##### 使用 del 删除指定位置的元素

当列表中一个元素被删除后，位于它后面的元素会自动往前移动填补空出的位置，且列表长度减 1。
```python
>>> del marxes[-1]
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']	
```
del 是 Python 语句，是赋值语句（=）的逆过程：它将一个 Python 对象与它的名字分离。如果这个对象无其他名称引用，则其占用空间也被会清除

##### 使用 list 方法

###### 使用 append() 添加元素至尾部
```python
>>> marxes.append('Zeppo')
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Zeppo']
```
###### 使用 extend() 或 += 合并列表

使用 extend() 可以将一个列表合并到另一个列表中。
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
>>> others = ['Gummo', 'Karl']
>>> marxes.extend(others)
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Zeppo', 'Gummo', 'Karl']
>>> marxes += others
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Zeppo', 'Gummo', 'Karl']
```
###### 使用 insert() 在指定位置插入元素
```python
>>> marxes.insert(3, 'Gummo')
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']
```
###### 使用 remove() 删除具有指定值的元素
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']
>>> marxes.remove('Gummo')
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Zeppo']
```
###### 使用 pop() 获取并删除指定位置的元素
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
>>> marxes.pop()
'Zeppo'
>>> marxes
['Groucho', 'Chico', 'Harpo']
>>> marxes.pop(1)
'Chico'
>>> marxes
['Groucho', 'Harpo']
```
###### 使用 index() 查询具有特定值的元素位
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
>>> marxes.index('Chico')
1
```
###### 使用 count() 记录特定值出现的次数
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo']
>>> marxes.count('Harpo')
1
>>> marxes.count('Bob')
0
```
###### 使用 sort() 重新排列元素

列表方法 sort() 会对原列表进行排序，改变原列表内容；

此外，python 还提供了通用函数 sorted()，会返回排好序的列表副本，原列表内容不变

如果列表中的元素都是数字， 它们会默认地被排列成从小到大的升序。如果元素都是字符串，则会按照字母表顺序排列：

```python
>>> marxes = ['Groucho', 'Chico', 'Harpo']
>>> sorted_marxes = sorted(marxes)
>>> sorted_marxes
['Chico', 'Groucho', 'Harpo']
```
有些时候甚至多种类型也可——例如整型和浮点型——只要它们之间能够自动地互相转换：
```python
>>> numbers = [2, 1, 4.0, 3]
>>> numbers.sort()
>>> numbers
[1, 2, 3, 4.0]
```
默认的排序是升序的，通过添加参数 reverse=True 可以改变为降序排列

###### 使用 = 赋值， 使用 copy() 复制

#### 元组

##### 特性

1.	不可变；

2.	有序；

3.	允许重复；

4.	元素类型可不同；

##### 创建

- 使用 () 创建一个空元组：
  ```python
  >>> empty_tuple = ()
  >>> empty_tuple
  ()
  ```

- 使用后缀逗号创建非空元组：
  
  创建包含一个或多个元素的元组时，每一个元素后面都需要跟着一个逗号，即使只包含一个元素也不能省略：
  ```python
  >>> one_marx = 'Groucho',
  >>> one_marx
  ('Groucho',)
  ```
  如果创建的元组所包含的元素数量超过 1，最后一个元素后面的逗号可以省略：
  ```python
  >>> marx_tuple = 'Groucho', 'Chico', 'Harpo'
  >>> marx_tuple
  ('Groucho', 'Chico', 'Harpo')
  ```
  Python 的交互式解释器输出元组时会自动添加一对圆括号，定义元组真正靠的是每个元素的后缀逗号而不是括号——但使用括号可以使程序更加清晰；

##### 元组解包

```python
>>> marx_tuple = ('Groucho', 'Chico', 'Harpo')
>>> a, b, c = marx_tuple
>>> a
'Groucho'
>>> b
'Chico'
>>> c
'Harpo
```
利用元组在一条语句中对多个变量的值进行交换，而不需要借助临时变量：
```python
>>> password = 'swordfish'
>>> icecream = 'tuttifrutti'
>>> password, icecream = icecream, password
>>> password
'tuttifrutti'
>>> icecream
'swordfish'
```

##### 元组与列表的比较

- 相比列表，元组占用的空间较小

- 不会意外修改元组的值；

- 可以将元组用作字典的键；

- 命名元组可以作为对象的替代；

- 函数的参数是以元组形式传递的；

#### 字典

##### 特性

1.	无序性；

2.	键不可变，键值可变；

3.	元素允许重复；

##### 创建

用大括号（{}）将一系列以逗号隔开的键值对（key:value）包裹起来即可进行字典的创建。

- 创建空字典，它不包含任何键值对：
  ```python
  >>> empty_dict = {}
  >>> empty_dict
  {}
  ```

- 创建非空字典：
  ```python
  >>> bierce = {
  ... "day": "A period of twenty-four hours, mostly misspent",
  ... "positive": "Mistaken at the top of one's voice",
  ... "misfortune": "The kind of fortune that never misses",
  ... }
  # Python 允许在列表、元组或字典的最后一个元素后面添加逗号，这不会产生任何问题。
  ```
  在交互式解释器中输入字典名会打印出它所包含的所有键值对：
  ```python
  >>> bierce
  {'misfortune': 'The kind of fortune that never misses',
  'positive': "Mistaken at the top of one's voice",
  'day': 'A period of twenty-four hours, mostly misspent'}
  ```			
  注：字典中元素的顺序是无关紧要的，实际存储顺序可能取决于你添加元素的顺序；

##### 使用 [key] 添加或修改元素 

向字典中添加元素非常简单，只需指定该元素的键并赋予相应的值即可。
- 如果该元素的键已经存在于字典中， 那么该键对应的旧值会被新值取代。
- 如果该元素的键并未在字典中出现，则会被加入字典。
与列表不同，不需要担心赋值过程中 Python 会抛出越界异常。

##### 使用 del 删除具有指定键的元素

```python
>>> del pythons['Marx']
```

##### 使用 in 判断是否存在

用于判断某一个键（**不是键值**）是否存在于一个字典中；

##### 使用 dict 的方法

###### 使用 clear() 删除所有元素

###### 使用 update() 合并字典

使用 update() 可以将一个字典的键值对复制到另一个字典中去。如果待添加的字典与待扩充的字典包含同样的键，新归入字典的值会取代原有的值。

###### 使用 get() 获取键值

get() 方法需要指定字典名，键以及一个可选值。如果键存在，会得到与之对应的值：
```python
>>> pythons.get('Cleese')
'John'
```
若键不存在，且指定了可选值，那么 get() 函数将返回这个可选值：
```python
>>> pythons.get('Marx', 'Not a Python')
'Not a Python'
```
否则，会得到 None（在交互式解释器中什么也不会显示）：
```python
>>> pythons.get('Marx')
>>>
```

###### 使用 keys() 获取所有键

在 Python 3 中，keys() 方法会返回 `dict_keys()`，它是键的迭代形式，需要调用 list() 将 dict_keys 转换为列表类型。
```python
>>> signals = {'green': 'go', 'yellow': 'go faster', 'red': 'smile for the camera'}
>>> signals.keys()
dict_keys(['green', 'red', 'yellow'])
>>> list( signals.keys() )
['green', 'red', 'yellow']
```

###### 使用 values() 获取所有值

```python
>>> list( signals.values() )
['go', 'smile for the camera', 'go faster']
```

###### 使用 items() 获取所有键值对

```python
>>> list( signals.items() )
[('green', 'go'), ('red', 'smile for the camera'), ('yellow', 'go faster')]
```

每一个键值对以元组的形式返回。

###### 使用 = 赋值， 使用 copy() 复制

#### 集合

集合就像舍弃了值， 仅剩下键的字典一样。如果你仅仅想知道某一个元素是否存在而不关心其他的， 使用集合是个非常好的选择。如果需要为键附加其他信息的话，建议使用字典。

##### 特性

1.	无序；

2.	元素可变；

3.	不允许重复；

##### 创建

- 使用 set() 函数创建一个空集合

  ```python
  >>> empty_set = set()
  >>> empty_set
  set()
  # 由于 [] 能创建一个空列表，你可能期望 {} 也能创建空集。但事实上， {} 会创建一个空字典， 这也是为什么交互式解释器把空集输出为 set() 而不是{}
  ```

- 使用大括号创建非空集合
  ```python
  >>> even_numbers = {0, 2, 4, 6, 8}
  >>> even_numbers
  {0, 8, 2, 4, 6}
  ```

##### 使用 in 测试值是否存在

这是集合里最常用的功能。

##### 集合运算操作

###### 交集运算

使用特殊标点符号 & 或者集合函数 intersection() 获取集合的交集（两集合共有元素）
```python
>>> a = {1, 2}
>>> b = {2, 3}并集运算
>>> a & b
{2}
>>> a.intersection(b)
{2}
```

###### 并集运算

使用 | 或者 union() 函数来获取集合的并集（至少出现在一个集合中的元素）：
```python
>>> a | b
{1, 2, 3}
>>> a.union(b)
{1, 2, 3}
```

###### 差集运算

使用字符 - 或者 difference() 可以获得两个集合的差集（出现在第一个集合但不出现在第二个集合）：
```python
>>> a - b
{1}
>>> a.difference(b)
{1}
>>> bruss - wruss
set()
>>> wruss - bruss
{'cream'}
```
###### 异或集运算

使用 ^ 或者 symmetric_difference() 可以获得两个集合的异或集（仅在两个集合中出现一次）：
```python
>> a ^ b
{1, 3}
>>> a.symmetric_difference(b)
{1, 3}
```

###### 子集

使用 <= 或者 issubset() 可以判断一个集合是否是另一个集合的子集（第一个集合的所有元素都出现在第二个集合中）：
```python
>>> a <= b
False
>>> a.issubset(b)
False
```

使用 < 可以判断一个集合是否是另外一个集合的真子集；

###### 超集

使用 >= 或者 issuperset() 可以进行判断一个集合是否是另外一个集合的超集（第二个集合的所有元素都出现在第一个集合中）；
使用 > 可以判断一个集合是否是另外一个集合的真超集；

### 类型转换

#### 自动类型转换

如果混合使用多种不同的数字类型进行计算， Python 会自动地进行类型转换：
```python
>>> 4 + 7.0
11.0
```
与整数或浮点数混合使用时，布尔型的 False 会被当作 0 或 0.0， Ture 会被当作 1 或 1.0：
```python
>>> True + 2
3
>>> False + 5.0
5.0
```

#### 强制类型转换

##### int()

使用 int() 可以将其他类型转换为整型：
当将布尔值转换为整数时，它们分别代表 1 和 0：
```python
>>> int(True)
1
>>> int(False)
0
```
当将浮点数转换为整数时，所有小数点后面的部分会被舍去：
```python
>>> int(98.6)
98
>>> int(1.0e4)
10000
```
将仅包含数字和正负号的字符串转换为整数：
```python
>>> int('99')
99
>>> int('-23')
-23
>>> int('+12')
12
```
但无法接受包含小数点或指数的字符串：（可以通过 flaot() 实现类型转换）
```python
>>> int('98.6')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: '98.6'
>>> int('1.0e4')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: '1.0e4
```
如果将一个与数字无关的类型转化为整数，会得到一个异常：
```python
>>> int('99 bottles of beer on the wall')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: '99 bottles of beer on the wall'
>>> int('')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: ''
```

##### float()

使用 float() 函数可以将其他数字类型转换为浮点型：

布尔型在计算中等价于 1.0 和 0.0：
```python
>>> float(True)
1.0
>>> float(False)
0.0
```
将整数转换为浮点数仅仅需要添加一个小数点：
```python
>>> float(98)
98.0
>>> float('99')
99.0
```
此外，也可以将包含有效浮点数（数字、正负号、小数点、指数及指数的前缀 e）的字符串转换为真正的浮点型数字：
```python
>>> float('98.6')
98.6
>>> float('-1.5')
-1.5
>>> float('1.0e4')
10000.0
```

##### str()

使用 str() 可以将其他 Python 数据类型转换为字符串：
```python
>>> str(98.6)
'98.6'
>>> str(1.0e4)
'10000.0'
>>> str(True)
'True
```
当你调用 print() 函数或者进行字符串差值（string interpolation）时， Python 内部会自动使用 str() 将非字符串对象转换为字符串。

##### list()

list() 函数可以将其他数据类型转换成列表类型：
```python
>>> list('cat')
['c', 'a', 't']
>>> a_tuple = ('ready', 'fire', 'aim')
>>> list(a_tuple)
['ready', 'fire', 'aim']
```

##### tuple()

tuple() 函数可以用其他类型的数据来创建元组：
```python
>>> marx_list = ['Groucho', 'Chico', 'Harpo']
>>> tuple(marx_list)
('Groucho', 'Chico', 'Harpo')
```

##### dict()

使用 dict() 将任何包含双值子序列的序列转换成字典。

（双值子序列：每个子序列的第一个元素作为键，第二个元素作为值）
```python
# 包含双值列表的列表
>>> lol = [ ['a', 'b'], ['c', 'd'], ['e', 'f'] ]
>>> dict(lol)
{'c': 'd', 'a': 'b', 'e': 'f'}
# 包含双值元组的列表
>>> lot = [ ('a', 'b'), ('c', 'd'), ('e', 'f') ]
>>> dict(lot)
{'c': 'd', 'a': 'b', 'e': 'f'}
# 双字符的字符串组成的列表
>>> los = [ 'ab', 'cd', 'ef' ]
>>> dict(los)
{'c': 'd', 'a': 'b', 'e': 'f'}
```

##### set()

使用 set() 将其他类型转换为集合；

可以利用已有列表、字符串、元组或字典的内容来创建集合，其中重复的值会被丢弃。

例：
```python
>>> set( 'letters' )
{'l', 'e', 't', 'r', ' 	s'}
```
当字典作为参数传入 set() 函数时，只有键会被使用：
```python
>>> set( {'apple': 'red', 'orange': 'orange', 'cherry': 'red'} )
{'apple', 'cherry', 'orange'}
```

### 类型判断

#### 基本类型

基本类型可以用 type() 判断：
```python
>>> type(123)
<class 'int'>
>>> type('str')
<class 'str'>
>>> type(None)
<type(None) 'NoneType'>
```
if 语句中，可直接使用 int、str 等关键字：
```python
>>> type(123)==type(456)
True
>>> type(123)==int
True
>>> type('abc')==type('123')
True
>>> type('abc')==str
True
>>> type('abc')==type(123)
False
```

#### 引用类型

如果一个变量指向函数或者类，也可以用 type() 判断：
```python
>>> type(abs)
<class 'builtin_function_or_method'>
>>> type(a)
<class '__main__.Animal'>
```
若要在 if 语句中判断一个对象是否是函数，需要使用 types 模块中定义的常量：
```python
>>> import types
>>> def fn():
...     pass
...
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True
>>> type((x for x in range(10)))==types.GeneratorType
True
```

## 符号

### 运算符

#### 算术运算符

以下假设变量： a=10，b=20

| 运算符    | 描述                         | 实例                                       |
| ------ | -------------------------- | ---------------------------------------- |
| +      | 加 -  两个对象相加                | a + b 输出结果 30                            |
| -      | 减 -  得到负数或是一个数减去另一个数       | a - b 输出结果 -10                           |
| *      | 乘 -  两个数相乘或是返回一个被重复若干次的字符串 | a * b 输出结果 200                           |
| /      | 浮点数除法 - x 除以 y               | b / a 输出结果 2.0；  **“/****“的运算结构都为浮点数；**  |
| %      | 取模  - 返回除法的余数              | b % a 输出结果 0                             |
| **     | 幂 -  返回 x 的 y 次幂               | a**b 为 10 的 20 次方， 输出结果  100000000000000000000 |
| //     | 整数除法 - 返回商的整数部分（向上取整）      | 9//2 输出结果 4 , 9.0//2.0 输出结果 4.0；  **若运算对象包含浮点型，则结果为浮点型，若运算对象都是整形，则结果为整型；··** |
| divmod | 同时取得余数和商                   | divmod(9, 5) 输出结果为 (1, 4)；                 |

#### 布尔 / 关系运算符

返回布尔值。

| 运算符    | 描述                                       | 实例                                       |
| ------ | ---------------------------------------- | ---------------------------------------- |
| ==     | 等于  - 比较对象是否相等                           | (a == b) 返回  False。                      |
| !=     | 不等于  - 比较两个对象是否不相等                       | (a != b) 返回  true.                       |
| <>     | 不等于  - 比较两个对象是否不相等                       | (a <> b) 返回  true。这个运算符类似 != 。           |
| >      | 大于  - 返回 x 是否大于 y                           | (a > b) 返回  False。                       |
| <      | 小于  - 返回 x 是否小于 y。所有比较运算符返回 1 表示真，返回 0 表示假。这分别与特殊的变量 True 和 False 等价。注意，这些变量名的大写。 | (a < b) 返回  true。                        |
| >=     | 大于等于                                     | - 返回 x 是否大于等于 y。                            |
| <=     | 小于等于 -                                   | 返回 x 是否小于等于 y。                              |
| in     | 如果在指定的序列中找到值返回 True，否则返回 False。          | x 在 y  序列中 , 如果 x 在 y 序列中返回 True。        |
| not in | 如果在指定的序列中没有找到值返回 True，否则返回 False。        | x 不在  y 序列中 , 如果 x 不在 y 序列中返回 True。      |
| is     | is 是判断两个标识符是不是引用自一个对象                    | x is y, 类似  id(x) == id(y) , 如果引用的是同一个对象则返回 True，否则返回 False |
| is not | is not 是判断两个标识符是不是引用自不同对象                | x is not y ， 类似  id(a) != id(b)。如果引用的不是同一个对象则返回结果 True，否则返回 False。 |

#### 位运算符

| 运算符  | 描述                                       | 实例                                       |
| ---- | ---------------------------------------- | ---------------------------------------- |
| &    | 按位与运算符：参与运算的两个值，如果两个相应位都为 1, 则该位的结果为 1, 否则为 0 | (a & b) 输出结果 12 ，二进制解释： 0000 1100        |
| \|   | 按位或运算符：只要对应的二个二进位有一个为 1 时，结果位就为 1。          | (a \| b) 输出结果 61 ，二进制解释： 0011 1101       |
| ^    | 按位异或运算符：当两对应的二进位相异时，结果为 1                 | (a ^ b) 输出结果 49 ，二进制解释： 0011 0001        |
| ~    | 按位取反运算符：对数据的每个二进制位取反，即把 1 变为 0, 把 0 变为 1        | (~a ) 输出结果 -61 ，二进制解释： 1100 0011， 在一个有符号二进制数的补码形式。 |
| <<   | 左移动运算符：运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补 0。 | a << 2 输出结果 240 ，二进制解释： 1111 0000        |
| >>   | 右移动运算符：把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数 | a >> 2 输出结果 15 ，二进制解释： 0000 1111         |

#### 逻辑运算符

| 运算符  | 逻辑表达式   | 描述                                       |
| ---- | ------- | ---------------------------------------- |
| and  | x and y | 布尔"与" - 如果 x 为 False，x and y 返回 False，否则它返回 y 的计算值。 |
| or   | x or y  | 布尔"或"                                    |
| not  | not x   | 布尔"非" - 如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。 |

### 其它符号

| 符号                | 说明                                       | 备注                                       |
| ----------------- | ---------------------------------------- | :--------------------------------------- |
| tab               | 缩进                                       | Python 的代码块不使用大括号 **{}** 来控制类，函数以及其他逻辑判断，而是使用缩进来编写模块；  <br>缩进的空白数量是可变的，但是所有代码块语句必须包含相同的缩进空白数量 |
| /                 | 多行连接                                     | Python 语句中一般以新行作为为语句的结束符，但是我们可以使用斜杠（ \）将一行的语句分为多行显示：  <br>例：<br>    ```total = item_one + \          item_two + \          item_three    ```<br>若语句中包含 [], {} 或  () 括号就不需要使用多行连接符：<br>    ```days = ['Monday', 'Tuesday', 'Wednesday',          'Thursday', 'Friday']``` |
| \                 | 转义                                       |                                          |
| 引号                | 表示字符串：单引号 ( ' )、双引号 ( " )、三引号 ( ''' 或  """ ) | 引号的开始与结束必须的相同类型的。  其中三引号可以由多行组成，编写多行文本的快捷语法，常用语文档字符串，在文件的特定地点，被当做注释。<br>    word = 'word'<br>  sentence = "这是一个句子。"<br>  paragraph = """这是一个段落。<br>    包含了多个语句""" |
| #                 | 单行注释                                     | linux 下的 py 文件头：    <br>#!/usr/bin/python  <br># -*- coding: UTF-8 -*-  <br># 文件名：test.py |
| “”“  “”“或‘’‘  ‘’‘ | 多行注释：使用三个单引号 (''') 或三个双引号 (""")             |                                          |

## 代码结构

Python 通过代码缩进来区分代码块结构，避免输入太多的花括号和关键字，使用空白来区分代码结构；

### 流程控制

#### if、 elif 和 else

```python
if color == "red":
print("It's a tomato")
elif color == "green":
print("It's a green pepper")
elif color == "bee purple":
print("I don't know what it is, but only bees can see it")
else:
print("I've never heard of the color", color)
```

#### while、else

```python
# 如果 while 循环正常结束（没有使用 break 跳出），程序将进入到可选的 else 段
>>> while position < len(numbers):
... 	number = numbers[position]
... 	if number % 2 == 0:
... 		print('Found even number', number)
... 		break
... 	position += 1
... else: #没有执行 break
... 	print('No even number found')
```

#### for、else

列表（例如 rabbits）、字符串、元组、字典、集合等都是 Python 中可迭代的对象：

- 元组或者列表在一次迭代过程产生一项；

- 字符串在每一次迭代中会产生一个字符。；

- 对一个字典（或者字典的 keys() 函数）进行迭代将返回字典中的键，如果想对字典的值进行迭代，可以使用字典的 values() 函数，如果需要以元组的形式返回键值对进行迭代，可以使用字典的 items() 函数；

例：
```python
>>> for value in accusation.values():
... 	print(value)
>>> for item in accusation.items():
... 	print(item)
```
类似于 while， for 循环也可以使用可选的 else 代码段，用来判断 for 循环是否正常结束（没有调用 break 跳出），否则会执行 else 段。

例：
```python
>>> for cheese in cheeses:
... 	print('This shop has some lovely', cheese)
... 	break
... else: # 没有 break 表示没有找到奶酪
... 	print('This is not much of a cheese shop, is it?')
```

**注意：当使用 for .. in .. 进行迭代时，python 会将可迭代对象的所有迭代值都读到内存中，再进行迭代，因此，若迭代器包含了海量数据，直接迭代会导致大量内存被占用，此时可采用推导式或者生成器的方式。**

##### 使用 zip() 并行迭代

zip() 函数：

函数返回值既不是元组也不是列表，而是一个整合多个序列的可迭代变量，函数在最短的序列“用完”时就会停止。
```python
>>> english = 'Monday', 'Tuesday', 'Wednesday'
>>> french = 'Lundi', 'Mardi', 'Mercredi‘
>>> list( zip(english, french) )
[('Monday', 'Lundi'), ('Tuesday', 'Mardi'), ('Wednesday', 'Mercredi')]
>>> dict( zip(english, french) )
{'Monday': 'Lundi', 'Tuesday': 'Mardi', 'Wednesday': 'Mercredi'}
```

通过 zip() 函数可以实现对多个序列进行并行迭代：
```python
>>> days = ['Monday', 'Tuesday', 'Wednesday']
>>> fruits = ['banana', 'orange', 'peach']
>>> drinks = ['coffee', 'tea', 'beer']
>>> desserts = ['tiramisu', 'ice cream', 'pie', 'pudding']
>>> for day, fruit, drink, dessert in zip(days, fruits, drinks, desserts):
... print(day, ": drink", drink, "- eat", fruit, "- enjoy", dessert)
...
Monday : drink coffee - eat banana - enjoy tiramisu
Tuesday : drink tea - eat orange - enjoy ice cream
Wednesday : drink beer - eat peach - enjoy pie
```

##### 使用 range() 生成自然数序列

range（start, stop, step）返回在特定区间的自然数可迭代序列，即 start, start+step, start+2*step, ……, stop-1；

参数说明：
- start 的默认值为 0； 
- stop 没有默认值，为必须参数；
- step 的默认值是 1；

例：
```python
>>> for x in range(0,3):
... print(x)
...
0 
1
2
>>> for x in range(2, -1, -1):
... print(x)
...
2
1
0
>>> list( range(0, 11, 2) )
[0, 2, 4, 6, 8, 10]
```

### 推导式

推导式是从一个或者多个迭代器快速简洁地创建数据结构的一种方法。 它可以将循环和条件判断结合， 从而避免语法冗长的代码。使用推导式更符合 Python 的代码风格。

#### 列表推导式

语法：`[expression for item in iterable if condition]`

例 1：
```python
>>> for n in [number*2 for number in range(1,6) if number % 2 == 1]:
...     print(n)
...
2
6
10
```
例 2：
```python
>>> rows = range(1,4)
>>> cols = range(1,3)
>>> cells = [(row, col) for row in rows for col in cols]
>>> for row, col in cells:
... print(row, col)
...
1 1
1 2
2 1
2 2
3 1
3 2
```

#### 字典推导式

语法：`{ key_expression : value_expression for expression in iterable if condition }`

例：对字符串 'letters' 中出现的字母进行循环，计算出每个字母出现的次数
```python
>>> word = 'letters'
>>> letter_counts = {letter: word.count(letter) for letter in set(word)}
>>> letter_counts
{'t': 2, 'l': 1, 'e': 2, 'r': 1, 's': 1}
# 字符串中 t 和 e 都出现了两次，第一次调用 word.count() 时已经计算得到相应的值，利用集合使每个字母只出现一次，避免两次调用 word.count(letter) 浪费时间
```

#### 集合推导式

语法：`{ value_expression for expression in iterable if condition }`

例：
```python
>>> a_set = {number for number in range(1,6) if number % 3 == 1}
>>> a_set
{1, 4}
```

#### 生成器推导式

元组没有推导式，使用圆括号创建的是一个生成器推导式，生成器推导式返回一个生成器对象：
```python
>>> number_thing = (number for number in range(1, 6))
>>> type(number_thing)
<class 'generotor'>
```
可以直接对生成器对象进行迭代：
```python
>>> for number in number_thing:
... print(number)
...
1 
2 
3 
4 
5 
```
一个生成器只能运行一次。列表、集合、字符串和字典都存储在内存中，但是生成器仅在运行中产生值， 不会被存下来，所以不能重新使用或者备份一个生成器。如果想再一次迭代此生成器，会发现它被擦除了：
```python
>>> try_again = list(number_thing)
>>> try_again
[]
```

### 函数

代码复用的第一步是使用函数，它是命名的用于区分的代码段。 函数可以接受任何数字或者其他类型的输入作为参数，并且返回数字或者其他类型的结果；

函数命名规范和变量命名一样（必须使用字母或者下划线 _ 开头，仅能含有字母、数字和下划线）；

参数传递为值传递：当调用含参数的函数时， 这些参数的值会被复制给函数中的对应参数；

一个函数可以接受任何数量（包括 0）的任何类型的值作为输入变量，并且返回任何数量（包括 0）的任何类型的结果。如果函数不显式调用 return 函数，那么会默认返回 None；

#### 参数

##### 位置参数

即按参数的位置顺序依次传递参数值； 

尽管这种方式很常见，但是位置参数的一个弊端是必须熟记每个位置的参数的含义；

例：
```python
>>> def menu(wine, entree, dessert):
... return {'wine': wine, 'entree': entree, 'dessert': dessert}
...
>>> menu('chardonnay', 'chicken', 'cake')
```

##### 关键字参数

为了避免位置参数带来的混乱，调用参数时可以指定对应参数的名字，甚至可以采用与函
数定义不同的顺序调用：
```python
>>> menu(entree='beef', dessert='bagel', wine='bordeaux')
{'dessert': 'bagel', 'wine': 'bordeaux', 'entree': 'beef'}
```
可以把位置参数和关键字参数混合使用，如果同时出现两种参数形式，首先应该考虑的是位置参数：
```python
>>> menu('frontenac', dessert='flan', entree='fish')
```

##### 参数默认值

```python
>>> def menu(wine, entree, dessert='pudding'):
... return {'wine': wine, 'entree': entree, 'dessert': dessert}
```			
注：

默认参数值在函数被定义时已经计算出来，而不是在程序运行时。常见的一个错误是把可变的数据类型（例如列表或者字典）当作默认参数值。

###### 使用函数参数的默认值来实现函数静态变量的功能

Python 中是不支持静态变量的，但是我们可以通过函数的默认值，基于 “Python 中函数默认值的初始化只会被执行一次“ 这一理论，来实现静态变量的功能。

当函数的默认值是内容是可变的类（如列表）时，类的内容可变，而类的名字没变。（相当于开辟的内存区域没有变，而其中内容可以变化）。

例： 
```python
def f(a, L=[]):
    L.append(a)
    return L
 
print f(1)
print f(2)
print f(3)
print f(4,['x'])
print f(5)
输出是：
[1]
[1, 2]
[1, 2, 3]
['x', 4]
[1, 2, 3, 5]
```

为什么最后 “print f(5)”的输出是 “[1, 2, 3, 5]”呢？

这是因为 “print f(4,['x'])”时，默认变量并没有被改变，因为默认变量的初始化只是被执行了一次（第一次使用默认值调用)，初始化执行开辟的内存区（我们可以称之为默认变量）没有被改变，所以最后的输出结果是“[1, 2, 3, 5]”。

##### 使用*和**收集参数

###### 使用*收集位置参数

当参数被用在函数内部时， 星号将一组可变数量的位置参数集合成参数值的元组；
```python
>>> def print_args(*args):
... print('Positional argument tuple:', args)
>>> print_args(3, 2, 1, 'wait!', 'uh...')
Positional argument tuple: (3, 2, 1, 'wait!', 'uh...')
```
若函数同时有限定的位置参数，那么 *args 会收集剩下的参数；

###### 使用**收集关键字参数

使用两个星号可以将参数收集到一个字典中，参数的名字是字典的键，对应参数的值是字典的值。
```python
>>> def print_kwargs(**kwargs):
... print('Keyword arguments:', kwargs)
>>> print_kwargs(wine='merlot', entree='mutton', dessert='macaroon')
Keyword arguments: {'dessert': 'macaroon', 'wine': 'merlot', 'entree': 'mutton'}
```

#### 函数文档字符串

##### 定义
```python
# 可以定义非常长的文档字符串，加上详细的规范说明
def print_if_true(thing, check):
'''
Prints the first argument if a second argument is true.
The operation is:
1. Check whether the *second* argument is true.
2. If it is, print the *first* argument.
'''
if check:
print(thing)
```
##### 输出

1.	使用 help() 可以打印输出一个函数的参数列表和规范的文档，如 help(echo)；

2.	使用 print（函数名。__doc__) 可打印函数的文档字符串；

#### 内部函数

当需要在函数内部多次执行复杂的任务时，内部函数是非常有用的，从而避免了循环和代码的堆叠重复；

例：
```python
>>> def knights(saying):
... 	def inner(quote):
... 		return "We are the knights who say: '%s'" % quote
... 	return inner(saying)
>>> knights('Ni!')
"We are the knights who say: 'Ni!'"
```

#### 闭包

闭包是一个可以由另一个函数动态生成的函数， 并且可以改变和存储函数外创建的变量的值。

例：
```python
>>> def knights2(saying):
... 	def inner2():
... 		return "We are the knights who say: '%s'" % saying
... 	return inner2 
... 	# knights2() 返回的是 inner2 函数的复制（没有直接调用）。所以它就是一个闭包：一个被动态创建的可以记录外部变量的函数；
...
```
用不同的参数调用 knights2()，分别赋值给 a 和 b：
```python
>>> a = knights2('Duck')
>>> b = knights2('Hasenpfeffer')
>>> type(a)
<class 'function'>
>>> type(b)
<class 'function'>
```
它们是函数，同时也是闭包：
```python
>>> a
<function knights2.<locals>.inner2 at 0x10193e158>
>>> b
<function knights2.<locals>.inner2 at 0x10193e1e0>
```
如果调用它们，它们会记录被 knights2 函数创建时的外部变量参数 saying：
```python
>>> a()
"We are the knights who say: 'Duck'"
>>> b()
"We are the knights who say: 'Hasenpfeffer'"
```

#### 匿名函数：lambda()

Python 中， lambda 函数是用一个语句表达的匿名函数。可以用它来代替小的函数，常用于简化回调函数的定义；

例：
```python
>>> def edit_story(words, func):
... for word in words:
... print(func(word))
>>> stairs = ['thud', 'meow', 'thud', 'hiss']
```
调用 edit_story 函数的普通写法：
```python
>>> def enliven(word): # 让这些单词更有情感
... return word.capitalize() + '!'
>>> edit_story(stairs, enliven)
Thud!
Meow!
Thud!
Hiss!
```
调用 edit_story 函数的 lambda 函数写法：
```python
# lambda 函数接收一个参数 word，在冒号和末尾圆括号之间的部分为函数的定义；
# 语法： lambda 参数名：返回值表达式
>>> edit_story(stairs, lambda word: word.capitalize() + '!')
Thud!
Meow!
Thud!
Hiss!
```

### 生成器

参考文章：

https://pyzh.readthedocs.io/en/latest/the-python-yield-keyword-explained.html 

http://www.jianshu.com/p/d09778f4e055 

#### 可迭代对象

当你建立了一个列表，你可以逐项地读取这个列表，这叫做一个可迭代对象：
例：mylist 就是一个可迭代的对象
```python
>>> mylist = [1, 2, 3]
>>> for i in mylist :
...    print(i)
1
2
3
```
可迭代对象也可以使用列表推导式生成：
```python
>>> mylist = [x*x for x in range(3)]
>>> for i in mylist :
...    print(i)
0
1
4
```

生成器是用来创建 Python 可迭代对象的一个对象。使用它可以迭代庞大的序列，且不需要在内存中创建和存储整个序列。通常，生成器是为迭代器产生数据的，但它只能被读取一次，因为它并不把所有的值放在内存中，而是实时地生成数据；

例：
```python
>>> mygenerator = (x*x for x in range(3))
>>> for i in mygenerator :
...    print(i)
0
1
4
```
上边的代码执行完毕之后，不可以再次使用 for i in mygenerator , 因为生成器只能被使用一次；

每次迭代生成器时，它会记录上一次调用的位置，并且返回下一个值。这一点和普通的函数是不一样的，一般函数都不记录前一次调用，而且都会在函数的第一行开始执行；

#### 生成器函数

当需要创建一个比较大的可迭代对象时，若使用推导式的方式会导致代码很繁琐，这时就可以将序列的生成写成一个生成器函数，生成器函数和普通函数类似，但是它的返回值使用 yield 语句声明而不是 return，使用 yield 关键字的函数不再是普通函数，它返回一个生成器；

当迭代器对生成器函数进行迭代时，每迭代一次遇到 yield 时就返回 yield 后面的值作为迭代返回值。重点是：下一次迭代时，从上一次迭代遇到的 yield 后面的代码继续开始执行。直到将生成器函数执行完毕，生成器会返回一个异常，告知迭代器迭代结束；

带有 yield 的函数不仅仅只用于 for 循环中，而且可用于某个函数的参数，只要这个函数的参数允许迭代参数。比如 array.extend 函数，它的原型是 array.extend(iterable)

例：重写一个 range() 生成器
```python
>>> def my_range(first=0, last=10, step=1):
... 	number = first
... 	while number < last:
... 		yield number
... 	number += step
```
函数返回的是一个生成器对象
```python
>>> ranger = my_range(1, 5)
>>> ranger
<generator object my_range at 0x101a0a168>
```
使用
```python
>>> for x in ranger:
... print(x)
...
1
2
4
```
例：理解的关键在于：下次迭代时，代码从 yield 的下一跳语句开始执行
```python
def yield_test(n):  
    for i in range(n):  
        yield call(i)  
        print("i=",i)  
    #做一些其它的事情      
    print("do something.")      
    print("end.")  

def call(i):  
    return i*2  

#使用 for 循环  
for i in yield_test(5):  
    print(i,",")
```
执行结果：
```python
>>>   
0 ,  
i= 0  
2 ,  
i= 1  
4 ,  
i= 2  
6 ,  
i= 3  
8 ,  
i= 4  
do something.  
end.  
>>>

```

### 装饰器

TODO:

### 模块、包

#### 命令行参数

```python
import sys
print(‘program arguments: ’, sys.argv)
```
命令行中：
```shell
$ python test2.py
Program arguments: ['test2.py']
$ python test2.py tra la la
Program arguments: ['test2.py', 'tra', 'la', 'la']
```

#### import 语句

- import 语句使用形式：

  1)	`import subpackage1.a # 将模块 subpackage.a 导入全局命名空间，例如访问 a 中属性时用 subpackage1.a.attr`

  2)	`from subpackage1 import a #　将模块 a 导入全局命名空间，例如访问 a 中属性时用 a.attr_a`

  3)	`from subpackage.a import attr_a # 将模块 a 的属性直接导入到命名空间中，例如访问 a 中属性时直接用 attr_a`

  注：使用 from 语句可以把模块直接导入当前命名空间，from 语句并不引用导入对象的命名空间，而是将被导入对象直接引入当前命名空间；

- 引用机制：可以被 import 语句导入的对象是以下类型：

  - 模块文件（.py 文件）

  - C 或 C++ 扩展（已编译为共享库或 DLL 文件）

  - 包（包含多个模块）

  - 内建模块（使用 C 编写并已链接到 Python 解释器中）
 	

- 模块通常为单独的。py 文件，可以用 import 直接引用，可以作为模块的文件类型有。py、.pyo、.pyc、.pyd、.so、.dll；
 	
- import 语句可以在程序的任何位置使用，你可以在程序中多次导入同一个模块，但模块中的代码仅仅在该模块被首次导入时执行。后面的 import 语句只是简单的创建一个到模块名字空间的引用而已；
 	
- sys.modules 字典中保存着所有被导入模块的模块名到模块对象的映射；

- 当导入模块时，解释器按照 sys.path 列表中的目录顺序来查找导入文件（标准 sys 模块下的一系列目录名和 ZIP 压缩文件）：
  ```python
  import sys
  >>> print(sys.path)

  # Linux:
  ['', '/usr/local/lib/python3.4',
  '/usr/local/lib/python3.4/plat-sunos5',
  '/usr/local/lib/python3.4/lib-tk',
  '/usr/local/lib/python3.4/lib-dynload',
  '/usr/local/lib/python3.4/site-packages']

  # Windows:
  ['', 'C:\\WINDOWS\\system32\\python34.zip', 'C:\\Documents and Settings\\weizhong', 'C:\\Python34\\DLLs', 'C:\\Python34\\lib', 'C:\\Python34\\lib\\plat-win', 'C:\\Python34\\lib\\lib-tk', 'C:\\Python34\\Lib\\site-packages\\pythonwin', 'C:\\Python34', 'C:\\Python34\\lib\\site-packages', 'C:\\Python34\\lib\\site-packages\\win32', 'C:\\Python34\\lib\\site-packages\\win32\\lib', 'C:\\Python34\\lib\\site-packages\\wx-2.6-msw-unicode']

  # 其中 list 第一个元素空字符串代表当前目录
  ```

- 在导入模块后，解释器做以下工作：
  
  1)	已导入模块的名称创建新的命名空间，通过该命名空间就可以访问导入模块的属性和方法。
  
  2)	在新创建的命名空间中执行源代码文件。
  
  3)	创建一个名为源代码文件的对象，该对象引用模块的名字空间，这样就可以通过这个对象访问模块中的函数及变量

#### 模块

模块可以在多个文件之间创建和使用 Python 代码；

一个模块就是一个 py 文件，模块名是不带 .py 扩展的另外一个 Python 文件的文件名；

如果你想要保证实例的唯一性， 使用模块是最好的选择。不管模块在程序中被引用多少次，始终只有一个实例被加载。（可以把 Python 模块理解为 Java/C++ 中的单例）；

##### 导入整个模块

导入整个模块后，使用模块内对象只需加上模块名作为前缀即可；

例：
```python
def get_description(): #看到下面的文档字符串了吗？
"""Return random weather, just like the pros"""
from random import choice
possibilities = ['rain', 'snow', 'sleet', 'fog', 'sun', 'who knows']
return choice(possibilities)

import report
description = report.get_description()
print("Today's weather:", description)
```

##### 导入模块的某一部分

直接导入模块的某一部分（如函数、类）后，可直接使用，不用前缀；
```python
from random import choice
possibilities = ['rain', 'snow', 'sleet', 'fog', 'sun', 'who knows']
return choice(possibilities)
```

##### 导入后使用别名

```python
import report as wr
wr.get_description()
```

#### 包

为了使 Python 应用更具可扩展性，你可以把多个模块组织成文件层次，称之为包。

##### 包结构

多个相关联的模块组成一个包，以便于维护和使用，同时能有限的避免命名空间的冲突。一般来说，包的结构可以是这样的：

```
package
  |- subpackage1
      |- __init__.py
      |- a.py
  |- subpackage2
      |- __init__.py
      |- b.py
```

##### `__init__.py`

`__init__.py` 文件的作用是将文件夹变为一个 Python 模块，Python 中的每个模块的包中，都有`__init__.py` 文件。我们在导入一个包时，实际上是导入了它的`__init__.py`文件。

通常`__init__.py` 文件为空，但是我们还可以为它增加其他的功能：

- 批量导入模块

  可以在__init__.py 文件中批量导入我们所需要的模块，而不再需要一个一个的导入
  ```python
  # package
  # __init__.py
  import re
  import urllib
  import sys
  import os

  # a.py
  import package 
  print(package.re, package.urllib, package.sys, package.os)
  ```

- 定义`__all__`变量
  ```python
  # __init__.py
  __all__ = ['os', 'sys', 're', 'urllib']

  # a.py
  # 会把注册在__init__.py 文件中__all__列表中的模块和包导入到当前文件中来
  from package import *
  ```

例：

1.	在 sources 目录下新建 weekly.py 和 daily.py：

      boxes/sources/daily.py：
      ```python
      def forecast():
        'fake daily forecast'
        return 'like yesterday
      ```
      boxes/sources/weekly.py：
      ```python
      def forecast():
        """Fake weekly forecast"""
        return ['snow', 'more snow', 'sleet', 'freezing rain', 'rain', 'fog', 'hail']
      ```

2.	在 sources 目录下添加一个文件： init.py。这个文件可以是空的，但是 Python 需要它，以便把该目录作为一个包；

3.	在主程序中使用包：

      boxes/weather.py：
      ```python
      from sources import daily, weekly

      print("Daily forecast:", daily.forecast())
      print("Weekly forecast:")
      for number, outlook in enumerate(weekly.forecast(), 1):
        print(number, outlook)
      ```

#### 异常控制

Python 使用异常，它是一段只有错误发生时执行的代码。

##### try、except

例：
```python
try:
  position = int(value)
  print(short_list[position])
except IndexError as err:
  print('Bad index:', position)
except Exception as other:
  print('Something else broke:', other)
```

##### 自定义异常

一个异常是一个类，即类 Exception 的一个子类

例：编写异常 UppercaseException，在一个字符串中碰到大写字母会被抛出。
```python
>>> class UppercaseException(Exception):
... pass
...
# 即使没有定义 UppercaseException 的行为（注意到只使用 pass），也可以通过继承其父类 Exception 在抛出异常时输出错误提示
>>> words = ['eeenie', 'meenie', 'miny', 'MO']
>>> for word in words:
... 	if word.isupper():
... 		raise UppercaseException(word)
...
Traceback (most recent call last):
File "<stdin>", line 3, in <module>
__main__.UppercaseException: MO
```
访问异常对象本身，并且输出它：
```python
>>> try:
... 	raise OopsException('panic')
... except OopsException as exc:
... 	print(exc)
...
panic
```

## 面向对象

### 数据封装

#### 类定义

类 (Class): 用来描述具有相同的属性和方法的对象的集合。它定义了该集合中每个对象所共有的属性和方法。对象是类的实例。

在 Python 中，可以通过 class 关键字定义自己的类，class 之后为类的名称并以冒号结尾，然后通过自定义的类对象类创建实例对象。
```python
class ClassName:
   ‘类的帮助信息‘   #类文档字符串
   class_suite  #类体
```

- 使用点 (.) 来访问对象的属性和方法。

- 创建实例是通过类名 +() 实现的；

#### 属性

例：
```python
class Student(object):
    count = 0
    books = []
    def __init__(self, name, age):
        self.__name = name
        self.__age = age
pass
```
通过内建函数 dir()，或者访问类的字典属性__dict__，这两种方式都可以查看类有哪些属性；

##### 实例属性

在上述例子中，`self.__name`和`self.__age`属于实例属性；

实例数据属性只能通过实例访问，在类定义使用 self （相当于 Java、cpp 中的 this)，指向当前创建的类实例代表当前的实例；

创建类实例后，可以通过实例名动态地给一个实例变量绑定任何属性（即便这个属性在类定义中没有出现），但是这些添加的实例数据属性只属于该实例。对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的变量名称都可能不同；

通过定义 `__init__(self, xxx, xxx)` 方法，可以强制使得在创建类实例的时候就绑定一些属性，有了`__init__`方法，在创建实例的时候，就必须传入与__init__方法匹配的参数，但 self 不需要传，Python 解释器自己会把实例变量传进去；

##### 类属性

在上述例子中，count、books 都属于类属性；

类数据属性属于类本身，可以通过类名、实例名进行访问 / 修改（虽然通过实例可以访问类属性，但是，不建议这么做，最好还是通过类名来访问类属性，从而避免属性隐藏带来的不必要麻烦，例如下边的例子）；

在类定义之后，可以通过类名动态添加类数据属性，新增的类属性也被类和所有实例共有；

需要注意：

- 实例属性的优先级高于类属性，即 python 解释器会优先查找实例属性，找不到所需属性时再查找类属性，因此，当有实例属性与类属性同名且用户通过实例名调用属性时，类属性会被实例属性 “覆盖”；

例：
```python
>>> class Student(object):
...     name = 'Student'
...
>>> s = Student() # 创建实例 s
>>> print(s.name) # 打印 name 属性，因为实例并没有 name 属性，所以会继续查找 class 的 name 属性
Student
>>> print(Student.name) # 打印类的 name 属性
Student
>>> s.name = 'Michael' # 给实例绑定 name 属性
>>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的 name 属性
Michael
>>> print(Student.name) # 但是类属性并未消失，用 Student.name 仍然可以访问
Student
>>> del s.name # 如果删除实例的 name 属性
>>> print(s.name) # 再次调用 s.name，由于实例的 name 属性没有找到，类的 name 属性就显示出来了
Student
```

###### 内置类属性

对于所有的类，都有一组特殊的属性：

| 类属性        | 含义                                       |
| ---------- | ---------------------------------------- |
| __name__   | 类的名字                                     |
| __doc__    | 类的文档字符串                                  |
| __bases__  | 类的所有父类组成的元组                              |
| __dict__   | 类的属性组成的字典                                |
| __module__ | 类所属的模块                                   |
| __class__  | 类对象的类型                                   |
| __slots__  | 限制类的动态绑定范围；  如：__slots__  = ('name', 'age')  用 tuple 定义允许绑定的属性名称 |

- 使用`__slots__`

  为了限制动态绑定，Python 允许在定义 class 的时候，定义一个特殊的__slots__变量，来限制该 class 实例能添加的属性；
  
  例：
  ```python
  class Student(object):
      __slots__ = ('name', 'age') # 用 tuple 定义允许绑定的属性名称
  >>> s = Student() # 创建新的实例
  >>> s.name = 'Michael' # 绑定属性'name'
  >>> s.age = 25 # 绑定属性'age'
  >>> s.score = 99 # 绑定属性'score'
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  AttributeError: 'Student' object has no attribute 'score'
  ```
  
  注：

  `__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的，除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`；

- 关于`if __name__ == '__main__'`: 的作用解析：
  
  例：
  Test.py
  ```python
  class Test:
  def __init(self):
  pass
  def f(self):
  print 'Hello, World!'

  if __name__ == '__main__':
      Test().f()
  ```
  直接运行 python 文件时，__name__ == '__main__'
  ```
  C:> python Test.py
  Hello, World!
  ```
  使用 import 导入 python 模块后，运行该模块时，__name__ == 模块名
  ```
  C:>python
  >>>import Test
  >>>Test.__name__                #Test 模块的__name__
  'Test'
  >>>__name__                       #当前程序的__name__
  '__main__'
  ```
  从而通过 if __name__ == '__main__'来判断是否是在直接运行该。py 文件，即若该文件作为脚本入口被 python 直接运行时，执行 if __name__ == '__main__' 中的代码；若该文件被视为模块导入到其它 py 文件时，不执行 if __name__ == '__main__' 中的代码；

##### 实例属性和类属性的关系

http://blog.csdn.net/cc7756789w/article/details/46480829 

**每个类的实例属性互不相同，因此每个类的每个实例属性都占有一块属于自己的内存空间；**

**而当一个类定义了一个类属性，该属性在内存中只占有一块空间，无论创建了多少个实例，每个实例的该属性都是这块内存空间的引用而已；直到某个类实例对该类的某个类属性进行了重新赋值的操作，则 python 就会为该实例创建一块新的内存空间，即原类属性的副本（初始值相同），然后再执行赋值操作，也就是说，执行完赋值操作后实例的类属性事实上已经与原来的类属性脱离了关系，相当于转换成了实例属性；**

验证：
```python
class AAA():  
    aaa = 10  
  
obj1 = AAA()  
obj2 = AAA()  
print obj1.aaa, obj2.aaa, AAA.aaa  
print obj1.__dict__, obj2.__dict__, AAA.__dict__  
  
obj1.aaa += 2  
print obj1.aaa, obj2.aaa, AAA.aaa   
print obj1.__dict__, obj2.__dict__, AAA.__dict__  
  
AAA.aaa += 3  
print obj1.aaa, obj2.aaa, AAA.aaa  
print obj1.__dict__, obj2.__dict__, AAA.__dict__  
```
执行结果：
```python
10 10 10
{} {} {'__module__': '__main__', 'aaa': 10, '__doc__': None}
12 10 10
{'aaa': 12} {} {'__module__': '__main__', 'aaa': 10, '__doc__': None}
12 13 13
{'aaa': 12} {} {'__module__': '__main__', 'aaa': 13, '__doc__': None}
```

因此，好的习惯是：

- 尽量把需要用户传入的属性作为实例属性，而把同类都一样的属性作为类属性。实例属性在每创造一个实例时都会初始化一遍。不同的实例的实例属性可能不同，不同实例的类属性都相同，从而节省内存的使用；

- 实例属性最好在 `__init__()` 中初始化（每次创建实例都会调用），内部调用时都需要加上 self，外部调用时用`instancename.propertyname`；

  类属性最好在 `__init__()` 外初始化（不会每次创建实例都调用），在内部用 classname. 类属性名调用，外部用 classname. 类属性名调用（虽然既可以用 classname. 类属性名又可以用 instancename. 类属性名来调用）；

##### @property

@property 装饰器可以为属性创建 setter 和 getter 方法；

例：
```python
class Student(object):
@property
# 绑定一个实例属性 score，同时为其添加了 getter 方法
    def score(self):
        return self.__score
@score.setter
# 使用 @property 后，@property 本身又创建了另一个装饰器 @score.setter，负责创建一个 setter 方法
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self.__score = value
```
使用
```python
>>> s = Student()
>>> s.score = 60 # 实际转化为 s.set_score(60)
>>> s.score # 实际转化为 s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```
若只定义 getter 方法，不定义 setter 方法就是设置一个只读属性；

#### 方法

##### 实例方法

实例方法是类中最常用的方法类型；

和普通的函数相比，实例方法只有一点不同：第一个参数必须是 self，self 类似于 C++ 中的"this"，代表的是类的实例，代表当前对象的地址，并且在调用时不用传递该参数；

**实例方法只能通过类实例进行调用**，这时候"self"就代表这个类实例本身。通过"self"可以直接访问实例的属性；

例：
```python
class Student(object):
    '''
    this is a Student class
    '''
    count = 0
    books = []
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    def printInstanceInfo(self):
        print("%s is %d years old" % (self.name, self.age))
    pass
    
wilber = Student("Wilber", 28)
wilber.printInstanceInfo()
```

注：

- **self 不是 python 关键字，事实上可以将其换成任何字符串：**
  ```python
  class Test:
      def prt(xx):
          print(xx)
          print(xx.__class__)

  t = Test()
  t.prt()
  ```

- **类似实例属性可以被动态绑定，python 支持为类的实例动态绑定实例方法，且该方法只对该实例有效；**
  
  例：
  ```python
  >>> def set_age(self, age): # 定义一个函数作为实例方法
  ...     self.age = age
  ...
  >>> from types import MethodType
  >>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
  >>> s.set_age(25) # 调用实例方法
  >>> s.age # 测试结果
  25
  ```

##### 类方法

类方法定义时使用装饰器 @classmethod；

类方法是只在类中运行而不在实例中运行的方法；

类方法以 cls 作为第一个参数，cls 表示类本身，通过 cls 来访问类的相关属性；

类方法的参数不能传入 self，因此不能访问实例属性； 

类方法可以通过类名访问，也可以通过实例访问；

- 使用场景？意义何在？

  与类属性一样，类方法是类固有的方法与属性，不会因为实例不同而改变，不会因为创建多个实例而在内存中创建多个副本，其意义和目的是减少创建多个实例时所创造出来的内存空间，加快运行速度。

例：
```python
class Student(object):
    '''
    this is a Student class
    '''
    count = 0
    books = []
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    @classmethod
    def printClassInfo(cls):
        print(cls.__name__)
        print(dir(cls))
    pass
    
Student.printClassInfo()    
wilber = Student("Wilber", 28)
wilber.printClassInfo()
```

类似类属性可以被动态绑定，python 支持动态绑定类方法，该方法对该类的所有实例有效；
```python
>>> def set_score(self, score):
...     self.score = score
...
>>> Student.set_score = set_score
```
给 class 绑定方法后，所有实例均可调用：
```python
>>> s.set_score(100)
>>> s.score
100
>>> s2.set_score(99)
>>> s2.score
99
```

###### 魔法方法

命名为 __xxx__() 形式的类方法在 Python 中都是有特殊用途的，因此也被称为 “魔法方法”；

- `__len__()`
  
  `__len__`方法返回长度。在 Python 中，如果你调用 len() 函数试图获取一个对象的长度，实际上，在 len() 函数内部，它自动去调用该对象的`__len__()`方法，所以，下面的代码是等价的：
  ```python
  >>> len('ABC')
  3
  >>> 'ABC'.__len__()
  3
  ```
  我们自己写的类，如果也想用 len(myObj) 的话，就自己实现一个__len__() 方法：
  ```python
  >>> class MyDog(object):
  ...     def __len__(self):
  ...         return 100
  ...
  >>> dog = MyDog()
  >>> len(dog)
  100
  ```

- `__str__()`

  在使用 print 函数打印类时，python 解释器会自动调用类的 __str__() 方法；
  ```python
  >>> class Student(object):
  ...     def __init__(self, name):
  ...         self.name = name
  ...
  >>> print(Student('Michael')) # 调用缺省的 __str__() 方法
  <__main__.Student object at 0x109afb190>
  ```

  ```python
  >>> class Student(object):
  ...     def __init__(self, name):
  ...         self.name = name
  ...     def __str__(self):
  ...         return 'Student object (name: %s)' % self.name
  ...
  >>> print(Student('Michael')) # 调用自定义的__str__() 方法
  Student object (name: Michael)
  ```

- `__repr__()`

  当在交互式解释器中键入类的实例名时，会调用 __repr__() 方法；
  ```python
  >>> s = Student('Michael')
  >>> s
  <__main__.Student object at 0x109afb310>
  ```

  ```python
  class Student(object):
      def __init__(self, name):
          self.name = name
      def __str__(self):
          return 'Student object (name=%s)' % self.name
      __repr__ = __str__ # 通常__str__() 和__repr__() 代码都是一样的，所以采用偷懒的写法
  >>> s = Student('Michael')
  >>> s
  Student object (name: Michael)
  ```

- `__iter__()` 和 `__next__()`

  如果一个类想被用于 for ... in 循环，类似 list 或 tuple 那样，就必须实现一个__iter__() 方法，该方法返回一个迭代对象，然后，Python 的 for 循环就会不断调用该迭代对象的__next__() 方法拿到循环的下一个值，直到遇到 StopIteration 错误时退出循环

  以斐波那契数列为例，写一个 Fib 类，可以作用于 for 循环：
  <!-- TODO: 没搞懂？？？  -->
  ```python
  class Fib(object):
      def __init__(self):
          self.a, self.b = 0, 1 # 初始化两个计数器 a，b

      def __iter__(self):
          return self # 实例本身就是迭代对象，故返回自己

      def __next__(self):
          self.a, self.b = self.b, self.a + self.b # 计算下一个值 具体执行顺序是怎么样的？先谁给谁赋值？？
          if self.a > 100000: # 退出循环的条件
              raise StopIteration()
          return self.a # 返回下一个值

  >>> for n in Fib(): 
  # TODO: 具体迭代是怎么执行的？？？
  ...     print(n)
  ...
  1
  2
  3
  5
  ...
  46368
  75025
  ```

  
- [其它](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319098638265527beb24f7840aa97de564ccc7f20f6000 )

##### 静态方法

与实例方法和类方法不同，静态方法没有参数限制（既不需要实例参数，也不需要类参数），定义的时候使用装饰器 @staticmethod；
静态方法与类相关但是不依赖类与实例；

- 使用场景？意义何在？
  
  经常有一些跟类有关系的功能但在运行时又不需要实例和类参与的情况下需要用到静态方法。比如更改环境变量或者修改其他类的属性等能用到静态方法。这种情况虽然可以直接用函数解决，但会扩散类内部的代码，造成维护困难，采用类的静态方法是最好的选择；

例：
```python
class Student(object):
    '''
    this is a Student class
    '''
    count = 0
    books = []
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    @staticmethod
    def printClassAttr():
        print(Student.count)
        print(Student.books)
    pass
    
Student.printClassAttr()
wilber = Student("Wilber", 28)
wilber.printClassAttr()
```

静态方法与类方法的比较：

- **静态方法有点像函数工具库的作用，相当于一个相对独立的方法，而类方法则更接近类似 Java 面向对象概念中的静态方法；**

- 静态方法不能传入 self 参数，类成员方法需传入代表本类的 cls 参数；

- 静态方法与类方法都可以通过类或者实例来调用，都不能够调用实例属性；

##### 实例、静态、类方法辨析

https://www.zhihu.com/question/20021164 

```python
class Kls(object):
    def __init__(self, data):
        self.data = data
    def printd(self):
        print(self.data)
    @staticmethod
    def smethod(*arg):
        print('Static:', arg)
    @classmethod
    def cmethod(*arg):
        print('Class:', arg)
 
>>> ik = Kls(23)
>>> ik.printd()
23
>>> ik.smethod()
Static: ()
>>> ik.cmethod()
Class: (<class '__main__.Kls'>,)
>>> Kls.printd()
TypeError: unbound method printd() must be called with Kls instance as first argument (got nothing instead)
>>> Kls.smethod()
Static: ()
>>> Kls.cmethod()
Class: (<class '__main__.Kls'>,)
```

#### 访问控制

Python 中没有访问控制的关键字，例如 private、protected 等等。但是，在 Python 编码中，有一些约定来进行访问控制；

- 单下划线"`_`"
  
  在 Python 中，通过单下划线"`_`"来实现模块级别的私有化，一般约定以单下划线"`_`"开头的变量、函数为模块私有的，也就是说"from moduleName import *"将不会引入以单下划线"_"开头的变量、函数，如果访问将会抛出异常：xx is not defined；
  
  可以理解为以单下划线开头的表示的是 protected 类型的变量 / 方法，即保护类型只能允许其本身与子类进行访问，不能用于 from module import *；

- 双下划线"__"
  
  双下划线的表示的是私有类型 (private) 的变量 / 方法，不能在类地外部调用，只允许在这个类本身进行访问；

  通过内建函数 dir() 可以看到，以双下划线“`__`”开头命名的属性在运行时，属性名会被改为"`_类名__属性名`"（属性名前增加了单下划线和类名），如"`__address`"属性在运行时，属性名被改为了"`_Student__address`"，因此，若直接访问该属性，会抛出异常：object has no attribute；

  但即使是双下划线，也没有实现属性的私有化，通过下面的方式还是可以直接访问"__address"属性：
  ```python
  >>> wilber = Student("Wilber", 28)
  >>> print wilber._Student__address
  Shanghai
  ```
  需要注意：不同版本的 Python 解释器可能会把__address 改成不同的变量名；

因此，可以看到，python 中下划线"_"和" __"的使用，更多的是一种规范 / 约定，而不是像 cpp、Java 那样真正实现访问权限控制。

#### 获取类的信息

##### type()

使用 type() 返回对应的 Class 类型；

##### isinstance()

使用 isinstance() 可以判断 class 的继承关系，例：

如果继承关系是：`object -> Animal -> Dog -> Husky`
```python
>>> a = Animal()
>>> d = Dog()
>>> h = Husky()
>>> isinstance(h, Husky)
True
>>> isinstance(h, Dog)
True
>>> isinstance(h, Animal)
True
```
能用 type() 判断的基本类型也可以用 isinstance() 判断：
```python
>>> isinstance('a', str)
True
>>> isinstance(123, int)
True
>>> isinstance(b'a', bytes)
True
```
并且还可以判断一个变量是否是某些类型中的一种，比如下面的代码就可以判断是否是 list 或者 tuple：
```python
>>> isinstance([1, 2, 3], (list, tuple))
True
>>> isinstance((1, 2, 3), (list, tuple))
True
```

##### dir()

如果要获得一个对象的所有属性和方法，可以使用`dir()`函数，它返回一个包含字符串的 list，比如，获得一个 str 对象的所有属性和方法：
```python
>>> dir('ABC')
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```

##### getattr()、setattr()、hasattr()

如果试图获取不存在的属性，会抛出 AttributeError 的错误： 
```python
>>> getattr(obj, 'z') # 获取属性'z'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'MyObject' object has no attribute 'z'
```
因此，所有类的父类 Oject 类提供了 getattr()、setattr()、hasattr() 方法：
```python
>>> hasattr(obj, 'x') # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19
```
要注意的是，只有在不知道对象信息的时候，我们才会去获取对象信息。如果可以直接写：
```python
sum = obj.x + obj.y
```
就不要写：
```python
sum = getattr(obj, 'x') + getattr(obj, 'y')
```

#### 枚举类

Python 提供了 Enum 类来实现枚举类；

Enum 可以把一组相关常量定义在一个 class 中，且 class 不可变，而且成员可以直接比较，每个常量都是 class 的一个唯一实例；

- 方式一：
  ```python
  from enum import Enum
  # 定义 Month 类型的枚举类
  Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
  # 可以直接使用 Month.Jan 来引用一个常量
  for name, member in Month.__members__.items():
  print(name, '=>', member, ',', member.value)
  # value 属性则是自动赋给成员的 int 常量，默认从 1 开始计数
  ```

- 方式二：从 Enum 派生出自定义类
  ```python
  from enum import Enum, unique

  @unique
  # @unique 装饰器可以帮助我们检查保证没有重复值
  class Weekday(Enum):
      Sun = 0 # Sun 的 value 被设定为 0
      Mon = 1
      Tue = 2
      Wed = 3
      Thu = 4
      Fri = 5
      Sat = 6
  ```
  使用：
  ```python
  >>> day1 = Weekday.Mon
  >>> print(day1)
  Weekday.Mon
  >>> print(Weekday.Tue)
  Weekday.Tue
  >>> print(Weekday['Tue'])
  Weekday.Tue
  >>> print(Weekday.Tue.value)
  2
  >>> print(day1 == Weekday.Mon)
  True
  >>> print(day1 == Weekday.Tue)
  False
  >>> print(Weekday(1))
  Weekday.Mon
  >>> print(day1 == Weekday(1))
  True
  >>> Weekday(7)
  Traceback (most recent call last):
    ...
  ValueError: 7 is not a valid Weekday
  >>> for name, member in Weekday.__members__.items():
  ...     print(name, '=>', member)
  ...
  Sun => Weekday.Sun
  Mon => Weekday.Mon
  Tue => Weekday.Tue
  Wed => Weekday.Wed
  Thu => Weekday.Thu
  Fri => Weekday.Fri
  Sat => Weekday.Sat

  ```

#### 元类

[参考原文](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319106919344c4ef8b1e04c48778bb45796e0335839000 )

### 继承

类的继承是面向对象的三大特性之一；

在 python 中实现类继承的语法：基类名写在括号（即继承元组）里，若多重继承（继承了多个类），则将多个基类写个括号里
```python
class SubClassName (ParentClass1[, ParentClass2, ...]):
   'Optional class documentation string'
   class_suite
```

在 python 中继承中特点：

- 所有类最终都会继承 Object 类； 

- 在继承中基类的构造（`__init__()`方法）不会被自动调用，它需要在其派生类的构造中使用`super()`方法获取父类定义，`super().__init__(xxx)`；

- 在调用基类的方法时，需要加上基类的类名前缀，且需要带上 self 参数变量。区别于在类中调用普通函数时并不需要带上 self 参数；

- Python 总是首先查找对应类型的方法，如果它不能在派生类中找到对应的方法，它才开始到基类中逐个查找。（先在本类中查找调用的方法，找不到才去基类中找）；

#### 方法重写

如果父类方法的功能不能满足子类的需求，可以在子类重写父类的方法；

例：
```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
class Parent:        # 定义父类
   def myMethod(self):
      print('调用父类方法')
 
class Child(Parent): # 定义子类
   def myMethod(self):
      print('调用子类方法')
 
c = Child()          # 子类实例
c.myMethod()         # 子类调用重写方法
```

##### 基础重写方法

[参考原文](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319098638265527beb24f7840aa97de564ccc7f20f6000 )

- `__init__ ( self [,args...] )`

  构造函数；
  
  简单的调用方法：obj = className(args)；

- `__del__( self )`

  析构方法，删除一个对象；

  简单的调用方法 : dell obj；

- `__repr__( self )`

  转化为供解释器读取的形式；
  
  简单的调用方法 : repr(obj)；

  例：
  ```python
  class Student(object):
      def __init__(self, name):
          self.name = name
      def __repr__(self):
          return 'Student object (name=%s)' % self.name
  >>>Student('Michael')
  Student object (name: Michael)

  ```

- `__str__( self )`

  用于将值转化为适于阅读的形式；
  
  简单的调用方法 : str(obj)；

  eg:
  ```python
  >>> class Student(object):
  ...     def __init__(self, name):
  ...         self.name = name
  ...     def __str__(self):
  ...         return 'Student object (name: %s)' % self.name
  ...
  >>> print(Student('Michael'))
  Student object (name: Michael)

  ```

- `__cmp__ ( self, x )`

  对象比较；
  
  简单的调用方法 : cmp(obj, x)；

- `__iter__ (self)`

  如果一个类想被用于 for ... in 循环，类似 list 或 tuple 那样，就必须实现一个__iter__() 方法，该方法返回一个迭代对象，然后，Python 的 for 循环就会不断调用该迭代对象的__next__() 方法拿到循环的下一个值，直到遇到 StopIteration 错误时退出循环

  以斐波那契数列为例，写一个 Fib 类，可以作用于 for 循环：
  ```python
  class Fib(object):
      def __init__(self):
          self.a, self.b = 0, 1 # 初始化两个计数器 a，b

      def __iter__(self):
          return self # 实例本身就是迭代对象，故返回自己

      def __next__(self):
          self.a, self.b = self.b, self.a + self.b # 计算下一个值
          if self.a > 100000: # 退出循环的条件
              raise StopIteration()
          return self.a # 返回下一个值
  ```
  使用
  ```python
  >>> for n in Fib():
  ...     print(n)
  ```

- `__getitem__(self, n)`

  使用索引下表查找元素（像 list[0]）

  eg:
  ```python
  class Fib(object):
      def __getitem__(self, n):
          a, b = 1, 1
          for x in range(n):
              a, b = b, a + b
          return a
  ```
  使用
  ```python
  >>> f = Fib()
  >>> f[0]
  1
  >>> f[1]
  1
  >>> f[2]
  2
  >>> f[100]
  573147844013817084101
  ```

- `__getattr__`

- `__call__`

##### 运算符重载

可以使用 Python 的特殊方法（special method），有时也被称作魔术方法（magic method），来实现运算 / 比较操作符的功能。

- 比较运算符重载

  | 方法名                 | 使用            |
  | ------------------- | ------------- |
  | __eq__(self, other) | self == other |
  | __ne__(self, other) | self != other |
  | __lt__(self, other) | self < other  |
  | __gt__(self, other) | self > other  |
  | __le__(self, other) | self <= other |
  | __ge__(self, other) | self >= other |

- 数学运算符重载

  | 方法名                       | 使用            |
  | ------------------------- | ------------- |
  | __add__(self, other)      | self + other  |
  | __sub__(self, other)      | self - other  |
  | __mul__(self, other)      | self * other  |
  | __floordiv__(self, other) | self // other |
  | __truediv__(self, other)  | self / other  |
  | __mod__(self, other)      | self % other  |
  | __pow__(self, other)      | self ** other |

例：重载 + 运算符
```python
#!/usr/bin/python
 
class Vector:
   def __init__(self, a, b):
      self.a = a
      self.b = b
 
   def __str__(self):
      return 'Vector (%d, %d)' % (self.a, self.b)
   
   def __add__(self,other):
      return Vector(self.a + other.a, self.b + other.b)
 
v1 = Vector(2,10)
v2 = Vector(5,-2)
print v1 + v2
```

#### 多重继承

通过多重继承，一个子类就可以同时获得多个父类的所有功能。

例：
```python
class Bat(Mammal, Flyable):
    pass
```

### 多态

例：
```python
class Animal(object):
    def run(self):
        print('Animal is running...')
class Dog(Animal):

    def run(self):
        print('Dog is running...')

class Cat(Animal):

    def run(self):
        print('Cat is running...')

def run_twice(animal):
    animal.run()
```
当我们传入 Animal 的实例时：
```python
>>> run_twice(Animal())
Animal is running...
```
当我们传入 Dog 的实例时：
```python
>>> run_twice(Dog())
Dog is running...
```
当我们传入 Cat 的实例时：
```python
>>> run_twice(Cat())
Cat is running...
```

#### “开闭”原则

比如：
```python
class Animal(object):
    def run(self):
        print('Animal is running...')

class Dog(Animal):

    def run(self):
        print('Dog is running...')

class Cat(Animal):

    def run(self):
        print('Cat is running...')
```

多态的好处就是，当我们需要传入 Dog、Cat、Tortoise……时，我们只需要接收 Animal 类型就可以了，因为 Dog、Cat、Tortoise……都是 Animal 类型，然后，按照 Animal 类型进行操作即可。由于 Animal 类型有 run() 方法，因此，传入的任意类型，只要是 Animal 类或者子类，就会自动调用实际类型的 run() 方法，这就是多态的意思：

对于一个变量，我们只需要知道它是 Animal 类型，无需确切地知道它的子类型，就可以放心地调用 run() 方法，而具体调用的 run() 方法是作用在 Animal、Dog、Cat 还是 Tortoise 对象上，由运行时该对象的确切类型决定，这就是多态真正的威力：调用方只管调用，不管细节，而当我们新增一种 Animal 的子类时，只要确保 run() 方法编写正确，不用管原来的代码是如何调用的。

这就是著名的“开闭”原则：

1)	对扩展开放：允许新增 Animal 子类；

2)	对修改封闭：不需要修改依赖 Animal 类型的 run_twice() 等函数；

#### 鸭子类型

对于静态语言（例如 Java）来说，如果需要传入 Animal 类型，则传入的对象必须是 Animal 类型或者它的子类，否则，将无法调用 run() 方法。

对于 Python 这样的动态语言来说，则不一定需要传入 Animal 类型。我们只需要保证传入的对象有一个 run() 方法就可以了：
```python
class Timer(object):
    def run(self):
        print('Start...')
```
这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。

<!-- TODO: 理解：看有没有操作的方法，而不看类的类型是什么 -- 弱化了类型的概念和限制 -- 动态语言的特点 -- “看实力，不看学历” -->

Python 的 “file-like object“ 就是一种鸭子类型。对真正的文件对象，它有一个 read() 方法，返回其内容。但是，许多对象，只要有 read() 方法，都被视为 “file-like object“。许多函数接收的参数就是“file-like object“，你不一定要传入真正的文件对象，完全可以传入任何实现了 read() 方法的对象。

例如：

要实现一个读取图像的功能：
```python
def readImage(fp):
    if hasattr(fp, 'read'):
        return readData(fp)
return None
```
根据鸭子类型，有 read() 方法，不代表该 fp 对象就是一个文件流，它也可能是网络流，也可能是内存中的一个字节流，但只要 read() 方法返回的是有效的图像数据，就不影响读取图像的功能；
