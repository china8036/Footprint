- [Java 集合：Map 族 - EnumMap 实现类](#java-集合map-族---enummap-实现类)
    - [1. 基本概念](#1-基本概念)
    - [2. 常用 API](#2-常用-api)
    - [3. Refer Links](#3-refer-links)

# Java 集合：Map 族 - EnumMap 实现类

## 1. 基本概念

EnumMap 是一个与枚举类一起使用的 Map 实现，EnumMap 中的所有 Key 都必须是单个枚举类的枚举值。创建 EnumMap 时必须显式或隐式指定它对应的枚举类。

EnumMap 对应以下特征：
- EnumMap 在内部以数组的形式保存，因此这种实现方式非常紧凑、高效。
- EnumMap 根据 Key 的自然顺序（即枚举值在枚举类中的定义顺序）来维护 key-value 对的顺序。当程序通过 keySey()、entrySet()、values() 方法遍历 EnumMap 时可以看到这种顺序。
- EnumMap 中不允许使用 null 作为 Key，但允许使用 null 作为 value。

## 2. 常用 API

与创建普通的 Map 有区别的是，创建 EnumMap 时必须指定一个枚举类，从而将该 EnumMap 和指定枚举类关联起来。
```java
EnumMap enumMap = new EnumMap(season.class);
```

## 3. Refer Links
