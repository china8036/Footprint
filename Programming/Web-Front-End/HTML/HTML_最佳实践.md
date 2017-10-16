- [HTML 最佳实践](#html-%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5)
  - [命名规范](#%E5%91%BD%E5%90%8D%E8%A7%84%E8%8C%83)
  - [编码规范](#%E7%BC%96%E7%A0%81%E8%A7%84%E8%8C%83)
  - [编写标准的 HTML 代码](#%E7%BC%96%E5%86%99%E6%A0%87%E5%87%86%E7%9A%84-html-%E4%BB%A3%E7%A0%81)
  - [高可读性的 HTML](#%E9%AB%98%E5%8F%AF%E8%AF%BB%E6%80%A7%E7%9A%84-html)
    - [使用语义化的 HTML](#%E4%BD%BF%E7%94%A8%E8%AF%AD%E4%B9%89%E5%8C%96%E7%9A%84-html)
    - [设置网页标题层级](#%E8%AE%BE%E7%BD%AE%E7%BD%91%E9%A1%B5%E6%A0%87%E9%A2%98%E5%B1%82%E7%BA%A7)
    - [正确设计表单](#%E6%AD%A3%E7%A1%AE%E8%AE%BE%E8%AE%A1%E8%A1%A8%E5%8D%95)
    - [精简 HTML 代码](#%E7%B2%BE%E7%AE%80-html-%E4%BB%A3%E7%A0%81)
    - [抛弃块级元素和行内元素的概念](#%E6%8A%9B%E5%BC%83%E5%9D%97%E7%BA%A7%E5%85%83%E7%B4%A0%E5%92%8C%E8%A1%8C%E5%86%85%E5%85%83%E7%B4%A0%E7%9A%84%E6%A6%82%E5%BF%B5)
  - [积极使用 HTML5](#%E7%A7%AF%E6%9E%81%E4%BD%BF%E7%94%A8-html5)
    - [使用 HTML5 简化的定义方式](#%E4%BD%BF%E7%94%A8-html5-%E7%AE%80%E5%8C%96%E7%9A%84%E5%AE%9A%E4%B9%89%E6%96%B9%E5%BC%8F)
    - [使用 HTML5 的新标签和新属性](#%E4%BD%BF%E7%94%A8-html5-%E7%9A%84%E6%96%B0%E6%A0%87%E7%AD%BE%E5%92%8C%E6%96%B0%E5%B1%9E%E6%80%A7)
    - [不适用 HTML5 废弃的标签和属性](#%E4%B8%8D%E9%80%82%E7%94%A8-html5-%E5%BA%9F%E5%BC%83%E7%9A%84%E6%A0%87%E7%AD%BE%E5%92%8C%E5%B1%9E%E6%80%A7)
    - [处理浏览器的兼容问题](#%E5%A4%84%E7%90%86%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84%E5%85%BC%E5%AE%B9%E9%97%AE%E9%A2%98)
  - [Refer Links](#refer-links)

# HTML 最佳实践

## 命名规范

- HTML 代码的所有标签名和属性应为小写；

- 给元素定义 id 和 class 时，推荐根据语义和 DOM Tree 的层级关系来定义合适的名称，名称中全部使用小写，id 名称中关键词使用下划线“_”连接，class 名称中关键词使用中划线“-”连接，以最大限度保证命名不重复；

- HTML 中的注释不宜过度，添加原则是在保证代码维护性的基础上尽量让 HTML 代码简洁；注释要单独占一行，不应在代码后直接添加注释；

## 编码规范

- 使用两个空格的 soft tabs；

- 嵌套的节点应该缩进（两个空格）；

- 属性值使用双引号闭合，不要使用单引号；

- 在每个 HTML 页面开头都应使用 HTML5 doctype 来启用标准模式，使其每个浏览器中尽可能一致的展现；

- 根据 HTML5 规范：鼓励网站作者在 html 元素上指定 lang 属性，来指出页面的语言，如：en、zh 等；

- 通过声明一个明确的字符编码，让浏览器轻松、快速的确定适合网页内容的渲染方式；

- 通过 edge mode 来通知 IE 使用最新的兼容模式；

- 尽量遵循 HTML 标准和语义，但是不应该以浪费实用性作为代价。任何时候都要用尽量小的复杂度和尽量少的标签来解决问题；

- HTML 属性应该按照特定的顺序出现以保证易读性：
		class   
		id, name    
		data-*    
		src, for, type, href, value   
		title, alt   
		role, aria-*    
  Classes 是为高可复用组件设计的，所以他们处在第一位。Ids 更加具体而且应该尽量少使用（例如，页内书签），所以他们处在第二位；

- 在编写 HTML 代码时，需要尽量避免多余的父节点。很多时候，需要通过迭代和重构来使 HTML 变得更少；   
  例：
  ```html
    <!-- Not so great -->
    <span class="avatar">
      <img src="...">
    </span>
    
    <!-- Better -->
    <img class="avatar" src="...">
  ```

## 编写标准的 HTML 代码

- 当客户端 JavaScript 被禁用时，推荐的做法是在技术实现和网站可用性之间做一个平衡，提示用法 JavaScript 被禁用，同时提供一个功能简单、不依赖于 JavaScript 的代替网站共用户浏览，以平稳降级；可以直接跳转到一个不依赖 JavaScript 的页面中：
  ```html
  <noscript><meta http-equiv="refresh" content="0; url=/baidu.com.html?from=noscript"/></noscript>
  ```

- 添加必要的`<meta>`标签，`<meta>`标签用于标识网站，包含了网站的描述信息，有助于搜索引擎更准确的识别网页的内容，也有助于第三方工具抓取网站基本信息；

## 高可读性的 HTML

### 使用语义化的 HTML

- HTML 标签语义化是 Web 网页标准化的重要一欢，也是标准制定时重要的设计原则；

- HTML 标签语义化有助于第三方内容抓取工具更容易读懂页面代码，同时提高了页面代码可读性；

- 什么样的页面才是高语义化的页面呢？
  - 从页面外观上说，由于浏览器会对语义化的标签设计默认的样式，因此高语义化的页面中去除 CSS 样式后页面仍能保持良好的外观，并可正常阅读；
  - 从代码角度上说，高语义化的页面中会尽量使用合适的语义化标签；    
   
- 原则
  - 使用高语义化的 HTML 标签  
    - 使用频率较高的语义标签有：`<p>`、`<em>`、`<strong>`、`<table>`、`<site>`、`<blockquote>`、`<header>`、`<footer>`、`<article>`、`<section>`、`<nav>`、`<aside>` 等；
    - 例：   
      由于`<div>`和`<span>`标签是最没有语义的两个标签，因此在使用此类标签时应使用更具语义的标签代替，如 `ul`、`ol`等；    
      **段落使用 p，引用使用 q 和 blockquote，强调用 em 和 strong，术语定义用 dl，无序列表用 ul，有序列表用 ol，文章用 article，导航用 nav，分节用 section，按钮用 input[type=button]**；  
      建议的布局模式：        
      ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/15/2dceb671dc9003016703a694fb66162d.jpg)

  - 为 HTML 标签设置必要的属性
    如 alt 和 title 属性的设置，即可提高 HTML 的语义；在`<img>`标签中 alt 属性可用于解释说明图片的额外信息，在`<a>`标签中 title 属性可用于添加额外的说明信息；

  - 样式与结构分离   
    例：利用：before 和：after 伪类解决由于多余元素而破坏 HTML 代码语义的问题：
    ```html
    <div id="main">
      <div class="f-l sidebar">xxx</div>
      <div class="f-r content">xxx</div>
      <div class="clear">xxx</div>
    </div>
    ```
    对于 CSS：
    ```css
    .clear {
      clear: both;
    }
    ```
    应改为：
    ```html
    <div id="main clearfix">
      <div class="sidebar">xxx</div>
      <div class="content">xxx</div>
    </div>
    ```
    对于 CSS：
    ```css
    .clearfix {
      *zoom: 1;
    }
    .clearfix:before,
    .clearfix:after {
      display: table;
      content: "";
    }
    .clearfix:after {
      clear: both;
    }
    ```
    类似的典型错误还有`<br/>`标签的滥用，`<br/>`标签仅仅用于**文本内容**中的换行，若要实现元素之间的换行或者增加行距，应通过设置 display、margin、padding、line-high 等样式来实现；

  - 为空标签添加隐藏文字，用于说明标签功能
    HTML 设计中，有时会把文字描述替换成为图片表达，这样的做法虽然美观，但降低了代码的语义。因此，建议给空标签添加一定的说明文字，并设置`text-indent: -9999px;`来达到隐藏文字的效果；

### 设置网页标题层级

搜索引擎会给`<hx>`标签中的内容设置更高的权重，因此，构建合理合适的页面标题结构是 HTML 页面语义化的一个重要环节；

- 在页面内容的标题部分使用`<hx>`标签

- 页面中只是用一个`<h1>`标签

- `<hx>`标签使用过程中不要跳级

- 不要单纯使用`<hx>`标签给内容设置样式

### 正确设计表单

- 使用`<label>`标签，并设置`<label>`标签的 for 属性
  `<label>`标签用于为输入控件定义文本标签，即显示在空间旁的说明性文字；当用户点击该标签时，对应的输入空间也将获得焦点（需要将控件 id 赋值给`<label>`标签的 for 属性）；

- 为输入控件设置合适的水印提示，即 placeholder 属性

- 在必要情况下可设置控件的 tab 顺序
  ```html
  <input type="text" tabindex="2" />
  ```

- 使用 HTML5 引入的控件类型
  HTML5 为输入控件引入了多种类型，如 url、email、date、search、number 等，各高级浏览器针对这些类型做了易用性的增强（尤其是移动浏览器），因此可积极使用这类 HTML5 引入的输入控件类型；    
  在不支持的浏览器中，这些控件类型会以 text 类型的输入控件展现，因此无需担心兼容问题；

示例：
```html
<form action="/service/user" method="post">
  <fieldset>
    <legend>Sign in to begin.</legend>

    <label for="userName">UserName: </label>
    <input type="text" id="userName" name="userName" />
    <label for="password">Password: </label>
    <input type="password" id="password" name="password" />
  </fieldset>
  <input type="submit" value="Login" />
</form>
```

### 精简 HTML 代码

- 删除多余容器
  一般页面中最多的无用元素就是`<div>`和`<span>`；

- 装饰性元素使用 CSS 替换实现
  其中有一个很有用的技巧是使用：before 和：after 伪元素；

- 避免使用 table 布局
  高度语义化的布局中基本上不会使用 table 布局，实现相同的布局效果，table 布局会使用更多的标签；且 table 布局存在性能问题，table 中很小的改动会导致整个 table 的内容重绘或重排，降低了性能；

### 抛弃块级元素和行内元素的概念

块级元素和行内元素的概念，导致 HTML 的一系列标签从字面上与 CSS 样式有着很深的联系，有悖于 web 规范中倡导的表现与样式分离的核心思想；

在 HTML5 新规范中，抛弃了 HTML4.01 中规定的内容模型，使开发者在开发中无须受到块级元素和行内元素对应的内容模型规则的约束，而是结合实际的页面设计需求，使用合乎语义的页面元素；

## 积极使用 HTML5

### 使用 HTML5 简化的定义方式

- 文档类型声明
  ```html
  <!DOCTYPE html>
  ```

- 页面编码声明
  ```html
  <meta charset="UTF-8">
  ```

- 样式与脚本文件引用
  根据 HTML5 规范，通常在引入 CSS 和 JavaScript 时不需要指明 type，因为 text/css 和 text/javascript 分别是他们的默认值；   
  例：
  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=Edge">
      <!-- External CSS -->
      <link rel="stylesheet" href="code-guide.css">
      <title>Page title</title>
    </head>
    <body>
      <img src="images/company-logo.png" alt="Company">
      <h1 class="hello-world">Hello, world!</h1>
      <!-- JavaScript -->
      <script src="code-guide.js"></script>
    </body>
  </html>
  ```

### 使用 HTML5 的新标签和新属性

### 不适用 HTML5 废弃的标签和属性

### 处理浏览器的兼容问题

让旧浏览器可以识别 HTML5 中的新标签，最好的方式是直接使用成熟的兼容框架，如 html5shim；

html5shim 使用：在页面的 head 部分引入 js 文件即可：
```html
<!-- [if lt IE 9]>
<script src="html5shim.js"></script>
<![endif] -->
```

## Refer Links
https://zoomzhao.github.io/code-guide/

《Web 前端开发最佳实践》 党建 著