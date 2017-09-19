<!-- toc -->
# CSS 网页布局 #

http://zh.learnlayout.com/

布局的传统解决方案，基于盒状模型，依赖 display 属性 + position属性 + float属性。

## display ##
display 是CSS中最重要的用于控制布局的属性。每个元素都有一个默认的 display 值，这与元素的类型有关。

对于大多数元素它们的默认值通常是 block 或 inline 。一个 block 元素通常被叫做块级元素。一个 inline 元素通常被叫做行内元素。


***TODO 学习CSS布局 - Flex 阮一峰 + 淘宝h5 笔记 - CSS基础语法笔记 - 移动web那种结构到底该怎么弄出来 - 看完慕课hello移动web***



## 绝对布局 ##



## 流式布局 ##




## 弹性布局：Flex ##

http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html  
http://www.ruanyifeng.com/blog/2015/07/flex-examples.html  


传统布局模型对于那些特殊布局非常不方便，比如，垂直居中就不容易实现。


2009年，W3C 提出了一种新的方案----Flex 布局，可以简便、完整、响应式地实现各种页面布局。目前，它已经得到了所有浏览器的支持，Flex 布局将成为未来布局的首选方案。

Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。



任何一个容器都可以指定为 Flex 布局。

```css
.box{
  display: flex;
}
```
行内元素也可以使用 Flex 布局。

```css
.box{
  display: inline-flex;
}
```
Webkit 内核的浏览器，必须加上-webkit前缀。

```css
.box{
  display: -webkit-flex; /* Safari */
  display: flex;
}
```
注意：设为 Flex 布局以后，子元素的 **float**、**clear** 和 **vertical-align** 属性将失效。




































