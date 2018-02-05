- [ECMAScript 6 NOTE](#ecmascript-6-note)
  - [1. 变量扩展语法](#1-%E5%8F%98%E9%87%8F%E6%89%A9%E5%B1%95%E8%AF%AD%E6%B3%95)
    - [1.1. let 和 const](#11-let-%E5%92%8C-const)
    - [1.2. 解构赋值](#12-%E8%A7%A3%E6%9E%84%E8%B5%8B%E5%80%BC)
      - [1.2.1. 数组的解构赋值](#121-%E6%95%B0%E7%BB%84%E7%9A%84%E8%A7%A3%E6%9E%84%E8%B5%8B%E5%80%BC)
      - [1.2.2. 对象的解构赋值](#122-%E5%AF%B9%E8%B1%A1%E7%9A%84%E8%A7%A3%E6%9E%84%E8%B5%8B%E5%80%BC)
      - [1.2.3. 其它数据类型的解构赋值](#123-%E5%85%B6%E5%AE%83%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E7%9A%84%E8%A7%A3%E6%9E%84%E8%B5%8B%E5%80%BC)
      - [1.2.4. 圆括号问题](#124-%E5%9C%86%E6%8B%AC%E5%8F%B7%E9%97%AE%E9%A2%98)
      - [1.2.5. 应用](#125-%E5%BA%94%E7%94%A8)
    - [1.3. 模板字符串](#13-%E6%A8%A1%E6%9D%BF%E5%AD%97%E7%AC%A6%E4%B8%B2)
  - [2. 函数扩展语法](#2-%E5%87%BD%E6%95%B0%E6%89%A9%E5%B1%95%E8%AF%AD%E6%B3%95)
    - [2.1. name 属性](#21-name-%E5%B1%9E%E6%80%A7)
    - [2.2. 默认参数](#22-%E9%BB%98%E8%AE%A4%E5%8F%82%E6%95%B0)
    - [2.3. 变长参数 （rest） 和扩展运算符](#23-%E5%8F%98%E9%95%BF%E5%8F%82%E6%95%B0-%EF%BC%88rest%EF%BC%89-%E5%92%8C%E6%89%A9%E5%B1%95%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [2.4. 箭头函数](#24-%E7%AE%AD%E5%A4%B4%E5%87%BD%E6%95%B0)
      - [2.4.1. 基本使用](#241-%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
      - [2.4.2. 没有 this、arguments、caller](#242-%E6%B2%A1%E6%9C%89-this%E3%80%81arguments%E3%80%81caller)
      - [2.4.3. 没有 prototype 属性](#243-%E6%B2%A1%E6%9C%89-prototype-%E5%B1%9E%E6%80%A7)
      - [2.4.4. 不可被 new](#244-%E4%B8%8D%E5%8F%AF%E8%A2%AB-new)
      - [2.4.5. 不适用 yield](#245-%E4%B8%8D%E9%80%82%E7%94%A8-yield)
      - [2.4.6. 可定义为递归函数](#246-%E5%8F%AF%E5%AE%9A%E4%B9%89%E4%B8%BA%E9%80%92%E5%BD%92%E5%87%BD%E6%95%B0)
      - [2.4.7. 应用场景](#247-%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF)
    - [2.5. 装饰器函数（Decorator）](#25-%E8%A3%85%E9%A5%B0%E5%99%A8%E5%87%BD%E6%95%B0%EF%BC%88decorator%EF%BC%89)
  - [3. 数组扩展语法](#3-%E6%95%B0%E7%BB%84%E6%89%A9%E5%B1%95%E8%AF%AD%E6%B3%95)
  - [4. 类的扩展语法](#4-%E7%B1%BB%E7%9A%84%E6%89%A9%E5%B1%95%E8%AF%AD%E6%B3%95)
    - [4.1. 类的定义](#41-%E7%B1%BB%E7%9A%84%E5%AE%9A%E4%B9%89)
    - [4.2. 构造函数](#42-%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)
    - [4.3. 原型方法](#43-%E5%8E%9F%E5%9E%8B%E6%96%B9%E6%B3%95)
    - [4.4. static 方法](#44-static-%E6%96%B9%E6%B3%95)
    - [4.5. 不会自动装箱](#45-%E4%B8%8D%E4%BC%9A%E8%87%AA%E5%8A%A8%E8%A3%85%E7%AE%B1)
    - [4.6. 类的继承](#46-%E7%B1%BB%E7%9A%84%E7%BB%A7%E6%89%BF)
    - [4.7. Symbol.species](#47-symbolspecies)
    - [4.8. Mix-ins 混合](#48-mix-ins-%E6%B7%B7%E5%90%88)
  - [5. 对象扩展语法](#5-%E5%AF%B9%E8%B1%A1%E6%89%A9%E5%B1%95%E8%AF%AD%E6%B3%95)
    - [5.1. 属性简写 & 方法简写](#51-%E5%B1%9E%E6%80%A7%E7%AE%80%E5%86%99-%E6%96%B9%E6%B3%95%E7%AE%80%E5%86%99)
    - [5.2. getter](#52-getter)
    - [5.3. setter](#53-setter)
    - [5.4. Object.is()](#54-objectis)
    - [5.5. Object.assign()](#55-objectassign)
    - [5.6. Object.keys() & Object.values() & Object.entries()](#56-objectkeys-objectvalues-objectentries)
    - [5.7. Generator 方法](#57-generator-%E6%96%B9%E6%B3%95)
    - [5.8. Async 方法](#58-async-%E6%96%B9%E6%B3%95)
    - [5.9. Async 生成器方法](#59-async-%E7%94%9F%E6%88%90%E5%99%A8%E6%96%B9%E6%B3%95)
  - [6. JavaScript 的第七种数据类型：Symbols](#6-javascript-%E7%9A%84%E7%AC%AC%E4%B8%83%E7%A7%8D%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%EF%BC%9Asymbols)
  - [7. 数据结构扩展](#7-%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E6%89%A9%E5%B1%95)
    - [7.1. Set](#71-set)
    - [7.2. Map](#72-map)
  - [8. 遍历器 (Iterator) 与 for-of 循环](#8-%E9%81%8D%E5%8E%86%E5%99%A8-iterator-%E4%B8%8E-for-of-%E5%BE%AA%E7%8E%AF)
    - [8.1. 遍历方法的发展](#81-%E9%81%8D%E5%8E%86%E6%96%B9%E6%B3%95%E7%9A%84%E5%8F%91%E5%B1%95)
    - [8.2. for-of 循环](#82-for-of-%E5%BE%AA%E7%8E%AF)
    - [8.3. 遍历器](#83-%E9%81%8D%E5%8E%86%E5%99%A8)
  - [9. 模块（Module）](#9-%E6%A8%A1%E5%9D%97%EF%BC%88module%EF%BC%89)
    - [9.1. export](#91-export)
    - [9.2. import](#92-import)
    - [9.3. 跨模块常量](#93-%E8%B7%A8%E6%A8%A1%E5%9D%97%E5%B8%B8%E9%87%8F)
  - [10. Refer Links](#10-refer-links)

# ECMAScript 6 NOTE

ES6 既是一个历史名词，也是一个泛指，含义是 5.1 版以后的 JavaScript 的下一代标准，涵盖了 ES2015、ES2016、ES2017 等，而 ES2015 则是正式名称，特指该年发布的正式版本的语言标准。

ECMA 的第 39 号技术专家委员会（Technical Committee 39，简称 [TC39](https://github.com/tc39/ecma262)）负责制订 ECMAScript 标准，成员包括 Microsoft、Mozilla、Google 等大公司。

各大浏览器的最新版本，对 ES6 的支持可以在[这里](kangax.github.io/es5-compat-table/es6/) 查看。

## 1. 变量扩展语法

### 1.1. let 和 const

ES5 只有全局作用域和函数作用域，没有块级作用域。一不小心变量就会被覆盖。
在 ES6 中，let 和 const 提供了块级作用域，

- let 和 const 用途与 var 类似，区别在于 var 是全局生效的，let 和 const 只在当前代码块有效。

- 不存在变量提升，变量要在声明后才能够使用。

- 不允许在相同作用域重复命名同一变量。

- let 用于定义变量，而 const 用于定义常量，即定义后不能再改变，不能赋值，同样不能变量提升，不可重复声明。

### 1.2. 解构赋值

ES6 允许按照一定的模式匹配，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

#### 1.2.1. 数组的解构赋值

数组的元素是按次序排列的，因此对于数组的解构赋值，变量的取值由它的位置决定。

```javascript
let [a, b, c] = [1, 2, 3];
a //1
b //2
c //3

let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined, 如果解构不成功，变量的值就等于 undefined
z // []
```

- 如果解构不成功，变量的值就等于 undefined。
  ```javascript
  let [foo] = [];
  let [bar, foo] = [1];
  ```
- 不完全解构：
  ```javascript
  // 这种情况下，解构依然可以成功
  let [x, y] = [1, 2, 3];
  x // 1
  y // 2

  let [a, [b], d] = [1, [2, 3], 4];
  a // 1
  b // 2
  d // 4
  ```
- 若解构赋值时等号右边不是可遍历的结构（即不具有 Iterator 接口的数据结构)，将会报错：
  ```javascript
  // 报错
  let [foo] = 1;
  let [foo] = false;
  let [foo] = NaN;
  let [foo] = undefined;
  let [foo] = null;
  let [foo] = {};
  // 正确赋值
  let [x, y, z] = new Set(['a', 'b', 'c']);
  x // a
  ```

- 解构赋值允许指定默认值，当赋值元素的值**严格等于 undefined**则默认值生效：
  ```javascript
  // 默认值生效
  let [foo = true] = []; // foo=true

  let [x, y = 'b'] = ['a']; // x='a', y='b'
  let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'

  // 默认值不生效
  let [x = 1] = [null]; // x=null，因为 null 不严格等于 undefined
  ```
  如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在默认值生效的时候，才会求值：
  ```javascript
  function f() {
    console.log('aaa');
  }

  let [x = f()] = [1];// 因为 x 能取到值，所以函数 f 根本不会执行
  ```
  默认值可以引用解构赋值的其他变量，但该变量必须已经声明：
  ```javascript
  let [x = 1, y = x] = [];     // x=1; y=1
  let [x = 1, y = x] = [2];    // x=2; y=2
  let [x = 1, y = x] = [1, 2]; // x=1; y=2
  let [x = y, y = 1] = [];     // ReferenceError
  ```

#### 1.2.2. 对象的解构赋值

对象的属性没有次序，因此对于对象的解构赋值，变量必须与属性同名，才能取到正确的值，否则为 undefined:
```javascript
let { foo, bar } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
```

- 实际上，对象的解构赋值是下面形式的简写：
  ```javascript
  let { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };
  ```
  也就是说，对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者：
  ```javascript
  let { foo: baz } = { foo: "aaa", bar: "bbb" };
  baz // "aaa"
  foo // error: foo is not defined
  // foo 是匹配的模式，baz 才是变量。真正被赋值的是变量 baz，而不是模式 foo。
  ```

- 对象的解构也可以指定默认值，当对象的属性值严格等于 undefined 时，默认值生效：
  ```javascript
  var {x = 3} = {};
  x // 3

  var {x, y = 5} = {x: 1};
  x // 1
  y // 5

  var {x: y = 3} = {};
  y // 3

  var {x: y = 3} = {x: 5};
  y // 5
  ```

- 如果解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错。
  ```javascript
  // 报错
  let {foo: {bar}} = {baz: 'baz'};
  ```

- 如果要将一个已经声明的变量用于解构赋值，必须非常小心：
  ```javascript
  // 错误的写法
  let x;
  {x} = {x: 1};
  // SyntaxError: syntax error
  ```
  JavaScript 引擎会将{x}理解成一个代码块，从而发生语法错误。只有不将大括号写在行首，避免 JavaScript 将其解释为代码块，才能解决这个问题：
  ```javascript
  // 正确的写法
  let x;
  ({x} = {x: 1});
  ```

- 解构赋值允许等号左边的模式之中，不放置任何变量名。因此，可以写出非常古怪的赋值表达式。
  ```javascript
  ({} = [true, false]);
  ({} = 'abc');
  ({} = []);
  ```
  上面的表达式虽然毫无意义，但是语法是合法的，可以执行。

- 由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。
  ```javascript
  let arr = [1, 2, 3];
  let {0 : first, [arr.length - 1] : last} = arr;
  first // 1
  last // 3
  ```

#### 1.2.3. 其它数据类型的解构赋值

解构赋值时，只要等号右边的值不是对象或数组，就先将其转为对象。

- 字符串
  
  字符串也可以解构赋值。此时字符串被转换成了一个类似数组的对象：
  ```javascript
  const [a, b, c, d, e] = 'hello';
  a // "h"
  b // "e"
  c // "l"
  d // "l"
  e // "o"
  ```
  类似数组的对象都有一个 length 属性，因此还可以对这个属性解构赋值：
  ```javascript
  let {length : len} = 'hello';
  len // 5
  ```
- 数值和布尔值

  解构赋值时，如果等号右边是数值和布尔值，则会先转为对象：
  ```javascript
  let {toString: s} = 123;
  s === Number.prototype.toString // true

  let {toString: s} = true;
  s === Boolean.prototype.toString // true
  ```

- undefined 和 null

  根据解构赋值的规则，由于 undefined 和 null 无法转为对象，所以对它们进行解构赋值，都会报错。

#### 1.2.4. 圆括号问题

ES6 的规则是，只要有可能导致解构的歧义，就不得使用圆括号。但是，这条规则实际上不那么容易辨别，处理起来相当麻烦。因此，建议只要有可能，就不要在模式中放置圆括号。

可以使用圆括号的情况只有一种：赋值语句的非模式部分，可以使用圆括号：
```javascript
[(b)] = [3]; // 正确
({ p: (d) } = {}); // 正确
[(parseInt.prop)] = [3]; // 正确
```

#### 1.2.5. 应用

- 使用解构赋值避免为属性创建冗余的临时变量或对象：
  ```javascript
  // bad
  function getFullName(user) {
    const firstName = user.firstName;
    const lastName = user.lastName;
    return `${firstName} ${lastName}`;
  }
  // good
  function getFullName(obj) {
    const { firstName, lastName } = obj;
    return `${firstName} ${lastName}`;
  }
  // best
  function getFullName({ firstName, lastName }) {
    return `${firstName} ${lastName}`;
  }
  ```

- 交换变量的值
  ```javascript
  let x = 1;
  let y = 2;

  [x, y] = [y, x];
  ```

- 用于函数参数的定义
  - 解构赋值可以方便地将一组参数与变量名对应起来
    ```javascript
    // 参数是一组有次序的值
    function f([x, y, z]) { ... }
    f([1, 2, 3]);

    // 参数是一组无次序的值
    function f({x, y, z}) { ... }
    f({z: 3, y: 2, x: 1});
    ```
  - 指定函数参数的默认值，就避免了在函数体内部再写`var foo = config.foo || 'default foo';`这样的语句
    ```javascript
    jQuery.ajax = function (url, {
      async = true,
      beforeSend = function () {},
      cache = true,
      complete = function () {},
      crossDomain = false,
      global = true,
      // ... more config
    }) {
      // ... do stuff
    };
    ```

- 遍历 Map 结构
  ```javascript
  const map = new Map();
  map.set('first', 'hello');
  map.set('second', 'world');

  for (let [key, value] of map) {
    console.log(key + " is " + value);
  }
  // first is hello
  // second is world

  // 只获取键名
  for (let [key] of map) {
    // ...
  }

  // 只获取键值
  for (let [,value] of map) {
    // ...
  }
  ```

- 输入模块的指定方法
  ```javascript
  const { SourceMapConsumer, SourceNode } = require("source-map");
  ```

### 1.3. 模板字符串

```javascript
let a=1,b=2;
docuemnt.getElementById('test').appendChild(`<b>${a}</b><b>${b}</b>`);// 使用反引号``创建一个模板字符串，在模板字符串中使用 ${x} 引用并输出变量值
```

## 2. 函数扩展语法

### 2.1. name 属性

函数的 name 属性，返回函数名。

对象方法也是函数，因此也有 name 属性：
```javascript
const person = {
  sayName() {
    console.log('hello!');
  },
};

person.sayName.name   // "sayName"
```

### 2.2. 默认参数

```javascript
function log(x,y='小明'){
    console.log(x,y);
}
log('hello');    //hello 小明
log('hello','xiaoming');    //hello xiaoming
log('nihao','');    //nihao
```

### 2.3. 变长参数 （rest） 和扩展运算符

```javascript
let fun = function(a,...other){  // 定义函数使用 rest 参数 获取其余的参数
    for(let v of other){
        console.log(v);
    }
    console.log(a);
}
fun(1,2,3,4,5,6);// 直接调用函数

let arr = [2,3,4,5,6];
fun(1,...arr);  // 调用参数时使用扩展运算符，给函数传参
```

```javascript
// 以下三种写法效果相同
Math.max(null,[14,3,77]);

Math.max(...[14,3,7]);

Math.max(14,3,7)
```

### 2.4. 箭头函数

[MDN Arrow Function](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

#### 2.4.1. 基本使用

```javascript
//es5
var sum = function(num1, num2){
    return num1+num2;
}

//es6
var sum = (num1, num2) =>  num1 + num2;
```

- 当函数只有一个参数时，可省略括号：
  ```javascript
  //es5
  var f = function(v){
      return v;
  }

  //es6  两种写法都可以
  var f = (v) => v;
  var f = v => v;
  ```
  尽管语法支持在单参数情况下可省略括号，但一般推荐总是用括号包裹参数，因为省略括号降低了程序的可读性。

- 当函数不包含参数时，不可省略括号：
  ```javascript
  //es5
  var f = function(){
      return 1;
  }

  //es6
  var f = () => 1
  ```

- 代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用 return 语句返回。

- 若直接返回一个对象，必须在对象外面加上括号。
  ```javascript
  let fun = (a, b) => {
    let c = a+b;
    return({
      a:a,
      b:b,
      c:c
    }) 
  };
  console.log(fun(1,2));
  ```

- 箭头函数在参数和箭头之间不能换行。
  ```javascript
  var func = ()
            => 1; 
  // SyntaxError: expected expression, got '=>'
  ```

- 虽然箭头函数中的箭头不是运算符，但箭头函数具有与常规函数不同的特殊运算符优先级解析规则。

  ```javascript
  let callback;

  callback = callback || function() {}; // ok

  callback = callback || () => {};      
  // SyntaxError: invalid arrow-function arguments

  callback = callback || (() => {});    // ok
  ```

#### 2.4.2. 没有 this、arguments、caller

箭头函数的设计来源可以追溯到 lambda 演算与图灵机的思想：数学家们喜欢用纯函数式编程语言，纯函数的特点是没有副作用，给予特定的输入，总是产生确定的输出，甚至有些情况下通过输出能够反推输入。要实现纯函数，必须使函数的执行过程不依赖于任何外部状态，整个函数就像一个数学公式，给定一套输入参数，就有一个固定的输出值，不随外部环境而变化。

因此，箭头函数要实现类似纯函数的效果，必须剔除外部状态。**当你定义一个箭头函数，在普通函数里常见的 this、arguments、caller 是统统没有的。**

在箭头函数中使用的 this、arguments、caller，其实都是父级作用域中的 this、arguments、caller，箭头函数引用了父级的变量，构成了一个闭包：

例 1：
```javascript
function foo() {
  this.a = 1
  let b = () => console.log(this.a)
  b()
}
foo()  // 1
```
```javascript
function foo() {
  return () => console.log(arguments[0])
}
foo(1, 2)(3, 4)  // 1
```

例 2：
```javascript
var obj = {
  i: 10,
  b: () => console.log(this.i, this),
  c: function() {
    console.log( this.i, this)
  }
}
obj.b(); 
// undefined
obj.c(); 
// 10, Object {...}
```

例 3：
```javascript
var obj = {
  a: 10
};

Object.defineProperty(obj, "b", {
  get: () => {
    console.log(this.a, typeof this.a, this);
    return this.a+10; 
   // 代表全局对象 'Window', 因此 'this.a' 返回 'undefined'
  }
});
```

#### 2.4.3. 没有 prototype 属性

```javascript
var Foo = () => {};
console.log(Foo.prototype); // undefined
```

#### 2.4.4. 不可被 new

箭头函数不能用作构造器，和 new 一起用会抛出错误。

```javascript
var Foo = () => {};
var foo = new Foo(); // TypeError: Foo is not a constructor
```

#### 2.4.5. 不适用 yield

yield 关键字通常不能在箭头函数中使用（除非是嵌套在允许使用的函数内）。因此，箭头函数不能用作生成器。

#### 2.4.6. 可定义为递归函数

```javascript
var fact = (x) => ( x==0 ?  1 : x*fact(x-1) );
fact(5);       // 120
```

#### 2.4.7. 应用场景

- 箭头函数适合于无复杂逻辑或者无副作用的纯函数场景下，例如用在 map、reduce、filter 的回调函数定义中；

  ```javascript
  var materials = [
    'Hydrogen',
    'Helium',
    'Lithium',
    'Beryllium'
  ];

  materials.map(function(material) { 
    return material.length; 
  }); // [8, 6, 7, 9]

  materials.map((material) => {
    return material.length;
  }); // [8, 6, 7, 9]

  materials.map(material => material.length); // [8, 6, 7, 9]
  ```

- 不要在最外层定义箭头函数，因为在函数内部操作 this 会很容易污染全局作用域。最起码在箭头函数外部包一层普通函数，将 this 控制在可见的范围内；

- 箭头函数最吸引人的地方是简洁。在有多层函数嵌套的情况下，箭头函数的简洁性并没有很大的提升，反而影响了函数的作用范围的识别度，这种情况不建议使用箭头函数。

### 2.5. 装饰器函数（Decorator）

TODO:

## 3. 数组扩展语法

使用 `...` 复制数组：
```javascript
// bad
const len = items.length;
const itemsCopy = [];
let i;
for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}
// good
const itemsCopy = [...items];
```

## 4. 类的扩展语法

ES6 中引入了 class 关键字，支持使用 class 直接定义一个类，但实质上这只是 JavaScript 现有的基于原型的继承的语法糖。 类语法不是向 JavaScript 引入一个新的面向对象的继承模型，但提供了一个更简单和更清晰的语法来创建对象并处理继承。

### 4.1. 类的定义

推荐总是使用 class 关键字，避免直接修改 prototype：
```javascript
//es5
var Animal=function(name){
    this.name=name;
}
animal.prototype={
    speak:function(){
        console.log("I am"+this.name);
    }
}
var animal = new Animal("cat");
animal.speak();  //I am cat

//es6
class Animal(){
    constructor(name){
        this.name=name;
    }
    speak(){
        console.log("I am"+this.name);
    }
}
const animal=new Animal("cat");
animal.speak();    //I am cat
```

NOTE:
- 函数声明和类声明之间的一个重要区别是函数声明会声明提升，类声明不会。你首先需要声明你的类，然后访问它，否则像下面的代码会抛出一个 ReferenceError：
  ```javascript
  let p = new Rectangle(); 
  // ReferenceError

  class Rectangle {}
  ```

- 类的声明支持匿名：
  ```javascript
  /* 匿名类 */ 
  let Rectangle = class {
    constructor(height, width) {
      this.height = height;
      this.width = width;
    }
  };

  /* 命名的类 */ 
  let Rectangle = class Rectangle {
    constructor(height, width) {
      this.height = height;
      this.width = width;
    }
  };
  ```

### 4.2. 构造函数

构造函数方法是一个特殊的方法，其用于创建和初始化使用一个类创建的一个对象。一个类只能拥有一个名为 “constructor”的特殊方法。如果类包含多个构造函数的方法，则将抛出一个 SyntaxError 。

一个构造函数可以使用 super 关键字来调用一个父类的构造函数。

### 4.3. 原型方法

```javascript
class Rectangle {
    // constructor
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }
    // Getter
    get area() {
        return this.calcArea()
    }
    // Method
    calcArea() {
        return this.height * this.width;
    }
}
const square = new Rectangle(10, 10);

console.log(square.area);
// 100
```

一般推荐在方法中返回 this 以方便**链式调用**：
```javascript
// bad
Jedi.prototype.jump = function() {
  this.jumping = true;
  return true;
};
Jedi.prototype.setHeight = function(height) {
  this.height = height;
};
const luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined
// good
class Jedi {
  jump() {
    this.jumping = true;
    return this;
  }
  setHeight(height) {
    this.height = height;
    return this;
  }
}
const luke = new Jedi();
luke.jump()
  .setHeight(20);
```

### 4.4. static 方法

static 关键字用来定义一个类的一个静态方法。

调用静态方法不需要实例化该类，但不能通过一个类实例调用静态方法。

静态方法通常用于为一个应用程序创建工具函数。

```javascript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    static distance(a, b) {
        const dx = a.x - b.x;
        const dy = a.y - b.y;

        return Math.hypot(dx, dy);
    }
}

const p1 = new Point(5, 5);
const p2 = new Point(10, 10);

console.log(Point.distance(p1, p2));
```

### 4.5. 不会自动装箱

当一个对象调用静态或原型方法时，如果该对象没有“this”值（或“this”作为布尔，字符串，数字，未定义或 null) ，那么“this”值在被调用的函数内部将为 undefined。不会发生自动装箱。

因此如果我们没有指定 this 的值，this 值将为 undefined。

```javascript
class Animal { 
  speak() {
    return this;
  }
  static eat() {
    return this;
  }
}

let obj = new Animal();
obj.speak(); // Animal {}
let speak = obj.speak;
speak(); // undefined

Animal.eat() // class Animal
let eat = Animal.eat;
eat(); // undefined
```

### 4.6. 类的继承

- ES6 中提供了 extends 关键字，在类声明或类表达式中直接继承另一个类。

  ```javascript
  class Animal { 
    constructor(name) {
      this.name = name;
    }
    
    speak() {
      console.log(this.name + ' makes a noise.');
    }
  }

  class Dog extends Animal {
    speak() {
      console.log(this.name + ' barks.');
    }
  }

  var d = new Dog('Mitzie');
  // 'Mitzie barks.'
  d.speak();
  ```

- 在子类中可使用关键字`super`获取超类，但要在子类调用 this 之前：
  
  ```javascript
  class Animal { 
    constructor(name) {
      this.name = name;
    }
    
    speak() {
      console.log(this.name + ' makes a noise.');
    }
  }

  class Dog extends Animal {
    constructor(name, weight) {
      super(name);
      this.weight = weight;
    }

    speak() {
      super.speak();
      console.log(this.weight + ' weight.');
    }
  }

  var d = new Dog('Mitzie', '20kg');
  d.speak();//Mitzie makes a noise. + 20kg weight. 
  ```

### 4.7. Symbol.species

你可能希望在派生数组类 MyArray 中返回 Array 对象。这种类 / 种类模式允许你覆盖默认的构造函数。

例如，当使用像 map() 返回默认构造函数的方法时，您希望这些方法返回一个父 Array 对象，而不是 MyArray 对象。Symbol.species 符号可以让你这样做：
```javascript
class MyArray extends Array {
  // Overwrite species to the parent Array constructor
  static get [Symbol.species]() { return Array; }
}
var a = new MyArray(1,2,3);
var mapped = a.map(x => x * x);

console.log(mapped instanceof MyArray); 
// false
console.log(mapped instanceof Array);   
// true
```

### 4.8. Mix-ins 混合

抽象子类或者 mix-ins 是类的模板。 一个 ECMAScript 类只能有一个单超类，所以想要从工具类来多重继承的行为是不可能的。子类继承的只能是父类提供的功能性。因此，例如，从工具类的多重继承是不可能的。该功能必须由超类提供。

一个以超类作为输入的函数和一个继承该超类的子类作为输出可以用于在 ECMAScript 中实现混合：
```javascript
var calculatorMixin = Base => class extends Base {
  calc() { }
};
var randomizerMixin = Base => class extends Base {
  randomize() { }
};

// 使用 mix-ins 的类可以像下面这样写：
class Foo { }
class Bar extends calculatorMixin(randomizerMixin(Foo)) { }
```

## 5. 对象扩展语法

### 5.1. 属性简写 & 方法简写

从 ECMAScript 2015 开始，在对象初始器中引入了一种更简短定义方法的语法，这是一种把方法名直接赋给函数的简写方式。

Syntax：
```javascript
var obj = {
  property( parameters… ) {},
  *generator( parameters… ) {},
  async property( parameters… ) {},
  async* generator( parameters… ) {},

  // with computed keys:
  [property]( parameters… ) {},
  *[generator]( parameters… ) {},
  async [property]( parameters… ) {},

  // compare getter/setter syntax:
  get property() {},
  set property(value) {}
};
```

- 属性简写
  ```javascript
  // es5
  const foo = 'bar';
  const baz = {foo};
  baz // {foo: "bar"}

  // es6
  const baz = {foo: foo};
  ```

- 方法简写
  ```javascript
  //es5
  var obj = {
    foo: function() {
      /* code */
    },
    bar: function() {
      /* code */
    }
  };

  //es6
  let obj = {
    foo() {
      /* code */
    },
    bar() {
      /* code */
    }
  };
  ```

例：
```javascript
let birth = '2000/01/01';

const Person = {

  name: '张三',

  // 等同于 birth: birth
  birth,

  // 等同于 hello: function ()...
  hello() { console.log('我的名字是', this.name); }

};
```
```javascript
function getPoint() {
  const x = 1;
  const y = 10;
  return {x, y};
}

getPoint()
// {x:1, y:10}
```

### 5.2. getter

get 语法将对象属性绑定到查询该属性时将被调用的函数。

Syntax:
```javascript
{get prop() { ... } }
{get [expression]() { ... } }
```

NOTE:
- 可以使用数值或字符串作为标识；
- 必须不带参数；
- 它不能与另一个 get 或具有相同属性的数据条目同时出现在一个对象字面量中（不允许使用 { get x() { }, get x() { } } 和 { x: ..., get x() { } }）。
- 可通过 delete 操作符删除 getter。

eg:
```javascript
var obj = {
  log: ['example','test'],
  get latest() {
    if (this.log.length == 0) 
      return undefined;
    return this.log[this.log.length - 1];
  }
}
console.log(obj.latest); // "test".
console.log(obj.log);//["example", "test"]
obj,latest='abc';
console.log(obj.latest);// "test".
delete obj.latest;// true
console.log(obj.latest);// undefined
```

### 5.3. setter

当尝试设置属性时，set 语法将对象属性绑定到要调用的函数。

Syntax:
```javascript
{set prop(val) { . . . }}
{set [expression](val) { . . . }}
```

NOTE:
- 它的标识符可以是数字或字符串；
- 它必须有一个明确的参数；
- 在对象字面量中，不能为一个已有真实值的变量使用 set ，也不能为一个属性设置多个 set。( `{ set x(v) { }, set x(v) { } } 和 { x: ..., set x(v) { } }` 是不允许的 )
- setter 可以用 delete 操作来移除。

Parameters:
- prop
  
  The name of the property to bind to the given function.

- val
  
  An alias for the variable that holds the value attempted to be assigned to prop.

- expression
  
  Starting with ECMAScript 2015, you can also use expressions for a computed property name to bind to the given function.

eg:
```javascript
var language = {
  set current(name) {
    this.log.push(name);
  },
  log: []
}

language.current = 'EN';
console.log(language.log); // ['EN']

language.current = 'FA';
console.log(language.log); // ['EN', 'FA']
```

```javascript
const cart = {
  _wheels: 4,

  get wheels () {
    return this._wheels;
  },

  set wheels (value) {
    if (value < this._wheels) {
      throw new Error('数值太小了！');
    }
    this._wheels = value;
  }
}
```

```javascript
const obj = {
  class () {}
};

// 等同于

var obj = {
  'class': function() {}// 简洁写法的属性名总是字符串，所以不会因为 class 属于关键字，而导致语法解析报错
};
```

### 5.4. Object.is()

ES5 比较两个值是否相等，只有两个运算符：相等运算符（==）和严格相等运算符（===）。它们都有缺点，前者会自动转换数据类型，后者的 NaN 不等于自身，以及 +0 等于 -0。JavaScript 缺乏一种运算，在所有环境中，只要两个值是一样的，它们就应该相等。

ES6 中 Object.is 就是解决这个问题的新方法。它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致，不同之处只有两个：一是 +0 不等于 -0，二是 NaN 等于自身。

```javascript
Object.is('foo', 'foo')
// true
Object.is({}, {})
// false

+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

### 5.5. Object.assign()

Object.assign 方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。

- Object.assign 方法的第一个参数是目标对象，后面的参数都是源对象。如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

  ```javascript
  const target = { a: 1, b: 1 };

  const source1 = { b: 2, c: 2 };
  const source2 = { c: 3 };

  Object.assign(target, source1, source2);
  target // {a:1, b:2, c:3}
  ```

- 如果非对象参数出现在源对象的位置（即非首参数），那么处理规则有所不同。首先，这些参数都会转成对象，如果无法转成对象，就会跳过。这意味着，如果 undefined 和 null 不在首参数，就不会报错。
  ```javascript
  typeof Object.assign(2) // "object"
  Object.assign(undefined) // 报错
  Object.assign(null) // 报错
  ```

- Object.assign 拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（enumerable: false）。
  ```javascript
  //Object.assign 要拷贝的对象只有一个不可枚举属性 invisible，这个属性并没有被拷贝进去。
  Object.assign({b: 'c'},
    Object.defineProperty({}, 'invisible', {
      enumerable: false,
      value: 'hello'
    })
  )
  // { b: 'c' }
  ```

- 属性名为 Symbol 值的属性，也会被 Object.assign 拷贝。
  ```javscript
  Object.assign({ a: 'b' }, { [Symbol('c')]: 'd' })
  // { a: 'b', Symbol(c): 'd' }
  ```

- Object.assign 方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。
  ```javascript
  const obj1 = {a: {b: 1}};
  const obj2 = Object.assign({}, obj1);

  obj1.a.b = 2;
  obj2.a.b // 2
  ```

- Object.assign 可以用来处理数组，但是会把数组视为对象。
  ```javascript
  // Object.assign 把数组视为属性名为 0、1、2 的对象，因此源数组的 0 号属性 4 覆盖了目标数组的 0 号属性 1。
  Object.assign([1, 2, 3], [4, 5])
  // [4, 5, 3]
  ```

- Object.assign 只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制。
  ```javascript
  const source = {
    get foo() { return 1 }
  };
  const target = {};

  Object.assign(target, source)
  // { foo: 1 }
  ```

- 应用
  - 为对象添加属性、方法
  - 克隆对象：将原始对象拷贝到一个空对象，就得到了原始对象的克隆。
  - 合并多个对象。
  - 为属性指定默认值。

### 5.6. Object.keys() & Object.values() & Object.entries()

ES5 引入了 Object.keys 方法，返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名。ES2017 引入了跟 Object.keys 配套的 Object.values 和 Object.entries，作为遍历一个对象的补充手段，供 for...of 循环使用。

```javascript
let {keys, values, entries} = Object;
let obj = { a: 1, b: 2, c: 3 };

for (let key of keys(obj)) {
  console.log(key); // 'a', 'b', 'c'
}

for (let value of values(obj)) {
  console.log(value); // 1, 2, 3
}

for (let [key, value] of entries(obj)) {
  console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
}
```

### 5.7. Generator 方法

如果某个方法的值是一个生成器方法，前面需要加上星号（`* g(){}`可以正常工作，而`g *(){}`不行）。

非生成器方法定义可能不包含 yield 关键字。这意味着遗留的生成器函数也不会工作，并且将抛出 SyntaxError。始终使用 yield 与星号（*）结合使用。

eg:
```javascript
// 用有属性名的语法定义方法（ES6 之前）：
var obj2 = {
  g: function*() {
    var index = 0;
    while(true)
      yield index++;
  }
};

// 同一个方法，简写语法：
var obj2 = { 
  * g() {
    var index = 0;
    while(true)
      yield index++;
  }
};

var it = obj2.g();
console.log(it.next().value); // 0
console.log(it.next().value); // 1
```

### 5.8. Async 方法

```javascript
// Using a named property
var obj3 = {
  f: async function () {
    await some_promise;
  }
};

// The same object using shorthand syntax
var obj3 = { 
  async f() {
    await some_promise;
  }
};
```

### 5.9. Async 生成器方法

```javascript
var obj4 = {
  f: async function* () {
    yield 1;
    yield 2;
    yield 3;
  }
};

// The same object using shorthand syntax
var obj4 = {
  async* f() {
   yield 1;
   yield 2;
   yield 3;
  }
};
```

## 6. JavaScript 的第七种数据类型：Symbols

## 7. 数据结构扩展

### 7.1. Set

属性：
```javascript
Set.prototype.constructor：构造函数，默认就是 Set 函数。 
Set.prototype.size：返回 Set 实例的成员总数。
```

方法：
```javascript
add(value)：添加某个值，返回 Set 结构本身。 
delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。 
has(value)：返回一个布尔值，表示该值是否为 Set 的成员。 
clear()：清除所有成员，没有返回值。

// 遍历
keys()：返回键名的遍历器 
values()：返回键值的遍历器 
entries()：返回键值对的遍历器 
forEach()：使用回调函数遍历每个成员
```

eg：
```javascript
let arr = new Set([1,2,2,3,3]);  // 能接受一个数组或者类似数组的对象
arr.add(4);
console.log(arr);
```
eg:
```javascript
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

### 7.2. Map

TODO:

## 8. 遍历器 (Iterator) 与 for-of 循环

[深入浅出 ES6（二）：迭代器和 for-of 循环](http://www.infoq.com/cn/articles/es6-in-depth-iterators-and-the-for-of-loop)

### 8.1. 遍历方法的发展

- JavaScript 刚萌生时，一般这样实现数组遍历：
  ```javascript
  for (var index = 0; index < myArray.length; index++) {
    console.log(myArray[index]);
  }
  ```
- 自 ES5 正式发布后，可以使用内建的 forEach 方法来遍历数组：
  ```javascript
  myArray.forEach(function (value) {
  console.log(value);
  });
  ```
  但 forEach 方法存在缺陷：不能使用 break 语句中断循环，也不能使用 return 语句返回到外层函数。

- 可以使用`for...in`循环遍历对象，但不适用于遍历数组：
  - 作用于数组的 for-in 循环体除了遍历数组元素外，还会遍历自定义属性。举个例子，如果你的数组中有一个可枚举属性 myArray.name，循环将额外执行一次，遍历到名为“name”的索引。就连数组原型链上的属性都能被访问到。
  - 在某些情况下，for...in 循环会按照随机顺序遍历数组元素。

- 如果想通过修正 for-in 循环增加数组遍历支持会让这一切变得更加混乱，因此，标准委员会在 ES6 中增加了一种新的循环语法`for...of`来解决目前的问题：
  ```javascript
  for (var value of myArray) {
    console.log(value);
  }
  ```
  这是最简洁、最直接的遍历数组元素的语法，它避开了 for-in 循环的所有缺陷，可以正确响应 break、continue 和 return 语句。

### 8.2. for-of 循环

- for-of 循环不仅支持数组，还支持大多数类数组对象，例如 DOM NodeList 对象。

- for-of 循环也支持字符串遍历，它将字符串视为一系列的 Unicode 字符来进行遍历。

- for-of 循环同样支持 Map 和 Set 对象遍历。

- for-of 循环不支持普通对象，但如果你想迭代一个对象的属性，你可以用 for-in 循环（这也是它的本职工作）或内建的 Object.keys() 方法：
  ```javascript
  // 向控制台输出对象的可枚举属性
  for (var key of Object.keys(someObject)) {
    console.log(key + ": " + someObject[key]);
  }
  ```

### 8.3. 遍历器

正如其它语言中的 for/foreach 语句一样，for-of 循环语句通过方法调用来遍历各种集合。数组、Maps 对象、Sets 对象以及其它在我们讨论的对象有一个共同点，它们都有一个迭代器方法。

当你向任意对象添加了迭代器 `myObject[Symbol.iterator]()` 方法，就可以遍历这个对象了。

例：想让 jQuery 对象也支持 for-of 循环
```javascript
// 因为 jQuery 对象与数组相似
// 可以为其添加与数组一致的迭代器方法
jQuery.prototype[Symbol.iterator] = Array.prototype[Symbol.iterator];
```

for-of 循环首先调用集合的 `[Symbol.iterator]()` 方法，紧接着返回一个新的迭代器对象。迭代器对象可以是任意具有`.next()`方法的对象；for-of 循环将重复调用这个方法，每次循环调用一次。举个例子，一个简单的迭代器：
```javascript
var zeroesForeverIterator = {
 [Symbol.iterator]: function () {
   return this;
  },
  next: function () {
  return {done: false, value: 0};
 }
};
```
每一次调用。next() 方法，它都返回相同的结果，返回给 for-of 循环的结果有两种可能：(a) 我们尚未完成迭代；(b) 下一个值为 0。这意味着 (value of zeroesForeverIterator) {}将会是一个无限循环。当然，一般来说迭代器不会如此简单。

这个迭代器的设计，以及它的。done 和。value 属性，从表面上看与其它语言中的迭代器不太一样。在 Java 中，迭代器有分离的。hasNext() 和。next() 方法。在 Python 中，他们只有一个。next() 方法，当没有更多值时抛出 StopIteration 异常。但是所有这三种设计从根本上讲都返回了相同的信息。

迭代器对象也可以实现可选的。return() 和。throw(exc) 方法。如果 for-of 循环过早退出会调用。`return()` 方法，异常、break 语句或 return 语句均可触发过早退出。如果迭代器需要执行一些清洁或释放资源的操作，可以在。`return()` 方法中实现。大多数迭代器方法无须实现这一方法。`.throw(exc)` 方法的使用场景就更特殊了：for-of 循环永远不会调用它。

## 9. 模块（Module）

在 ES6 中，export 命令用于规定模块的对外接口，import 命令用于输入其他模块提供的功能。

import 和 export 命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作用域内，就会报错。

NOTE:
- ES6 的模块自动采用严格模式，不管你有没有在模块头部加上`"use strict"`;。
- ES6 模块之中，顶层的 this 指向 undefined，即不应该在顶层代码使用 this。
- 变量必须声明后再使用。

### 9.1. export

一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。如果你希望外部能够读取模块内部的某个变量，就必须使用 export 关键字输出该变量。

- 导出变量、函数、类
  - 写法一：
    ```javascript
    // profile.js
    export var firstName = 'Michael';
    export var lastName = 'Jackson';
    export var year = 1958;
    ```

  - 写法二：
    ```javascript
    // profile.js
    var firstName = 'Michael';
    var lastName = 'Jackson';
    var year = 1958;

    export {firstName, lastName, year};
    ```
    应该优先考虑使用这种写法。因为这样就可以在脚本尾部，一眼看清楚输出了哪些变量。

- 使用 as 关键字，重命名对外接口：
  ```javascript
  function v1() { ... }
  function v2() { ... }

  export {
    v1 as streamV1,
    v2 as streamV2,
    v2 as streamLatestVersion
    // 重命名后，v2 可以用不同的名字输出两次
  };
  ```

- export 命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系：
  ```javascript
  // 报错
  export 1;

  // 报错
  var m = 1;
  export m;
  ```

- export 语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。
  ```javascript
  export var foo = 'bar';
  setTimeout(() => foo = 'baz', 500);
  ```
  对比 CommonJS：CommonJS 模块输出的是值的缓存，不存在动态更新

- 如果模块默认输出一个函数，函数名的首字母应该小写；如果模块默认输出一个对象，对象名的首字母应该大写。
  ```javascript
  function makeStyleGuide() {
  }
  export default makeStyleGuide;
  ```
  ```javascript
  const StyleGuide = {
    es6: {
    }
  };
  export default StyleGuide;
  ```

例：
```javascript
import store from '../store/index'
import {mapState, mapMutations, mapActions} from 'vuex'
import axios from '../assets/js/request'
import util from '../utils/js/util.js'

export default {
  created () {
    this.getClassify(); 

    this.RESET_VALUE();
    console.log('created' ,new Date().getTime());

  }
}
```

### 9.2. import

使用 export 命令定义了模块的对外接口以后，其他 JS 文件就可以通过 import 命令加载这个模块。import 命令加载指定文件，并从文件中输入变量。

- import 命令接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块中的对外接口名称相同。
  ```javascript
  // main.js
  import {firstName, lastName, year} from './profile.js';

  function setName(element) {
    element.textContent = firstName + ' ' + lastName;
  }
  ```

- 使用 as 关键字，可为输入的变量重命名。
  ```javascript
  import { lastName as surname } from './profile.js';
  ```

- import 命令输入的变量都是只读的，因为它的本质是输入接口。但是，如果输入变量是一个对象，改写其属性是允许的，并且其他模块也可以读到改写后的值。不过，这种写法很难查错，建议凡是输入的变量，都当作完全只读，轻易不要改变它的属性。
  ```javascript
  import {a} from './xxx.js'

  a = {}; // Syntax Error : 'a' is read-only;
  ```
  ```javascript
  import {a} from './xxx.js'

  a.foo = 'hello'; // 合法操作
  ```

- import 后面的 from 指定模块文件的位置，可以是相对路径，也可以是绝对路径，.js 后缀可以省略。如果只是模块名，不带有路径，那么必须有配置文件，告诉 JavaScript 引擎该模块的位置。

- import 命令具有提升效果，会提升到整个模块的头部，首先执行。
  ```javascript
  foo();

  import { foo } from 'my_module';
  ```

- 由于 import 是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构。
  ```javascript
  // 报错
  import { 'f' + 'oo' } from 'my_module';

  // 报错
  let module = 'my_module';
  import { foo } from module;

  // 报错
  if (x === 1) {
    import { foo } from 'module1';
  } else {
    import { foo } from 'module2';
  }
  ```

- 不要使用通配符 * 的 import，这样可以确保你的模块之中，有一个默认输出（export default）:
  ```javascript
  // bad
  import * as myObject from './importModule';

  // good
  import myObject from './importModule';
  ```

- 不要直接从一个 import 上 export:
  ```javascript
  // bad
  // filename es6.js
  export default { es6 } from './airbnbStyleGuide';
  // good
  // filename es6.js
  import { es6 } from './AirbnbStyleGuide';
  export default es6;
  ```

- import 语句是 Singleton 模式。
  ```javascript
  import { foo } from 'my_module';
  import { bar } from 'my_module';

  // 等同于
  import { foo, bar } from 'my_module';
  ```

- 通过 Babel 转码，CommonJS 模块的 require 命令和 ES6 模块的 import 命令，可以写在同一个模块里面，但是最好不要这样做。因为 import 在静态解析阶段执行，所以它是一个模块之中最早执行的，这样的代码可能不会得到预期结果。

- 可以使用 `export default` 命令，为模块指定默认输出。当其他模块加载该模块时，import 命令可以为该匿名函数指定任意名字。一个模块只能有一个默认输出。
  ```javascript
  export default function () {
    console.log('foo');
  }

  import customName from './export-default';
  customName(); // 'foo'
  ```
  ```javascript
  // MyClass.js
  export default class { ... }

  // main.js
  import MyClass from 'MyClass';
  let o = new MyClass();
  ```
  需要注意的是，这时 import 命令后面，不使用大括号。

  本质上，export default 就是输出一个叫做 default 的变量或方法，然后系统允许你为它取任意名字。
  ```javascript
  function add(x, y) {
    return x * y;
  }
  export {add as default};
  // 等同于
  // export default add;

  // app.js
  import { default as foo } from 'modules';
  // 等同于
  // import foo from 'modules';
  ```
  因此，export default 后不能跟变量声明语句：
  ```javascript
  // 正确
  var a = 1;
  export default a;

  // 错误
  export default var a = 1;

  // 正确
  export default 42;
  ```
  可在一条 import 语句中，同时输入默认方法和其他接口：
  ```javascript
  // lodash.js
  export default function (obj) {
    // ···
  }

  export function each(obj, iterator, context) {
    // ···
  }

  export { each as forEach };

  // other.js
  import _, { each, each as forEach } from 'lodash';
  ```

### 9.3. 跨模块常量

const 声明的常量只在当前代码块有效。如果想设置跨模块的常量（即跨多个文件），或者说一个值要被多个模块共享，可以采用下面的写法：
```javascript
// constants.js 模块
export const A = 1;
export const B = 3;
export const C = 4;

// test1.js 模块
import * as constants from './constants';
console.log(constants.A); // 1
console.log(constants.B); // 3

// test2.js 模块
import {A, B} from './constants';
console.log(A); // 1
console.log(B); // 3
```

## 10. Refer Links

[ES6 新特性](http://es6katas.org/)

[阮一峰 - ECMAScript 6 标准入门](http://es6.ruanyifeng.com/)

[ES6 简单入门分享](http://blog.csdn.net/longwenxia1234/article/details/59831509)
