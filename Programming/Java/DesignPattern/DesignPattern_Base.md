- [Design Pattern 基础概念](#design-pattern-%E5%9F%BA%E7%A1%80%E6%A6%82%E5%BF%B5)
  - [1. 发展历史](#1-%E5%8F%91%E5%B1%95%E5%8E%86%E5%8F%B2)
  - [2. 基本概念](#2-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
  - [3. 面向对象设计原则](#3-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E8%AE%BE%E8%AE%A1%E5%8E%9F%E5%88%99)
  - [4. GOF 设计模式](#4-gof-%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F)
    - [4.1. 创建型模式 (Creational Pattern)](#41-%E5%88%9B%E5%BB%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-creational-pattern)
    - [4.2. 结构型模式 (Structural Pattern)](#42-%E7%BB%93%E6%9E%84%E5%9E%8B%E6%A8%A1%E5%BC%8F-structural-pattern)
    - [4.3. 行为型模式 (Behavioral Pattern)](#43-%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
  - [5. Refer Links](#5-refer-links)

# Design Pattern 基础概念

## 1. 发展历史

最早将模式的思想引入软件工程方法学的是 1991-1992 年以“四人组 (Gang of Four，简称**GoF**，分别是 Erich Gamma, Richard Helm, Ralph Johnson 和 John Vlissides)”自称的四位著名软件工程学者，他们在 1994 年归纳发表了 23 种在软件开发中使用频率较高的设计模式，旨在用模式来统一沟通面向对象方法在分析、设计和实现间的鸿沟。GoF 将模式的概念引入软件工程领域，这标志着软件模式的诞生。

**软件模式**(Software Patterns) 是将模式的一般概念应用于软件开发领域，即软件开发的总体指导思路或参照样板。软件模式并非仅限于设计模式，还包括架构模式、分析模式和过程模式等，实际上，在软件开发生命周期的每一个阶段都存在着一些被认同的模式。

软件模式是在软件开发中某些可重现问题的一些有效解决方法，软件模式的基础结构主要由四部分构成，包括问题描述【待解决的问题是什么】、前提条件【在何种环境或约束条件下使用】、解法【如何解决】和效果【有哪些优缺点】。

在软件模式中，**设计模式是研究最为深入的分支，设计模式用于在特定的条件下为一些重复出现的软件设计问题提供合理的、有效的解决方案，它融合了众多专家的设计经验，已经在成千上万的软件中得以应用。**1995 年， GoF 将收集和整理好的 23 种设计模式汇编成 Design Patterns: Elements of Reusable Object-Oriented Software，即《设计模式：可复用面向对象软件的基础》一书，该书的出版也标志着设计模式正式成为面向对象 (Object Oriented) 软件工程的一个重要研究分支。

## 2. 基本概念

> In software engineering, a software design pattern is a general, reusable solution to a commonly occurring problem within a given context in software design. It is not a finished design that can be transformed directly into source or machine code. It is a description or template for how to solve a problem that can be used in many different situations. Design patterns are formalized best practices that the programmer can use to solve common problems when designing an application or system.

设计模式 (Design Pattern) 是对软件设计中普遍存在（反复出现）的各种问题所提出的解决方案，这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。它是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。

狭义的设计模式是指 GoF 在《设计模式：可复用面向对象软件的基础》一书中所介绍的 23 种经典设计模式，不过设计模式并不仅仅只有这 23 种，随着软件开发技术的发展，越来越多的新模式不断诞生并得以应用。

设计模式的出现可以让我们站在前人的肩膀上，通过一些成熟的设计方案来指导新项目的开发和设计，以便于我们开发出具有更好的灵活性和可扩展性，也更易于复用的软件系统。

## 3. 面向对象设计原则

对于面向对象软件系统的设计而言，在支持可维护性的同时，提高系统的可复用性是一个至关重要的问题，如何同时提高一个软件系统的可维护性和可复用性是面向对象设计需要解决的核心问题之一。

**面向对象设计原则**为支持可维护性复用而诞生，这些原则蕴含在很多设计模式中，它们是从许多设计方案中总结出的指导性原则，也是我们用于评价一个设计模式的使用效果的重要指标之一。

- 开闭原则（Open Close Principle）

  开闭原则的意思是：**对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果**。简言之，是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类。

- 里氏代换原则（Liskov Substitution Principle）

  里氏代换原则 LSP 是面向对象设计的基本原则之一。里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。LSP 是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。
  
  里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。

- 依赖倒转原则（Dependence Inversion Principle）

  这个原则是开闭原则的基础，具体内容：针对接口编程，依赖于抽象而不依赖于具体。

- 接口隔离原则（Interface Segregation Principle）

  这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。

- 迪米特法则，又称最少知道原则（Demeter Principle）

  最少知道原则是指：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。

- 合成复用原则（Composite Reuse Principle）

  合成复用原则是指：尽量使用合成 / 聚合的方式，而不是使用继承。

## 4. GOF 设计模式

根据设计模式的参考书 Design Patterns - Elements of Reusable Object-Oriented Software（中文译名：设计模式 - 可复用的面向对象软件元素） 中所提到的，总共有 23 种设计模式。这些模式可以分为三大类：创建型模式（Creational Patterns）、结构型模式（Structural Patterns）、行为型模式（Behavioral Patterns）。

### 4.1. 创建型模式 (Creational Pattern)

| 模式名称                        | 学习难度 | 使用频率  |
| ------------------------------- | -------- | ----- |
| 单例模式 Singleton Pattern           | ★☆☆☆☆    | ★★★★☆ |
| 简单工厂模式 Simple Factory Pattern    | ★★☆☆☆    | ★★★☆☆ |
| 工厂方法模式 Factory Method Pattern    | ★★☆☆☆    | ★★★★★ |
| 抽象工厂模式 Abstract  Factory Pattern | ★★★★☆    | ★★★★★ |
| 原型模式 Prototype Pattern           | ★★★☆☆    | ★★★☆☆ |
| 建造者模式 Builder Pattern            | ★★★★☆    | ★★☆☆☆ |

### 4.2. 结构型模式 (Structural Pattern)

| 模式名称                   | 学习难度  | 使用频率  |
| ---------------------- | ----- | ----- |
| 适配器模式 Adapter Pattern   | ★★☆☆☆ | ★★★★☆ |
| 桥接模式 Bridge  Pattern    | ★★★☆☆ | ★★★☆☆ |
| 组合模式 Composite  Pattern | ★★★☆☆ | ★★★★☆ |
| 装饰模式 Decorator  Pattern | ★★★☆☆ | ★★★☆☆ |
| 外观模式 Façade  Pattern    | ★☆☆☆☆ | ★★★★★ |
| 享元模式 Flyweight  Pattern | ★★★★☆ | ★☆☆☆☆ |
| 代理模式 Proxy  Pattern     | ★★★☆☆ | ★★★★☆ |

### 4.3. 行为型模式 (Behavioral Pattern)

| 模式名称                                  | 学习难度  | 使用频率  |
| ------------------------------------- | ----- | ----- |
| 职责链模式 Chain  of Responsibility Pattern | ★★★☆☆ | ★★☆☆☆ |
| 命令模式 Command  Pattern                  | ★★★☆☆ | ★★★★☆ |
| 解释器模式 Interpreter  Pattern             | ★★★★★ | ★☆☆☆☆ |
| 迭代器模式 Iterator  Pattern                | ★★★☆☆ | ★★★★★ |
| 中介者模式 Mediator  Pattern                | ★★★☆☆ | ★★☆☆☆ |
| 备忘录模式 Memento  Pattern                 | ★★☆☆☆ | ★★☆☆☆ |
| 观察者模式 Observer  Pattern                | ★★★☆☆ | ★★★★★ |
| 状态模式 State  Pattern                    | ★★★☆☆ | ★★★☆☆ |
| 策略模式 Strategy  Pattern                 | ★☆☆☆☆ | ★★★★☆ |
| 模板方法模式 Template  Method Pattern        | ★★☆☆☆ | ★★★☆☆ |
| 访问者模式 Visitor  Pattern                 | ★★★★☆ | ★☆☆☆☆ |

## 5. Refer Links

[菜鸟教程：设计模式](http://www.runoob.com/design-pattern/design-pattern-tutorial.html)

[图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/index.html)

[设计模式导学目录（完整版）](http://blog.csdn.net/lovelion/article/details/17517213)
