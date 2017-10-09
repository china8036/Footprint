- [HTML 最佳实践](#html-%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5)
  - [命名规范](#%E5%91%BD%E5%90%8D%E8%A7%84%E8%8C%83)
  - [编码规范](#%E7%BC%96%E7%A0%81%E8%A7%84%E8%8C%83)
  - [编写标准的 HTML 代码](#%E7%BC%96%E5%86%99%E6%A0%87%E5%87%86%E7%9A%84-html-%E4%BB%A3%E7%A0%81)
  - [高可读性的 HTML](#%E9%AB%98%E5%8F%AF%E8%AF%BB%E6%80%A7%E7%9A%84-html)
    - [使用语义化的 HTML](#%E4%BD%BF%E7%94%A8%E8%AF%AD%E4%B9%89%E5%8C%96%E7%9A%84-html)
  - [积极使用 HTML5](#%E7%A7%AF%E6%9E%81%E4%BD%BF%E7%94%A8-html5)
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

- 根据 HTML5 规范，通常在引入 CSS 和 JavaScript 时不需要指明 type，因为 text/css 和 text/javascript 分别是他们的默认值；   
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

HTML 标签语义化是 Web 网页标准化的重要一欢，也是标准制定时重要的设计原则；

HTML 标签语义化有助于第三方内容抓取工具更容易读懂页面代码，同时提高了页面代码可读性；

- 什么样的页面才是高语义化的页面呢？
  - 从页面外观上说，由于浏览器会对语义化的标签设计默认的样式，因此高语义化的页面中去除 CSS 样式后页面仍能保持良好的外观，并可正常阅读；
  - 从代码角度上说，高语义化的页面中会尽量使用合适的语义化标签；    
   
- 原则
  - 使用高语义化的 HTML 标签
    - 例如，由于`<div>`和`<span>`标签是最没有语义的两个标签，因此在使用此类标签时应使用更具语义的标签代替，如 `ul`、`ol`、`li`等；  
    - 使用频率较高的语义标签有：`<p>`、`<em>`、`<strong>`、`<table>`、`<site>`、`<blockquote>`、`<header>`、`<footer>`、`<article>`、`<section>`、`<nav>`、`<aside>` 等；

  - 为 HTML 标签设置必要的属性
    如 alt 和 title 属性的设置，即可提高 HTML 的语义；在`<img>`标签中 alt 属性可用于解释说明图片的额外信息，在`<a>`标签中 title 属性可用于添加额外的说明信息；

  - 

## 积极使用 HTML5

## Refer Links
https://zoomzhao.github.io/code-guide/

《Web 前端开发最佳实践》 党建 著