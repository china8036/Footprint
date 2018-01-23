- [HTML 语义化标签](#html-%E8%AF%AD%E4%B9%89%E5%8C%96%E6%A0%87%E7%AD%BE)
  - [概述](#%E6%A6%82%E8%BF%B0)
  - [EXAMPLES](#examples)
    - [头部——header 和 nav 标签](#%E5%A4%B4%E9%83%A8%E2%80%94%E2%80%94header-%E5%92%8C-nav-%E6%A0%87%E7%AD%BE)
    - [主体部分——main 和 section](#%E4%B8%BB%E4%BD%93%E9%83%A8%E5%88%86%E2%80%94%E2%80%94main-%E5%92%8C-section)
    - [尾部——footer](#%E5%B0%BE%E9%83%A8%E2%80%94%E2%80%94footer)
    - [OTHERS](#others)
  - [Refer Links](#refer-links)

# HTML 语义化标签

## 概述

HTML 标签语义化是 Web 网页标准化的重要一欢，也是标准制定时重要的设计原则；

- HTML 标签语义化有助于第三方内容抓取工具更容易读懂页面代码，同时提高了页面代码可读性；

- 什么样的页面才是高语义化的页面呢？

  - 从页面外观上说，由于浏览器会对语义化的标签设计默认的样式，因此高语义化的页面中去除 CSS 样式后页面仍能保持良好的外观，并可正常阅读；

  - 从代码角度上说，高语义化的页面中会尽量使用合适的语义化标签；    

  - 可使用在线工具 [HTML 5 Outline](https://gsnedders.html5.org/outliner/) 对页面进行抽象，若解析得到的 outline 基本符合设想，则说明页面标签的语义化已达到标准。

## EXAMPLES

段落使用 p，引用使用 q 和 blockquote，强调用 em 和 strong，术语定义用 dl，无序列表用 ul，有序列表用 ol，文章用 article，导航用 nav，分节用 section，按钮用 input[type=button]；  

建议的布局模式：   

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/15/2dceb671dc9003016703a694fb66162d.jpg)

### 头部——header 和 nav 标签

header 头部，body 下的直接子元素 header 一般用于放页面的介绍性的信息如网站名称、logo 或者导航栏 nav。

```html
<header>
  <h1>html5 语义化标签</h1>
  <nav>
    <h1>导航</h1>
    <ul>
      <li>章节标签</li>
      <li>标题标签</li>
    </ul>
  </nav>
</header>
```

NOTE：

上面用了两个 h1 标签，在传统的 html4 里面，h1 标题一般只能用一个，用于表示页面的大标题。但是在 html5 页面中，所有的标题标签 h1~h6 都可用于不同的章节里。如上，第二个 h1 标签隶属于 nav 标签，与第一个 h1 是不同级别的。在 Chrome 里面第二个 h1 的样式字体小于第一个。搜索引擎也能根据标签正确解析出文章的大纲。

### 主体部分——main 和 section

```html
<main>
  <section>
    <hgroup>
        <h2>章节标签</h2>
        <p>为页面区分不同的章节</p>
    </hgroup>
    <div>包括 section article nav aside </div>
  </section>
  <section>
    <hgroup>
        <h2>标题标签</h2>
        <p>为不同的章节定义标题</p>
    </hgroup> 
    <div>h1 h2 h3 h4 h5 h6 六个标题标签</div>
  </section>
</main>
```

NOTE:

- `<main>` 元素中的内容对于文档来说应当是唯一的。它不应包含在文档中重复出现的内容，比如侧栏、导航栏、版权信息、站点标志或搜索表单。

  - `<main>`标签是后来出的标签，所有 IE 浏览器都不支持该标签，会把其子元素标签变成相邻的标签，从而页面错乱。解决办法是，IE8 及以下用 document.createElement(“main”) 的办法让其识别，而 IE9 及以上设置 css: main{display: block}即可。

- section 是一个章节标签，构建页面的大纲 (outline)

- 多个标题可以用 hgroup 包括起来，在页面提纲里成为独立的一条内容

### 尾部——footer

```html
<footer>
  <p>copyright &copy hello, world</p>
</footer>
```

footer 和 header 一样，用在不同的章节里，可以显示该章节（如 body 整个页面）相关的外链、版权等信息。


### OTHERS

- 由于`<div>`和`<span>`标签是最没有语义的两个标签，因此在使用此类标签时应使用更具语义的标签代替，如 `ul`、`ol`等；

- 以下几个标签定义的标题不会给该标签所在的章节贡献轮廓作用：`blockquote`、 `body`、 `details`、 `dialog`、`fieldset`、`figure`、`td`

  例：
    ```html
    <body>
        <h1>一级大纲</h1>
        <blockquote>
            <h2>blockquote内的标题</h2>
        </blockquote>
        <div>
            <h2>div内的标题</h2>
        </div>
    </body>
    ```
    解析后：

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/c130c0061dbfca0eb3674d26d87d9f5e.jpg)

- `<figure>`用作照片及其注释的容器
  ```html
  <figure>
    <img src="Mars.jpg" alt="">
    <figcaption>火星</figcaption>
  </figure>
  ```

## Refer Links

https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list

http://yincheng.site/html5-label
