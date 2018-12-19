- [ECMAScript6 Fetch](#ecmascript6-fetch)
  - [1. 概述](#1-%E6%A6%82%E8%BF%B0)
  - [2. 基本使用](#2-%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
  - [3. API](#3-api)
    - [3.1. WindowOrWorkerGlobalScope.fetch()](#31-windoworworkerglobalscopefetch)
    - [3.2. Headers](#32-headers)
    - [3.3. Request](#33-request)
    - [3.4. Response](#34-response)
  - [4. 兼容性](#4-%E5%85%BC%E5%AE%B9%E6%80%A7)
  - [5. 缺点](#5-%E7%BC%BA%E7%82%B9)
  - [6. Refer Links](#6-refer-links)

# ECMAScript6 Fetch

## 1. 概述

在 Ajax 中涉及到的 JavaScript 方面的技术，即 XMLHttpRequest（以下简称 XHR）。很长一段时间我们都是通过 XHR 来与服务器建立异步通信。然而在使用的过程中，我们发现 XHR 是基于事件的异步模型，在设计上将输入、输出和事件监听混杂在一个对象里，且必须通过实例化方式来发请求。配置和调用方式混乱，不符合关注分点离原则。

正是由于 XHR 在使用上的不便，许多前端库就将进行封装，方便开发者调用。其中影响和使用范围最广的当属 jQuery 提供的 $.ajax 方法。该方法最为先进之处在于，从 jQuery 1.5 开始，$.ajax() 返回的 jqXHR 对象 实现了 Promise 接口，使它拥有了 Promise 的所有属性，方法和行为。

Fetch API 是一个 HTML5 的 API，旨在修正上述缺陷，它提供了与 HTTP 语义相同的 JS 语法，却让前端和服务器端的异步通信方面更进了一步。简单来说，它引入了 fetch() 这个实用的方法来获取网络资源。

> A modern replacement for XMLHttpRequest.

Fetch 并不是 XHR 的升级版本，而是从一个全新的角度来思考的一种设计。Fetch 是基于 Promise 语法结构，语法简洁，更加语义化，而且它的设计足够低阶，这表示它可以在实际需求中进行更多的弹性设计。对于 XHR 所提供的能力来说，Fetch 已经足够取代 XHR ，并且提供了更多拓展的可能性。

## 2. 基本使用

```javascript
// 获取 some.json 资源
fetch('some.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(data) {
    console.log('data', data);
  })
  .catch(function(error) {
    console.log('Fetch Error: ', error);
  });
```
可配合使用 ES2016 中的 async / await 语法，更加优雅：
```javascript
async function getReq(url) {
  try {
    const response = await fetch(url);
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.log('Fetch Error: ', error)
  }
}
```

NOTE

- Fetch 请求默认是不带 cookie 的，需要设置 fetch(url, {credentials: 'include'})

- 服务器返回 400，500 错误码时并不会 reject，只有网络错误这些导致请求不能完成时，fetch 才会被 reject。

## 3. API

### 3.1. WindowOrWorkerGlobalScope.fetch()

https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalFetch/fetch

Window 和 WorkerGlobalScope 都实现了 WorkerOrGlobalScope。 ——这意味着基本在任何场景下只要你想获取资源，都可以使用 位于 WorkerOrGlobalScope 中的 fetch() 方法。

> Promise fetch(String url, [, Object options])
> Promise fetch(Request req, [, Object options])

参数
- url 或 Request 对象

- 配置信息对象（可选）

  配置项对象可以包含以下字段：
  - method: 请求的方法，例如：GET, POST。
  - headers: 请求头部信息，可以是一个简单的对象，也可以是 Headers 类实例化的一个对象。
  - body: 需要发送的信息内容，可以是 Blob, BufferSource, FormData, URLSearchParams 或者 USVString。注意，GET, HEAD 方法不能包含 body。
  - mode: 请求模式，分别有 cors, no-cors, same-origin, navigate 这几个可选值。
    - cors: 允许跨域，要求响应中 Acess-Control-Allow-Origin 这样的头部表示允许跨域。
    - no-cors: 只允许使用 HEAD, GET, POST 方法。
    - same-origin: 只允许同源请求，否则直接报错。
    - navigate: 支持页面导航。
  - credentials: 表示是否发送 cookie，有三个选项
    - omit: 不发送 cookie。
    - same-origin: 仅在同源时发送 cookie。
    - include: 发送 cookie。
  - cache: 表示处理缓存的策略。
  - redirect: 表示发生重定向时，有三个选项
    - follow: 跟随。
    - error: 发生错误。
    - manual: 需要用户手动跟随。
  - integrity: 包含一个用于验证资资源完整性的字符串。

返回值：一个 Promise，resolve 时回传 Response 对象。

### 3.2. Headers

https://developer.mozilla.org/zh-CN/docs/Web/API/Headers

Fetch API 的 Headers 接口允许您对 HTTP 请求和响应头执行各种操作。 这些操作包括检索，设置，添加和删除。 一个 Headers 对象具有关联的头列表，它最初为空，由零个或多个键值对组成。

出于安全考虑，某些头只能由用户代理控制。这些头信息包括 forbidden header names  和 forbidden response header names。

方法
- `Headers.append()`：给现有的 header 添加一个值，或者添加一个未存在的 header 并赋值。

- `Headers.delete()`：从 Headers 对象中删除指定 header。

- `Headers.entries()`：以 迭代器 的形式返回 Headers 对象中所有的键值对。

- `Headers.get()`：从 Headers 对象中返回指定 header 的第一个值。

- `Headers.getAll()`：以数组的形式从 Headers 对象中返回指定 header 的全部值。

- `Headers.has()`：以布尔值的形式从 Headers 对象中返回是否存在指定的 header。

- `Headers.keys()`：以迭代器的形式返回 Headers 对象中所有存在的 header 名。

- `Headers.set()`：替换现有的 header 的值，或者添加一个未存在的 header 并赋值。

- `Headers.values()`：以迭代器的形式返回 Headers 对象中所有存在的 header 的值。

E.G.
```javascript
var headers = new Headers({
  "Content-Type": "text/plain",
  "Content-Length": content.length.toString(),
  "X-Custom-Header": "ProcessThisImmediately",
});
headers.append("X-Custom-Header", "AnotherValue");
headers.has("Content-Type") // true
headers.getAll("X-Custom-Header"); // ["ProcessThisImmediately", "AnotherValue"]
```

### 3.3. Request

https://developer.mozilla.org/zh-CN/docs/Web/API/Request

Fetch API 的 Request 接口？用来表示资源请求。事实上 fetch 方法在调用时，会将传入的参数构造出一个 Request 对象并执行。

属性（只读）
- `Request.method`：请求使用的方法 (GET, POST, 等。)
- `Request.url`：请求使用的 URL。
- `Request.headers`：请求所关联的 Headers 对象。
- `Request.context`  ：请求的上下文 例如：（例如：audio, image, iframe, 等)
- `Request.referrer`：? 请求的来源 （例如：client).
- `Request.mode`：请求的模式 （例如： cors, no-cors, same-origin).
- `Request.credentials`：请求的凭证 （例如： omit, same-origin).
- `Request.redirect`：? 如何处理重定向模式 （例如： follow, error, or manual)
- `Request.integrity`：请求内容的 subresource integrity 值 （例如：sha256-BpfBw7ivV8q2jLiT13fxDYAe2tJllusRSZ273h2nFSE=).
- `Request.cache`：请求的缓存模式 （例如： default, reload, no-cache).

Request 对象可以从已有的 Request 对象中继承，并拓展新的配置：
```javascript
var URL = '//api.some.com';
var getReq = new Request(URL, {method: 'GET', headers: headers });
// 基于已存在的 Request 实例，拓展创建新的 Request 实例
var postReq = new Request(getReq, {method: 'POST'});
```

E.G.
```javscript
var URL = '//api.some.com';
// 实例化 Headers
var headers = new Headers({
  "Content-Type": "text/plain",
  "Content-Length": content.length.toString(),
  "X-Custom-Header": "ProcessThisImmediately",
});
var getReq = new Request(URL, {method: 'GET', headers: headers });
fetch(getReq).then(function(response) {
  return response.json();
}).catch(function(error) {
  console.log('Fetch Error: ', error);
});
```

### 3.4. Response

https://developer.mozilla.org/zh-CN/docs/Web/API/Response

Response 实例是在 fentch() 处理完 promises 之后返回的。它的实例也可用通过 JavaScript 来创建，但只有在 ServiceWorkers 中才真正有用。

构造函数
> var res = new Response(body, init);

- body：body 可以是 Bolb, BufferSource, FormData, URLSearchParams, USVString 这些类型的值。

- init：init 是一个对象，可以包括以下这些字段
  - status: 响应状态码
  - statusText: 状态信息
  - headers: 头部信息，可以是对象或者 Headers 实例

属性（只读）
- bodyUsed: 用于表示响应内容是否被使用过，类型为 Boolean. bodyUsed 是 Body 对象的属性，但由于 Request 实现了 Body，因此 Request 中也有 bodyUsed 属性。
- headers: 头部信息
- ok: 表明请求是否成功，响应状态为 200 ~ 299 时，值为 true
- status: 状态码
- statusText: 状态信息
- type: 响应类型
  - basic: 同源
  - cors: 跨域
  - error: 出错
  - opaque: Request mode 设置为 “no-cors”的响应
- url: 响应地址

方法
- `Response.clone()`: 复制一个响应对象。
- `Response.arrayBuffer()`: 将响应数据转换为 arrayBuffer 后 reslove 。
- `Response.bolb()`: 把响应数据转换为 Bolb 后 reslove 。
- `Response.formData()`: 把响应数据转换为 formData 后 reslove 。
- `Response.json()`: 把响应内容解析为对象后 reslove 。
- `Response.text()`: 把响应数据当做字符串后 reslove 。

## 4. 兼容性

![image](http://img.cdn.firejq.com/jpg/2018/2/3/879bfbb6b50db99e3173b243820ce6a6.jpg)

可以通过检查 Headers、Request、Response 或 fetch 在 window 或 worker 作用域中是否存在，来检查是否支持 Fetch API。

目前 Fetch API 的原生支持率并不高，但引入下面这些 polyfill 后最低可完美支持 IE8+ ：
- 由于 IE8 是 ES3，需要引入 ES5 的 polyfill: [es5-shim, es5-sham](https://github.com/es-shims/es5-shim)。
- 引入 Promise 的 polyfill: [es6-promise](https://github.com/jakearchibald/es6-promise)。
- 引入 fetch 探测库：[fetch-detector](https://github.com/camsong/fetch-detector)。
- 引入 fetch 的 polyfill: [fetch-ie8](https://github.com/camsong/fetch-ie8)。
- 可选：如果你还使用了 jsonp，引入 [fetch-jsonp](https://github.com/camsong/fetch-jsonp)。
- 可选：开启 Babel 的 runtime 模式，即使用 async/await。

## 5. 缺点

Fetch API 是基于 Promise，由于 Promise 没有处理 timeout 的机制，所以无法通过原生方式处理请求超时后的中断，和读取进度的能力。但是相信未来为了支持流，Fetch API 最终将会提供可以中断执行读取资源的能力，并且提供可以读取进度的 API。

## 6. Refer Links

[传统 Ajax 已死，Fetch 永生](https://github.com/camsong/blog/issues/2)

[fetch API 简介](http://bubkoo.com/2015/05/08/introduction-to-fetch/)

[凹凸实验室：了解 Fetch API](https://aotu.io/notes/2017/04/10/fetch-API/index.html)

[MDN Fetch API](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API)