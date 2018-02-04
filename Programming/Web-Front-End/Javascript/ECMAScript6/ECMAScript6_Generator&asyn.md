- [ECMAScript6 异步编程](#ecmascript6-%E5%BC%82%E6%AD%A5%E7%BC%96%E7%A8%8B)
  - [Generator 函数](#generator-%E5%87%BD%E6%95%B0)
    - [概述](#%E6%A6%82%E8%BF%B0)
    - [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
    - [Generator.prototype.next()](#generatorprototypenext)
    - [Generator.prototype.throw()](#generatorprototypethrow)
    - [Generator.prototype.return()](#generatorprototypereturn)
    - [yield](#yield)
    - [yield*](#yield)
    - [for...of](#forof)
    - [自动执行器](#%E8%87%AA%E5%8A%A8%E6%89%A7%E8%A1%8C%E5%99%A8)
      - [Thunk 函数](#thunk-%E5%87%BD%E6%95%B0)
      - [co 函数库](#co-%E5%87%BD%E6%95%B0%E5%BA%93)
  - [asyn 函数](#asyn-%E5%87%BD%E6%95%B0)
    - [与 Generator 函数的对比](#%E4%B8%8E-generator-%E5%87%BD%E6%95%B0%E7%9A%84%E5%AF%B9%E6%AF%94)
    - [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
    - [NOTE](#note)
    - [实现原理](#%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
  - [Refer Links](#refer-links)

# ECMAScript6 异步编程

[传统 JavaScript 中异步编程](http://www.ruanyifeng.com/blog/2012/12/asynchronous%EF%BC%BFjavascript.html) 的方法，大概有下面四种：
- 回调函数
- 事件监听
- 发布 / 订阅
- Promise 对象
**异步编程的语法目标，就是怎样让它更像同步编程。**ECMAScript 6 作为下一代 JavaScript 语言，将 JavaScript 异步编程带入了一个全新的阶段。

## Generator 函数

### 概述

传统的编程语言，早有异步编程的解决方案（其实是多任务的解决方案）。其中有一种叫做"协程"（coroutine），意思是多个线程互相协作，完成异步任务。它的运行流程大致如下：

- 第一步，协程 A 开始执行。

- 第二步，协程 A 执行到一半，进入暂停，执行权转移到协程 B。

- 第三步，（一段时间后）协程 B 交还执行权。

- 第四步，协程 A 恢复执行。

它的最大优点，就是代码的写法非常像同步操作。

**Generator 函数就是协程在 ES6 的实现**，最大特点就是可以交出函数的执行权（即暂停执行）。

Generator 函数有多种理解角度：
- 语法上，Generator 函数是一个状态机，封装了多个内部状态。
- 执行 Generator 函数会返回一个遍历器对象，因此 Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。
- 形式上，Generator 函数是一个特殊的函数。特殊之处在于：
  - function 关键字与函数名之间有一个星号。
  - 函数体内部使用 yield 表达式，定义不同的内部状态。

### 基本使用

Generator 函数的调用方法与普通函数一样，也是在函数名后面加上一对圆括号。不同的是，调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象 (Iterator Object)。必须调用遍历器对象的 next 方法，使得指针移向下一个状态。也就是说，每次调用 next 方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个 yield 表达式。next 方法返回的对象的 value 属性就是当前 yield 表达式的值，done 属性表示是否到达最终状态。
```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
hw.next()
// { value: 'hello', done: false }
hw.next()
// { value: 'world', done: false }
hw.next()
// { value: 'ending', done: true }
hw.next()
// { value: undefined, done: true }
```

### Generator.prototype.next()

遍历器对象的 next 方法的运行逻辑如下：
- 遇到 yield 表达式，就暂停执行后面的操作，并将紧跟在 yield 后面的那个表达式的值，作为返回的对象的 value 属性值。

- 下一次调用 next 方法时，再继续往下执行，直到遇到下一个 yield 表达式。

- 如果没有再遇到新的 yield 表达式，就一直运行到函数结束，直到 return 语句为止，并将 return 语句后面的表达式的值，作为返回的对象的 value 属性值。

- 如果该函数没有 return 语句，则返回的对象的 value 属性值为 undefined。

yield 表达式本身没有返回值，或者说总是返回 undefined。但若 next 方法带一个参数，该参数就会被当作上一个 yield 表达式的返回值：
```javascript
function* f() {
  for(var i = 0; true; i++) {
    var reset = yield i;
    if(reset) { i = -1; }
  }
}

var g = f();

g.next() // { value: 0, done: false }
g.next() // { value: 1, done: false }
g.next(true) // { value: 0, done: false }
```

### Generator.prototype.throw()

Generator 函数返回的遍历器对象，都有一个 throw 方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。

```javascript
var g = function* () {
  try {
    yield;
  } catch (e) {
    console.log('内部捕获', e);
  }
};

var i = g();
i.next();

try {
  i.throw('a');
  i.throw('b');
} catch (e) {
  console.log('外部捕获', e);
}
// 内部捕获 a
// 外部捕获 b
```
上面代码中，遍历器对象 i 连续抛出两个错误。第一个错误被 Generator 函数体内的 catch 语句捕获。i 第二次抛出错误，由于 Generator 函数内部的 catch 语句已经执行过了，不会再捕捉到这个错误了，所以这个错误就被抛出了 Generator 函数体，被函数体外的 catch 语句捕获。

throw 方法可以接受一个参数，该参数会被 catch 语句接收，建议抛出 Error 对象的实例。
```javascript
var g = function* () {
  try {
    yield;
  } catch (e) {
    console.log(e);
  }
};

var i = g();
i.next();
i.throw(new Error('出错了！'));
// Error: 出错了！(…)
```

### Generator.prototype.return()

Generator 函数返回的遍历器对象，还有一个 return 方法，可以返回给定的值，并且终结遍历 Generator 函数。

```javascript
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

var g = gen();

g.next()        // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next()        // { value: undefined, done: true }
```
上面代码中，遍历器对象 g 调用 return 方法后，返回值的 value 属性就是 return 方法的参数 foo。并且，Generator 函数的遍历就终止了，返回值的 done 属性为 true，以后再调用 next 方法，done 属性总是返回 true。

### yield

yield 表达式只能用在 Generator 函数里面，用在其他地方都会报错。

yield 表达式如果用在另一个表达式之中，必须放在圆括号里面。
```javascript
function* demo() {
  console.log('Hello' + yield); // SyntaxError
  console.log('Hello' + yield 123); // SyntaxError

  console.log('Hello' + (yield)); // OK
  console.log('Hello' + (yield 123)); // OK
}
```
yield 表达式用作函数参数或放在赋值表达式的右边时，可以不加括号。
```javascript
function* demo() {
  foo(yield 'a', yield 'b'); // OK
  let input = yield; // OK
}
```

### yield*

如果在 Generator 函数内部，调用另一个 Generator 函数，默认情况下是没有效果的。这个时候就需要用到 yield*表达式，用来在一个 Generator 函数里面执行另一个 Generator 函数。

```javascript
function* bar() {
  yield 'x';
  yield* foo();
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  yield 'a';
  yield 'b';
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  for (let v of foo()) {
    yield v;
  }
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
// "x"
// "a"
// "b"
// "y"
```

### for...of

for...of 循环可以自动遍历 Generator 函数时生成的 Iterator 对象，且此时不再需要调用 next 方法。

```javascript
function* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for (let v of foo()) {
  console.log(v);
}
// 1 2 3 4 5
```
需要注意，一旦 next 方法的返回对象的 done 属性为 true，for...of 循环就会中止，且不包含该返回对象，所以上面代码的 return 语句返回的 6，不包括在 for...of 循环之中。

实例：实现斐波那契数列
```javascript
function* fibonacci() {
  let [prev, curr] = [0, 1];
  for (;;) {
    [prev, curr] = [curr, prev + curr];
    yield curr;
  }
}

for (let n of fibonacci()) {
  if (n > 1000) break;
  console.log(n);
}
```

### 自动执行器

Generator 函数实质上就是一个异步操作的容器。虽然 Generator 函数将异步操作表示得很简洁，但是流程管理却不方便（即何时执行第一阶段、何时执行第二阶段）。

因此，Generator 函数需要一种自动执行的机制，当异步操作有了结果，能够自动交回执行权。两种方法可以做到这一点：
- 回调函数。将异步操作包装成 Thunk 函数，在回调函数里面交回执行权。
- Promise 对象。将异步操作包装成 Promise 对象，用 then 方法交回执行权。

#### Thunk 函数

http://www.ruanyifeng.com/blog/2015/05/thunk.html

Thunk 函数是基于回调函数的 Generator 自动执行器
```javascript
// 基于 Thunk 函数的 Generator 执行器
function run(fn) {
  var gen = fn();

  function next(err, data) {
    var result = gen.next(data);
    if (result.done) return;
    result.value(next);
  }

  next();
}

// 生成器函数 gen 封装了 n 个异步的读取文件操作，只要执行 run 函数，这些操作就会自动完成。
var gen = function* (){
  var f1 = yield readFile('fileA');
  var f2 = yield readFile('fileB');
  // ...
  var fn = yield readFile('fileN');
};

run(gen);
```

#### co 函数库

http://www.ruanyifeng.com/blog/2015/05/co.html

co 函数库是对基于回调函数的 Generator 自动执行器和基于 promise 的 Generator 自动执行器的包装。
```javascript
// 有一个 Generator 函数，用于依次读取两个文件。
var gen = function* (){
  var f1 = yield readFile('/etc/fstab');
  var f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};

// 使用 co 函数库可以让你不用编写 Generator 函数的执行器。
// 只要将 Generator 函数传入 co 函数，就会自动执行。
var co = require('co');
// co 函数返回一个 Promise 对象，因此可以用 then 方法添加回调函数
co(gen).then(function (){
  console.log('Generator 函数执行完成');
})
```

## asyn 函数

> ES2017 标准引入了 async 函数，使得异步操作变得更加方便。async 函数是什么？一句话，async 函数就是 Generator 函数的语法糖。

### 与 Generator 函数的对比

async 函数与 Generator 函数的对比：
```javascript
var gen = function* (){
  var f1 = yield readFile('/etc/fstab');
  var f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};

// 相当于

var asyncReadFile = async function (){
  var f1 = await readFile('/etc/fstab');
  var f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```
async 函数就是将 Generator 函数的星号（*）替换成 async，将 yield 替换成 await。

async 函数对 Generator 函数的改进，体现在以下三点：

- 内置执行器。 Generator 函数的执行必须靠执行器，所以才有了 co 函数库，而 async 函数自带执行器。也就是说，async 函数的执行，与普通函数一模一样，只要一行。
  ```javscript
  var result = asyncReadFile();
  ```
- 更好的语义。 async 和 await，比起星号和 yield，语义更清楚了。async 表示函数里有异步操作，await 表示紧跟在后面的表达式需要等待结果。

- 更广的适用性。 co 函数库约定，yield 命令后面只能是 Thunk 函数或 Promise 对象，而 async 函数的 await 命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）。

### 基本使用

- async 函数返回一个 Promise 对象，可以使用 then 方法添加回调函数。**async 函数内部 return 语句返回的值，会成为 then 方法回调函数的参数。**

- 当 async 函数执行的时候，一旦遇到 await 就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。而 async 函数返回的 Promise 对象，必须等到内部所有 await 命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到 return 语句或者抛出错误。
  ```javascript
  // 获取股票报价
  async function getStockPriceByName(name) {
    const symbol = await getStockSymbol(name);
    const stockPrice = await getStockPrice(symbol);
    return stockPrice;
  }

  getStockPriceByName('goog').then(function (result) {
    console.log(result);
  });
  ```
  ```javascript
  async function getTitle(url) {
    let response = await fetch(url);
    let html = await response.text();
    return html.match(/<title>([\s\S]+)<\/title>/i)[1];
  }
  getTitle('https://tc39.github.io/ecma262/').then(console.log)
  // "ECMAScript 2017 Language Specification"
  // 上面代码中，函数 getTitle 内部有三个操作：抓取网页、取出文本、匹配页面标题。只有这三个操作全部完成，才会执行 then 方法里面的 console.log。
  ```

- async 函数有多种使用形式：
  ```javascript
  // 函数声明
  async function foo() {}

  // 函数表达式
  const foo = async function () {};

  // 对象的方法
  let obj = { async foo() {} };
  obj.foo().then(...)

  // Class 的方法
  class Storage {
    constructor() {
      this.cachePromise = caches.open('avatars');
    }

    async getAvatar(name) {
      const cache = await this.cachePromise;
      return cache.match(`/avatars/${name}.jpg`);
    }
  }

  const storage = new Storage();
  storage.getAvatar('jake').then(…);

  // 箭头函数
  const foo = async () => {};
  ```

- 正常情况下，await 命令后面是一个 Promise 对象。如果不是，会被转成一个立即 resolve 的 Promise 对象。
  ```javascript
  async function f() {
    return await 123;
  }

  f().then(v => console.log(v))
  // 123
  // 上面代码中，await 命令的参数是数值 123，它被转成 Promise 对象，并立即 resolve。
  ```

- await 命令后面的 Promise 对象如果变为 reject 状态，则 reject 的参数会被 catch 方法的回调函数接收到。
  ```javascript
  async function f() {
    await Promise.reject('出错了');
  }

  f()
  .then(v => console.log(v))
  .catch(e => console.log(e))
  // 出错了
  ```

### NOTE

- await 命令后面的 Promise 对象，运行结果可能是 rejected，所以最好把 await 命令放在 try...catch 代码块中。
  ```javascript
  async function myFunction() {
    try {
      await somethingThatReturnsAPromise();
    } catch (err) {
      console.log(err);
    }
  }

  // 另一种写法

  async function myFunction() {
    await somethingThatReturnsAPromise()
    .catch(function (err) {
      console.log(err);
    });
  }
  ```

- 多个 await 命令后面的异步操作，如果不存在继发关系，最好让它们同时触发。
  ```javascript
  let foo = await getFoo();
  let bar = await getBar();
  ```
  上面代码中，getFoo 和 getBar 是两个独立的异步操作（即互不依赖），被写成继发关系。这样比较耗时，因为只有 getFoo 完成以后，才会执行 getBar，完全可以让它们同时触发。
  ```javascript
  // 写法一
  let [foo, bar] = await Promise.all([getFoo(), getBar()]);

  // 写法二
  let fooPromise = getFoo();
  let barPromise = getBar();
  let foo = await fooPromise;
  let bar = await barPromise;
  ```

### 实现原理

async 函数的实现原理，就是将 Generator 函数和自动执行器，包装在一个函数里。

```javascript
async function fn(args) {
  // ...
}
```
等同于
```javascript
function spawn(genF) {
  return new Promise(function(resolve, reject) {
    const gen = genF();
    function step(nextF) {
      let next;
      try {
        next = nextF();
      } catch(e) {
        return reject(e);
      }
      if(next.done) {
        return resolve(next.value);
      }
      Promise.resolve(next.value).then(function(v) {
        step(function() { return gen.next(v); });
      }, function(e) {
        step(function() { return gen.throw(e); });
      });
    }
    step(function() { return gen.next(undefined); });
  });
}

function fn(args) {
  return spawn(function* () {
    // ...
  });
}
```

## Refer Links

[Javascript 异步编程的 4 种方法](http://www.ruanyifeng.com/blog/2012/12/asynchronous%EF%BC%BFjavascript.html)

[利用 Generator 解决异步回调原理](https://www.cnblogs.com/dojo-lzz/p/5496661.html)

[深入掌握 ECMAScript 6 异步编程：Generator 函数的含义与用法](http://www.ruanyifeng.com/blog/2015/04/generator.html)

[深入掌握 ECMAScript 6 异步编程：async 函数的含义和用法](http://www.ruanyifeng.com/blog/2015/05/async.html)

[ECMAScript 6 入门：Generator 函数的语法](http://es6.ruanyifeng.com/#docs/generator)

[ECMAScript 6 入门：Generator 函数的异步应用](http://es6.ruanyifeng.com/#docs/generator-async)

[ECMAScript 6 入门：async 函数](http://es6.ruanyifeng.com/#docs/async)