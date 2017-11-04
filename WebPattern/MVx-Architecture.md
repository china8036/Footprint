- [MVC MVP MVVM](#mvc-mvp-mvvm)
  - [MVC(Model+View+Controller)](#mvcmodelviewcontroller)
  - [MVP(Model+View+Presenter)](#mvpmodelviewpresenter)
  - [MVVM(Model+View+ViewModel)](#mvvmmodelviewviewmodel)
  - [Refer Links](#refer-links)

# MV* 架构模式

## 概述

> M-V-*架构的本质都是一样的，都是在于 M-V 之间的桥梁耀靠 X 来牵线，而 X 的模式之间的不同，主要是 M 与 V 的数据传递的流程不同。
>
> 数据传递的流程不同来源于运行环境技术栈能够做到的事情不同。所以无论是复杂化 简单化 还是修改流程，基本都是因为技术栈变化了 对应做的调整。
>
> 在相同技术栈下，能够实现的各种 X 都可以是大同小异的。  
> 
> 在不同技术栈下，相同的 X 可能实现都大相径庭，仅有非常抽象的流程类似。

## MVC(Model+View+Controller)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/4/5cc1af29dfd007c81fb8391c5457a946.jpg)

视图被用户看到；
用户使用控制器；
控制器操作模型；
模型更新视图；


## MVP(Model+View+Presenter)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/4/86dea35f532b275ea53319d778c78ca3.jpg)

对 MVC 的改进的思想：切断的 View 和 Model 的联系，让 View 只和 Presenter（原 Controller）交互，减少在需求变化中需要维护的对象的数量。

各部分之间的通信，都是双向的。

View 非常薄，不部署任何业务逻辑，称为"被动视图"（Passive View），即没有任何主动性，而 Presenter 非常厚，所有逻辑都部署在那里。

## MVVM(Model+View+ViewModel)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/4/d6929975db518d443290d87582287a79.jpg)

> 个人不认为 MVVM 是从 MVP 进化而来，我只觉得这是在 MVP 之后出现的一种“更好的”UI 模式解决方案，但是用 MVP 来与之对比比较容易说明问题。

其中，ViewModel 大致上就是 MVP 中的 Presenter 和 MVC 中的 Controller 了，而 View 和 ViewModel 间没有了 MVP 的界面接口，而是直接交互，用数据“绑定”的形式让数据更新的事件不需要开发人员手动去编写特殊用例，而是自动地双向同步。数据绑定你可以认为是 Observer 模式或者是 Publish/Subscribe 模式，**原理都是为了用一种统一的集中的方式实现频繁需要被实现的数据更新问题**。

比起 MVP，MVVM 不仅简化了业务与界面的依赖关系，还优化了数据频繁更新的解决方案，甚至可以说提供了一种有效的解决模式。

正因为如此，许多人认为 MVVM 是最好的一种模式，没有之一。

## Refer Links

http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html

https://draveness.me/mvx

https://www.zhihu.com/question/20148405
