
- [CSS 网页定位与布局](#css-%E7%BD%91%E9%A1%B5%E5%AE%9A%E4%BD%8D%E4%B8%8E%E5%B8%83%E5%B1%80)
  - [Box Model](#box-model)
  - [display 属性](#display-%E5%B1%9E%E6%80%A7)
    - [block](#block)
    - [inline](#inline)
    - [none](#none)
    - [inline-block](#inline-block)
    - [flex](#flex)
    - [inline-flex](#inline-flex)
  - [position 属性](#position-%E5%B1%9E%E6%80%A7)
    - [没有定位 static](#%E6%B2%A1%E6%9C%89%E5%AE%9A%E4%BD%8D-static)
    - [相对定位 relative](#%E7%9B%B8%E5%AF%B9%E5%AE%9A%E4%BD%8D-relative)
    - [绝对定位 absolute](#%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D-absolute)
    - [固定定位 fixed](#%E5%9B%BA%E5%AE%9A%E5%AE%9A%E4%BD%8D-fixed)
    - [父级定位 inherit](#%E7%88%B6%E7%BA%A7%E5%AE%9A%E4%BD%8D-inherit)
  - [浮动定位](#%E6%B5%AE%E5%8A%A8%E5%AE%9A%E4%BD%8D)
    - [概述](#%E6%A6%82%E8%BF%B0)
    - [float 属性](#float-%E5%B1%9E%E6%80%A7)
    - [clear 属性](#clear-%E5%B1%9E%E6%80%A7)
    - [实例 ###](#%E5%AE%9E%E4%BE%8B)
  - [弹性布局：Flex](#%E5%BC%B9%E6%80%A7%E5%B8%83%E5%B1%80%EF%BC%9Aflex)
    - [概述](#%E6%A6%82%E8%BF%B0)
    - [基本概念](#%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
      - [container 属性](#container-%E5%B1%9E%E6%80%A7)
      - [item 属性](#item-%E5%B1%9E%E6%80%A7)
    - [实例](#%E5%AE%9E%E4%BE%8B)

# CSS 网页定位与布局 #

学习 CSS 布局： http://zh.learnlayout.com/   
W3School CSS 布局： http://www.w3school.com.cn/css   

网页中的布局一般可分为三种状态：块挨着块，块包含着块，块叠加着块；

布局的传统解决方案，基于盒状模型，依赖 display 属性 + position 属性 + float 属性，有三种基本的定位机制：普通流定位、绝对定位和浮动定位；

## Box Model ##
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/20/a1f534f712fb212dc88e657143ae0930.jpg)

- 由于元素的边框和内边距会撑开元素，导致两个相同宽度的元素显示的实际宽度却不一样：
```css
	.simple {
	  width: 500px;
	  margin: 20px auto;
	}
	
	.fancy {
	  width: 500px;
	  margin: 20px auto;
	  padding: 50px;
	  border-width: 10px;
	}
```
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/20/c4252d472c01636c8e11a0d46eab8361.jpg)    
CSS 提供了 box-sizing 属性，当设置 `box-sizing: border-box;` 时，此元素的内边距和边框不再会增加它的宽度：
```css
	.simple {
	  width: 500px;
	  margin: 20px auto;
	  -webkit-box-sizing: border-box;
	     -moz-box-sizing: border-box;
	          box-sizing: border-box;
	}
	
	.fancy {
	  width: 500px;
	  margin: 20px auto;
	  padding: 50px;
	  border: solid blue 10px;
	  -webkit-box-sizing: border-box;
	     -moz-box-sizing: border-box;
	          box-sizing: border-box;
	}
```
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/20/b4efe51de515082c76388cc37b67bfb2.jpg)   
在所有元素中都使用此特性：
```css
	* {
	  -webkit-box-sizing: border-box;
	     -moz-box-sizing: border-box;
	          box-sizing: border-box;
	}
```

## display 属性 ##
https://developer.mozilla.org/en-US/docs/CSS/display
display 是 CSS 中最重要的用于控制布局的属性。每个元素都有一个默认的 display 值，这与元素的类型有关。

对于大多数元素它们的默认值通常是 block 或 inline 。一个 block 元素通常被叫做块级元素。一个 inline 元素通常被叫做行内元素。

### block ###
块级元素，如 div、h1 或 p；

块级元素会尽可能的充满全部 width，因此它们从上到下一个接一个地排列，框之间的垂直距离是由框的垂直外边距计算出来；

- 设置块元素水平居中的方法：
可以通过设置块级元素的 width 以防止它从左到右撑满整个容器，然后再设置左右外边距为 auto 来使其水平居中（auto 即元素会占据你所指定的宽度，然后剩余的宽度会一分为二成为左右外边距）：
```css
#main {
  width: 600px;
  margin: 0 auto; 
}
```
存在问题：当浏览器窗口比元素的宽度还要窄时，浏览器会显示一个水平滚动条来容纳页面；   
解决方案：使用 max-width 替代 width 可以使浏览器更好地处理小窗口的情况（这点在移动设备上显得尤为重要）：
```css
#main {
  max-width: 600px;
  margin: 0 auto; 
}
```

### inline ###
行内元素，如 span 和 strong；

行内元素不会充满全部 width，因此它们在一行中水平布置。可以使用水平内边距、边框和外边距调整它们的间距。但是，垂直内边距、边框和外边距不影响行内框的高度。由一行形成的水平框称为行框（Line Box），行框的高度总是足以容纳它包含的所有行内框。不过，设置行高可以增加这个框的高度；

### none ###
在不删除元素的情况下隐藏元素，且不占用文档中的空间；

注意：
设置 ` visibility: hidden;` 只是将元素隐藏，同样会占用文档空间；

### inline-block ###
inline-block 即行内块元素；

例：
```css
.box {
  float: left;
  width: 200px;
  height: 100px;
  margin: 1em;
}
.after-box {
  clear: left;
}
```
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/20/c52d9b3a178d8964127dc0164cd6395a.jpg)    
等同于
```css
.box2 {
  display: inline-block;
  width: 200px;
  height: 100px;
  margin: 1em;
}
```
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/20/a5709e58d88a8482306768096f6ea87b.jpg)

注意：
- vertical-align 属性会影响到 inline-block 元素，可将它的值设置为 top；
- 需要设置每一列的宽度；
- 如果 HTML 源代码中元素之间有空格，那么列与列之间会产生空隙；

### flex ###
使用 flex 布局；

### inline-flex ###

行内元素使用 flex 布局；

## position 属性 ##
CSS 定位 (Positioning) 属性定义了元素的定位方式；

定位的基本思想很简单，它允许你定义元素框相对于其正常位置应该出现的位置，或者相对于父元素、另一个元素甚至浏览器窗口本身的位置；

### 没有定位 static ###
默认值。没有定位，元素框正常生成，即块级元素生成一个矩形框，作为文档流的一部分从上到下布局，行内元素则会创建一个或多个行框，置于其父元素中从左到右布局（直至充满一行后换行），元素出现在正常的文档流中，（忽略 top, bottom, left, right 或者 z-index 声明）；

### 相对定位 relative ###
生成相对定位的元素，**相对于其正常位置**进行定位，元素仍保持其未定位前的形状，它原本所占的空间仍保留；

相对定位实际上被看作普通流定位模型的一部分，因为元素的位置相对于它在普通流中的位置；

例：
```css
#box_relative {
  position: relative;
  left: 30px;
  top: 20px;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/20/5a1b022df5936086723a9fd54e560e12.jpg)

注意，在使用相对定位时，无论是否进行移动，元素仍然占据原来的空间。因此，移动元素会导致它覆盖其它框。

### 绝对定位 absolute ###
元素框从文档流完全删除，生成绝对定位的元素，**相对于 static 定位以外的第一个父元素**进行定位，元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定；

例：
```css
#box_relative {
  position: absolute;
  left: 30px;
  top: 20px;
}
```
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/20/513ad580f907b37aecac67c25209fb27.jpg)

注意：因为绝对定位的框与文档流无关，所以它们可以覆盖页面上的其它元素。可以通过设置 z-index 属性来控制这些框的堆放次序；

### 固定定位 fixed ###
生成绝对定位的元素，**相对于浏览器窗口**进行定位，元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定；

fixed 定位的元素框的表现类似于 absolute，区别在于其相对的是浏览器窗口；

fixed 定位在移动端会有兼容性问题；

### 父级定位 inherit ###
规定从父元素继承 position 属性的值；

## 浮动定位 ##
### 概述 ###
http://www.w3school.com.cn/css/css_positioning_floating.asp   
当元素框使用浮动定位时，它将**脱离文档流**并且**向浮动的方向移动**，直到它的外边缘碰到包含框或另一个浮动框的边框为止。

由于浮动框不在文档的普通流中，所以**文档的普通流中的块框表现得就像浮动框不存在一样**。

浮动定位解析：

当把框 1 向右浮动时，它脱离文档流并且向右移动，直到它的右边缘碰到包含框的右边缘：   
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/20/f1514673b71c96550d3519768823aec2.jpg)

当框 1 向左浮动时，它脱离文档流并且向左移动，直到它的左边缘碰到包含框的左边缘。因为它不再处于文档流中，所以它不占据空间，实际上覆盖住了框 2，使框 2 从视图中消失；   
而如果把所有三个框都向左移动，那么三个框都会脱离文档流，框 1 向左浮动直到碰到包含框，另外两个框向左浮动直到碰到前一个浮动框：   
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/20/519735da7245c0ac8154865250681d06.jpg)

如果包含框太窄，无法容纳水平排列的三个浮动元素，那么其它浮动块向下移动，直到有足够的空间。如果浮动元素的高度不同，那么当它们向下移动时可能被其它浮动元素“卡住”：   
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/20/46bee55d96dd709b52650cabe5e8d192.jpg)

### float 属性 ###
浮动布局使用 CSS 中的 float 属性来定义，其取值如下：
- none：默认值。元素不浮动，并会显示在其在文本中出现的位置。
- left：元素向左浮动。
- right：元素向右浮动。
- inherit：规定应该从父元素继承 float 属性的值。

### clear 属性 ###
clear 属性被用于控制浮动，浮动布局中可使用 clear 属性规定元素的哪一侧不允许其他浮动元素；

取值：
- none：默认值。允许浮动元素出现在两侧。
- left：在左侧不允许浮动元素。
- right：在右侧不允许浮动元素。
- both：在左右两侧均不允许浮动元素。
- inherit：规定应该从父元素继承 clear 属性的值。

### 实例 ###   
（PC Web 常用布局结构）     
圣杯布局（Holy Grail Layout）指的是一种最常见的网站布局。页面从上到下，分成三个部分：头部（header），躯干（body），尾部（footer）。其中躯干又水平分成三栏，从左到右为：导航、主栏、副栏。    
http://www.imooc.com/learn/57
```html
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title>混合布局</title>
    <style>
    body {
        margin: 0;
        padding: 0;
        font-size: 30px;
        font-weight: bold
    }

    div {
        text-align: center;
        line-height: 50px
    }

    .top {
        height: 100px;
        background: #9CF;
    }

    .head,
    .main {
        width: 960px;
        margin: 0 auto;
        height: 600px;
    }

    .head {
        height: 100px;
        background: #F90
    }

    .left {
        width: 220px;
        height: 600px;
        background: #ccc;
        float: left;
    }

    .right {
        width: 740px;
        height: 600px;
        background: #FCC;
        float: right
    }

    .r_sub_left {
        width: 540px;
        height: 600px;
        background: #9C3;
        float: left
    }

    .r_sub_right {
        width: 200px;
        height: 600px;
        background: #9FC;
        float: right;
    }

    .footer {
        height: 50px;
        background: #9F9;
        width: 960px;
        margin: 0 auto;
    }
    </style>
</head>

<body>
    <div class="top">
        <div class="head">head</div>
    </div>
    <div class="main">
        <div class="left">left</div>
        <div class="right">
            <div class="r_sub_left">sub_left</div>
            <div class=" r_sub_right">sub_right</div>
        </div>
    </div>
    <div class="footer">footer</div>
</body>

</html>
```
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/afa1ec968fd859827b8ec8758a7deb05.jpg)

## 弹性布局：Flex ##

http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html  
http://www.ruanyifeng.com/blog/2015/07/flex-examples.html  

### 概述 ###
传统布局模型（固定布局、流体布局）对于那些特殊布局非常不方便，比如，垂直居中就不容易实现；2009 年，W3C 提出了一种新的方案 ----Flex 布局，可以简便、完整、响应式地实现各种页面布局。目前，它已经得到了所有浏览器的支持，Flex 布局将成为未来布局的首选方案。

Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

新版 (flex) 与 旧版 (flexbox)
IOS 上支持使用最新的 flex 布局；
Android 4.4+ 才支持使用最新的 flex 布局；

对比：
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/efcd3305d502ac6edd33d60ed41e0243.jpg)

### 基本概念 ###
任何一个容器都可以指定为 Flex 布局，此后该容器内的所有子元素都成为 flex item：   
```css
.box{
  display: flex;
}
```

行内元素也可以使用 Flex 布局：   
```css
.box{
  display: inline-flex;
}
```
Webkit 内核的浏览器，必须加上 -webkit 前缀。

```css
.box{
  display: -webkit-flex; /* Safari */
  display: flex;
}
```
注意：设为 Flex 布局以后，子元素的 **float**、**clear** 和 **vertical-align** 属性都将失效；

采用 Flex 布局的元素，称为 flex container，它的所有子元素自动成为容器成员，称为 flex item；

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做 main start，结束位置叫做 main end；交叉轴的开始位置叫做 cross start，结束位置叫做 cross end；

项目默认沿主轴排列。单个项目占据的主轴空间叫做 main size，占据的交叉轴空间叫做 cross size；

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/20/8ba27544f05af9829f3eedcc5ea5b636.jpg)

#### container 属性 ####

- flex-direction
	- flex-direction 属性决定主轴的方向（即项目的排列方向）；
	- 取值
		- row（默认值）：主轴为水平方向，起点在左端；
		- row-reverse：主轴为水平方向，起点在右端；
		- column：主轴为垂直方向，起点在上沿；
		- column-reverse：主轴为垂直方向，起点在下沿；
- flex-wrap
	- 默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap 属性定义，如果一条轴线排不下，如何换行；
	- 取值
		- nowrap（默认）：不换行；
		- wrap：换行，换行后第一行在上方；
		- wrap-reverse：换行，换行后第一行在下方；
- flex-flow
	- flex-flow 属性是 flex-direction 属性和 flex-wrap 属性的简写形式，默认值为 row nowrap；
- justify-content
	- justify-content 属性定义了项目在主轴上的对齐方式；
	- 取值（具体对齐方式与轴的方向有关，下面假设主轴为从左到右）
		- flex-start（默认值）：左对齐；
		- flex-end：右对齐；
		- center： 居中；
		- space-between：两端对齐，项目之间的间隔都相等；
		- space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍；
- align-items
	- align-items 属性定义项目在交叉轴上如何对齐；
	- 取值（具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下）
		- flex-start：交叉轴的起点对齐；
		- flex-end：交叉轴的终点对齐；
		- center：交叉轴的中点对齐；
		- baseline: 项目的第一行文字的基线对齐；
		- stretch（默认值）：如果项目未设置高度或设为 auto，将占满整个容器的高度；
- align-content
	- align-content 属性定义了多根轴线的对齐方式，如果项目只有一根轴线，该属性不起作用；
	- 取值
		- flex-start：与交叉轴的起点对齐；
		- flex-end：与交叉轴的终点对齐；
		- center：与交叉轴的中点对齐；
		- space-between：与交叉轴两端对齐，轴线之间的间隔平均分布；
		- space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍；
		- stretch（默认值）：轴线占满整个交叉轴；

#### item 属性 ####
- order
	- order 属性定义项目的排列顺序，数值越小，排列越靠前，默认为 0；
- flex-grow
	- flex-grow 属性定义项目的放大比例，默认为 0，即如果存在剩余空间，也不放大；
	- 如果所有项目的 flex-grow 属性都为 1，则它们将等分剩余空间（如果有的话）。如果一个项目的 flex-grow 属性为 2，其他项目都为 1，则前者占据的剩余空间将比其他项多一倍，即每个 item 占容器空间的比例为 x / (x + n)；
- flex-shrink
	- flex-shrink 属性定义了项目的缩小比例，默认为 1，即如果空间不足，该项目将缩小；
	- 如果所有项目的 flex-shrink 属性都为 1，当空间不足时，都将等比例缩小。如果一个项目的 flex-shrink 属性为 0，其他项目都为 1，则空间不足时，前者不缩小；
	- 负值对该属性无效；
- flex-basis
	- flex-basis 属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为 auto，即项目的本来大小；
	- 它可以设为跟 width 或 height 属性一样的值（比如 350px），则项目将占据固定空间；
- flex
	- flex 属性是 flex-grow, flex-shrink 和 flex-basis 的简写，默认值为 0 1 auto。后两个属性可选；
	- 该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)；建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值；
- align-self
	- align-self 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性。默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch；
	- 该属性可能取 6 个值，除了 auto，其他都与 align-items 属性完全一致；

### 实例 ###
使用弹性布局实现不定高宽的垂直居中：   
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/934f1d7b4b3dedab23e8494c4e2d32f1.jpg)