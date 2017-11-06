- [CSS BFC](#css-bfc)
  - [概述](#%E6%A6%82%E8%BF%B0)
  - [应用](#%E5%BA%94%E7%94%A8)
    - [解决 margin 叠加问题](#%E8%A7%A3%E5%86%B3-margin-%E5%8F%A0%E5%8A%A0%E9%97%AE%E9%A2%98)
    - [用于不重叠的 float 布局](#%E7%94%A8%E4%BA%8E%E4%B8%8D%E9%87%8D%E5%8F%A0%E7%9A%84-float-%E5%B8%83%E5%B1%80)
    - [解决浮动的高度塌陷问题](#%E8%A7%A3%E5%86%B3%E6%B5%AE%E5%8A%A8%E7%9A%84%E9%AB%98%E5%BA%A6%E5%A1%8C%E9%99%B7%E9%97%AE%E9%A2%98)
  - [Refer Links](#refer-links)

# CSS BFC

## 概述

- BFC 的特性：

  - 内部的 Box 会在垂直方向，从顶部开始一个接一个地放置。

  - Box 垂直方向的距离由 margin 决定。属于同一个 BFC 的两个相邻 Box 的 margin 会发生叠加。

  - 每个元素的 margin box 的左边， 与包含块 border box 的左边相接触（对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

  - BFC 的区域不会与 float box 叠加。

  - BFC 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。

  - 计算 BFC 的高度时，浮动元素也参与计算。
  
  通俗地来说：**创建了 BFC 的元素就是一个独立的盒子，BFC是页面元素里一个独立存在作用块，它不影响它外面的布局，外面的元素也不会影响到BFC里面的布局，同时 BFC 任然属于文档中的普通流。**

- 如何触发 BFC？在以下情况都可以创建 BFC：

  - float 除了 none 以外的值 
  
  - overflow 除了 visible 以外的值（hidden，auto，scroll ） 
  
  - display (table-cell，table-caption，inline-block) 
  
  - position（absolute，fixed） 
  
  - fieldset 元素

## 应用

### 解决 margin 叠加问题 

问题：
```html
<p>
    hello world
</p>
<p>
    hello world
</p>
<p>
    hello world
</p>
```
```css
p {
  margin: 50px;
}
```
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/8e4c84929f07cc35913db875539c2197.jpg)

三 P 每个 p 之间的距离为 50px，发生了外边距叠加。 要解决这个叠加问题，即使得每个 P 之间是 100px，我们可以通过可以给 p 元素添加一个父元素，让它新建一个 BFC。如下：
```html
<div class="BFC">
    <p>
    hello world
    </p>
</div>
 
<p>
    hello world
</p>
<p>
    hello world
</p>
```

```css
.BFC{
    overflow:hidden;
}
p{
    margin:50px;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/1c7077e778f4425aaec36904f39c9765.jpg)

### 用于不重叠的 float 布局

```html
<div class="aside"></div>
<div class="main"></div>
```

```css
body {
  width: 300px;
  position: relative;
}
.aside {
  width: 100px;
  height: 150px;
  float: left;
  background: blue;
}
.main {
  height: 200px;
  background: #f00;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/7fb3fdae15b132ff3e8d568d1d3ce652.jpg)

从图中我们会发现上面 BFC 的第三个特性，就是元素的左外边距会触碰到包含块容器的做外边框，就算存在浮动也会如此。那么我们如何解决这个问题呢？看上面 BFC 第四个特性，BFC 不会与浮动盒子叠加，那么我们可以用`overflow:hidden`触发 main 元素的 BFC 来解决这个问题呢？

```html
<div class="aside"></div>
<div class="main"></div>
```

```css
body {
  width: 300px;
  position: relative;
}
.aside {
  width: 100px;
  height: 150px;
  float: left;
  background: blue;
}
.main {
  height: 200px;
  background: #f00;
  overflow:hidden;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/e3ac09dc897ae4a6850e884a2f1a3f60.jpg)

### 解决浮动的高度塌陷问题

```html
<div class="BFC">
  <div class="box"></div>
  <div class="box"></div>
</div>
```

```css
.BFC {
  border: 5px solid #f00;
  width: 300px;
}
.box {
  border: 5px solid blue;
  width:100px;
  height: 100px;
  float: left;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/ca0b3f7d6fdbb38010effa855433bd58.jpg)

我们发现由于里面两个子元素浮动的关系，两个box已经脱离了父元素的包含块，父元素高度已经塌陷，我们需要让父元素包含两个box子元素，这样计算高度时，两个浮动子元素就会参与，所以我们可以通过`overflow:hidden`，触发父元素的BFC来闭合浮动：

```html
<div class="BFC">
  <div class="box"></div>
  <div class="box"></div>
</div>
```

```css
.BFC {
  border: 5px solid #f00;
  width: 300px;
  overflow:hidden;
}
.box {
  border: 5px solid blue;
  width:100px;
  height: 100px;
  float: left;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/bad7e40b5ccf4cdbce66e96a711ce4e4.jpg)

## Refer Links

http://www.jianshu.com/p/fc1d61dace7b

http://www.html-js.com/article/1866

http://www.iyunlu.com/view/css-xhtml/55.html