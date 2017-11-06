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
  - [居中](#%E5%B1%85%E4%B8%AD)
    - [水平居中](#%E6%B0%B4%E5%B9%B3%E5%B1%85%E4%B8%AD)
    - [垂直居中](#%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD)
    - [水平垂直居中](#%E6%B0%B4%E5%B9%B3%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD)
  - [图片上传相关](#%E5%9B%BE%E7%89%87%E4%B8%8A%E4%BC%A0%E7%9B%B8%E5%85%B3)
  - [使用 history 和 window.onpopstate 让 ajax 分页支持前进、后退](#%E4%BD%BF%E7%94%A8-history-%E5%92%8C-windowonpopstate-%E8%AE%A9-ajax-%E5%88%86%E9%A1%B5%E6%94%AF%E6%8C%81%E5%89%8D%E8%BF%9B%E3%80%81%E5%90%8E%E9%80%80)

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

## 使用 history 和 window.onpopstate 让 ajax 分页支持前进、后退

http://yincheng.site/h5-history
