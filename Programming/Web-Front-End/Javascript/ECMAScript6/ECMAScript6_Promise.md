- [ECMAScript 6 Promise](#ecmascript-6-promise)
  - [概述](#%E6%A6%82%E8%BF%B0)
  - [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
  - [Promise.prototype.then()](#promiseprototypethen)
  - [Promise.prototype.catch()](#promiseprototypecatch)
  - [Promise.prototype.finally()](#promiseprototypefinally)
  - [Promise.all()](#promiseall)
  - [Promise.race()](#promiserace)
  - [Promise.resolve()](#promiseresolve)
  - [Promise.reject()](#promisereject)
  - [Others](#others)
    - [红绿灯问题](#%E7%BA%A2%E7%BB%BF%E7%81%AF%E9%97%AE%E9%A2%98)
  - [Refer Links](#refer-links)

# ECMAScript 6 Promise

## 概述

传统 JavaScript 中使用回掉函数实现异步编程，但当出现多个回调函数嵌套时，代码不是纵向发展，而是横向发展，很快就会乱成一团，无法管理 -- "callback hell"。

Promise 就是为了解决这个问题而提出的。它不是新的语法功能，而是一种新的写法，允许将回调函数的横向加载，改成纵向加载。

Promise 由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了 Promise 对象。

所谓 Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

Promise 的思想是，每一个异步任务立刻返回一个 Promise 对象，由于是立刻返回，所以可以采用同步操作的流程。这个 Promises 对象有一个 then 方法，允许指定回调函数，在异步任务完成后调用。

Promise 对象只有三种状态：
- 异步操作“未完成”（pending）
- 异步操作“已完成”（resolved，又称 fulfilled）
- 异步操作“失败”（rejected）

这三种的状态的变化途径只有两种：
- 异步操作从“未完成”到“已完成”
- 异步操作从“未完成”到“失败”。
这种变化只能发生一次，一旦当前状态变为“已完成”或“失败”，就意味着不会再有新的状态变化了。

有了 Promise 对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise 对象提供统一的接口，使得控制异步操作更加容易。

Promise 也有一些缺点：
- 代码冗余。不管什么操作，一眼看去都是一堆 then，原来的语义变得很不清楚。
- 无法取消 Promise，一旦新建它就会立即执行，无法中途取消。
- 如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。
- 当处于 pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

如果某些事件不断地反复发生，一般来说，使用 Stream 模式是比部署 Promise 更好的选择。

## 基本使用

Promise 构造函数接受一个函数作为参数，该函数的两个参数分别是 resolve 和 reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。
- resolve 函数的作用是，将 Promise 对象的状态从“未完成”变为“成功”（即从 Pending 变为 Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；
- reject 函数的作用是，将 Promise 对象的状态从“未完成”变为“失败”（即从 Pending 变为 Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
```javascript
var promise = new Promise((resolve, reject) => {
  // 当异步代码执行成功时，我们才会调用 resolve(...), 当异步代码失败时就会调用 reject(...)
  if (/* 异步操作成功 */){
    resolve('成功！');
  } else {
    reject('失败！');
  }
});

promise.then(value => {
  // 当 promise 状态为 resolve，触发该函数执行，传入参数的值是上面调用 resolve(...) 方法传入的值，即‘成功！’
}, value => {
  // 当 promise 状态为 reject，触发该函数执行，传入参数的值是上面调用 reject(...) 方法传入的值，即‘失败！’
});
```

例：使用 Promise 对象操作 ajax：
```javascript
function search(term) {
  var url = 'http://example.com/search?q=' + term;
  var xhr = new XMLHttpRequest();
  var result;

  var p = new Promise(function (resolve, reject) {
    xhr.open('GET', url, true);
    xhr.onload = function (e) {
      if (this.status === 200) {
        result = JSON.parse(this.responseText);
        resolve(result);
      }
    };
    xhr.onerror = function (e) {
      reject(e);
    };
    xhr.send();
  });

  return p;
}

search("Hello World").then(console.log, console.error);
```

比较下面两种写法，可以发现 reject 方法的作用，等同于抛出错误：
```javascript
// 写法一
const promise = new Promise(function(resolve, reject) {
  try {
    throw new Error('test');
  } catch(e) {
    reject(e);
  }
});
promise.catch(function(error) {
  console.log(error);
});

// 写法二
const promise = new Promise(function(resolve, reject) {
  reject(new Error('test'));
});
promise.catch(function(error) {
  console.log(error);
});
```

## Promise.prototype.then()

Promise 实例生成以后，**可以用 then 方法分别指定 Resolved 状态和 Reject 状态的回调函数，then 方法的第一个参数是 resolved 状态的回调函数，第二个参数（可选）是 rejected 状态的回调函数**。

then 方法返回的是一个新的 Promise 实例，因此，可以采用链式写法，第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数：
```javascript
promise
  .then(step1)
  .then(step2)
  .then(step3)
  .then(
    console.log,
    console.error
  );
```

采用链式的 then，可以指定一组按照次序调用的回调函数。这时，前一个回调函数，有可能返回的还是一个 Promise 对象（即有异步操作），这时后一个回调函数，就会等待该 Promise 对象的状态发生变化，才会被调用：
```javascript
getJSON("/post/1.json").then(
  post => getJSON(post.commentURL)
).then(
  comments => console.log("resolved: ", comments),
  err => console.log("rejected: ", err)
);
```
上面代码中，第一个 then 方法指定的回调函数，返回的是另一个 Promise 对象。这时，第二个 then 方法指定的回调函数，就会等待这个新的 Promise 对象状态发生变化。如果变为 resolved，就调用 funcA，如果状态变为 rejected，就调用 funcB。

## Promise.prototype.catch()

Promise.prototype.catch 方法是。then(null, rejection) 的别名，用于指定发生错误时的回调函数。

```javascript
getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
```

## Promise.prototype.finally()

finally 方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。

不管 promise 最后的状态，在执行完 then 或 catch 指定的回调函数以后，都会执行 finally 方法指定的回调函数。
```javascript
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```

## Promise.all()

Promise.all 方法接受一个数组作为参数，用于将多个 Promise 实例，包装成一个新的 Promise 实例。

```javascript
const p = Promise.all([p1, p2, p3]);
```
p 的状态由 p1、p2、p3 决定，分成两种情况：
- 只有 p1、p2、p3 的状态都变成 fulfilled，p 的状态才会变成 fulfilled，此时 p1、p2、p3 的返回值组成一个数组，传递给 p 的回调函数。
- 只要 p1、p2、p3 之中有一个被 rejected，p 的状态就变成 rejected，此时第一个被 reject 的实例的返回值，会传递给 p 的回调函数。

E.G.
```javascript
const databasePromise = connectDatabase();

const booksPromise = databasePromise
  .then(findAllBooks);

const userPromise = databasePromise
  .then(getCurrentUser);

Promise.all([
  booksPromise,
  userPromise
])
.then(([books, user]) => pickTopRecommentations(books, user));
```

## Promise.race()

Promise.race 方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。
```javascript
const p = Promise.race([p1, p2, p3]);
```
只要 p1、p2、p3 之中有一个实例率先改变状态，p 的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给 p 的回调函数。

E.G. 如果 5 秒之内 fetch 方法无法返回结果，变量 p 的状态就会变为 rejected，从而触发 catch 方法指定的回调函数。
```javascript
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
]);
p.then(response => console.log(response));
p.catch(error => console.log(error));
```

## Promise.resolve() 

Promise.resolve() 用于将现有对象转为 Promise 对象。
```javascript
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

E.G.
```javascript
const jsPromise = Promise.resolve($.ajax('/whatever.json'));
```

## Promise.reject() 

Promise.reject(reason) 方法也会返回一个新的 Promise 实例，该实例的状态为 rejected。

```javascript
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)
});
// 出错了
```

## Others

### 红绿灯问题

题目：
```javascript
function red(){
    console.log('red');
}
function green(){
    console.log('green');
}
function yellow(){
    console.log('yellow');
}
```
红灯三秒亮一次，绿灯一秒亮一次，黄灯 2 秒亮一次；如何让三个灯不断交替重复亮灯？（用 Promse 实现）

考察：
- promise 应用

  如果把问题简化一下，如果只需要一个周期，那么利用 Promise 应该这样写：
  ```javascript
  var d = new Promise(function(resolve, reject){resolve();});
  var step = function(def) {
      def.then(function(){
          return tic(3000, red);
      }).then(function(){
          return tic(2000, green);
      }).then(function(){
          return tic(1000, yellow);
      });
  }
  ```

- setTimeout 相关的异步队列会挂起直到主进程空闲

  一个周期已经有了，剩下的问题是如何让他无限循环。如果使用 while 无限循环，主进程永远不会空闲，setTimeout 的函数永远不会执行！因此，正确的解决方法应采用递归实现：
  ```javascript
  var d = new Promise(function(resolve, reject){resolve();});
  var step = function(def) {
      def.then(function(){
          return tic(3000, red);
      }).then(function(){
          return tic(2000, green);
      }).then(function(){
          return tic(1000, yellow);
      }).then(function(){
          step(def);
      });
  }
  ```

答案：
```javascript
function red(){
    console.log('red');
}
function green(){
    console.log('green');
}
function yellow(){
    console.log('yellow');
}

var tic = function(timmer, cb){
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            cb();
            resolve();
        }, timmer);
    });
};

var d = new Promise(function(resolve, reject){resolve();});
var step = function(def) {
    def.then(function(){
        return tic(3000, red);
    }).then(function(){
        return tic(2000, green);
    }).then(function(){
        return tic(1000, yellow);
    }).then(function(){
        step(def);
    });
}

step(d);
```
使用 Generator 减少回调，优化代码：
```javascript
var tic = function(timmer, str){
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            console.log(str);
            resolve(1);
        }, timmer);
    });
};

function *gen(){
    yield tic(3000, 'red');
    yield tic(1000, 'green');
    yield tic(2000, 'yellow');
}

var iterator = gen();
var step = function(gen, iterator){
    var s = iterator.next();
    if (s.done) {
        step(gen, gen());
    } else {
        s.value.then(function() {
            step(gen, iterator);
        });
    }
}

step(gen, iterator);
```

## Refer Links

http://wiki.jikexueyuan.com/project/es6/promise.html

http://javascript.ruanyifeng.com/advanced/promise.html

http://es6.ruanyifeng.com/#docs/promise

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise

http://caibaojian.com/toutiao/7312

https://www.cnblogs.com/dojo-lzz/p/5495671.html
