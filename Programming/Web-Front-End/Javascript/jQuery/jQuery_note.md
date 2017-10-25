- [jQuery Note](#jquery-note)
  - [介绍](#%E4%BB%8B%E7%BB%8D)
  - [基础](#%E5%9F%BA%E7%A1%80)
    - [加载](#%E5%8A%A0%E8%BD%BD)
    - [jQuery 对象](#jquery-%E5%AF%B9%E8%B1%A1)
    - [构造函数](#%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)
  - [实例方法](#%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95)
    - [结果集过滤方法](#%E7%BB%93%E6%9E%9C%E9%9B%86%E8%BF%87%E6%BB%A4%E6%96%B9%E6%B3%95)
    - [DOM 操作方法](#dom-%E6%93%8D%E4%BD%9C%E6%96%B9%E6%B3%95)
    - [动画效果方法](#%E5%8A%A8%E7%94%BB%E6%95%88%E6%9E%9C%E6%96%B9%E6%B3%95)
    - [时间处理方法](#%E6%97%B6%E9%97%B4%E5%A4%84%E7%90%86%E6%96%B9%E6%B3%95)
    - [其它方法](#%E5%85%B6%E5%AE%83%E6%96%B9%E6%B3%95)
  - [工具方法](#%E5%B7%A5%E5%85%B7%E6%96%B9%E6%B3%95)
    - [类型判断](#%E7%B1%BB%E5%9E%8B%E5%88%A4%E6%96%AD)
    - [AJAX 方法](#ajax-%E6%96%B9%E6%B3%95)
      - [基本方法](#%E5%9F%BA%E6%9C%AC%E6%96%B9%E6%B3%95)
        - [方法调用](#%E6%96%B9%E6%B3%95%E8%B0%83%E7%94%A8)
        - [参数说明](#%E5%8F%82%E6%95%B0%E8%AF%B4%E6%98%8E)
        - [返回值说明](#%E8%BF%94%E5%9B%9E%E5%80%BC%E8%AF%B4%E6%98%8E)
      - [简便写法](#%E7%AE%80%E4%BE%BF%E5%86%99%E6%B3%95)
        - [$.get()、$.post()](#get%E3%80%81post)
        - [$.getJSON()](#getjson)
        - [$.getScript()](#getscript)
        - [$.fn.load()](#fnload)
      - [AJAX 事件](#ajax-%E4%BA%8B%E4%BB%B6)
      - [JSONP](#jsonp)
      - [AJAX 文件上传](#ajax-%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0)
  - [插件](#%E6%8F%92%E4%BB%B6)
  - [Deferred 对象](#deferred-%E5%AF%B9%E8%B1%A1)
  - [Refer Links](#refer-links)

# jQuery Note

## 介绍

jQuery 是目前使用最广泛的 JavaScript 函数库。

jQuery 的最大优势有两个。首先，它基本是一个 DOM 操作工具，可以使操作 DOM 对象变得异常容易。其次，它统一了不同浏览器的 API 接口，使得代码在所有现代浏览器均能运行，开发者不用担心浏览器之间的差异。

## 基础

### 加载

将以下代码放到网页 body 尾部（浏览器按顺序解析标签，优先显示静态元素而将 js 最后解析以提高用户体验）；

如果需要支持 IE 6/7/8，就使用 jQuery 1.x 版，否则使用最新版；
```html
<!-- 使用 CDN 加载 jquery 文件，注意采用协议无关的加载网址（使用双斜线表示），同时支持 http 协议和 https 协议 -->
<script type="text/javascript" src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<!-- 如果 CDN 加载失败，则退回到本地加载 -->
<script>
window.jQuery ||
  document.write(
    '<script src="js/jquery-1.11.0.min.js" type="text/javascript"><\/script>'
  );
</script>
```

### jQuery 对象

### 构造函数

jQuery 的核心思想是“先选中某些网页元素，然后对其进行某种处理”（find something, do something），也就是说，先选择后处理，这是 jQuery 的基本操作模式。

## 实例方法

链式写法比滥用选择器的速度快的多

$('#x') 比 getElementById('x') 慢 40 倍

### 结果集过滤方法

### DOM 操作方法

### 动画效果方法

### 时间处理方法

### 其它方法

## 工具方法
jQuery 函数库提供了一个 jQuery 对象（简写为 $），这个对象本身是一个构造函数，可以用来生成 jQuery 对象的实例。有了实例以后，就可以调用许多针对实例的方法，它们定义 jQuery.prototype 对象上面（简写为 $.fn）。

除了实例对象的方法以外，jQuery 对象本身还提供一些方法（即直接定义 jQuery 对象上面），不需要生成实例就能使用。由于这些方法类似“通用工具”的性质，所以我们把它们称为“工具方法”（utilities）。

### 类型判断

jQuery 提供一系列工具方法，用来判断数据类型，以弥补 JavaScript 原生的 typeof 运算符的不足。以下方法对参数进行判断，返回一个布尔值：
```javascript
jQuery.isArray()：是否为数组。
jQuery.isEmptyObject()：是否为空对象（不含可枚举的属性）。
jQuery.isFunction()：是否为函数。
jQuery.isNumeric()：是否为数值（整数或浮点数）。
jQuery.isPlainObject()：是否为使用“{}”或“new Object”生成的对象，而不是浏览器原生提供的对象。
jQuery.isWindow()：是否为 window 对象。
jQuery.isXMLDoc()：判断一个 DOM 节点是否处于 XML 文档之中。
jQuery.type()：返回一个变量的数据类型。它的实质是用 Object.prototype.toString 方法读取对象内部的 [[Class]] 属性
```
例：
```javascript
$.isEmptyObject({}) // true
$.isPlainObject(document.location) // false
$.isWindow(window) // true
$.isXMLDoc(document.body) // false
$.type(/test/) // "regexp"
```

### AJAX 方法
jQuery 对象上面还定义了 Ajax 方法（$.ajax()），用来处理 Ajax 操作。调用该方法后，浏览器就会向服务器发出一个 HTTP 请求：

#### 基本方法

##### 方法调用

```javascript
$.ajax(url[, options])
$.ajax([options])
```

##### 参数说明
- url 指的是服务器网址；

- options 则是一个对象参数，设置 Ajax 请求的具体参数：
  - accepts：将本机所能处理的数据类型，告诉服务器。
  - async：该项默认为 true，如果设为 false，则表示发出的是同步请求。
  - beforeSend：指定发出请求前，所要调用的函数，通常用来对发出的数据进行修改。
  - cache：该项默认为 true，如果设为 false，则浏览器不缓存返回服务器返回的数据。注意，浏览器本身就不会缓存 POST 请求返回的数据，所以即使设为 false，也只对 HEAD 和 GET 请求有效。
  - complete：指定当 HTTP 请求结束时（请求成功或请求失败的回调函数，此时已经运行完毕）的回调函数。不管请求成功或失败，该回调函数都会执行。它的参数为发出请求的原始对象以及返回的状态信息。
  - contentType：发送到服务器的数据类型。
  - context：指定一个对象，作为所有 Ajax 相关的回调函数的 this 对象。
  - crossDomain：该属性设为 true，将强制向相同域名发送一个跨域请求（比如 JSONP）。
  - data：向服务器发送的数据，如果使用 GET 方法，此项将转为查询字符串，附在网址的最后。
  - dataType：向服务器请求的数据类型，可以设为 text、html、script、json、jsonp 和 xml。
  - error：请求失败时的回调函数，函数参数为发出请求的原始对象以及返回的状态信息。
  - headers：指定 HTTP 请求的头信息。
  - ifModified：如果该属性设为 true，则只有当服务器端的内容与上次请求不一样时，才会发出本次请求。
  - jsonp：指定 JSONP 请求“callback=?”中的 callback 的名称。
  - jsonpCallback: 指定 JSONP 请求中回调函数的名称。
  - mimeType：指定 HTTP 请求的 mime type。
  - password：指定 HTTP 认证所需要的密码。
  - statusCode：值为一个对象，为服务器返回的状态码，指定特别的回调函数。
  - success：请求成功时的回调函数，函数参数为服务器传回的数据、状态信息、发出请求的原始对象。
  - timeout: 等待的最长毫秒数。如果过了这个时间，请求还没有返回，则自动将请求状态改为失败。
  - type：向服务器发送信息所使用的 HTTP 动词，默认为 GET，其他动词有 POST、PUT、DELETE。
  - **url：服务器端网址。这是唯一必需的一个属性，其他属性都可以省略。且 url 可以独立出来，作为 ajax 方法的第一个参数**。
  - username：指定 HTTP 认证的用户名。
  - xhr：指定生成 XMLHttpRequest 对象时的回调函数。

例：
```javascript
$.ajax({
  async: true,
  url: '/url/to/json',
  type: 'GET',
  data : { id : 123 },
  dataType: 'json',
  timeout: 30000,
  success: successCallback,
  error: errorCallback,
  complete: completeCallback,
  statusCode: {
        404: handler404,
        500: handler500
  }
})
// 相当于
$.ajax('/url/to/json',{
  type: 'GET',
  dataType: 'json',
  success: successCallback,
  error: errorCallback
...
})

// 作为向服务器发送的数据，data 属性也可以写成一个对象
$.ajax({
  url: '/remote/url',
  data: {
    param1: 'value1',
    param2: 'value2',
    ...
  }
});

// 相当于
$.ajax({
    url: '/remote/url?param1=value1&param2=value2...'
}});

```

##### 返回值说明
ajax 方法返回的是一个 deferred 对象，可以用 then 方法为该对象指定回调函数

例：
```javascript
$.ajax({
  url: '/data/people.json',
  dataType: 'json'
}).then(function (resp){
  console.log(resp.people);
})
```

#### 简便写法

```javascript
$.get()：发出 GET 请求。
$.getScript()：读取一个 JavaScript 脚本文件并执行。
$.getJSON()：发出 GET 请求，读取一个 JSON 文件。
$.post()：发出 POST 请求。
$.fn.load()：读取一个 html 文件，并将其放入当前元素之中。
```
这些简便方法依次接受三个参数：url、数据、成功时的回调函数。

##### $.get()、$.post()

这两个方法分别对应 HTTP 的 GET 方法和 POST 方法。

例：
```javascript
$.get('/data/people.html', function(html){
  $('#target').html(html);
});

$.post('/data/save', {name: 'Rebecca'}, function (resp){
  console.log(JSON.parse(resp));
});
```

##### $.getJSON()
当服务器端返回 JSON 格式的数据，可以用这个方法代替 $.ajax 方法。

例：
```javascript
$.getJSON('url/to/json', {'a': 1}, function(data){
	console.log(data);
});
```
上面的代码等同于下面的写法。
```javascript
$.ajax({
  dataType: "json",
  url: '/url/to/data',
  data: {'a': 1},
  success: function(data){
	console.log(data);
  }
});
```

##### $.getScript()
$.getScript 方法用于从服务器端加载一个脚本文件。

例：
```javascript
$.getScript('/static/js/myScript.js', function() {
	functionFromMyScript();
});
// 上面代码先从服务器加载 myScript.js 脚本，然后在回调函数中执行该脚本提供的函数。
```
getScript 的回调函数接受三个参数，分别是脚本文件的内容，HTTP 响应的状态信息和 ajax 对象实例。
```javascript
$.getScript( "ajax/test.js", function (data, textStatus, jqxhr){
  console.log( data ); // test.js 的内容
  console.log( textStatus ); // Success
  console.log( jqxhr.status ); // 200
});
```
getScript 是 ajax 方法的简便写法，因此返回的是一个 deferred 对象，可以使用 deferred 接口。
```javascript
jQuery.getScript("/path/to/myscript.js")
	.done(function() {
		// ...
	})
	.fail(function() {
		// ...
});
```

##### $.fn.load()
$.fn.load 不是 jQuery 的工具方法，而是定义在 jQuery 对象实例上的方法，用于获取服务器端的 HTML 文件，将其放入当前元素。
```javascript
$('#newContent').load('/foo.html');
```
$.fn.load 方法还可以指定一个选择器，将远程文件中匹配选择器的部分，放入当前元素，并指定操作完成时的回调函数。
```javascript
$('#newContent').load('/foo.html #myDiv h1:first',
	function(html) {
		console.log('内容更新！');
});
```

#### AJAX 事件
jQuery 提供以下**实例方法（不是工具方法）**，用于指定特定的 AJAX 事件的回调函数。
```javascript
.ajaxComplete()：ajax 请求完成。
.ajaxError()：ajax 请求出错。
.ajaxSend()：ajax 请求发出之前。
.ajaxStart()：第一个 ajax 请求开始发出，即没有还未完成 ajax 请求。
.ajaxStop()：所有 ajax 请求完成之后。
.ajaxSuccess()：ajax 请求成功之后。
```
例：
```javascript
$('#loading_indicator')
.ajaxStart(function (){$(this).show();})
.ajaxStop(function (){$(this).hide();});
```
例：
```javascript
// 处理 Ajax 请求出错（返回 404 或 500 错误）
$(document).ajaxError(function (e, xhr, settings, error) {
  console.log(error);
});
```

#### JSONP
由于浏览器存在“同域限制”，ajax 方法只能向当前网页所在的域名发出 HTTP 请求。但是，通过在当前网页中插入 script 元素（`<script>`），可以向不同的域名发出 GET 请求，这种变通方法叫做 JSONP（JSON with Padding）。

ajax 方法可以发出 JSONP 请求，方法是在对象参数中指定 dataType 为 JSONP。

例：
```javascript
$.ajax({
  url: '/data/search.jsonp',
  data: {q: 'a'},
  dataType: 'jsonp',
  success: function(resp) {
    $('#target').html('Results: ' + resp.results.length);
  }
});)
```
JSONP 的通常做法是，在所要请求的 URL 后面加在回调函数的名称。ajax 方法规定，如果所请求的网址以类似“callback=?”的形式结尾，则自动采用 JSONP 形式。所以，上面的代码还可以写成下面这样。
```javascript
$.getJSON('/data/search.jsonp?q=a&callback=?',
  function(resp) {
    $('#target').html('Results: ' + resp.results.length);
  }
);
```

#### AJAX 文件上传
```html
<input type="file" id="test-input">
```
JavaScript
```javascript
// 方式一：将文件作为表单数据发送
var file = $('#test-input')[0].files[0];
var formData = new FormData();

formData.append('file', file);

$.ajax('myserver/uploads', {
  method: 'POST',
  contentType: false,
  processData: false,
  data: formData
});

// 方式二：直接发送文件。
var file = $('#test-input')[0].files[0];

$.ajax('myserver/uploads', {
  method: 'POST',
  contentType: file.type,
  processData: false,
  data: file
});
```

## 插件

插件使用 https://www.cnblogs.com/zhangziqiu/archive/2009/05/22/jQuery-Learn-11.html 

自定义插件 http://javascript.ruanyifeng.com/jquery/plugin.html 

## Deferred 对象

http://javascript.ruanyifeng.com/jquery/deferred.html 

## Refer Links

http://javascript.ruanyifeng.com/jquery/basic.html

http://javascript.ruanyifeng.com/jquery/utility.html 

https://www.cnblogs.com/zhangziqiu/archive/2009/04/30/jquery-learn-1.html 
