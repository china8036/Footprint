<!-- toc -->

# JavaScript Note - DOM #


DOM是JavaScript操作网页的接口，全称为“文档对象模型”（Document Object Model），是针对 XML 但经过扩展用于 HTML 的应用程序编程接口，它的作用是将网页转为一个JavaScript对象，从而可以用脚本进行各种操作（比如增删内容）；

DOM 有自己的国际标准：
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/24/1ef8ea54173a36c50c66c2ef98179283.jpg)

严格地说，DOM 不属于 JavaScript，但是操作 DOM 是 JavaScript 最常见的任务，而 JavaScript 也是最常用于 DOM 操作的语言；

DOM 将整个页面映射为一个多层节点结构，即 DOM 模型；

## Node 节点 ##

DOM 的最小组成单位叫做节点（node）。文档的树形结构（DOM树），就是由各种不同类型的节点组成。每个节点可以看作是文档树的一片叶子。

DOM Node 类型（浏览器原生提供的节点对象的派生对象）：
- Document：整个文档树的顶层节点
- DocumentType：doctype标签（比如`<!DOCTYPE html>`）
- Element：网页的各种HTML标签（比如 `<body>`、`<a>`等）
- Attribute：网页元素的属性（比如class="right"）
- Text：标签之间或标签包含的文本
- Comment：注释
- DocumentFragment：文档的片段

所有节点对象都是浏览器内置的 Node 对象的实例，继承了 Node 属性和方法；

### 查找 Element ###

- 通过 id 找到 HTML 元素
```javascript
var x = document.getElementById("intro");
```
- 通过标签名找到 HTML 元素
```javascript
var x = document.getElementById("main");
var y = x.getElementsByTagName("p");
```
- 通过类名找到 HTML 元素



### 修改 HTML ###
- 修改 HTML 内容：使用 innerHTML 属性
```javascript
document.getElementById(id).innerHTML=new HTML
```
- 修改 HTML 属性：使用 attribute 属性
```javascript
document.getElementById(id).attribute=new value
```
- 修改 HTML 样式：使用 style 属性
```javascript
document.getElementById(id).style.property=new style
```


### 添加 Element ###

```html
<div id="div1">
	<p id="p1">这是一个段落</p>
	<p id="p2">这是另一个段落</p>
</div>

<script>
	var para=document.createElement("p");
	var node=document.createTextNode("这是新段落。");
	para.appendChild(node);
	
	var element=document.getElementById("div1");
	element.appendChild(para);
</script>
```


### 删除 Element ###

```html
<div id="div1">
	<p id="p1">这是一个段落。</p>
	<p id="p2">这是另一个段落。</p>
</div>

<script>
	var parent=document.getElementById("div1");
	var child=document.getElementById("p1");
	parent.removeChild(child);
</script>
```




## 事件 ##
事件是一种异步编程的实现方式，本质上是程序各个组成部分之间的通信，在传统软件工程中被称为观察者模式，支持页面行为与页面外观的松散耦合；

### 事件流 ###
事件流指的是从页面中接受事件的顺序；

#### 事件冒泡 ####
事件冒泡指的是事件开始时由最具体的元素接受，然后沿 DOM Tree 逐级向上传播，在每一级节点上都会发生，直至传播到 document 对象；

#### 事件捕获 ####
事件捕获指的是事件开始时由最不具体的元素接受，然后沿 DOM Tree 逐级向下传播，在每一级节点上都会发生，直至传播到事件的实际目标；

#### DOM 事件流 ####

“DOM2 级事件” 规定的事件流包括三个阶段：   
第一阶段：从 window 对象传导到目标节点，称为 “捕获阶段”（capture phase）；   
第二阶段：在目标节点上触发，称为 “目标阶段”（target phase）；   
第三阶段：从目标节点传导回 window 对象，称为 “冒泡阶段”（bubbling phase）；

例：
```html
<div>
  <p>Click Me</p>
</div>
```
如果对这两个节点的click事件都设定监听函数，则click事件会被触发四次：
1. 捕获阶段：事件从`<div>`向`<p>`传播时，触发`<div>`的click事件；
2. 目标阶段：事件从`<div>`到达`<p>`时，触发`<p>`的click事件；
3. 目标阶段：事件离开`<p>`时，触发`<p>`的click事件；
4. 冒泡阶段：事件从`<p>`传回`<div>`时，再次触发`<div>`的click事件；






### 监听函数 ###

监听函数（listener）是事件发生时，程序所要执行的函数，它是事件驱动编程模式的主要编程方式；

DOM 提供三种方法，可以用来为事件绑定监听函数：

#### HTML 标签的 on- 属性 ####
HTML语言允许在元素标签的属性中，直接定义某些事件的监听代码：
```html
<body onload="doSomething()">
	<div onclick="console.log('触发事件')">
```
注意：
- on- 属性的值是将会执行的代码，而不是一个函数
```html
	<!-- 正确 -->
	<body onload="doSomething()">
	
	<!-- 错误 -->
	<body onload="doSomething">
```
- 该方式虽然方便，但违反了HTML与JavaScript代码相分离的原则；


#### Element 节点的事件属性 ####
每个节点对象都由其事件处理属性，这些属性通常采用小写且以 on 开头：
```javascript
div.onclick = function(event){
	console.log('触发事件');
};
```
注意：   
- 使用这个方法指定的监听函数，只会在冒泡阶段触发；
- 同一个事件只能定义一个监听函数，也就是说，如果定义两次onclick属性，后一次定义会覆盖前一次；


#### EventTarget 接口的 addEventListener 方法 ####
addEventListener 是推荐的指定监听函数的方法；

通过 Element 节点、document 节点、window 对象的 addEventListener() 方法，可以定义事件的监听函数；
```javascript
window.addEventListener('load', doSomething, false);
```

特点：
- 可以针对同一个事件，添加多个监听函数；
- 能够指定在哪个阶段（捕获阶段还是冒泡阶段）触发回监听函数；
- 除了 DOM 节点，还可以部署在 window、XMLHttpRequest 等对象上面，等于统一了整个 JavaScript 的监听函数接口；


##### EventTarget 接口 #####
EventTarget 接口封装了 DOM 的事件操作（监听和触发）；

DOM 中的 Element 节点、document 节点和 window 对象，都部署了 EventTarget 接口。此外，BOM 中的 XMLHttpRequest、AudioNode、AudioContext等浏览器内置对象，也部署了这个接口；

EventTarget 接口包含三个方法：
- addEventListener：绑定事件的监听函数
	- 函数原型
	```javascript
	target.addEventListener(type, listener[, useCapture]);
	```
	参数说明：
		- type：事件名称，大小写敏感；
		- listener：监听函数。事件发生时，会调用该监听函数；
		- useCapture：布尔值，表示监听函数是否在捕获阶段（capture）触发，默认为false（监听函数只在冒泡阶段被触发）。老式浏览器规定该参数必写，较新版本的浏览器允许该参数可选。为了保持兼容，建议总是写上该参数；
	- 实例
	```javascript
	window.addEventListener('load', function () {...}, false);
	request.addEventListener('readystatechange', function () {...}, false);
	``` 
- removeEventListener：移除事件的监听函数
	- 实例
	```javascript
	div.addEventListener('click', listener, false);
	div.removeEventListener('click', listener, false);
	```

- dispatchEvent：在当前节点上触发指定事件，从而触发监听函数的执行；该方法返回一个布尔值，只要有一个监听函数调用了Event.preventDefault()，则返回值为false，否则为true；
	- 实例
	```javascript
	para.addEventListener('click', hello, false);
	var event = new Event('click');
	para.dispatchEvent(event);
	```






### 事件种类 ###
http://javascript.ruanyifeng.com/dom/event-type.html 


#### 鼠标事件 ####

- click
- dbclick

- mouseout
- mouseleave

- mouseover
- mouseenter

- mouseup
- mousedown
- mousemove

- contextmenu

#### 键盘事件 ####



#### 触摸事件 ####




#### 表单事件 ####




#### 文档事件 ####
- beforeunload
- unload
- load
- error
- pageshow
- pagehide























































