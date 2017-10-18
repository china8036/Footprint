- [移动 WebApp 页面适配](#%E7%A7%BB%E5%8A%A8-webapp-%E9%A1%B5%E9%9D%A2%E9%80%82%E9%85%8D)
  - [基础要点](#%E5%9F%BA%E7%A1%80%E8%A6%81%E7%82%B9)
    - [meta 标签：viewport](#meta-%E6%A0%87%E7%AD%BE%EF%BC%9Aviewport)
    - [流式布局：百分比](#%E6%B5%81%E5%BC%8F%E5%B8%83%E5%B1%80%EF%BC%9A%E7%99%BE%E5%88%86%E6%AF%94)
    - [CSS Media queries](#css-media-queries)
    - [REM](#rem)
  - [适配方法实现](#%E9%80%82%E9%85%8D%E6%96%B9%E6%B3%95%E5%AE%9E%E7%8E%B0)
    - [固定高度，宽度自适应](#%E5%9B%BA%E5%AE%9A%E9%AB%98%E5%BA%A6%EF%BC%8C%E5%AE%BD%E5%BA%A6%E8%87%AA%E9%80%82%E5%BA%94)
    - [固定宽度，viewport 动态缩放](#%E5%9B%BA%E5%AE%9A%E5%AE%BD%E5%BA%A6%EF%BC%8Cviewport-%E5%8A%A8%E6%80%81%E7%BC%A9%E6%94%BE)
    - [REM 做宽度，viewport 缩放](#rem-%E5%81%9A%E5%AE%BD%E5%BA%A6%EF%BC%8Cviewport-%E7%BC%A9%E6%94%BE)
      - [阿里 REM 解决方案](#%E9%98%BF%E9%87%8C-rem-%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)
      - [淘宝 REM 解决方案](#%E6%B7%98%E5%AE%9D-rem-%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)
    - [rem 做宽度，动态设置 font-size   viewport 不缩放](#rem-%E5%81%9A%E5%AE%BD%E5%BA%A6%EF%BC%8C%E5%8A%A8%E6%80%81%E8%AE%BE%E7%BD%AE-font-size-viewport-%E4%B8%8D%E7%BC%A9%E6%94%BE)
      - [媒体查询设置 font-size](#%E5%AA%92%E4%BD%93%E6%9F%A5%E8%AF%A2%E8%AE%BE%E7%BD%AE-font-size)
      - [根据设备 clientWidth 动态设置 font-size](#%E6%A0%B9%E6%8D%AE%E8%AE%BE%E5%A4%87-clientwidth-%E5%8A%A8%E6%80%81%E8%AE%BE%E7%BD%AE-font-size)
      - [使用 vw 设置 font-size](#%E4%BD%BF%E7%94%A8-vw-%E8%AE%BE%E7%BD%AE-font-size)
    - [全局使用 vw、vh、vm](#%E5%85%A8%E5%B1%80%E4%BD%BF%E7%94%A8-vw%E3%80%81vh%E3%80%81vm)
  - [参考](#%E5%8F%82%E8%80%83)

# 移动 WebApp 页面适配 #

>   做过 PC 页面的人聊的最多的就是『兼容』，这是因为浏览器之间的差异引起的，不再多说。而移动端是基本没有『兼容』的问题的，全是 CSS3，简直不要太开心。可是『适配』问题随之而来。
>
>   什么是『适配』？做 PC 页面的时候，我们按照设计图的尺寸来就好，这个侧边栏 200px，那个按钮 50px 的。可是，当我们开始做移动端页面的时候，设计师给了一份宽度为 640px 的设计图。那么，我们把这份设计图实现在各个手机上的过程就是『适配』。

如设计师给的 UI 图上标注了某个段落的字体大小为 64px。为了保证在各种屏幕上的不失真，就要根据实际屏幕宽度做等比例换算，才能写进 CSS，即满足：

> 写入 CSS 的尺寸 / 屏幕宽度 = UI 图标注的尺寸 /UI 图宽度

因此：

>  写入 CSS 的尺寸 = UI 图标注的尺寸 /UI 图宽度*屏幕宽度

例，当在宽度为 1440px 的 UI 图上标注尺寸为 64px 时，针对不同屏幕宽度，写入 CSS 的属性分别为：
```
设备屏幕宽度 320px：font-size: 64/1440*320 = 14.22px
设备屏幕宽度 360px：font-size: 64/1440*360 = 16px
设备屏幕宽度 375px：font-size: 64/1440*375 = 16.67px
...
设备屏幕宽度 1440px：font-size: 64/1440*1440 = 64px
```

## 基础要点 ##

### meta 标签：viewport ###

H5 移动端页面自适应普遍使用的方法，理论上讲使用这个标签是可以适应所有尺寸的屏幕的，但是各设备对该标签的解释方式及支持程度不同造成了不能兼容所有浏览器或系统。   

viewport 是用户网页的可视区域，翻译为中文可以叫做"视区"。   

手机浏览器是把页面放在一个虚拟的"窗口"（viewport）中，通常这个虚拟的"窗口"（viewport）比屏幕宽，这样就不用把每个网页挤到很小的窗口中（这样会破坏没有针对手机浏览器优化的网页的布局），用户可以通过平移和缩放来看网页的不同部分。   

viewport 标签极其属性：

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

属性说明：

| 属性名           | 取值                  | 描述                               |
| ------------- | ------------------- | -------------------------------- |
| width         | 正整数 或 device-width  | 定义视口的宽度，单位为像素                    |
| height        | 正整数 或 device-height | 定义视口的高度，单位为像素，一般不用               |
| initial-scale | [0.0-10.0]          | 定义初始缩放值                          |
| minimum-scale | [0.0-10.0]          | 定义缩小最小比例，它必须小于或等于 maximum-scale 设置 |
| maximum-scale | [0.0-10.0]          | 定义放大最大比例，它必须大于或等于 minimum-scale 设置 |
| user-scalable | yes/no              | 定义是否允许用户手动缩放页面，默认值 yes            |

注意：
尽管可设置多个属性，但从用户体验的角度上考虑，应避免设置这些属性值，允许用户缩放页面：

```css
<meta name="viewport" content="width=device-width,initial-scale=1.0">
```

### 流式布局：百分比 ###

流式布局与固定布局是相对的，核心思想是使用百分比来设置页面各部分的宽度，百分比指的是父元素，如子元素宽度 50%，那么父元素的宽度就是 100%；

所以 body 默认宽度是屏幕宽度（PC 中指的是浏览器宽度）子孙元素按百分比定位（或指定尺寸）就可以了，这只适合布局简单的页面，复杂的页面实现很困难。

为了确保页面兼容各种尺寸设备，页面种各模块宽度、内外边距以及边框的宽度设置必须是相对值，推荐使用百分比设置。

### CSS Media queries ###

媒体查询是 CSS3 引入的新特性，通过指定媒体类型的方式来限定所包含的样式的作用范围。

媒体查询的功能就是为不同的媒体设置不同的 css 样式，这里的“媒体”包括页面尺寸，设备屏幕尺寸等。

媒体查询是响应式设计的核心技术。

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

### REM ###

REM 是 CSS3 新增的一个相对单位（root em，根 em）, 使用 rem 为元素设定字体大小时，是相对大小，但相对的只是 HTML 根元素。通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。

rem 就是相对于根元素<html>的 font-size 来做计算。

根据屏幕宽度设定 `rem` 值，需要适配的元素都使用 `rem` 为单位，不需要适配的元素还是使用 `px` 为单位。

比较：
rem 是相对于根元素 (<html>) 的字体大小，em 是相对于父元素的字体大小。

目前，除了 IE8 及更早版本外，所有浏览器均已支持 rem。对于不支持它的浏览器多写一个绝对单位的声明。这些浏览器会忽略用 rem 设定的字体大小：
```css
p {font-size:14px; font-size:.875rem;}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/19/232af31012a0894ff3f12aabedfd441b.jpg)

默认 html 的 font-size 是 16px，即 1rem=16px，如果某 div 宽度为 32px 你可以设为 2rem。

通常情况下，为了便于计算数值则使用 62.5%，即默认的 10px 作为基数。当然这个基数可以为任何数值，视具体情况而定。设置方法如下：

```css
Html{font-size:62.5%(10/16*100%)}
```

具体不同屏幕下的规则定义，即基数的定义方式：可以通过 CSS 媒体查询，在不同宽度范围里定义不同的基数值：
![](http://img.caibaojian.com/uploads/2016/08/Center5)

更加严谨的做法是，通过 js 获取设备的屏幕分辨率，计算出相应的字体的大小，以适配所有屏幕的大小，代码如下：

```html
<script type="text/javascript">
   (function (doc, win) {
      var docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function () {
          var clientWidth = docEl.clientWidth;
          if (!clientWidth) return;
          docEl.style.fontSize = 20 * (clientWidth / 320) + 'px';// 其中“20”根据你设置的 html 的 font-size 属性值做适当的变化
        };

      if (!doc.addEventListener) return;
      win.addEventListener(resizeEvt, recalc, false);
      doc.addEventListener('DOMContentLoaded', recalc, false);
    })(document, window);
</script>
```

rem 也并不是万能的，有一些场景是不适于使用 rem 的：   
PC 端浏览器对 rem 单位支持并不友好，因此 rem 单位最好不用在 PC 端；   
一般 font-size 不应该使用 rem 等相对单位，因为字体大小趋向于阅读实用性，并不适合排版布局；   
类似的，当用作图片或者一些不能缩放的展示时，应使用固定的 px 值，因为缩放可能会导致图片压缩变形等；

## 适配方法实现 ##

### 固定高度，宽度自适应 ###

这是目前使用最多的方法，垂直方向用定值，水平方向用百分比或定值或 flex。[腾讯](http://xw.qq.com/index.htm)、[京东](http://m.jd.com/)、[百度](https://www.baidu.com/)、[天猫](https://www.tmall.com/)、[亚马逊](http://www.amazon.cn/) 的首页都是使用的这种方法。

随着屏幕宽度变化，页面也会跟着变化，效果类似于 PC 页面的流体布局，在哪个宽度需要调整的时候使用_响应式布局_加以调整（比如[网易新闻](http://news.163.com/mobile/)），这样就实现了『适配』。

原理：

```html
<meta name="viewport" content="width=device-width,initial-scale=1">
```

这样设置之后，我们就可以不用管手机屏幕的尺寸进行开发了。

### 固定宽度，viewport 动态缩放 ###

设计图、页面宽度、viewport width 使用一个宽度，浏览器帮我们完成缩放。单位使用 px 即可。

目前已知[荔枝 FM](http://m.lizhi.fm/)、[网易新闻](http://c.3g.163.com/CreditMarket/default.html) 在使用这种方法。

原理：

这种方法需要根据客户端 dpr 来动态生成`viewport`：
```javascript
var scale = 1 / window.devicePixelRatio;
document.querySelector('meta[name="viewport"]').setAttribute('content','initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
```

这样设置之后，客户端的 css 像素将自动缩放成为与设计图完全等比例的大小，代码中的 css 像素可以直接按照设计图的标注尺寸写死，使用 px 单位，不需要什么 rem，也不需要 vm，在不同的屏幕下会自动缩放。

缺点：     
很多安卓机型和浏览器不支持 viewport 设置，并且内联文本元素是无法缩放的。

### REM 做宽度，viewport 缩放 ###
https://www.zhihu.com/question/35710806

注意：
- **绝不是每个地方都要用 rem，rem 只适用于固定尺寸，在很多布局情境中（比如底部导航元素平分屏幕宽，大尺寸元素），必须使用百分比或者 flex 才能完美布局，rem 适合用于随屏幕伸缩的尺寸，而正文文字大小、细边框等不宜随屏幕伸缩的尺寸应该使用固定尺寸 px；**  
  - 考虑到字体的点阵信息，一般文字尺寸多会采用 16px 20px 24px 等值，若以 rem 指定文字尺寸，会产生诸如 21px，19px 这样的值，会导致字形难看，毛刺，甚至黑块，故大部分文字应该以 px 设置。但一般标题类文字，可能也有要求随屏幕缩放，且考虑到这类文字一般都比较大，超过 30px 的话，也可以用 rem 设置字体。
- **用于 REM 计算的 js 的加载最好放在 `<head>` 标签中，必须在其它 js 加载之前加载；**   

#### 阿里 REM 解决方案 ####

加载 js：
```html
<script>!function(e){function t(a){if(i[a])return i[a].exports;var n=i[a]={exports:{},id:a,loaded:!1};return e[a].call(n.exports,n,n.exports,t),n.loaded=!0,n.exports}var i={};return t.m=e,t.c=i,t.p="",t(0)}([function(e,t){"use strict";Object.defineProperty(t,"__esModule",{value:!0});var i=window;t["default"]=i.flex=function(normal,e,t){var a=e||100,n=t||1,r=i.document,o=navigator.userAgent,d=o.match(/Android[\S\s]+AppleWebkit\/(\d{3})/i),l=o.match(/U3\/((\d+|\.){5,})/i),c=l&&parseInt(l[1].split(".").join(""),10)>=80,p=navigator.appVersion.match(/(iphone|ipad|ipod)/gi),s=i.devicePixelRatio||1;p||d&&d[1]>534||c||(s=1);var u=normal?1:1/s,m=r.querySelector('meta[name="viewport"]');m||(m=r.createElement("meta"),m.setAttribute("name","viewport"),r.head.appendChild(m)),m.setAttribute("content","width=device-width,user-scalable=no,initial-scale="+u+",maximum-scale="+u+",minimum-scale="+u),r.documentElement.style.fontSize=normal?"50px": a/2*s*n+"px"},e.exports=t["default"]}]);  flex(false,100, 1);</script>
```

此方案默认 1rem = 100px，因此布局的时候，完全可以按照设计师给你的效果图写各种尺寸：
效果图上量取的某个按钮元素长 55px, 宽 37px ，那么可以这样写样式：
```css
.myBtn {
   width: 0.55rem;
   height: 0.37rem;
}
```

加载 js 后，无需再手动设置 viewport，该方案已帮你设置；

#### 淘宝 REM 解决方案 ####
https://github.com/amfe/article/issues/17   

此方案使用 JavaScript 根据设备 DPR 动态设置 scale 并且动态计算 font-size 的值；
- 根据设备 DPR 动态设置 scale
  ```javascript
  var scale = 1 / window.devicePixelRatio;
  document.querySelector('meta[name="viewport"]').setAttribute('content','initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
  ```
- 动态计算 html 的 font-size
```javascript
document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px';
```

也可以直接加载淘宝的 flexible 方案：将代码中的{{version}}换成对应的版本号 0.3.4（强烈建议对 JS 做内联处理，在所有资源加载之前执行这个 JS)
```html
<script src="http://g.tbcdn.cn/mtb/lib-flexible/{{version}}/??flexible_css.js,flexible.js"/>
```
执行这个 JS 后，会在<html>元素上增加一个 data-dpr 属性，以及一个 font-size 样式。JS 会根据不同的设备添加不同的 data-dpr 值，比如说 2 或者 3，同时会给 html 加上对应的 font-size 的值，比如说 75px。     
如此一来，页面中的元素，都可以通过 rem 单位来设置。他们会根据 html 元素的 font-size 值做相应的计算，从而实现屏幕的适配效果。

布局的时候，各元素的 css 尺寸 = 设计稿标注尺寸 / 设计稿横向分辨率 /10；     
例，**如果设计稿是 750 的（一般设计稿是 640px 或者 750px（现在最流行）），则 html 的 font-size 就是 750/10=75，若该设计稿上某个元素是 150px 的宽，换算成 rem 就是 150 / 75 = 2rem**。

优点：  
能维持能整体的布局效果，移动端兼容性好，不用写多个 css 代码，而且还可以利用 @media 进行优化。

缺点：   
开头要引入一段 js 代码，单位都要改成 rem(font-size 可以用 px)，计算 rem 比较麻烦（可以引用预处理器，但是增加了编译过程，相对麻烦了点)。pc 和 mobile 要分开。

###  rem 做宽度，动态设置 font-size   viewport 不缩放

https://www.cnblogs.com/gymmer/p/6883063.html

http://www.zhangxinxu.com/wordpress/2016/08/vw-viewport-responsive-layout-typography/

#### 媒体查询设置 font-size

<!-- TODO 如果用这种方案怎么计算 css 像素？？？ -->

使用媒体查询设置 html 的 font-size：
```less
// 对 html 做媒体查询，定义基准的 font-size
html {
    font-size: 16px;
}
// 使用 vw 作为单位进行媒体查询，使 font-size 变化更平滑
@media screen and (min-width: 375px) {
    html {
        /* iPhone6 的 375px 尺寸作为 16px 基准，414px 正好 18px 大小，600 20px */
        font-size: calc(100% + 2 * (100vw - 375px) / 39);// 百分比是为了适配 Safari 浏览器
        font-size: calc(16px + 2 * (100vw - 375px) / 39);
    }
}
@media screen and (min-width: 414px) {
    html {
        /* 414px-1000px 每 100 像素宽字体增加 1px(18px-22px) */
        font-size: calc(112.5% + 4 * (100vw - 414px) / 586);
        font-size: calc(18px + 4 * (100vw - 414px) / 586);
    }
}
@media screen and (min-width: 600px) {
    html {
        /* 600px-1000px 每 100 像素宽字体增加 1px(20px-24px) */
        font-size: calc(125% + 4 * (100vw - 600px) / 400);
        font-size: calc(20px + 4 * (100vw - 600px) / 400);
    }
}
@media screen and (min-width: 1000px) {
    html {
        /* 1000px 往后是每 100 像素 0.5px 增加 */
        font-size: calc(137.5% + 6 * (100vw - 1000px) / 1000);
        font-size: calc(22px + 6 * (100vw - 1000px) / 1000);
    }
}

// CSS 单位使用 rem
p.intro {
    font-size: 1rem;
}
```

#### 根据设备 clientWidth 动态设置 font-size

```javascript
<script type="text/javascript">
  (function (doc, win) {
    var docEl = doc.documentElement,
      resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
      recalc = function () {
        var clientWidth = docEl.clientWidth;
        if (!clientWidth) return;
        docEl.style.fontSize = clientWidth / 7.5 + 'px';
      };

    if (!doc.addEventListener) return;
    win.addEventListener(resizeEvt, recalc, false);
    doc.addEventListener('DOMContentLoaded', recalc, false);
  })(document, window);
</script>
```
若设计图以 ip6 为标准，则设计图 width=750，因为为了方便写代码时计算 css 像素的 rem 值，将客户端 html 的 font-size 动态设置为
```javascript
docEl.style.fontSize = clientWidth / 7.5 + 'px'
```
则**当要实现设计图上尺寸为 x px 的目标时，代码中的 css 像素为 x/100 rem**。

解释：
```
要实现：         
客户端尺寸 / 客户端宽度 = 设计图尺寸 / 设计图宽度 = 设计图尺寸 /750 = 设计图尺寸 /(100*7.5)
因此：      
客户端尺寸 = (客户端宽度 /7.5) * (设计图尺寸 /100) = 1 rem * (设计图尺寸 /100) 
```
#### 使用 vw 设置 font-size

<!-- TODO 如果用这种方案怎么计算 css 像素？？？ -->

```html
html{
  font-size:5vw;
}
```
以此为基准，全局使用 rem 布局

5vw 在 320（iphone4/iphone5） 屏幕上面 刚好是 16px ，也就是 1rem=16px

于是

在 iphone4/iphone5 1rem=16px      
在 iphone6 1rem = 18.75px      
在 iphone6 plus 1rem = 20.7px  

则**当要实现设计图上尺寸为 x px 的目标时，代码中的 css 像素为 x/37.5 rem**。

### 全局使用 vw、vh、vm

缺点：目前兼容性还不如 rem；

## 参考 ##
https://www.cnblogs.com/wenzheshen/p/6589459.html   

http://web.jobbole.com/90084/

http://caibaojian.com/toutiao/6215

http://www.jianshu.com/p/985d26b40199

https://zhuanlan.zhihu.com/p/26141351

https://www.w3cplus.com/css/vw-for-layout.html

http://www.alloyteam.com/2016/03/mobile-web-adaptation-tool-rem/ 
