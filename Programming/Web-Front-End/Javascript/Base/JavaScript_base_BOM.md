- [JavaScript Note - BOM](#javascript-note---bom)
  - [1. window 对象](#1-window-%E5%AF%B9%E8%B1%A1)
    - [1.1. 浏览器各种高度、宽度](#11-%E6%B5%8F%E8%A7%88%E5%99%A8%E5%90%84%E7%A7%8D%E9%AB%98%E5%BA%A6%E3%80%81%E5%AE%BD%E5%BA%A6)
  - [2. history 对象](#2-history-%E5%AF%B9%E8%B1%A1)
    - [2.1. history.length](#21-historylength)
    - [2.2. back()、forward()、go()](#22-back%E3%80%81forward%E3%80%81go)
    - [2.3. history.pushState()](#23-historypushstate)
    - [2.4. history.replaceState()](#24-historyreplacestate)
    - [2.5. history.state](#25-historystate)
    - [2.6. popstate 事件](#26-popstate-%E4%BA%8B%E4%BB%B6)
  - [3. Cookie](#3-cookie)
  - [4. Web Storage](#4-web-storage)
  - [5. 同源政策](#5-%E5%90%8C%E6%BA%90%E6%94%BF%E7%AD%96)
  - [6. Ajax](#6-ajax)
    - [6.1. XMLHttpRequest](#61-xmlhttprequest)
      - [6.1.1. AJAX request](#611-ajax-request)
        - [6.1.1.1. send()](#6111-send)
        - [6.1.1.2. 设置 request header:setRequestHeader()](#6112-%E8%AE%BE%E7%BD%AE-request-headersetrequestheader)
        - [6.1.1.3. header 设置限制](#6113-header-%E8%AE%BE%E7%BD%AE%E9%99%90%E5%88%B6)
        - [6.1.1.4. timeout](#6114-timeout)
        - [6.1.1.5. withCredentials](#6115-withcredentials)
        - [6.1.1.6. progress 事件](#6116-progress-%E4%BA%8B%E4%BB%B6)
      - [6.1.2. AJAX respones](#612-ajax-respones)
        - [6.1.2.1. response](#6121-response)
        - [6.1.2.2. responseType](#6122-responsetype)
        - [6.1.2.3. 请求回调函数 -- 处理响应](#6123-%E8%AF%B7%E6%B1%82%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0----%E5%A4%84%E7%90%86%E5%93%8D%E5%BA%94)
  - [7. CORS](#7-cors)
  - [8. Refer Links](#8-refer-links)

# JavaScript Note - BOM #

浏览器对象模型 BOM 提供给开发人员控制浏览器的接口，虽 BOM 作为 JavaScript 实现的一部分，但 BOM 至今仍无统一标准。

## 1. window 对象

所有浏览器都支持 window 对象，它表示浏览器窗口；

所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员；

全局变量是 window 对象的属性，全局函数是 window 对象的方法；
例：HTML DOM 的 document 也是 window 对象的属性之一
```javascript
window.document.getElementById("header");
```
等同于
```javascript
document.getElementById("header");
```

### 1.1. 浏览器各种高度、宽度

原文：https://www.w3cplus.com/css/vw-for-layout.html

![image](http://img.cdn.firejq.com/jpg/2017/11/10/def5e9e3794e5dee214d9dd2dc968c68.jpg)

## 2. history 对象

浏览器窗口有一个 history 对象，用来保存浏览历史，浏览器能够支持在用户访问过的页面间进行前进 / 后退的操作，就是依赖于该 history 对象。

### 2.1. history.length

> The History.length read-only property returns an Integer representing the number of elements in the session history, including the currently loaded page. For example, for a page loaded in a new tab this property returns 1.

如果当前窗口先后访问了三个网址，那么 history 对象就包括三项，history.length 属性等于 3。

### 2.2. back()、forward()、go()

history 对象提供了一系列方法，允许在浏览历史之间移动。
- back()：移动到上一个访问页面，等同于浏览器的后退键。
- forward()：移动到下一个访问页面，等同于浏览器的前进键。
- go()：接受一个整数作为参数，移动到该整数指定的页面。

  `go(1)`相当于`forward()`，`go(-1)`相当于`back()`，`go(0)`相当于`location.reload()`。

注意：
- 返回上一页时，页面通常是从浏览器缓存之中加载，而不是重新要求服务器发送新的网页。
- 如果移动的位置超出了访问历史的边界，以上三个方法并不报错，而是默默的失败。
- 出于安全性的考虑，浏览器并不允许 JavaScript 脚本对该对象进行增删改之类写操作，而只是可以通过 history. back/forward() 等方法进行访问。

### 2.3. history.pushState()

history.pushState() 是 H5 新增的方法，用来在浏览历史中添加一条新记录。但 pushState 方法不会触发页面刷新，只是导致 history 对象发生变化，在地址栏反应变化。

- 检测浏览器支持：
  ```javascript
  if (!!(window.history && history.pushState)){
    // 支持 History API
  } else {
    // 不支持
  }
  ```

- history.pushState 方法接受三个参数，依次为：
  - state：一个与指定网址相关的状态对象，popstate 事件触发时，该对象会传入回调函数。如果不需要这个对象，此处可以填 null。
  - title：新页面的标题，但是所有浏览器目前都忽略这个值，因此这里可以填 null。
  - url：**新的网址，必须与当前页面处在同一个域**。浏览器的地址栏将显示这个网址。

- 例：

  当前网址是 example.com/1.html，我们使用 pushState 方法在浏览记录（history 对象）中添加一个新记录：
  ```javascript
  history.pushState({ foo: 'bar' }, 'page 2', '2.html');
  ```
  添加上面这个新记录后，浏览器地址栏立刻显示 example.com/2.html，但并不会跳转到 2.html，甚至也不会检查 2.html 是否存在，它只是成为浏览历史中的最新记录。这时，你在地址栏输入一个新的地址（比如访问 google.com)，然后点击了倒退按钮，页面的 URL 将显示 2.html；你再点击一次倒退按钮，URL 将显示 1.html。

### 2.4. history.replaceState()

history.replaceState 方法的参数与 pushState 方法一模一样，区别是它修改浏览历史中当前纪录而不是添加一条新的记录。

### 2.5. history.state

history.state 属性返回当前页面的 state 对象：
```javascript
history.pushState({page: 1}, 'title 1', '?page=1');

history.state
// { page: 1 }
```

### 2.6. popstate 事件

**每当同一个文档的浏览历史（即 history 对象）出现变化时，就会触发 popstate 事件。**

需要注意的是，**仅仅调用 pushState 方法或 replaceState 方法 ，并不会触发该事件，只有用户点击浏览器倒退按钮和前进按钮，或者使用 JavaScript 调用 back、forward、go 方法时才会触发。**另外，该事件只针对同一个文档，如果浏览历史的切换，导致加载不同的文档，该事件也不会触发。

```javascript
window.onpopstate = function (event) {
  console.log('location: ' + document.location);
  console.log('state: ' + JSON.stringify(event.state));
};

// 或者

window.addEventListener('popstate', function(event) {
  console.log('location: ' + document.location);
  console.log('state: ' + JSON.stringify(event.state));
});
```
回调函数的参数是一个 event 事件对象，它的 state 属性指向 pushState 和 replaceState 方法为当前 URL 所提供的状态对象（即这两个方法的第一个参数）。上面代码中的 event.state，就是通过 pushState 和 replaceState 方法，为当前 URL 绑定的 state 对象。

## 3. Cookie

## 4. Web Storage

## 5. 同源政策

## 6. Ajax

https://www.w3schools.com/xml/ajax_intro.asp

http://javascript.ruanyifeng.com/bom/ajax.html

- AJAX(Asynchronous JavaScript and XML) 即异步 JavaScript 和 XML，通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

- AJAX 可以是同步请求，也可以是异步请求。但是，大多数情况下，特指异步请求。因为同步的 Ajax 请求，对浏览器有“堵塞效应”。

- 除了 HTTP，它还支持通过其他协议传送（比如 File 和 FTP）。

### 6.1. XMLHttpRequest

XMLHttpRequest 是 AJAX 的基础，用于在后台与服务器交换数据。

创建 XMLHttpRequest 对象：
```javascript
xhr = new XMLHttpRequest();
```

- XMLHttpRequest Object Properties

  | Property           | Description                              |
  | ------------------ | ---------------------------------------- |
  | readyState         | Holds the status of the XMLHttpRequest: <br/>0: request not initialized <br/>1: server connection established <br/>2: request received <br/>3: processing request <br/>4: request finished and response is ready |
  | responseText       | 获得字符串形式的响应数据。如果来自服务器的响应并非 XML，请使用 responseText 属性。该属性为只读。如果本次请求没有成功或者数据不完整，该属性就会等于 null。    |
  | responseXML        | 获得 XML 形式的响应数据。如果来自服务器的响应是 XML，而且需要作为 XML 对象进行解析，请使用 responseXML 属性。该属性为只读。如果本次请求没有成功，或者数据不完整，或者不能被解析为 XML 或 HTML，该属性等于 null。    |
  | status             | Returns the status-number of a request:<br/>200: "OK"<br/>403: "Forbidden"<br/>404: "Not Found"<br/>For a complete list go to the [Http Messages Reference](https://www.w3schools.com/tags/ref_httpmessages.asp) |
  | statusText         | Returns the status-text (e.g. "OK" or "Not Found") |
  | onreadystatechange | Defines a function to be called when the readyState property changes |
  |onloadstart |XHR2 增加的事件监听接口，监听事件：请求发出|
  |onprogress | XHR2 增加的事件监听接口，监听事件：正在发送和加载数据|
  |onabort | XHR2 增加的事件监听接口，监听事件：请求被中止，比如用户调用了 abort() 方法|
  |onerror | XHR2 增加的事件监听接口，监听事件：请求失败，如果发生网络错误（比如服务器无法连通），onerror 事件无法获取报错信息，所以只能显示报错。|
  |onload | XHR2 增加的事件监听接口，监听事件：请求成功完成|
  |ontimeout | XHR2 增加的事件监听接口，监听事件：用户指定的时限到期，请求还未完成|
  |onloadend | XHR2 增加的事件监听接口，监听事件：请求完成，不管成果或失败|


- XMLHttpRequest Object Methods

  | Method                            | Description                              |
  | --------------------------------- | ---------------------------------------- |
  | new XMLHttpRequest()              | Creates a new XMLHttpRequest object      |
  | abort()                           | Cancels the current request              |
  | getAllResponseHeaders()           | 返回服务器发来的所有 HTTP 头信息。格式为字符串，每个头信息之间使用 CRLF 分隔，如果没有受到服务器回应，该属性返回 null。              |
  | getResponseHeader()               | 返回 HTTP 头信息指定字段的值，如果还没有收到服务器回应或者指定字段不存在，则该属性为 null。如果有多个字段同名，则它们的值会被连接为一个字符串，每个字段之间使用“逗号 + 空格”分隔。      |
  | open(*method,url,async,user,psw*) | Specifies the request: <br/>*method*: the request type GET or POST<br/>*url*: the file location<br/>*async*: true (asynchronous) or false (synchronous)<br/>*user*: optional user name<br/>*psw*: optional password |
  | send()                            | Sends the request to the serverUsed for GET requests |
  | send(*string*)                    | Sends the request to the server.Used for POST requests |
  | setRequestHeader()                | Adds a label/value pair to the header to be sent |

#### 6.1.1. AJAX request

如需将请求发送到服务器，我们须使用 XMLHttpRequest 对象的 open() 和 send() 方法：

| 方法                           | 描述                                       |
| ---------------------------- | ---------------------------------------- |
| open(*method*,*url*,*async*) | 规定请求的类型、URL 以及是否异步处理请求。<br/>*method*：请求的类型；GET 或 POST<br/>*url*：文件在服务器上的位置<br/>*async*：true（异步）或 false（同步） |
| send()               | 将请求发送到服务器（GET 请求）。           |
| send(*string*)               | 将请求发送到服务器（POST 请求）。           |

XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true.

  - 当使用 async=true 时，请规定在响应处于 onreadystatechange 事件中的就绪状态时执行的函数：
    ```javascript
    xmlhttp.onreadystatechange=function() {
      if (xmlhttp.readyState==4 && xmlhttp.status==200) {
        document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
      }
    };
    xmlhttp.open("GET","test1.txt",true);
    xmlhttp.send();
    ```

  - 当使用 async=false 时，直接在 send() 之后编写处理函数即可。

##### 6.1.1.1. send()

send 方法用于实际发出 HTTP 请求。如果不带参数，就表示 HTTP 请求只包含头信息，也就是只有一个 URL，典型例子就是 GET 请求；如果带有参数，就表示除了头信息，还带有包含具体数据的信息体，典型例子就是 POST 请求。

send 方法的参数就是发送的数据。多种格式的数据，都可以作为它的参数。
```javascript
void send();
void send(ArrayBufferView data);
void send(Blob data);
void send(Document data);
void send(String data);
void send(FormData data);
```

实际上，带参数的 send() 方法也可以用于 GET 请求：
```javascript
ajax.open('GET'
  , 'http://www.example.com/somepage.php?id=' + encodeURIComponent(id)
  , true
);

// 等同于
var data = 'id=' + encodeURIComponent(id));
ajax.open('GET', 'http://www.example.com/somepage.php', true);
ajax.send(data);
```

POST 请求：
```javascript
var data = 'email='
  + encodeURIComponent(email)
  + '&password='
  + encodeURIComponent(password);
ajax.open('POST', 'http://www.example.com/somepage.php', true);
ajax.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
ajax.send(data);
```

NOTE: 所有 XMLHttpRequest 的监听事件，都必须在 send() 方法调用之前设定。

##### 6.1.1.2. 设置 request header:setRequestHeader()

如需改变 request header，需要使用 XMLHttpRequest 对象的 setRequestHeader() 方法：

| 方法                                 | 描述                                       |
| ---------------------------------- | ---------------------------------------- |
| setRequestHeader(*header*,*value*) | 向请求添加 HTTP 头。*header*: 规定头的名称*value*: 规定头的值 |

setRequestHeader 方法用于设置 HTTP 头信息。该方法必须在 open() 之后、send() 之前调用。如果该方法多次调用，设定同一个字段，则每一次调用的值会被合并成一个单一的值发送。

例：
```javascript
xmlhttp.open("POST","ajax_test.asp",true);
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xmlhttp.send("fname=Bill&lname=Gates");
```

##### 6.1.1.3. header 设置限制

参见：

https://xhr.spec.whatwg.org/#the-setrequestheader()-method

https://fetch.spec.whatwg.org/#terminology-headers

https://fetch.spec.whatwg.org/#forbidden-header-name

使用 AJAX 发送 request 时，由于 JavaScript 最终依赖于浏览器运行，因此无法完全无限制的定制 request headers（受限于 W3C 标准）：
![image](http://img.cdn.firejq.com/jpg/2017/10/12/e65bd2fa99c02a77e7836a21d661f88f.jpg)

> A forbidden header name is a header name that is a byte-case-insensitive match for one of:
>
> `Accept-Charset`
`Accept-Encoding`
`Access-Control-Request-Headers`
`Access-Control-Request-Method`
`Connection`
`Content-Length`
`Cookie`
`Cookie2`
`Date`
`DNT`
`Expect`
`Host`
`Keep-Alive`
`Origin`
`Referer`
`TE`
`Trailer`
`Transfer-Encoding`
`Upgrade`
`User-Agent`
`Via`
or a header name that starts with a byte-case-insensitive match for `Proxy-` or `Sec-` (including being a byte-case-insensitive match for just `Proxy-` or `Sec-`).

注：大部分浏览器遵循了该 W3C 标准，但早期的 IE 不遵循，可以设置如 UA 等 header；

例：使用 ajax 发送 request 时设置 UA：
```javascript
$.ajax({
  url: 'https://www.baidu.com',
  type: "GET",
  headers: {
    'User-Agent': '6666666'
  },
  error: function(jqXHR, textStatus, errorThrown) {
    //....
  },
  success: function(data, textStatus, jqXHR) {
    //....
  }
});
```
chrome 报错：

![image](http://img.cdn.firejq.com/jpg/2017/10/12/8d89b40ec27526ee23573f5975ca6712.jpg)

##### 6.1.1.4. timeout

timeout 属性等于一个整数，表示多少毫秒后，如果请求仍然没有得到结果，就会自动终止。如果该属性等于 0，就表示没有时间限制。

例：
```javascript
var xhr = new XMLHttpRequest();
xhr.ontimeout = function () {// 监听超时事件
  console.error("The request for " + url + " timed out.");
};
xhr.onload = function() {
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      callback.apply(xhr, args);
    } else {
      console.error(xhr.statusText);
    }
  }
};
xhr.open("GET", url, true);
xhr.timeout = timeout;
xhr.send(null);
```

##### 6.1.1.5. withCredentials

withCredentials 属性是一个布尔值，表示跨域请求时，用户信息（比如 Cookie 和认证的 HTTP 头信息）是否会包含在请求之中，默认为 false。即向 example.com 发出跨域请求时，不会发送 example.com 设置在本机上的 Cookie（如果有的话）。

如果你需要通过跨域 AJAX 发送 Cookie，需要打开 withCredentials。
```javascript
xhr.withCredentials = true;
```
为了让这个属性生效，服务器必须显式返回 Access-Control-Allow-Credentials 这个头信息。
```
Access-Control-Allow-Credentials: true
```
.withCredentials 属性打开的话，不仅会发送 Cookie，还会设置远程主机指定的 Cookie。注意，此时你的脚本还是遵守同源政策，无法 从 document.cookie 或者 HTTP 回应的头信息之中，读取这些 Cookie。

##### 6.1.1.6. progress 事件

上传文件时，XMLHTTPRequest 对象的 upload 属性有一个 progress，会不断返回上传的进度。

假定网页上有一个 progress 元素：
```html
<progress min="0" max="100" value="0">0% complete</progress>
```
文件上传时，对 upload 属性指定 progress 事件回调函数，即可获得上传的进度：
```javascript
function upload(blobOrFile) {
  var xhr = new XMLHttpRequest();
  xhr.open('POST', '/server', true);
  xhr.onload = function(e) { ... };

  // Listen to the upload progress.
  var progressBar = document.querySelector('progress');
  xhr.upload.onprogress = function(e) {
    if (e.lengthComputable) {
      progressBar.value = (e.loaded / e.total) * 100;
      progressBar.textContent = progressBar.value; // Fallback for unsupported browsers.
    }
  };

  xhr.send(blobOrFile);
}

upload(new Blob(['hello world'], {type: 'text/plain'}));
```

#### 6.1.2. AJAX respones

##### 6.1.2.1. response

response 属性为只读，返回接收到的数据体（即 body 部分）。它的类型可以是 ArrayBuffer、Blob、Document、JSON 对象、或者一个字符串，这由 XMLHttpRequest.responseType 属性的值决定。

如果本次请求没有成功或者数据不完整，该属性就会等于 null。

##### 6.1.2.2. responseType

responseType 属性用来指定服务器返回数据（xhr.response）的类型：
```
“”：字符串（默认值）
“arraybuffer”：ArrayBuffer 对象
“blob”：Blob 对象
“document”：Document 对象
“json”：JSON 对象
“text”：字符串
```
- text 类型适合大多数情况，而且直接处理文本也比较方便

- document 类型适合返回 XML 文档的情况

- blob 类型适合读取二进制数据，比如图片文件：
  ```javascript
  var xhr = new XMLHttpRequest();
  xhr.open('GET', '/path/to/image.png', true);
  xhr.responseType = 'blob';

  xhr.onload = function(e) {
    if (this.status == 200) {
      var blob = new Blob([this.response], {type: 'image/png'});
      // 或者
      var blob = oReq.response;
    }
  };

  xhr.send();
  ```

- 若设为 ArrayBuffer，就可以按照数组的方式处理二进制数据
  ```javascript
  var xhr = new XMLHttpRequest();
  xhr.open('GET', '/path/to/image.png', true);
  xhr.responseType = 'arraybuffer';

  xhr.onload = function(e) {
    var uInt8Array = new Uint8Array(this.response);
    for (var i = 0, len = binStr.length; i < len; ++i) {
    // var byte = uInt8Array[i];
    }
  };

  xhr.send();
  ```

- 若设为“json”，支持 JSON 的浏览器（Firefox>9，chrome>30），就会自动对返回数据调用 JSON.parse() 方法

##### 6.1.2.3. 请求回调函数 -- 处理响应

- 请求成功回调函数：

  - 方法一
    ```javascript
    var xhttp=new XMLHttpRequest();
    xhttp.onreadystatechange = function() {
      // 当 readyState 等于 4 且状态为 200 时，表示响应已就绪
      if (this.readyState === 4 && this.status === 200) {
        console.log(this.responseText);
      }
    };
    xhttp.open("GET", url, true);
    xhttp.send();
    ```

  - 方法二
    ```javascript
    // 定义数据成功发送并返回后执行的操作
    xhr.addEventListener('load', function(event) {
      alert('Yeah! Data sent and response loaded.');
      console.log(this.responseText);
    });
    ```

- 请求失败回调函数：

  - 方法一
    ```javascript
    var xhttp=new XMLHttpRequest();
    xhttp.onreadystatechange = function() {
      // 当 readyState 等于 4 且状态不为 200 时，表示请求出错
      if (this.readyState === 4 && this.status !== 200) {
        console.log(this.responseText);
      }
    };
    xhttp.open("GET", url, true);
    xhttp.send();
    ```

  - 方法二
    ```javascript
    // 定义发生错误时执行的操作
    xhr.addEventListener('error', function(event) {
      alert('Oups! Something goes wrong.');
      console.log(this.responseText);
    });
    ```

## 7. CORS

## 8. Refer Links

W3C AJAX API：https://xhr.spec.whatwg.org/

http://javascript.ruanyifeng.com/
