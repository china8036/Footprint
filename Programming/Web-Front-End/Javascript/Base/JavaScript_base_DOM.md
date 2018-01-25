- [JavaScript - DOM](#javascript---dom)
  - [Node 节点](#node-%E8%8A%82%E7%82%B9)
    - [创建 DOM 节点](#%E5%88%9B%E5%BB%BA-dom-%E8%8A%82%E7%82%B9)
    - [查询 DOM 节点](#%E6%9F%A5%E8%AF%A2-dom-%E8%8A%82%E7%82%B9)
    - [修改 DOM](#%E4%BF%AE%E6%94%B9-dom)
      - [DOM 节点修改](#dom-%E8%8A%82%E7%82%B9%E4%BF%AE%E6%94%B9)
      - [DOM 属性修改](#dom-%E5%B1%9E%E6%80%A7%E4%BF%AE%E6%94%B9)
    - [常见问题](#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
      - [innerHTML, outerHTML, innerText, outerText 的区别？](#innerhtml-outerhtml-innertext-outertext-%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9F)
      - [jQuery 的 html() 与 innerHTML 的区别？](#jquery-%E7%9A%84-html-%E4%B8%8E-innerhtml-%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9F)
  - [表单操作](#%E8%A1%A8%E5%8D%95%E6%93%8D%E4%BD%9C)
    - [获取值](#%E8%8E%B7%E5%8F%96%E5%80%BC)
    - [设置值](#%E8%AE%BE%E7%BD%AE%E5%80%BC)
    - [表单提交](#%E8%A1%A8%E5%8D%95%E6%8F%90%E4%BA%A4)
      - [使用 `<form>` 节点对象的 `submit()`](#%E4%BD%BF%E7%94%A8-form-%E8%8A%82%E7%82%B9%E5%AF%B9%E8%B1%A1%E7%9A%84-submit)
      - [响应`<form>`本身的 onsubmit 事件](#%E5%93%8D%E5%BA%94form%E6%9C%AC%E8%BA%AB%E7%9A%84-onsubmit-%E4%BA%8B%E4%BB%B6)
      - [使用 FormData 对象](#%E4%BD%BF%E7%94%A8-formdata-%E5%AF%B9%E8%B1%A1)
  - [事件](#%E4%BA%8B%E4%BB%B6)
    - [事件流](#%E4%BA%8B%E4%BB%B6%E6%B5%81)
      - [事件冒泡](#%E4%BA%8B%E4%BB%B6%E5%86%92%E6%B3%A1)
      - [事件捕获](#%E4%BA%8B%E4%BB%B6%E6%8D%95%E8%8E%B7)
      - [DOM 事件流](#dom-%E4%BA%8B%E4%BB%B6%E6%B5%81)
    - [EventTarget 接口](#eventtarget-%E6%8E%A5%E5%8F%A3)
    - [监听函数](#%E7%9B%91%E5%90%AC%E5%87%BD%E6%95%B0)
      - [绑定监听函数](#%E7%BB%91%E5%AE%9A%E7%9B%91%E5%90%AC%E5%87%BD%E6%95%B0)
        - [HTML 标签的 on- 属性](#html-%E6%A0%87%E7%AD%BE%E7%9A%84-on--%E5%B1%9E%E6%80%A7)
        - [Element 节点的事件属性 `onXxx`](#element-%E8%8A%82%E7%82%B9%E7%9A%84%E4%BA%8B%E4%BB%B6%E5%B1%9E%E6%80%A7-onxxx)
        - [EventTarget 接口的 addEventListener 方法](#eventtarget-%E6%8E%A5%E5%8F%A3%E7%9A%84-addeventlistener-%E6%96%B9%E6%B3%95)
      - [解绑监听函数](#%E8%A7%A3%E7%BB%91%E7%9B%91%E5%90%AC%E5%87%BD%E6%95%B0)
        - [覆盖 Element 节点的事件属性 `onXxx`](#%E8%A6%86%E7%9B%96-element-%E8%8A%82%E7%82%B9%E7%9A%84%E4%BA%8B%E4%BB%B6%E5%B1%9E%E6%80%A7-onxxx)
        - [EventTarget 接口的 removeEventListener 方法](#eventtarget-%E6%8E%A5%E5%8F%A3%E7%9A%84-removeeventlistener-%E6%96%B9%E6%B3%95)
    - [事件种类](#%E4%BA%8B%E4%BB%B6%E7%A7%8D%E7%B1%BB)
      - [customEvent](#customevent)
      - [鼠标事件](#%E9%BC%A0%E6%A0%87%E4%BA%8B%E4%BB%B6)
      - [进度事件](#%E8%BF%9B%E5%BA%A6%E4%BA%8B%E4%BB%B6)
      - [键盘事件](#%E9%94%AE%E7%9B%98%E4%BA%8B%E4%BB%B6)
      - [触摸事件](#%E8%A7%A6%E6%91%B8%E4%BA%8B%E4%BB%B6)
      - [表单事件](#%E8%A1%A8%E5%8D%95%E4%BA%8B%E4%BB%B6)
      - [文档事件](#%E6%96%87%E6%A1%A3%E4%BA%8B%E4%BB%B6)

# JavaScript - DOM 

[原生 JavaScript 的 DOM 操作汇总](http://harttle.land/2015/10/01/javascript-dom-api.html)

DOM 是 JavaScript 操作网页的接口（通过 window.documnet 提供），全称为“文档对象模型”（Document Object Model），是针对 XML 但经过扩展用于 HTML 的应用程序编程接口，它的作用是将网页转为一个 JavaScript 对象，从而可以用脚本进行各种操作（比如增删内容）。

DOM 有自己的国际标准：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/9/24/1ef8ea54173a36c50c66c2ef98179283.jpg)

DOM 将整个页面映射为一个多层节点结构，即 DOM 模型。

严格地说，DOM 并不是编程语言，它是文档对象的模型，该模型是独立于编程语言的。比如在 Python 中也可以操作 DOM，DOM 不属于 JavaScript，但是操作 DOM 是 JavaScript 最常见的任务，而 JavaScript 也是最常用于 DOM 操作的语言。

## Node 节点

DOM 的最小组成单位叫做节点（node）。文档的树形结构（DOM 树），就是由各种不同类型的节点组成。每个节点可以看作是文档树的一片叶子。

DOM Node 类型（浏览器原生提供的节点对象的派生对象）：
- Document：整个文档树的顶层节点
- DocumentType：doctype 标签（比如`<!DOCTYPE html>`）
- Element：网页的各种 HTML 标签（比如 `<body>`、`<a>`等）
- Attribute：网页元素的属性（比如 class="right"）
- Text：标签之间或标签包含的文本
- Comment：注释
- DocumentFragment：文档的片段

所有节点对象都是浏览器内置的 Node 对象的实例，继承了 Node 属性和方法；

### 创建 DOM 节点

DOM 节点创建最常用的是 document.createElement 和 document.createTextNode 方法。

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

### 查询 DOM 节点

DOM 查询的 API 返回的的结果是 DOM 节点或 DOM 节点的列表。document 提供了三种类型的 Query 方法：
- 使用选择器查询

  ```javascript
  // 返回当前文档中第一个类名为 "myclass" 的元素
  var el = document.querySelector(".myclass");

  // 返回一个文档中所有的 class 为"note"或者 "alert"的 div 元素
  var els = document.querySelectorAll("div.note, div.alert");
  ```

- 直接查询

  - 通过 id 找到 HTML 元素，返回匹配的唯一元素
    ```javascript
    var x = document.getElementById("intro");
    ```

  - 通过标签名找到 HTML 元素，返回匹配的所有元素列表
    ```javascript
    var y = x.getElementsByTagName("p");
    ```

  - 通过类名找到 HTML 元素
    ```javascript
    var els = document.getElementsByClassName('highlight');
    ```

- 相对位置查询

  ```javascript
  // 获取父元素、父节点
  var parent = ele.parentElement;
  var parent = ele.parentNode;

  // 获取子节点，子节点可以是任何一种节点，可以通过 nodeType 来判断
  var nodes = ele.children;    

  // 通过迭代查询子元素
  var els = ele.getElementsByTagName('td');
  var els = ele.getElementsByClassName('highlight');

  // 当前元素的第一个 / 最后一个子元素节点
  var el = ele.firstElementChild;
  var el = ele.lastElementChild;

  // 下一个 / 上一个兄弟元素节点
  var el = ele.nextElementSibling;
  var el = ele.previousElementSibling;
  ```

### 修改 DOM

#### DOM 节点修改

- 添加、删除子元素
  ```javascript
  ele.appendChild(el);
  ele.removeChild(el);
  ```

- 替换子元素
  ```javascript
  ele.replaceChild(el1, el2);
  ```

- 插入子元素
  ```javascript
  parentElement.insertBefore(newElement, referenceElement);
  ```
#### DOM 属性修改

- 获取一个{name, value}的数组
  ```javascript
  var attrs = el.attributes;
  ```

- 获取、设置属性
  ```javascript
  var c = el.getAttribute('class');
  el.setAttribute('class', 'highlight');
  ```

- 判断、移除属性
  ```javascript
  el.hasAttribute('class');
  el.removeAttribute('class');
  ```

- 是否有属性设置
  ```javascript
  el.hasAttributes();     
  ```

- 快速属性操作
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

### 常见问题

#### innerHTML, outerHTML, innerText, outerText 的区别？

对于这样一个 HTML 元素：`<div>content<br/></div>`:
```
innerHTML：内部 HTML，content<br/>；
outerHTML：外部 HTML，<div>content<br/></div>；
innerText：内部文本，content ；
outerText：内部文本，content ；
```
上述四个属性不仅可以读取，还可以赋值。outerText 和 innerText 的区别在于 outerText 赋值时会把标签一起赋值掉，另外 xxText 赋值时 HTML 特殊字符会被转义。

#### jQuery 的 html() 与 innerHTML 的区别？
jQuery 的。html() 会调用。innerHTML 来操作，但同时也会 catch 异常，然后用。empty(), .append() 来重新操作。 这是因为 IE8 中有些元素的。innerHTML 是只读的。

## 表单操作

- [廖雪峰的 JavaScript 教程：操作表单](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434499922277b890fd537901490a84fc24b2b8b8867e000)

- [表单提交的方法](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest#Submitting_forms_and_uploading_files)

用 JavaScript 操作表单和操作 DOM 是类似的，因为表单本身也是 DOM 树。

### 获取值

如果我们获得了一个`<input>`节点的引用，就可以直接调用 value 获得对应的用户输入值：
```html
<input type="text" id="email">
```
```javascript
var input = document.getElementById('email');
input.value; // '用户输入的值'
```

这种方式可以应用于 text、password、hidden 以及 select 类型的 input。但对于单选框和复选框，value 属性返回的永远是 HTML 预设的值，而我们需要获得的实际是用户是否“勾上了”选项，所以应该用 checked 判断：

```javascript
// <label><input type="radio" name="weekday" id="monday" value="1"> Monday</label>
// <label><input type="radio" name="weekday" id="tuesday" value="2"> Tuesday</label>
var mon = document.getElementById('monday');
var tue = document.getElementById('tuesday');
mon.value; // '1'
tue.value; // '2'
mon.checked; // true 或者 false
tue.checked; // true 或者 false
```

### 设置值

设置值和获取值类似，对于 text、password、hidden 以及 select，直接设置 value 就可以：

```javascript
// <input type="text" id="email">
var input = document.getElementById('email');
input.value = 'test@example.com'; // 文本框的内容已更新
```

对于单选框和复选框，设置 checked 为 true 或 false 即可。

### 表单提交

#### 使用 `<form>` 节点对象的 `submit()`

```html
<!-- HTML -->
<form id="test-form">
	<input type="text" name="test">
	<button type="button" onclick="doSubmitForm()">Submit</button>
</form>

<script>
function doSubmitForm() {
	var form = document.getElementById('test-form');
	
	form.submit();// 提交 form:
}
</script>
```

缺点：

浏览器默认点击<button type="submit">时提交表单，或者用户在最后一个输入框按回车键时提交表单。而此方法无法触发`<form>`的 onsubmit 事件，会扰乱浏览器对于 form 的正常提交。

#### 响应`<form>`本身的 onsubmit 事件

```html
<!-- HTML -->
<form id="test-form" onsubmit="return checkForm()">
    <input type="text" name="test">
    <button type="submit">Submit</button>
</form>

<script>
function checkForm() {
    var form = document.getElementById('test-form');
    // 可以在此修改 form 的 input...
    // 返回 true，表示继续下一步的提交操作
		// 如果 return false，浏览器将不会继续提交 form，这种情况通常对应用户输入有误，提示用户错误信息后终止提交 form
    return true;
}
</script>
```

#### 使用 FormData 对象

https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/Using_FormData_Objects

通过 FormData 对象可以组装一组用 XMLHttpRequest 发送请求的键 / 值对。它可以更灵活方便的发送表单数据，因为可以独立于表单使用。如果你把表单的编码类型设置为 multipart/form-data ，则通过 FormData 传输的数据格式和表单通过 `submit()` 方法传输的数据格式相同。

formData 对象的用法大概有两种：通过`append()`手动添加进一系列要发送的数据之后发送，或者是直接把表单中的数据导入该 formData 对象中，然后发送。这两种方式还可以结合起来。

NOTE:

formData 对象是"只写"的，也就是说，你可以把数据添加到该对象中，但你无法从该对象中获取到所包含的值，实际读取到的对象总是为空。

- 向 FormData 对象中手动添加数据

  ```javascript
	var formData = new FormData();

	formData.append("username", "Groucho");
	formData.append("accountnum", 123456); // 数字 123456 会被立即转换成字符串 "123456"

	// HTML 文件类型 input，由用户选择
	formData.append("userfile", fileInputElement.files[0]);

	// JavaScript file-like 对象
	var content = '<a id="a"><b id="b">hey!</b></a>'; // 新文件的正文。..
	var blob = new Blob([content], { type: "text/xml"});

	// 字段 "webmasterfile" 是 Blob 类型。一个 Blob 对象表示一个不可变的，原始数据的类似文件对象。Blob 表示的数据不一定是一个 JavaScript 原生格式。 File 接口基于 Blob，继承 blob 功能并将其扩展为支持用户系统上的文件。你可以通过 Blob() 构造函数创建一个 Blob 对象。
	formData.append("webmasterfile", blob);

	// 定义数据成功发送并返回后执行的操作
	XHR.addEventListener('load', function(event) {
		alert('Yeah! Data sent and response loaded.');
	});

	// 定义发生错误时执行的操作
	XHR.addEventListener('error', function(event) {
		alert('Oups! Something goes wrong.');
	});

	// 设置请求地址和方法
	XHR.open('POST', 'http://ucommbieber.unl.edu/CORS/cors.php');

	// 发送这个 formData 对象，HTTP 请求头会自动设置
	XHR.send(FD);
  ```

- 使用一个表单元素初始化 FormData 对象

  ```html
  <form id="myForm">
    <label for="myName">Send me your name:</label>
    <input id="myName" name="name" value="John">
    <input type="submit" value="Send Me!">
  </form>
  ```
  使用异步 AJAX 发送表单数据：
  ```javascript
  window.addEventListener("load", function () {
    function sendData() {
      var XHR = new XMLHttpRequest();

      // We bind the FormData object and the form element
      var FD  = new FormData(form);

      // We define what will happen if the data are successfully sent
      XHR.addEventListener("load", function(event) {
        alert(event.target.responseText);
      });

      // We define what will happen if case of error
      XHR.addEventListener("error", function(event) {
        alert('Oups! Something goes wrong.');
      });

      // We setup our request
      XHR.open("POST", "http://ucommbieber.unl.edu/CORS/cors.php");

      // The data send are the one the user provide in the form
      XHR.send(FD);
    }
  
    // We need to access the form element
    var form = document.getElementById("myForm");

    // to takeover its submit event.
    form.addEventListener("submit", function (event) {
      event.preventDefault();

      sendData();
    });
  });
  ```

- 发送二进制数据

  如果你用来初始化 formData 对象的那个表单中包含了一个文件输入框 (type=file 的 input 元素), 则在发送 AJAX 时，用户在这个文件输入框中选定的文件也会被发送，和正常的表单提交一样。而且即使你没有用表单初始化这个 formData 对象，你同样可以手动向这个 formData 对象中添加若干个二进制数据。

  二进制数据的来源主要有三种：FileReader API,Canvas API,WebRTC API。

  使用 formData 发送二进制数据非常简单，只需要调用 append 方法将你需要发送的 File 对象或者 Blob 对象添加进去。

  例：使用了 FileReader API 来访问二进制数据，然后发送这个请求
  ```html
  <form id="myForm">
    <p>
      <label for="i1">text data:</label>
      <input id="i1" name="myText" value="Some text data">
    </p>
    <p>
      <label for="i2">file data:</label>
      <input id="i2" name="myFile" type="file">
    </p>
    <button>Send Me!</button>
  </form>
  ```

  ```javascript
  // 注册 load 事件处理函数。
  window.addEventListener('load', function () {

    // This variables will be used to store the form data
    var text = document.getElementById("i1");;
    var file = {
          dom    : document.getElementById("i2"),
          binary : null,
        };
  
    // 使用 FileReader API 来访问文件内容
    var reader = new FileReader();

    // 应为 FileReader API 是异步的，我们需要把读取到的内容存储下来
    reader.addEventListener("load", function () {
      file.binary = reader.result;
    });

    // 在文件加载完成后，如果已经有选择的文件，我们读取它。
    if(file.dom.files[0]) {
      reader.readAsBinaryString(file.dom.files[0]);
    }

    // 更主要的，我们需要在用户选择文件后读取选中的文件。
    file.dom.addEventListener("change", function () {
      if(reader.readyState === FileReader.LOADING) {
        reader.abort();
      }
      
      reader.readAsBinaryString(file.dom.files[0]);
    });

    // sendData 函数是核心函数
    function sendData() {
      // At first, there is a file selected, we have to wait it is read
      // If it is not, we delay the execution of the function
      if(!file.binary && file.dom.files.length > 0) {
        setTimeout(sendData, 10);
        return;
      }

      // To construct our multipart form data request,
      // We need an HMLHttpRequest instance
      var XHR      = new XMLHttpRequest();

      // We need a sperator to define each part of the request
      var boundary = "blob";

      // And we'll store our body request as a string.
      var data     = "";

      // So, if the user has selected a file
      if (file.dom.files[0]) {
        // We start a new part in our body's request
        data += "--" + boundary + "\r\n";

        // We said it's form data (it could be something else)
        data += 'content-disposition: form-data; '
        // We define the name of the form data
              + 'name="'         + file.dom.name          + '"; '
        // We provide the real name of the file
              + 'filename="'     + file.dom.files[0].name + '"\r\n';
        // We provide the mime type of the file
        data += 'Content-Type: ' + file.dom.files[0].type + '\r\n';

        // There is always a blank line between the meta-data and the data
        data += '\r\n';
        
        // We happen the binary data to our body's request
        data += file.binary + '\r\n';
      }

      // For text data, it's simpler
      // We start a new part in our body's request
      data += "--" + boundary + "\r\n";

      // We said it's form data and give it a name
      data += 'content-disposition: form-data; name="' + text.name + '"\r\n';
      // There is always a blank line between the meta-data and the data
      data += '\r\n';

      // We happen the text data to our body's request
      data += text.value + '\r\n';

      // Once we are done, we "close" the body's request
      data += "--" + boundary + "--";

      // We define what will happen if the data are successfully sent
      XHR.addEventListener('load', function(event) {
        alert('Yeah! Data sent and response loaded.');
      });

      // We define what will happen in case of error
      XHR.addEventListener('error', function(event) {
        alert('Oups! Something goes wrong.');
      });

      // We setup our request
      XHR.open('POST', 'http://ucommbieber.unl.edu/CORS/cors.php');

      // We add the required HTTP header to handle a multipart form data POST request
      XHR.setRequestHeader('Content-Type','multipart/form-data; boundary=' + boundary);
      XHR.setRequestHeader('Content-Length', data.length);

      // And finally, We send our data.
      // Due to Firefox's bug 416178, it's required to use sendAsBinary() instead of send()
      XHR.sendAsBinary(data);
    }

    // 获取表单
    var form   = document.getElementById("myForm");

    // 接管表单提交事件
    form.addEventListener('submit', function (event) {
      event.preventDefault();
      sendData();
    });
  });
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
如果对这两个节点的 click 事件都设定监听函数，则 click 事件会被触发四次：
1. 捕获阶段：事件从`<div>`向`<p>`传播时，触发`<div>`的 click 事件；
2. 目标阶段：事件从`<div>`到达`<p>`时，触发`<p>`的 click 事件；
3. 目标阶段：事件离开`<p>`时，触发`<p>`的 click 事件；
4. 冒泡阶段：事件从`<p>`传回`<div>`时，再次触发`<div>`的 click 事件；

### EventTarget 接口 

EventTarget 接口封装了 DOM 的事件操作（监听和触发）；

DOM 中的 Element 节点、document 节点和 window 对象，都部署了 EventTarget 接口。此外，BOM 中的 XMLHttpRequest、AudioNode、AudioContext 等浏览器内置对象，也部署了这个接口；

EventTarget 接口包含三个方法：

- `addEventListener`：绑定事件的监听函数
	- 函数原型
    ```javascript
    target.addEventListener(type, listener[, useCapture]);
    ```
    参数说明：
      - type：事件名称，大小写敏感；
      - listener：监听函数。事件发生时，会调用该监听函数；
      - useCapture：布尔值，表示监听函数是否在捕获阶段（capture）触发，默认为 false（监听函数只在冒泡阶段被触发）。老式浏览器规定该参数必写，较新版本的浏览器允许该参数可选。为了保持兼容，建议总是写上该参数；
	- 实例
    ```javascript
    window.addEventListener('load', function () {...}, false);
    request.addEventListener('readystatechange', function () {...}, false);
    ``` 
- `removeEventListener`：移除事件的监听函数
  - 函数原型
    ```javascript
    target.removeEventListener(type, listener[, useCapture])
    ```
    参数说明：
    - type：一个字符串，表示需要移除的事件类型，如 "click"。
    - listener：需要移除的 EventListener 函数（先前使用 addEventListener 方法定义的，这里必须是同一个函数才能正确移除）
    - useCapture：（可选) 指定需要移除的 EventListener 函数是否为事件捕获。如果无此参数，默认值为 false。如果同一个监听事件分别为“事件捕获”和“事件冒泡”注册了一次，一共两次，这两次事件需要分别移除。两者不会互相干扰。
	
  - 实例
    ```javascript
    div.addEventListener('click', listener, false);
    div.removeEventListener('click', listener, false);
    ```

- `dispatchEvent`：在当前节点上触发指定事件，从而触发监听函数的执行；该方法返回一个布尔值，只要有一个监听函数调用了 Event.preventDefault()，则返回值为 false，否则为 true；
	- 实例
    ```javascript
    para.addEventListener('click', hello, false);
    var event = new Event('click');
    para.dispatchEvent(event);
    ```

### 监听函数

监听函数（listener）是事件发生时，程序所要执行的函数，它是事件驱动编程模式的主要编程方式；

#### 绑定监听函数

DOM 提供三种方法，可以用来为事件绑定监听函数：

##### HTML 标签的 on- 属性

HTML 语言允许在元素标签的属性中，直接定义某些事件的监听代码：
```html
<body onload="doSomething()">
	<div onclick="console.log('触发事件')">
```
注意：
- `on*` 属性的值是将会执行的代码，而不是一个函数
```html
	<!-- 正确 -->
	<body onload="doSomething()">
	
	<!-- 错误 -->
	<body onload="doSomething">
```
- 该方式虽然方便，但违反了 HTML 与 JavaScript 代码相分离的原则；

##### Element 节点的事件属性 `onXxx`

每个节点对象都由其事件处理属性，这些属性通常采用小写且以 on 开头：
```javascript
div.onclick = function(event){
	console.log('触发事件');
};
```

注意：   
- 使用这个方法指定的监听函数，只会在冒泡阶段触发；
- 同一个事件只能定义一个监听函数，也就是说，如果定义两次 onclick 属性，后一次定义会覆盖前一次；

##### EventTarget 接口的 addEventListener 方法

addEventListener 是推荐的指定监听函数的方法；

通过 Element 节点、document 节点、window 对象的 addEventListener() 方法，可以定义事件的监听函数；
```javascript
window.addEventListener('load', doSomething, false);
```

特点：
- 可以针对同一个事件，添加多个监听函数；
- 能够指定在哪个阶段（捕获阶段还是冒泡阶段）触发回监听函数；
- 除了 DOM 节点，还可以部署在 window、XMLHttpRequest 等对象上面，等于统一了整个 JavaScript 的监听函数接口；

与节点属性`onXxx`的区别：
- `onXxx` 方法会被覆盖，也就是多次执行只会执行最后一次赋值的函数。
- `addEventListener` 不会被覆盖，会按冒泡规则全部触发。

#### 解绑监听函数

##### 覆盖 Element 节点的事件属性 `onXxx`

可对 Element 节点的事件属性 `onXxx` 进行重新赋值，覆盖原来的属性值：
```javscript
document.getElementById('xxx').onclick=function(){
  // 函数体可为空
};

```

##### EventTarget 接口的 removeEventListener 方法

可使用 EventTarget 接口的 removeEventListener 方法，解绑监听函数：
```javascript
div.addEventListener('click', listener, false);
div.removeEventListener('click', listener, false);
```
NOTE：
- 监听函数必须与绑定时的监听函数完全相同，才能成功解绑。
- 一个 EventTarget 上的 EventListener 被移除之后，如果此事件正在执行，会立即停止。 EventListener 移除之后不能被触发，但可以重新绑定。

- 使用 removeEventListener() 移除 EventTarget 上未绑定的 EventListener 不会起任何作用。

### 事件种类

http://javascript.ruanyifeng.com/dom/event-type.html 

#### customEvent

#### 鼠标事件

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

#### 进度事件

#### 键盘事件

- altKey
- ctrlKey
- metaKey
- shiftKey
- key
- charCode

#### 触摸事件

- touch

#### 表单事件

- input
- select
- change
- reset
- submit

#### 文档事件

- beforeunload
- unload
- load
- error
- pageshow
- pagehide

- scroll

  当子元素的高度超过父元素的高度时，会在父容器上形成滚动条，即会触发子元素的 scroll 事件。

  element 的 scroll 事件不冒泡，但是 document 的 defaultView 的 scroll 事件冒泡。
  
  scroll 事件无法取消。

  由于该事件会连续高频度地大量触发，所以它的监听函数之中不应该有非常耗费计算的操作（如 DOM 操作就不该放于 scroll 的 handle 函数中）。推荐的做法是使用 requestAnimationFrame, setTimeout 或 customEvent 控制该事件的触发频率，然后可以结合 customEvent 抛出一个新事件：
  ```javascript

  (function() {
    window.addEventListener('scroll', scrollThrottler, false);

    var scrollTimeout;
    function scrollThrottler() {
      if (!scrollTimeout) {
        scrollTimeout = setTimeout(function() {
          scrollTimeout = null;
          actualScrollHandler();
        }, 66);
      }
    }

    function actualScrollHandler() {
      // ...
    }
  }());
  ```

  - 滚动分为两种：body 滚动和局部滚动
    - body 滚动：

      ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/12/26/28eb591bae39802849091b15cbbe1508.jpg)
    - 局部滚动：

      ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/12/26/0c2a38277d430b3704caff6e5605b9bd.jpg)

      在移动端如果使用局部滚动，意思就是我们的滚动在一个固定宽高的 div 内触发，将该 div 设置成`overflow:scroll/auto`; 来形成 div 内部的滚动。

- resize

- hashchange
- popstate
- cut、copy、paste
- focus
