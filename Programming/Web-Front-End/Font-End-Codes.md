- [JavaScript 实用代码段](#javascript-%E5%AE%9E%E7%94%A8%E4%BB%A3%E7%A0%81%E6%AE%B5)
  - [防止网页被 iframe 引用](#%E9%98%B2%E6%AD%A2%E7%BD%91%E9%A1%B5%E8%A2%AB-iframe-%E5%BC%95%E7%94%A8)
  - [页面跳转](#%E9%A1%B5%E9%9D%A2%E8%B7%B3%E8%BD%AC)
  - [使 input 输入框不能编辑](#%E4%BD%BF-input-%E8%BE%93%E5%85%A5%E6%A1%86%E4%B8%8D%E8%83%BD%E7%BC%96%E8%BE%91)
  - [文字超出限制显示省略号](#%E6%96%87%E5%AD%97%E8%B6%85%E5%87%BA%E9%99%90%E5%88%B6%E6%98%BE%E7%A4%BA%E7%9C%81%E7%95%A5%E5%8F%B7)
  - [Android 浏览器下 line-height 垂直居中偏离问题](#android-%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%8B-line-height-%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD%E5%81%8F%E7%A6%BB%E9%97%AE%E9%A2%98)
  - [用伪元素实现文字两端均分对齐](#%E7%94%A8%E4%BC%AA%E5%85%83%E7%B4%A0%E5%AE%9E%E7%8E%B0%E6%96%87%E5%AD%97%E4%B8%A4%E7%AB%AF%E5%9D%87%E5%88%86%E5%AF%B9%E9%BD%90)
  - [通过 `:empty` 选择器区分样式](#%E9%80%9A%E8%BF%87-empty-%E9%80%89%E6%8B%A9%E5%99%A8%E5%8C%BA%E5%88%86%E6%A0%B7%E5%BC%8F)
  - [实现绝对底部（Sticky Footer）](#%E5%AE%9E%E7%8E%B0%E7%BB%9D%E5%AF%B9%E5%BA%95%E9%83%A8%EF%BC%88sticky-footer%EF%BC%89)
  - [input 标签相关样式 Reset](#input-%E6%A0%87%E7%AD%BE%E7%9B%B8%E5%85%B3%E6%A0%B7%E5%BC%8F-reset)
  - [使用：invalid 实现输入不合法时改变按钮样式](#%E4%BD%BF%E7%94%A8%EF%BC%9Ainvalid-%E5%AE%9E%E7%8E%B0%E8%BE%93%E5%85%A5%E4%B8%8D%E5%90%88%E6%B3%95%E6%97%B6%E6%94%B9%E5%8F%98%E6%8C%89%E9%92%AE%E6%A0%B7%E5%BC%8F)
  - [隐藏一个元素的实现方法](#%E9%9A%90%E8%97%8F%E4%B8%80%E4%B8%AA%E5%85%83%E7%B4%A0%E7%9A%84%E5%AE%9E%E7%8E%B0%E6%96%B9%E6%B3%95)
  - [三角形画法](#%E4%B8%89%E8%A7%92%E5%BD%A2%E7%94%BB%E6%B3%95)
  - [圆形画法](#%E5%9C%86%E5%BD%A2%E7%94%BB%E6%B3%95)
  - [居中](#%E5%B1%85%E4%B8%AD)
    - [水平居中](#%E6%B0%B4%E5%B9%B3%E5%B1%85%E4%B8%AD)
    - [垂直居中](#%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD)
    - [水平垂直居中](#%E6%B0%B4%E5%B9%B3%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD)
  - [图片上传相关](#%E5%9B%BE%E7%89%87%E4%B8%8A%E4%BC%A0%E7%9B%B8%E5%85%B3)
    - [HTML5 调用手机摄像机、相册功能](#html5-%E8%B0%83%E7%94%A8%E6%89%8B%E6%9C%BA%E6%91%84%E5%83%8F%E6%9C%BA%E3%80%81%E7%9B%B8%E5%86%8C%E5%8A%9F%E8%83%BD)
    - [使用 canvas 进行图片压缩](#%E4%BD%BF%E7%94%A8-canvas-%E8%BF%9B%E8%A1%8C%E5%9B%BE%E7%89%87%E5%8E%8B%E7%BC%A9)
  - [使用 history 和 window.onpopstate 让 ajax 分页支持前进、后退](#%E4%BD%BF%E7%94%A8-history-%E5%92%8C-windowonpopstate-%E8%AE%A9-ajax-%E5%88%86%E9%A1%B5%E6%94%AF%E6%8C%81%E5%89%8D%E8%BF%9B%E3%80%81%E5%90%8E%E9%80%80)
  - [不指定高度，实现固定 & 自适应的高宽比](#%E4%B8%8D%E6%8C%87%E5%AE%9A%E9%AB%98%E5%BA%A6%EF%BC%8C%E5%AE%9E%E7%8E%B0%E5%9B%BA%E5%AE%9A-%E8%87%AA%E9%80%82%E5%BA%94%E7%9A%84%E9%AB%98%E5%AE%BD%E6%AF%94)
    - [固定高宽比](#%E5%9B%BA%E5%AE%9A%E9%AB%98%E5%AE%BD%E6%AF%94)
      - [使用 max-height](#%E4%BD%BF%E7%94%A8-max-height)
      - [使用基于宽度的百分比来设置 padding](#%E4%BD%BF%E7%94%A8%E5%9F%BA%E4%BA%8E%E5%AE%BD%E5%BA%A6%E7%9A%84%E7%99%BE%E5%88%86%E6%AF%94%E6%9D%A5%E8%AE%BE%E7%BD%AE-padding)
        - [padding](#padding)
        - [padding & 伪元素](#padding-%E4%BC%AA%E5%85%83%E7%B4%A0)
        - [padding & calc()](#padding-calc)
        - [padding & CSS 变量](#padding-css-%E5%8F%98%E9%87%8F)
      - [使用视窗单位 vw](#%E4%BD%BF%E7%94%A8%E8%A7%86%E7%AA%97%E5%8D%95%E4%BD%8D-vw)
        - [vw](#vw)
        - [vw & CSS Grid](#vw-css-grid)
    - [自适应高宽比](#%E8%87%AA%E9%80%82%E5%BA%94%E9%AB%98%E5%AE%BD%E6%AF%94)
  - [Retina 屏中实现 1px 的 border](#retina-%E5%B1%8F%E4%B8%AD%E5%AE%9E%E7%8E%B0-1px-%E7%9A%84-border)
  - [outline 圆角实现](#outline-%E5%9C%86%E8%A7%92%E5%AE%9E%E7%8E%B0)
  - [Android 平台中弹出虚拟键盘对布局的影响](#android-%E5%B9%B3%E5%8F%B0%E4%B8%AD%E5%BC%B9%E5%87%BA%E8%99%9A%E6%8B%9F%E9%94%AE%E7%9B%98%E5%AF%B9%E5%B8%83%E5%B1%80%E7%9A%84%E5%BD%B1%E5%93%8D)
    - [破坏 fixed 布局](#%E7%A0%B4%E5%9D%8F-fixed-%E5%B8%83%E5%B1%80)
    - [改变 viewport，导致 vh 变小](#%E6%94%B9%E5%8F%98-viewport%EF%BC%8C%E5%AF%BC%E8%87%B4-vh-%E5%8F%98%E5%B0%8F)
  - [输入长度限制监控](#%E8%BE%93%E5%85%A5%E9%95%BF%E5%BA%A6%E9%99%90%E5%88%B6%E7%9B%91%E6%8E%A7)
    - [非直接的文字输入](#%E9%9D%9E%E7%9B%B4%E6%8E%A5%E7%9A%84%E6%96%87%E5%AD%97%E8%BE%93%E5%85%A5)
    - [emoji 表情的输入](#emoji-%E8%A1%A8%E6%83%85%E7%9A%84%E8%BE%93%E5%85%A5)
  - [去除容器内 img 下方的空白](#%E5%8E%BB%E9%99%A4%E5%AE%B9%E5%99%A8%E5%86%85-img-%E4%B8%8B%E6%96%B9%E7%9A%84%E7%A9%BA%E7%99%BD)
  - [表单输入验证](#%E8%A1%A8%E5%8D%95%E8%BE%93%E5%85%A5%E9%AA%8C%E8%AF%81)
    - [validity 对象](#validity-%E5%AF%B9%E8%B1%A1)
    - [实践](#%E5%AE%9E%E8%B7%B5)
  - [平滑滚动的实现](#%E5%B9%B3%E6%BB%91%E6%BB%9A%E5%8A%A8%E7%9A%84%E5%AE%9E%E7%8E%B0)
    - [通过CSS实现](#%E9%80%9A%E8%BF%87css%E5%AE%9E%E7%8E%B0)
    - [通过JavaScript实现](#%E9%80%9A%E8%BF%87javascript%E5%AE%9E%E7%8E%B0)

# JavaScript 实用代码段

## 防止网页被 iframe 引用

http://www.ruanyifeng.com/blog/2008/10/anti-frameset_javascript_codes.html

http://www.ruanyifeng.com/blog/2010/08/anti-frameset_javascript_codes_continued.html

使用 iframe 可以将别的网站嵌入当前页面中。

1. 为什么反对这种做法？

    　　1）故意屏蔽了被嵌入网页的网址，侵犯了原作者的著作权，以及访问者的知情权；

    　　2）大量业者使用的是不可见框架，使得框架网页与被嵌入的网页视觉上完全相同，欺骗性极高；

    　　3）不良业者在被嵌入网页的上方或周围附加广告（甚至病毒和木马），不仅破坏原作者的设计意图和形象，而且属于侵权利用他人资源的谋利行为；

    　　4）如果访问者在框架内部，从一个网页点击到另一个网页，浏览器的地址栏是不变的，这是很差的用户体验，并且访问者会将这种体验归咎于原网页的作者。

2. 如果确有必要，将他人的网页嵌入自己的框架，那么应该同时满足以下三个条件：（即参考 Google 的做法）

    　　A. 在框架网页的醒目位置，清楚地说明该网页使用了框架技术，并明确列出原网页的 URL 网址。

    　　B. 在框架网页的醒目位置，向访问者提供"移除框架"的功能。

    　　C. 不得附加任何广告或恶意代码。

3. 防止网页被嵌入的 JavaScript 代码：
    ```javascript
    // 只要将其放入网页源码的头部，那些流氓就没有办法使用你的网页了
    try{
    　　top.location.hostname;// 浏览器的同源政策不允许 222.com 的网页操作 111.com 的网页，因此若不同域名，这里会抛出权限异常

    　　if (top.location.hostname != window.location.hostname) {// 适配 chrome 的代码
    　　　　top.location.href =window.location.href;
    　　}
    }
    catch(e){
    　　top.location.href = window.location.href;// 检测到不同域名，即被恶意引用，将 top 对象的网址自动导向被嵌入网页的网址
    }
    ```

## 页面跳转

http://blog.csdn.net/ithomer/article/details/7861313 

## 使 input 输入框不能编辑

http://blog.sina.com.cn/s/blog_69e220850100pyg2.html

1. 使用 JavaScript 实现

    使输入框无法获得焦点，不能编辑表单可以获得值，但可以复制。

    ```html
    <input type="text" value="fisker" onfocus="this.blur()"/>
    ```

2. 使用 input 标签的 readonly 属性

    输入框只读。不能编辑，同样表单可以获得值，也可以复制。

    ```html
    <input type="text" value="fisker" readonly />
    ```

3. 使用 input 标签的 disabled 属性

    输入框灰色，不能编辑，可以用 JS 改变或获得其值，但提交时并不提交该值。

    ```html
    <input type="text" value="fisker" disabled />
    ```

## 文字超出限制显示省略号

https://github.com/ruansongsong/H5Skills/blob/master/resources/textoverflow.md

单行文字超出显示省略号：
```css
.example {
    width: 200px;
    overflow: hidden;
    text-overflow: ellipsis;  /* 超过容器部分显示省略号 */
    white-space: nowrap; /* 使文本在一行上显示全部，不加此项效果出不来 */
}
```
多行文字超出显示省略号：
```css
.example {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2; /* 指定超过 2 行省略号 */
    text-overflow: ellipsis;
    overflow: hidden;
}
```

## Android 浏览器下 line-height 垂直居中偏离问题

https://github.com/ruansongsong/H5Skills/blob/master/resources/android_lineheight.md

移动端网页开发中，经常会涉及到一些小按钮或者小标签，比如这种：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/4/e8656025a650f948e7e182a8131fa1fd.jpg)

对于一般 PC 浏览器以及 iOS 设备的浏览器表现就是我们想要的居中效果，但是大部分 Android 设备的浏览器文字都会稍微向上偏离，如下图所示：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/4/3664a4fe64388fa5363cafc597c1d083.jpg)

测试表明，字体字号为奇数的两倍时（比如 10px、14px、18px、22px、26px），会出现严重偏移现象。

其实系统之间效果的差异跟我们的字体类型、系统排版引擎、浏览器都有关系，其实不影响用户浏览与操作等体验，我们可以忽略这些问题，对于一些居于使用场景偏离的比较明显的，可以使用下面提到的两种处理方案。

方案一：

我们可以通过 transform: scale 来处理，比如，字体大小是 8px，我们把字体设定为 16px，然后通过 scale(0.5) 缩放至一倍大小，简单粗暴。

注意：放大两倍会使得容器被撑开占位。

方案二：

结合行高、对齐的关系，结合伪元素得出的黑科技，亲测效果很理想。
```css
.jd::before {
    content: '';
    display: inline-block;
    vertical-align: middle;
    width: 0;
    height: 100%;
    margin-top: 1px;
}
```

## 用伪元素实现文字两端均分对齐

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/168932f5dc1950a4566707ea2c4f1070.jpg)

要实现以上效果，可采用 flex 布局的 `justify-content: space-between;`属性实现两端对齐，还可以通过伪元素来实现：
```html
<div class="jd">J D G W</div>
```

```css
.jd {
    display: block;
    text-align: justify;
}
.jd::after {
    content: '';
    display: inline-block;
    width: 100%;
}
```

## 通过 `:empty` 选择器区分样式

https://github.com/ruansongsong/H5Skills/blob/master/resources/empty.md

`:empty` 选择器匹配没有子元素（包括文本节点）的每个元素。

例：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/0ee5bce6b104b6d2810f2c0b1e37775a.jpg)

小红点有内容以及无内容的样式差异，按照常规的处理方式，我们一般是通过类名区分，但可以简单通过 `:empty` 选择器来区分：

```html
<div class="jd"><i>3</i></div>
```
```css
.jd i:empty {
    // 无内容的小红点样式
}
.jd i:not(:empty) {
    // 有内容的小红点样式
}
```

## 实现绝对底部（Sticky Footer）

https://github.com/ruansongsong/H5Skills/blob/master/resources/sticky_footer.md

Sticky Footer 效果：

如果页面内容不足够长时，页脚固定在浏览器窗口的底部；如果内容足够长时，页脚固定在页面的最底部。 总而言之，就是页脚一直处于最底：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/60aa41facb864cb49be756a8363b6c3c.jpg)

页面结构：
```html
<div class="wrapper">
    <div class="content"><!-- 页面主体内容区域 --></div>
    <div class="footer"><!-- 需要做到 Sticky Footer 效果的页脚 --></div>
</div>
```

说明：使用这个布局的前提，就是 footer 要在总的 div 容器之外，footer 使用一个层，其它所有内容使用一个总的层。如果确实需要到添加其它同级层，那这个同级层就必须使用 position:absolute 进行绝对定位。

- 方案一：absolute

  使得页脚一直定位在主容器预留占位位置即可：
  ```css
  html, body {
      height: 100%;
  }
  .wrapper {
      position: relative;
      min-height: 100%;
      padding-bottom: 50px;
      box-sizing: border-box;
  }
  .footer {
      position: absolute;
      bottom: 0;
      height: 50px;
  }
  ```
  需要指定 html、body 100% 的高度，且 content 的 padding-bottom 需要与 footer 的 height 一致。

- 方案二：calc

  通过计算函数 calc 计算（视窗高度 - 页脚高度）赋予内容区最小高度，不需要任何额外样式处理，代码量最少、最简单，但需要考虑 calc 和 vh 的兼容问题：
  ```css
  .content {
      min-height: calc(100vh - 50px);
  }
  .footer {
      height: 50px;
  }
  ```
  footer 的高度值需要与 content 其中的计算值一致。

- 方案三：Flex

  ```css
  html {
      height: 100%;
  }
  body {
      min-height: 100%;
      display: flex;
      flex-direction: column;
  }
  .content {
      flex: 1;
  }
  ```

##  input 标签相关样式 Reset

- 取消 input 默认样式
  ```css
  input {
    border: none;
    background: none;
    appearance: none;
    border-radius: 0;
  }
  ```

  - 关闭 iOS 默认输入法首字母大写
  ```html
  <input type="text" autocapitalize="off">
  ```

- 设置 placeholder 样式
  ```css
  /*css*/
  .example::-webkit-input-placeholder {
    color: red;
  }
  ```

  ```less
  //less
  input {
    &::placeholder {
      color: #7c7c7c;
    }
  }
  ```

- 去除 Chrome 中 input 被 focus 时出现的橙色 border
  ```less
  //less
  input,
  textarea {
    &:focus {
      outline: none;
    }
  }
  ```

- 去除 chrome 中表单自动填充导致出现的黄色背景
  ```less
  //less
  input:-webkit-autofill,
  input:-webkit-autofill:hover,
  input:-webkit-autofill:focus {
    box-shadow: 0 0 0 50px white inset;
    -webkit-text-fill-color: #333;
  }
  ```

## 使用：invalid 实现输入不合法时改变按钮样式

http://yincheng.site/using-html-css-instead-of-js

![image](http://yincheng.site/wp-content/uploads/2016/09/7.gif)

实现：通过 input 的 type 和 pattern 属性约束合法性，然后触发：invalid
```html
<style>
  input[type=email]:invalid + .next-step{
    opacity: 0.5;
  }
</style>
<input type="email">
<span class="next-step">Next</span>
```

## 隐藏一个元素的实现方法

- 方法一：
    ```css
    display: none;
    ```
    该方式会将元素从 DOM 中隐藏，不占 DOM 任何空间

- 方法二：
    ```css
    position: absolute;
    left: -9999999px;
    ```
    该方式通过偏移，将元素从 DOM 中移除

- 方法三
    ```css
    visibility: hidden;
    ```
    该方式设置元素不显示，但仍在 DOM 中存在，仍占据 DOM 空间

- 方法四：
    ```css
    opacity: 0
    ```
    类似方法三，设置元素完全透明，但仍在 DOM 中存在，仍占据 DOM 空间

## 三角形画法

http://yincheng.site/css-triangle

```css
div:after {
  content: "";
  width: 0;
  height: 0;
  overflow: hidden;
  border: 20px solid transparent;
  border-bottom-color: red;
}
```



## 圆形画法

将 border-rasius 的属性值设置为元素几何中心到顶点的距离，即：
```css
border-radius: 50%
```

## 居中

http://yincheng.site/css-align

### 水平居中

- 方法一
  
  使用 absolute 和 margin 负值实现
  ```css
  div {
    width: 200px;
    height: 200px;
    background: #000;
    position: absolute;
    left: 50%;
    margin-left: -100px;/*将被 div 本身 width 占据的空间拉回来，实现居中*/

  }
  ```

- 方法二

  容器 text-align:center，并固定元素 width，左右 margin 为 auto
  ```less
  div {
    text-align: center;
    img {
      width: 200px;
      margin: 0 auto;/*浏览器就会自动设置左右的 margin 值为容器剩余宽度的一半*/
    }
  }
  ```
  NOTE：这是最常见的水平居中方法，不仅适用于块元素也适用于行内元素，但这个办法对垂直居中不适用。

### 垂直居中

- 方法一

  通过将父元素设置为`table-cell`实现垂直居中

  ```css
  .div-container{ 
    display: table-cell;
    vertical-align: middle;
  }
  ```
  使用 table-cell 的缺点是容器的 magin 属性失效了，因为 margin 不适用于表格布局。所以如果要把 container 居中的话，使用 margin: 0 auto 就不起作用了。解决的办法是在 container 外层再套多一个容器，然后让这个 display 为 block 的容器 margin: 0 auto 就可以了。

  另外一个缺点是 table-cell 的元素设置宽高为百分比的时候将不起作用，常见的场景是要将宽度设置为外层容器宽度的 100%，解决办法是将 container 的宽度设置成一个很大的值，例如 3000px，这样就达到 100% 的目的。还有一个问题是不兼容 IE6/7，但现在生产环境基本上不需要兼容 IE6/7 了。

- 方法二

  使用 relative 定位，设置 top 为 50%，再设置 margin-top 为元素高度的负的一半
  ```html
  <style> 
      .mask{
          position: absolute; width: 100%; height: 100%;
      }
    .outer{
          position: relative;
          top: 50%;
          left: 50%;
          margin-top: -100px;
          margin-left: -100px;
      }
      .my-container{
          width: 200px;
          height: 200px;
          display: table-cell;
          vertical-align: middle;
      }
  </style>
  <div class="mask">
      <div class="outer">
          <div class="my-container">
  
          </div>
      </div>
  </div>
  ```

- 方法三

  使用 `transform: translate(0, -50%)`
  ```css
  .outer{
    position: relative;
    top: 50%;
    left: 50%;
    width: 200px;
    transform: translate(-50%, -50%);
  }
  ```

  方法二和三：margin-top 设置负值和 translate -50% 都有一个潜在的弊端，就是如果设置 left 为 50% 是借助 position 为 absolute 的话，可能会导致换行。

### 水平垂直居中

- 方法一

  使用 flex 布局
  ```css
  .container {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  ```

## 图片上传相关

http://yincheng.site/crop-upload-photo

图片上传涉及的功能，第一个是支持拖拽，第二个压缩，第三个是裁剪编辑，第四个是上传和上传进度显示。

### HTML5 调用手机摄像机、相册功能

https://www.w3.org/TR/html-media-capture/

http://blog.csdn.net/taohai123/article/details/53302405

https://imys.net/20150916/webapp-input-use-camera.html

不需要特殊环境，使用 input 标签 type 值为 file，可以调用系统默认的照相机、相册、摄像机、录音功能：

```html
<!-- capture=camera，调用手机相机功能 -->
<input type="file" accept="image/*;" capture="camera" >
<!-- capture=camcorder，调用手机摄像功能 -->
<input type="file" accept="video/*" capture="camcorder" > 
<!-- capture=microphone，调用手机录音功能 -->
<input type="file" accept="audio/*" capture="microphone" >
```

- accept 表示打开的系统文件目录

  NOTE: 若不将 accept 值设置为`image/*`，而是`image/jpeg`、`image/png`等值，则无法在手机端唤起相册

- capture 表示的是系统所捕获的默认设备，camera：照相机；camcorder：摄像机；microphone：录音；
  
  - 若不为 capture 赋值，则由移动设备自动选择捕获哪种设备，如：
    ```html
    <input type="file" accept="image/*" capture>
    ```
  - 如果不加上 capture, 则只会显示相应的，例如上述三种依次是：拍照或图库，录像或图库，录像或拍照或图库，加上 capture 之后不会调用图库。

- 其中还有一个属性 multiple，支持多选，当支持多选时，multiple 优先级高于 capture，所以只用写成：
  ```html
  <input type="file" accept="image/*" multiple>
  ```

### 使用 canvas 进行图片压缩
<!-- TODO: -->

https://imys.net/20150916/webapp-input-use-camera.html

```javascript
document.getElementById('file').addEventListener('change', function() {
    var reader = new FileReader();
    reader.onload = function (e) {
        compress(this.result);
    };
    reader.readAsDataURL(this.files[0]);
}, false);

// 省略了一些校监操作，如文件类型约束和文件大小判断（小于一定值可以不压缩）
var compress = function (res) {
    var img = new Image(),
        maxH = 160;
    img.onload = function () {
        var cvs = document.createElement('canvas'),
            ctx = cvs.getContext('2d');
        if(img.height > maxH) {
            img.width *= maxH / img.height;
            img.height = maxH;
        }
        cvs.width = img.width;
        cvs.height = img.height;
        ctx.clearRect(0, 0, cvs.width, cvs.height);
        ctx.drawImage(img, 0, 0, img.width, img.height);
        // 尺寸按比例缩放，然后把图片绘到画布上，最后调用 toDataURL 方法压缩图像质量
        // 该方法返回 base64 图像编码
        var dataUrl = cvs.toDataURL('image/jpeg', 0.6);
        // 上传略
    }
    img.src = res;
}
```

## 使用 history 和 window.onpopstate 让 ajax 分页支持前进、后退

http://yincheng.site/h5-history

## 不指定高度，实现固定 & 自适应的高宽比

https://www.w3cplus.com/css/flexible-images.html

https://www.w3cplus.com/css/aspect-ratio-boxes.html

https://www.w3cplus.com/css/aspect-ratio.html

### 固定高宽比

效果示意图：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/10/0444d75425397180d543c116217eb143.jpg)

#### 使用 max-height

```css
img { max-width: 100%; height: auto; }
```

#### 使用基于宽度的百分比来设置 padding

主要的原理是基于元素的 padding-top 或 padding-bottom 是根据元素的 width 进行计算的。假设你有一个 div 容器，它的宽度是 500px，你想让其高度也是和宽度一样，也就是说宽高比例是 1:1。这个时候借助 padding-top 或者 padding-bottom 的值为 100%，就可以计算出容器 div 的高度是 500px。如果我们的 padding-bottom 或 padding-top 不是 100%，而是 56.25%，其实这就是一个完美的宽高比 16:9，也就是 9 / 16 * 100% = 56.25%。

一般情况之下，我们知道图片的尺寸大小，根据纵横比例，我们可以通过下面的公式计算出内距 padding-top 或 padding-bottom 的百分比值：
```
padding-top 或 padding-bottom = （背景图片高度 / 背景图片宽度) * 100%
```

这种方案有一个必要条件，容器 div 的 height 为 0，同时 box-sizing 为 border-box，不然的话，容器不能带有 border。

##### padding

例：
```html
<div class="figure"> 
  <div class="inner"> 
    <div class="image-wrapper">
    </div> 
    <p class="figcaption">Lo, the robot walks</p> 
  </div>
</div>
```

```less
.figure {
  margin: .5em; // 背景图像宽度必须宽度为 700px max-width: 700px; // 图片的宽度 
  .inner {
    border: 10px solid hsla(333, 50%, 60%, .8);
    border-radius: 10px;
  }
  .image-wrapper {
    background: url("http://w3cplus-cdn2.u.qiniudn.com/sites/default/files/blogs/2014/1401/flexible-image.jpg") no-repeat center;
    background-size: cover;
    padding-top: 66.7142857%; // 467px / 700px = 0.667142857 
  } 
  p {
    background-color: hsla(333, 50%, 60%, .8);
    padding: 10px 10px 0;
    color: #fff;
  }
}
```

效果：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/10/e9ec48ae872e138f84f81e9edd9385fd.jpg)

##### padding & 伪元素

就这种方法而言，如果在 div 容器设置了 padding-top（或者 padding-bottom）会造成容器的内容往盒子外推，此时设置了若 `overflow:hidden`，溢出的内容就会看不见了。若把 overflow:hidden 换成 overflow:auto。但也是美中不足，会出现滚动条。因此，可**借助 CSS 的伪元素来做容器的高宽比例**：

https://codepen.io/airen/pen/XbVBZo

https://codepen.io/airen/pen/Rgomao

通过这种处理方式，如果内容不超出容器的时候，容器的大小还具有对应的宽高比，如果内容超出容器的时候，会扩展容器的高度，让内容能足已展示。

##### padding & calc()

https://www.w3cplus.com/css/aspect-ratio.html

##### padding & CSS 变量

https://www.w3cplus.com/css/aspect-ratio-boxes.html

#### 使用视窗单位 vw

##### vw

16:9 对应的就是 100vw * 9 / 16 = 56.25vw。这个值可以用在 padding-top 或者 padding-bottom 中。但这里演示的不再是 padding 了，而是把这个值给 height。

```css
.aspectration[data-ratio="16:9"] {
  width: 100vw;
  height: 56.25vw; 
}
```

##### vw & CSS Grid

https://www.w3cplus.com/css/aspect-ratio.html

### 自适应高宽比

效果示意图：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/10/e0830742a2f064f527fc5045f6f179e3.jpg)

在固定纵横比例的基础之上，做进一步的调整。假设我们在宽屏的 PC 上显示大的图片，而在移动设备上，我们不想使用相同的纵横比或图像变得太小。当然我们也不想使用完全相同的高度或让图像变得太高。我们更希望当宽度变小时，其高度也变得更小。而我们也把这种称为流体纵横比例。

实现方法：https://www.w3cplus.com/css/flexible-images.html

## Retina 屏中实现 1px 的 border

https://www.w3cplus.com/css/fix-1px-for-retina.html

## outline 圆角实现

http://www.zhangxinxu.com/wordpress/2015/04/css3-radius-outline/

outline 没有类似于 borderborder-radius 属性（只有`-moz-outline-radius`，仅适用于 Firefox），但可以通过设置 box-shadow 来模拟 outline 的圆角效果：
```css
img {
  border-radius: 1px;
  box-shadow: 0 0 0 30px #cd0000;
}
```

## Android 平台中弹出虚拟键盘对布局的影响

问题：

- 弹出虚拟键盘后，导致了 viewport 的 height 变小。

- 移动端虚拟键盘出现的条件是：文本框（文本类）获得焦点，但是文本框获得焦点未必会弹出键盘；收起虚拟键盘的条件是：文本框失焦

### 破坏 fixed 布局

http://www.jianshu.com/p/d535e643e59c

http://www.alloyteam.com/2017/03/moves-the-input-box-fill-series-a/

Html5 登录表单，但是登录表单下面有内容是固定在页面最底部。这种布局下，当键盘弹出输入时，ios 手机上是没问题的，但是在安卓手机上，键盘弹出后，页面底部的内容会挡住 form 表单。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/11/3a77793976020086f983f7d8296f937b.jpg)

- 解决方案 1：采用 css sticky footer 布局

  ```html
  <div id="wrap">
      <div id="main" class="clearfix">
          <div id="content">
          </div>
      </div>
  </div>
  <div id="footer">
  </div>
  ```

  ```css
  html, body, #wrap {
    height: 100%;
  }
  body > #wrap {
    height: auto;
    min-height: 100%;
  }
  #main {
    padding-bottom: 150px;
  }  /* 必须使用和 footer 相同的高度 */
  #footer {
    position: relative;
    margin-top: -150px; /* footer 高度的负值 */
    height: 150px;
    clear:both;
  }

  /*对 main 进行 clearfix*/
  .clearfix:after {
    content: ".";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
  }
  .clearfix {
    display: inline-block;
  }
  /* Hides from IE-mac \*/
  * html .clearfix { 
    height: 1%;
  }
  .clearfix {
    display: block;
  }
  /* End hide from IE-mac */
  ```

- 解决方案 2：监控键盘行为，键盘弹出时候将 fixed 元素设置为 static，键盘消失时候将 fixed 元素设置为 fixed

  https://www.cnblogs.com/yexiaochai/p/3561939.html

### 改变 viewport，导致 vh 变小

移动端若要使用视窗单位，最好统一使用 vw。

## 输入长度限制监控

### 非直接的文字输入

当输入汉字时必然会是非直接输入，需要我们点选才能正式输入。

当我们字数限制为 16 个字，需要实时检查是否到 16 字。输入文字时，当有非直接的文字输入时，监听 keydown 事件和 input 事件都会直接触发判断字数逻辑，会截断我们正在输入的文字。

解决办法：

监听 compositionend（当直接的文字输入时触发）这时，当没选中中文的时候不会进行字数判断：
```javascript
$('#input').on('compositionend', function(e) {
  var len = $(this).val().length;
  if (len > 16) {
    // 提示超过 16 字
  }
});
```   

### emoji 表情的输入

当输入 emoji 的时候，但是，当输入 emoji 表情的时候，js 中判断 emoji 表情的 length 为 2，因此 emoji 正常应该最多只能输入 8 个，但是 ios 端却把 emoji 的 length 算为 1，可以输入 16 个 emoji。这样就导致了两端的体验不同。因此需要在 js 中来进行字数限制。

再加上汉字的非直接输入问题，那么就加入一个标记位，来判断是否是直接的文字输入。然后监听 input，限制字数，当超过字数限制的时候，把前 16 个字截断显示出来就 ok 了。

```javascript
var cpLock;
$('#input').on('compositionstart', function(e) {
  cpLock = true；
});
$('#input').on('compositionend', function(e) {
  cpLock = false;
});
$('#input').on('input', function(e) {
  if (!cpLock) {
    if (e.target.value.length - 17 >= 0) {
      var txt = $(e.target).val().substring(0, 16);
      $(e.target).val(txt);
      // 超过 16 字提示
    }
  }
});
```

## 去除容器内 img 下方的空白

容器内图片下方的空白，是由 vertical-align 和 line-height 影响所造成，详细参见[这篇文章](http://www.zhangxinxu.com/wordpress/2015/08/css-deep-understand-vertical-align-and-line-height/)。

示意图：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/12/2932ba02399fa7c6ca8956f861751f11.jpg)

解决方法：

- 让 vertical-align 失效

  图片默认是 inline 水平的，而 vertical-align 对块状水平的元素无感。因此，我们只要让图片 display 水平为 block 就可以了，我们可以直接设置 display 或者浮动、绝对定位等（如果布局允许）。例如：
  ```css
  img { display: block; }
  ```

- 使用其他 vertical-align 值

  告别 baseline, 取用其他属性值，比方说 bottom/middle/top 都是可以的。
  ```less
  vertical-align:bottom;
  //vertical-align:middle;
  //vertical-align:top;
  ```

- 直接修改 line-height 值

  下面的空隙高度，实际上是文字计算后的行高值和字母 x 下边缘的距离。因此，只要行高足够小，实际文字占据的高度的底部就会在 x 的上面，下面没有了高度区域支撑，自然，图片就会有容器底边贴合在一起了。比方说，我们设置行高 5 像素：
  ```css
  div { line-height: 5px; }
  ```

- line-height 为相对单位，font-size 间接控制

  如果 line-height 是相对单位，例如 line-height:1.6 或者 line-height:160% 之类，也可以使用 font-size 间接控制，比方说来个狠的，font-size 设为大鸡蛋 0, 本质上还是改变 line-height 值。
  ```css
  div { font-size: 0; }
  ```

## 表单输入验证

https://www.w3cplus.com/blog/tags/627.html

https://www.w3schools.com/js/js_validation_api.asp

https://developer.mozilla.org/en-US/docs/Web/API/ValidityState

一个好的表单输入验证用户体验是：当用户离开一个字段时，即时的显示出错误信息，并在用户修复它之前一直保持错误信息的显示。

通过组合输入类型 （例如，<input type='email'>) 和验证属性 （像 required 和 pattern) 来使用原生的浏览器表单验证机制来进行表单输入验证简单轻便，但这种方式存在一些缺点：

- 你可以通过 :invalid 伪选择器对出现错误的字段域设置样式，但是你并不能对错误信息本身设置样式。
- 各浏览器之间的行为也不一致

因此，我们可以使用浏览器原生的 JavaScript API，来自定义表单验证的行为。

### validity 对象

每个 input 元素都包含了一个 validity 对象，该对象以布尔值 (true/false) 的形式提供了一系列关于表单域的信息：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/11/1a773b00fe60ad3f53dcf9ab1c0879be.jpg)

该对象包含以下属性：

| Property          | Description                              |
| ----------------- | ---------------------------------------- |
| `valid`           | 当字段通过验证时，属性值为 `true`。                    |
| `valueMissing`    | 当一个必填字段为空时，属性值为 `true`。                  |
| `typeMismatch`    | 当字段的 `type` 是 `email`或者 `url`但是输入的 `value` 不是正确的类型时，属性值为`true`。 |
| `tooShort`        | 当一个字段设置了 `minLength` 属性，且输入的 `value` 的长度小于设定值时，属性值为 `true`。 |
| `tooLong`         | 当一个字段设置了 `maxLength` 属性，且输入的 `value`的长度值大于设定值时，属性值为`true`。 |
| `patternMismatch` | 当一个字段包含 `pattern` 属性，且输入的 `value` 与 pattern 不匹配时，属性值为 `true`。 |
| `badInput`        | 当输入类型 `type` 值是 `number` ，且输入的`value`值不是一个数字时，属性值为 `true`。 |
| `stepMismatch`    | 当字段拥有 `step` 属性，且输入的 `value` 值不符合设定的间隔值时，该属性值为 `true`。 |
| `rangeOverflow`   | 当字段拥有 `max` 属性，且输入的数字 `value` 值大于设定的最大值时，该属性的值为`true`。 |

### 实践

1. 禁用原生表单验证

      给表单添加 novalidate 属性来禁用浏览器原生的验证方式，从而阻止原生的错误提示信息显示；

      最好的方法是，使用 JavaScript 来添加这个属性，如果我们的脚本在加载过程中出现错误，也可以保证浏览器原生的表单验证方式可以正常运行。

      ```javascript
      // 当 JS 加载时，为标注了 validate 的元素添加 novalidate 属性（可能存在部分 input 不需要进行验证） 
      var forms = document.querySelectorAll('.validate');
      for (var i = 0; i < forms.length; i++) { 
         forms[i].setAttribute('novalidate', true); 
      }
      ```

2. 当用户离开一个字段时，对该字段进行验证

      为了实现这个功能，我们需要注册事件监听器。

      我们可以通过事件冒泡（或者事件传播) 的机制来监听所有的 blur 事件，从而不用为每一个字段都添加一个监听器。
      ```javascript
      // 监听所有的失去焦点的事件
      document.addEventListener('blur', function (event) {

          // 只有当该字段是要验证的表单时才运行
          if (!event.target.form.classList.contains('validate')) return;

          // 验证字段
          // 如果 error 的值是 true, 那么这个字段就是符合验证规则的。否则，就会抛出错误。
          var error = event.target.validity;
          console.log(error);

      }, true);
      ```

3. 捕获错误

      一旦我们知道产生了一个错误，那弄清楚这个错误的具体信息，对于我们来说是非常有用的。我们可以使用其他 Validity State 属性来获取这些信息。

      如果没有错误，我们就返回 null。 否则，我们就检测每一个 Validity State 属性，直到找到错误。如果我们找到了这个属性，我们就返回一个包含错误信息的字符串。

      ```javascript
      // 验证字段
      var hasError = function (field) {

          // 不验证 submits, buttons, file 和 reset inputs, 以及被禁用的字段
          if (field.disabled || field.type === 'file' || field.type === 'reset' || field.type === 'submit' || field.type === 'button') return;

          // 获取 validity
          var validity = field.validity;

          // 如果通过验证，就返回 null
          if (validity.valid) return;

          // 如果是必填字段但是字段为空时
          if (validity.valueMissing) return 'Please fill out this field.';

          // 如果输入的类型不正确
          if (validity.typeMismatch) {
              // Email
              if (field.type === 'email') return 'Please enter an email address.';

              // URL
              if (field.type === 'url') return 'Please enter a URL.';

          }

          // 如果输入的字符数太短，检测出这个字段本身设定的长度以及字段实际的长度并将这些信息包含在提示中
          if (validity.tooShort) return 'Please lengthen this text to ' + field.getAttribute('minLength') + ' characters or more. You are currently using ' + field.value.length + ' characters.';

          // 如果输入的字符数太长，检测出这个字段本身设定的长度以及字段实际的长度并将这些信息包含在提示中
          if (validity.tooLong) return 'Please short this text to no more than ' + field.getAttribute('maxLength') + ' characters. You are currently using ' + field.value.length + ' characters.';

          // 如果数字输入类型输入的值不是数字
          if (validity.badInput) return 'Please enter a number.';

          // 如果数字值与步进间隔不匹配
          if (validity.stepMismatch) return 'Please select a valid value.';

          // 如果数字字段值超过 max 的值
          if (validity.rangeOverflow) return 'Please select a smaller value.';

          // 如果数字字段的值小于 min 的值
          if (validity.rangeUnderflow) return 'Please select a larger value.';

          // 如果模式不匹配，并且这个字段有一个 title, 我们就用 title 属性的值作为我们的错误信息，就像原生的浏览器行为一样
          if (validity.patternMismatch) {
              // 如果包含模式信息，返回自定义错误
              if (field.hasAttribute('title')) return field.getAttribute('title');
              // 否则，返回一般错误
              return 'Please match the requested format.';

          }

          // 如果没有一个属性的值是 true 但是 validity 的属性值却是 false 时，我们就返回一个通用的 "catchall" 错误信息
          return 'The value you entered for this field is invalid.';

      };
      // 监听所有的失去焦点的事件
      document.addEventListner('blur', function (event) {

          // 只有当该字段是要验证的表单时才运行
          if (!event.target.form.classList.contains('validate')) return;

          // 验证字段
          var error = hasError(event.target);

      }, true);
      ```

4. 显示错误信息

      一旦我们捕获到错误，我们就可以在字段的下面将它显示出来。我们可以创建一个 showError() 函数来实现这个功能，传入表单字段和错误信息，然后在我们的事件监听器里调用它。
      ```javascript
      // 监听所有的 blur 事件
      document.addEventListener('blur', function (event) {

          // 只有当该字段是要验证的表单时才运行
          if (!event.target.form.classList.contains('validate')) return;

          // 验证字段
          var error = hasError(event.target);

          // 如果有错误，就把它显示出来
          if (error) {
              showError(event.target, error);
          }

      }, true);

      var showError = function (field, error) {

          // 将错误类添加到字段
          field.classList.add('error');

          // 获取字段 id 或者 name
          var id = field.id || field.name;
          if (!id) return;

          // 检查错误消息字段是否已经存在
          // 如果没有，就创建一个
          // 使用 querySelector 方法，只在当前的表单字段上搜索错误信息，而不是整个 document
          var message = field.form.querySelector('.error-message#error-for-' + id );
          if (!message) {
              message = document.createElement('div');
              message.className = 'error-message';
              message.id = 'error-for-' + id;
              field.parentNode.insertBefore( message, field.nextSibling );
          }

          // 为了确保屏幕浏览器或以及其它的辅助技术知道我们的错误信息与字段是相关联的，需要添加一个 aria-describedby
          // 添加 ARIA role 到字段
          field.setAttribute('aria-describedby', 'error-for-' + id);

          // 更新错误信息
          message.innerHTML = error;

          // 显示错误信息
          message.style.display = 'block';
          message.style.visibility = 'visible';

      };
      ```

      设置错误信息样式：
      ```css
      .error {
          border-color: red;
      }

      .error-message {
          color: red;
          font-style: italic;
      }
      ```

5. 隐藏错误信息

      当字段验证通过，我们希望移除这个错误信息。所以，让我们来创建另外一个函数， removeError(), 并且传入字段。我们仍然在事件监听器中调用这个函数。
      ```javascript
      // 移除错误信息
      var removeError = function (field) {

        // 删除字段的错误类
        field.classList.remove('error');

        // 移除字段的 ARIA role
        field.removeAttribute('aria-describedby');

        // 获取字段的 id 或者 name
        var id = field.id || field.name;
        if (!id) return;

        // 检查 DOM 中是否有错误消息
        // 使用 querySelector 方法，只在当前的表单字段上搜索错误信息，而不是整个 document
        var message = field.form.querySelector('.error-message#error-for-' + id + '');
        if (!message) return;

        // 如果是这样的话，就隐藏它
        message.innerHTML = '';
        message.style.display = 'none';
        message.style.visibility = 'hidden';
      };

      // 监听所有的 blur 事件
      document.addEventListener('blur', function (event) {

        // 只有当该字段是要验证的表单时才运行
        if (!event.target.form.classList.contains('validate')) return;

        // 验证字段
        var error = event.target.validity;

        // 如果有错误，显示出来
        if (error) {
          showError(event.target, error);
          return;
        }

        // 否则，移除所有存在的错误信息
        removeError(event.target);

      }, true);
      ```

6. 在提交表单时检查所有的字段

      当用户提交表单时，我们要对表单里的每一个字段进行验证，并且在不合法的字段下面显示错误信息。我们也需要给出现错误的字段默认获得焦点，这样用户就能第一时间的去修复它们。

      更好的用户体验是直接将页面滚动到出现错误信息的第一个元素处，并 focus 在出现错误信息的 input 中。

      ```javascript
      // 在提交时检查所有的字段
      document.addEventListener('submit', function (event) {

          // 只能运行在标记为验证的表单上
          if (!event.target.classList.contains('validate')) return;

          // 获取所有的表单元素
          var fields = event.target.elements;

          // 验证每一个字段
          // 将具有错误的第一个字段存储到变量中以便稍后我们将其默认获得焦点
          var error, hasErrors;
          for (var i = 0; i < fields.length; i++) {
              error = hasError(fields[i]);
              if (error) {
                  showError(fields[i], error);
                  if (!hasErrors) {
                      hasErrors = fields[i];
                  }
              }
          }

          // 如果有错误，停止提交表单并使出现错误的第一个元素获得焦点 
          if (hasErrors) {
              event.preventDefault();
              hasErrors.focus();
          }

          // 否则，正常提交表单
          // 您也可以在此处填入 Ajax 表单提交过程
      }, false);
      ```

7. 使用 Polyfill 兼容 IE 和 Edge

      https://www.w3cplus.com/css/form-validation-part-3-a-validity-state-api-polyfill.html


## 平滑滚动的实现

https://iwenku.net/?article_167.html

http://www.zcfy.cc/article/smooth-page-scroll-in-5-lines-of-javascript-406.html

### 通过CSS实现

需要给“平滑滚动”的元素（通常是body）应用`scroll-behavior: smooth`:
```css
body {
    scroll-behavior: smooth;
}
```
但在浏览器兼容这方面可以发现也就firefox支持，在7.36%的浏览器上可以使用这个属性，目前来说还只是一个小众的属性，不推荐使用。

### 通过JavaScript实现

由于CSS的属性兼容性较差，因此一般平滑滚动的实现都是通过JavaScript的window.scrollTo方法来实现：
```javascript
window.scrollTo({"behavior": "smooth", "top": element.offsetTop, "left": element.offsetLeft});
```

例：
```javascript
var anchorLink = document.querySelector("a[href='#destination']"),
target = document.getElementById("destination");
anchorLink.addEventListener("click", function(e) {
    if (window.scrollTo) {
        e.preventDefault();
        window.scrollTo({"behavior": "smooth", "top": target.offsetTop});
    }
})
```

TODO: 在微信浏览器上两种方式都无法是是实现