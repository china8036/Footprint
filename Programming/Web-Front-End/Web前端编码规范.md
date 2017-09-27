

<!-- toc -->

* [前端编码规范](#前端编码规范)
  * [HTML](#html)
  * [CSS](#css)

<!-- toc stop -->

# 前端编码规范 #
原文：https://zoomzhao.github.io/code-guide/

## HTML ##

- 使用两个空格的 soft tabs；
- 嵌套的节点应该缩进（两个空格）；
- 在属性上，使用双引号，不要使用单引号；
- 在每个 HTML 页面开头都应使用 HTML5 doctype 来启用标准模式，使其每个浏览器中尽可能一致的展现；
- 根据 HTML5 规范：鼓励网站作者在 html 元素上指定 lang 属性，来指出页面的语言，如：en、zh等；
- 通过声明一个明确的字符编码，让浏览器轻松、快速的确定适合网页内容的渲染方式；
- 通过 edge mode 来通知 IE 使用最新的兼容模式；
- 根据 HTML5 规范, 通常在引入 CSS 和 JavaScript 时不需要指明 type，因为 text/css 和 text/javascript 分别是他们的默认值；
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
Classes 是为高可复用组件设计的，所以他们处在第一位。Ids 更加具体而且应该尽量少使用（例如, 页内书签），所以他们处在第二位；
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


## CSS ##

- 使用两个空格的 soft tabs；
- 使用组合选择器时，保持每个独立的选择器占用一行；
- 为了代码的易读性，在每个声明的左括号前增加一个空格，声明块的右括号应该另起一行；
- 每条声明冒号 “:” 之后应该插入一个空格，“，” 逗号之后应该插入一个空格；
- 每条声明应该只占用一行来保证错误报告更加准确；
- 所有声明应该以分号结尾。虽然最后一条声明后的分号是可选的，但是如果没有他，你的代码会更容易出错；
- 不要在颜色值 rgb(), rgba(), hsl(), hsla(), 和 rect() 中增加空格；
- 不要在属性取值或者颜色参数前面添加不必要的 0 (比如，使用 .5 替代 0.5 和 -.5px 替代 0.5px)；
- 所有的十六进制值都应该使用小写字母，例如 #fff。因为小写字母有更多样的外形，在浏览文档时，他们能够更轻松的被区分开来；
- 尽可能使用短的十六进制数值，例如使用 #fff 替代 #ffffff；
- 为选择器中的属性取值添加引号，例如 input[type="text"]。 他们只在某些情况下可有可无，所以都使用引号可以增加一致性；
- 不要为 0 指明单位，比如使用 `margin: 0`; 而不是 `margin: 0px;`；

例：
```css
/* Bad CSS */
.selector, .selector-secondary, .selector[type=text] {
  padding:15px;
  margin:0px 0px 15px;
  background-color:rgba(0, 0, 0, 0.5);
  box-shadow:0 1px 2px #CCC,inset 0 1px 0 #FFFFFF
}

/* Good CSS */
.selector,
.selector-secondary,
.selector[type="text"] {
  padding: 15px;
  margin-bottom: 15px;
  background-color: rgba(0,0,0,.5);
  box-shadow: 0px 1px 2px #ccc, inset 0 1px 0 #fff;
}
```

- 相关的属性声明应该以下面的顺序分组处理：
		 Positioning
		 Box model 盒模型
		 Typographic 排版
		 Visual 外观   
Positioning 处在第一位，因为他可以使一个元素脱离正常文本流，并且覆盖盒模型相关的样式。盒模型紧跟其后，因为他决定了一个组件的大小和位置；
其他属性只在组件内部起作用或者不会对前面两种情况的结果产生影响，所以他们排在后面；
例：
```css
.declaration-order {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  /* Box-model */
  display: block;
  float: right;
  width: 100px;
  height: 100px;

  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  /* Visual */
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  /* Misc */
  opacity: 1;
}
```
- 尽量将媒体查询的位置靠近他们相关的规则。不要将他们一起放到一个独立的样式文件中，或者丢在文档的最底部；
- 与 <link> 相比, @import 更慢，需要额外的页面请求，并且可能引发其他的意想不到的问题。应该避免使用他们，可使用多个 <link> 元素或 CSS 预处理器将样式编译到一个文件中；
- 当使用厂商前缀属性时，通过缩进使取值垂直对齐以便多行编辑，如：
例：
```css
/* Prefixed properties */
.selector {
  -webkit-box-shadow: 0 1px 2px rgba(0,0,0,.15);
          box-shadow: 0 1px 2px rgba(0,0,0,.15);
}
```
-  在一个声明块中只包含一条声明的情况下，为了易读性和快速编辑可以考虑移除其中的换行；所有包含多条声明的声明块应该分为多行；
例：
```css
/* Single declarations on one line */
.span1 { width: 60px; }
.span2 { width: 140px; }
.span3 { width: 220px; }
```
- 坚持限制属性取值简写的使用，属性简写需要你必须显式设置所有取值；
例：
```css
	/* Bad example */
	.element {
	  margin: 0 0 10px;
	  background: red;
	  background: url("image.jpg");
	  border-radius: 3px 3px 0 0;
	}
	
	/* Good example */
	.element {
	  margin-bottom: 10px;
	  background-color: red;
	  background-image: url("image.jpg");
	  border-top-left-radius: 3px;
	  border-top-right-radius: 3px;
	}
```
- 好的代码注释传达上下文和目标。不要简单地重申组件或者 class 名称；
- Class 命名：
	- 保持 Class 命名为全小写，可以使用短划线（不要使用下划线和 camelCase 命名）。短划线应该作为相关类的自然间断。(例如，.btn 和 .btn-danger)；
	- 避免过度使用简写。.btn 可以很好地描述 button，但是 .s 不能代表任何元素；
	- Class 的命名应该尽量短，也要尽量明确；
	- 命名时使用最近的父节点或者父 class 作为前缀；
	- 使用 .js-* classes 来表示行为(相对于样式)，但是不要在 CSS 中定义这些 classes；
例：
```css
	/* Bad example */
	.t { ... }
	.red { ... }
	.header { ... }
	
	/* Good example */
	.tweet { ... }
	.important { ... }
	.tweet-header { ... }
```
- 代码组织
	- 以组件为单位组织代码。
	- 制定一个一致的注释层级结构。
	- 使用一致的空白来分割代码块，这样做在查看大的文档时更有优势。
	- 当使用多个 CSS 文件时，通过组件而不是页面来区分他们。页面会被重新排列组合，而组件是可以移动的。