

# 移动 WebApp 页面适配 #



https://www.cnblogs.com/wenzheshen/p/6589459.html   

http://web.jobbole.com/90084/



>   做过PC页面的人聊的最多的就是『兼容』，这是因为浏览器之间的差异引起的，不再多说。而移动端是基本没有『兼容』的问题的，全是CSS3，简直不要太开心。可是『适配』问题随之而来。
>
>   什么是『适配』？做PC页面的时候，我们按照设计图的尺寸来就好，这个侧边栏200px，那个按钮50px的。可是，当我们开始做移动端页面的时候，设计师给了一份宽度为640px的设计图。那么，我们把这份设计图实现在各个手机上的过程就是『适配』。



##基础要点

###meta标签：viewport



H5移动端页面自适应普遍使用的方法，理论上讲使用这个标签是可以适应所有尺寸的屏幕的，但是各设备对该标签的解释方式及支持程度不同造成了不能兼容所有浏览器或系统。   

viewport 是用户网页的可视区域，翻译为中文可以叫做"视区"。   

手机浏览器是把页面放在一个虚拟的"窗口"（viewport）中，通常这个虚拟的"窗口"（viewport）比屏幕宽，这样就不用把每个网页挤到很小的窗口中（这样会破坏没有针对手机浏览器优化的网页的布局），用户可以通过平移和缩放来看网页的不同部分。   



viewport标签极其属性：

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

属性说明：

| 属性名           | 取值                  | 描述                               |
| ------------- | ------------------- | -------------------------------- |
| width         | 正整数 或 device-width  | 定义视口的宽度，单位为像素                    |
| height        | 正整数 或 device-height | 定义视口的高度，单位为像素，一般不用               |
| initial-scale | [0.0-10.0]          | 定义初始缩放值                          |
| minimum-scale | [0.0-10.0]          | 定义缩小最小比例，它必须小于或等于maximum-scale设置 |
| maximum-scale | [0.0-10.0]          | 定义放大最大比例，它必须大于或等于minimum-scale设置 |
| user-scalable | yes/no              | 定义是否允许用户手动缩放页面，默认值yes            |



注意：尽管可设置多个属性，但从用户体验的角度上考虑，应避免设置这些属性值，允许用户缩放页面：

```css
<meta name="viewport" content="width=device-width,initial-scale=1.0">
```





### 流式布局：百分比

流式布局与固定布局是相对的，核心思想是使用百分比来设置页面各部分的宽度，百分比指的是父元素，如子元素宽度50%，那么父元素的宽度就是100%；

所以body默认宽度是屏幕宽度（PC中指的是浏览器宽度）子孙元素按百分比定位（或指定尺寸）就可以了，这只适合布局简单的页面，复杂的页面实现很困难。

为了确保页面兼容各种尺寸设备，页面种各模块宽度、内外边距以及边框的宽度设置必须是相对值，推荐使用百分比设置。



### CSS Media queries



媒体查询是 CSS3 引入的新特性，通过指定媒体类型的方式来限定所包含的样式的作用范围。

媒体查询的功能就是为不同的媒体设置不同的css样式，这里的“媒体”包括页面尺寸，设备屏幕尺寸等。



例：

```css
/* 智能手机 < 480px */
@media (max-width: 480px) {
}

/* 桌面 PC > 1024px */
@media (min-width: 1024px) {
}
```







例：如果浏览器窗口小于 500px, 背景将变为浅蓝色

```css
@media only screen and (max-width: 500px) {
    body {
        background-color: lightblue;
    }
}
```











### REM 

REM 是CSS3新增的一个相对单位（root em，根em）,使用rem为元素设定字体大小时，是相对大小，但相对的只是HTML根元素。通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。

目前，除了IE8及更早版本外，所有浏览器均已支持rem。对于不支持它的浏览器多写一个绝对单位的声明。这些浏览器会忽略用rem设定的字体大小：

```css
p {font-size:14px; font-size:.875rem;}
```

 

默认html的font-size是16px，即1rem=16px，如果某div宽度为32px你可以设为2rem。

通常情况下，为了便于计算数值则使用62.5%，即默认的10px作为基数。当然这个基数可以为任何数值，视具体情况而定。设置方法如下：

```css
Html{font-size:62.5%(10/16*100%)}
```

 

具体不同屏幕下的规则定义，即基数的定义方式：可以通过CSS定义，不同宽度范围里定义不同的基数值，当然也可以通过js一次定义方法如下：

```html
<script type="text/javascript">
   (function (doc, win) {
      var docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function () {
          var clientWidth = docEl.clientWidth;
          if (!clientWidth) return;
          docEl.style.fontSize = 20 * (clientWidth / 320) + 'px';//其中“20”根据你设置的html的font-size属性值做适当的变化
        };

      if (!doc.addEventListener) return;
      win.addEventListener(resizeEvt, recalc, false);
      doc.addEventListener('DOMContentLoaded', recalc, false);
    })(document, window);
</script>
```



rem也并不是万能的，有一些场景是不适于使用rem的：

当用作图片或者一些不能缩放的展示时，必须要使用固定的px值，因为缩放可能会导致图片压缩变形等。







### 其它注意点

####不要使用<iframe>





####谨慎使用<table>

尽量使用<ul>、<ol>等列表控件代替<table>来显示数据；





##适配方法

### 固定高度，宽度自适应

这是目前使用最多的方法，垂直方向用定值，水平方向用百分比或定值或 flex。[腾讯](http://xw.qq.com/index.htm)、[京东](http://m.jd.com/)、[百度](https://www.baidu.com/)、[天猫](https://www.tmall.com/)、[亚马逊](http://www.amazon.cn/)的首页都是使用的这种方法。

随着屏幕宽度变化，页面也会跟着变化，效果类似于PC页面的流体布局，在哪个宽度需要调整的时候使用_响应式布局_加以调整（比如[网易新闻](http://news.163.com/mobile/)），这样就实现了『适配』。



原理：

```html
<meta name="viewport" content="width=device-width,initial-scale=1">
```

这样设置之后，我们就可以不用管手机屏幕的尺寸进行开发了。



### 固定宽度，viewport缩放

设计图、页面宽度、viewport width使用一个宽度，浏览器帮我们完成缩放。单位使用px即可。

目前已知[荔枝FM](http://m.lizhi.fm/)、[网易新闻](http://c.3g.163.com/CreditMarket/default.html)在使用这种方法。



原理：

这种方法需要根据屏幕宽度来动态生成`viewport`，生成的 viewport 基本是这样：

```html
<meta name="viewport" content="width=640,initial-scale=0.5,maximum-scale=0.5,minimum-scale=0.5,user-scalable=no">
```

640 是我们根据设计图定下的，0.5 是根据屏幕宽度动态生成的。

生成的viewport告诉浏览器网页的布局视口使用 640px，然后把页面缩放成50%，这是绝对的等比例缩放。图片、文字等等所有元素都被缩放在手机屏幕中。





### REM 做宽度，viewport缩放

根据屏幕宽度设定 `rem` 值，需要适配的元素都使用 `rem` 为单位，不需要适配的元素还是使用 `px` 为单位。











































