- [ECMAScript 6 NOTE](#ecmascript-6-note)
  - [变量扩展语法](#%E5%8F%98%E9%87%8F%E6%89%A9%E5%B1%95%E8%AF%AD%E6%B3%95)
    - [let 和 const](#let-%E5%92%8C-const)
    - [解构赋值](#%E8%A7%A3%E6%9E%84%E8%B5%8B%E5%80%BC)
    - [模板字符串](#%E6%A8%A1%E6%9D%BF%E5%AD%97%E7%AC%A6%E4%B8%B2)
  - [函数扩展语法](#%E5%87%BD%E6%95%B0%E6%89%A9%E5%B1%95%E8%AF%AD%E6%B3%95)
    - [默认参数](#%E9%BB%98%E8%AE%A4%E5%8F%82%E6%95%B0)
    - [变长参数 （rest） 和扩展运算符](#%E5%8F%98%E9%95%BF%E5%8F%82%E6%95%B0-%EF%BC%88rest%EF%BC%89-%E5%92%8C%E6%89%A9%E5%B1%95%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [箭头函数](#%E7%AE%AD%E5%A4%B4%E5%87%BD%E6%95%B0)
      - [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
      - [没有 this、arguments、caller](#%E6%B2%A1%E6%9C%89-this%E3%80%81arguments%E3%80%81caller)
      - [没有 prototype 属性](#%E6%B2%A1%E6%9C%89-prototype-%E5%B1%9E%E6%80%A7)
      - [不可被 new](#%E4%B8%8D%E5%8F%AF%E8%A2%AB-new)
      - [不适用 yield](#%E4%B8%8D%E9%80%82%E7%94%A8-yield)
      - [可定义为递归函数](#%E5%8F%AF%E5%AE%9A%E4%B9%89%E4%B8%BA%E9%80%92%E5%BD%92%E5%87%BD%E6%95%B0)
      - [应用场景](#%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF)
    - [装饰器函数（Decorator）](#%E8%A3%85%E9%A5%B0%E5%99%A8%E5%87%BD%E6%95%B0%EF%BC%88decorator%EF%BC%89)
  - [数组扩展语法](#%E6%95%B0%E7%BB%84%E6%89%A9%E5%B1%95%E8%AF%AD%E6%B3%95)
  - [类的扩展语法](#%E7%B1%BB%E7%9A%84%E6%89%A9%E5%B1%95%E8%AF%AD%E6%B3%95)
    - [类的定义](#%E7%B1%BB%E7%9A%84%E5%AE%9A%E4%B9%89)
    - [构造函数](#%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)
    - [原型方法](#%E5%8E%9F%E5%9E%8B%E6%96%B9%E6%B3%95)
    - [static 方法](#static-%E6%96%B9%E6%B3%95)
    - [不会自动装箱](#%E4%B8%8D%E4%BC%9A%E8%87%AA%E5%8A%A8%E8%A3%85%E7%AE%B1)
    - [类的继承](#%E7%B1%BB%E7%9A%84%E7%BB%A7%E6%89%BF)
    - [Symbol.species](#symbolspecies)
    - [Mix-ins 混合](#mix-ins-%E6%B7%B7%E5%90%88)
  - [对象扩展语法](#%E5%AF%B9%E8%B1%A1%E6%89%A9%E5%B1%95%E8%AF%AD%E6%B3%95)
    - [getter](#getter)
    - [setter](#setter)
    - [生成器方法](#%E7%94%9F%E6%88%90%E5%99%A8%E6%96%B9%E6%B3%95)
    - [Async 方法](#async-%E6%96%B9%E6%B3%95)
    - [Async 生成器方法](#async-%E7%94%9F%E6%88%90%E5%99%A8%E6%96%B9%E6%B3%95)
  - [JavaScript 的第七种数据类型：Symbols](#javascript-%E7%9A%84%E7%AC%AC%E4%B8%83%E7%A7%8D%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%EF%BC%9Asymbols)
  - [数据结构扩展](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E6%89%A9%E5%B1%95)
    - [Set](#set)
    - [Map](#map)
  - [Promise 对象](#promise-%E5%AF%B9%E8%B1%A1)
    - [promise 对象概述](#promise-%E5%AF%B9%E8%B1%A1%E6%A6%82%E8%BF%B0)
    - [promise 对象的状态和变化途径](#promise-%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%8A%B6%E6%80%81%E5%92%8C%E5%8F%98%E5%8C%96%E9%80%94%E5%BE%84)
    - [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
    - [例子：](#%E4%BE%8B%E5%AD%90%EF%BC%9A)
  - [遍历器 (Iterator) 与 for-of 循环](#%E9%81%8D%E5%8E%86%E5%99%A8-iterator-%E4%B8%8E-for-of-%E5%BE%AA%E7%8E%AF)
    - [遍历方法的发展](#%E9%81%8D%E5%8E%86%E6%96%B9%E6%B3%95%E7%9A%84%E5%8F%91%E5%B1%95)
    - [for-of 循环](#for-of-%E5%BE%AA%E7%8E%AF)
    - [遍历器](#%E9%81%8D%E5%8E%86%E5%99%A8)
  - [生成器（Generator）](#%E7%94%9F%E6%88%90%E5%99%A8%EF%BC%88generator%EF%BC%89)
  - [异步请求](#%E5%BC%82%E6%AD%A5%E8%AF%B7%E6%B1%82)
  - [模块（Module）](#%E6%A8%A1%E5%9D%97%EF%BC%88module%EF%BC%89)
  - [Refer Links](#refer-links)

# ECMAScript 6 NOTE

ES6 既是一个历史名词，也是一个泛指，含义是 5.1 版以后的 JavaScript 的下一代标准，涵盖了 ES2015、ES2016、ES2017 等，而 ES2015 则是正式名称，特指该年发布的正式版本的语言标准。

ECMA 的第 39 号技术专家委员会（Technical Committee 39，简称 [TC39](https://github.com/tc39/ecma262)）负责制订 ECMAScript 标准，成员包括 Microsoft、Mozilla、Google 等大公司。

各大浏览器的最新版本，对 ES6 的支持可以在[这里](kangax.github.io/es5-compat-table/es6/) 查看。

## 变量扩展语法

### let 和 const

ES5 只有全局作用域和函数作用域，没有块级作用域。一不小心变量就会被覆盖。
在 ES6 中，let 和 const 提供了块级作用域，

- let 和 const 用途与 var 类似，区别在于 var 是全局生效的，let 和 const 只在当前代码块有效。

- 不存在变量提升，变量要在声明后才能够使用。

- 不允许在相同作用域重复命名同一变量。

- let 用于定义变量，而 const 用于定义常量，即定义后不能再改变，不能赋值，同样不能变量提升，不可重复声明。

### 解构赋值

所谓解构，即把某种数据类型，分解成单个变量的过程。

```javascript
function  data(){
    var a=1,b=2;
    return [a,b];
}
var [x,y]=data();// 解构赋值
console.log(x);    //1
console.log(y);    //2
```

```javascript
function setting(){
    return {
      display:{
        color:'red'
      },
      keyboard:{
        layout:‘querty’
      }
    };
}
const {
  display:{
    color: displayColor
  },
  keyboard:{
    layout:keyboardLayout
  }
} = setting();// 解构赋值

console.log(displayColor, keyboardLayout);    //'red' 'querty'
```

```javascript
// 函数参数的解构赋值

// 参数为数组
let fun = function([x,y=3]){
    console.log(x+y);
}

fun([1]);

// 参数为对象
let fun = function({x=1,y=3}){
    console.log(x+y);
}
fun();
```

使用解构赋值避免为属性创建冗余的临时变量或对象：
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

NOTE：
- 可以为解构赋值设置默认值

  ```javascript
  let arr = [1,2,null];
  let [a,b,c,d,e='5'] = arr;//a=1,b=2,c=null,d=undefined,e='5'
  ```

- 如果解构不成功，变量被设置为 undefined 
- 数据结构具有 Iterator 接口 （ps： 新增加的一种接口形式，是可以被遍历的数据接口） 
- 数组和字符串的解构是按照顺序来赋值的，但对象是按照属性名对应变量名。因为对象的属性没有顺序

  ```javascript
  let obj = {'a':1,'b':2};
  let {a,b:str,c=5} = obj;
  console.log(a);
  console.log(str);
  console.log(c);
  ```

### 模板字符串

```javascript
let a=1,b=2;
docuemnt.getElementById('test').appendChild(`<b>${a}</b><b>${b}</b>`);// 使用反引号``创建一个模板字符串，在模板字符串中使用 ${x} 引用并输出变量值
```

## 函数扩展语法

### 默认参数

```javascript
function log(x,y='小明'){
    console.log(x,y);
}
log('hello');    //hello 小明
log('hello','xiaoming');    //hello xiaoming
log('nihao','');    //nihao
```

### 变长参数 （rest） 和扩展运算符

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

### 箭头函数

[MDN Arrow Function](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

#### 基本使用

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

#### 没有 this、arguments、caller

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

#### 没有 prototype 属性

```javascript
var Foo = () => {};
console.log(Foo.prototype); // undefined
```

#### 不可被 new

箭头函数不能用作构造器，和 new 一起用会抛出错误。

```javascript
var Foo = () => {};
var foo = new Foo(); // TypeError: Foo is not a constructor
```

#### 不适用 yield

yield 关键字通常不能在箭头函数中使用（除非是嵌套在允许使用的函数内）。因此，箭头函数不能用作生成器。

#### 可定义为递归函数

```javascript
var fact = (x) => ( x==0 ?  1 : x*fact(x-1) );
fact(5);       // 120
```

#### 应用场景

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

### 装饰器函数（Decorator）

TODO:

## 数组扩展语法

使用 `...` 拷贝数组：
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

## 类的扩展语法

ES6 中引入了 class 关键字，支持使用 class 直接定义一个类，但实质上这只是 JavaScript 现有的基于原型的继承的语法糖。 类语法不是向 JavaScript 引入一个新的面向对象的继承模型，但提供了一个更简单和更清晰的语法来创建对象并处理继承。

### 类的定义

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

### 构造函数

构造函数方法是一个特殊的方法，其用于创建和初始化使用一个类创建的一个对象。一个类只能拥有一个名为 “constructor”的特殊方法。如果类包含多个构造函数的方法，则将抛出一个 SyntaxError 。

一个构造函数可以使用 super 关键字来调用一个父类的构造函数。

### 原型方法

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

### static 方法

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

### 不会自动装箱

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

### 类的继承

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

### Symbol.species

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

### Mix-ins 混合

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

## 对象扩展语法

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

eg:
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

### getter

get 语法将对象属性绑定到查询该属性时将被调用的函数。

- Syntax:
  ```javascript
  {get prop() { ... } }
  {get [expression]() { ... } }
  ```

- NOTE:
  - 可以使用数值或字符串作为标识；
  - 必须不带参数；
  - 它不能与另一个 get 或具有相同属性的数据条目同时出现在一个对象字面量中（不允许使用 { get x() { }, get x() { } } 和 { x: ..., get x() { } }）。
  - 可通过 delete 操作符删除 getter。

- eg:
  ```javascript
  var obj = {
    log: ['example','test'],
    get latest() {
      if (this.log.length == 0) return undefined;
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

### setter

当尝试设置属性时，set 语法将对象属性绑定到要调用的函数。

- Syntax:
  ```javascript
  {set prop(val) { . . . }}
  {set [expression](val) { . . . }}
  ```

- NOTE:
  - 它的标识符可以是数字或字符串；
  - 它必须有一个明确的参数；
  - 在对象字面量中，不能为一个已有真实值的变量使用 set ，也不能为一个属性设置多个 set。( `{ set x(v) { }, set x(v) { } } 和 { x: ..., set x(v) { } }` 是不允许的 )
  - setter 可以用 delete 操作来移除。

- Parameters:
  - prop
    
    The name of the property to bind to the given function.

  - val
    
    An alias for the variable that holds the value attempted to be assigned to prop.

  - expression
    
    Starting with ECMAScript 2015, you can also use expressions for a computed property name to bind to the given function.

- eg:
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

### 生成器方法

简写语法中的星号（`*`）必须出现在生成器名前，也就是说`* g(){}`可以正常工作，而`g *(){}`不行。

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

### Async 方法

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

### Async 生成器方法

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

## JavaScript 的第七种数据类型：Symbols

## 数据结构扩展

### Set

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

### Map

TODO:

## Promise 对象

http://wiki.jikexueyuan.com/project/es6/promise.html

http://javascript.ruanyifeng.com/advanced/promise.html

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise

### promise 对象概述

Promise 对象是 CommonJS 工作组提出的一种规范，目的是为异步操作提供统一接口。Promises 原本只是社区提出的一个构想，一些外部函数库率先实现了这个功能。ECMAScript 6 将其写入语言标准，因此目前 JavaScript 语言原生支持 Promise 对象。

Promise 的思想是，每一个异步任务立刻返回一个 Promise 对象，由于是立刻返回，所以可以采用同步操作的流程。这个 Promises 对象有一个 then 方法，允许指定回调函数，在异步任务完成后调用。

### promise 对象的状态和变化途径

Promise 对象只有三种状态：
- 异步操作“未完成”（pending）
- 异步操作“已完成”（resolved，又称 fulfilled）
- 异步操作“失败”（rejected）

这三种的状态的变化途径只有两种：
- 异步操作从“未完成”到“已完成”
- 异步操作从“未完成”到“失败”。
这种变化只能发生一次，一旦当前状态变为“已完成”或“失败”，就意味着不会再有新的状态变化了。

### 基本使用

eg:
```javascript
var promise = new Promise(function(resolve, reject) {
  // 当异步代码执行成功时，我们才会调用 resolve(...), 当异步代码失败时就会调用 reject(...)
  if (/* 异步操作成功 */){
    resolve('成功！');
  } else {
    reject('失败！');
  }
});

promise.then(function(value) {
  // 当 promise 状态为 resolve，触发该函数执行，传入参数的值是上面调用 resolve(...) 方法传入的值，即‘成功！’
}, function(value) {
  // 当 promise 状态为 reject，触发该函数执行，传入参数的值是上面调用 reject(...) 方法传入的值，即‘失败！’
});
```
- Promise 构造函数接受一个函数作为参数，该函数的两个参数分别是 resolve 和 reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。
- resolve 函数的作用是，将 Promise 对象的状态从“未完成”变为“成功”（即从 Pending 变为 Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject 函数的作用是，将 Promise 对象的状态从“未完成”变为“失败”（即从 Pending 变为 Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
- Promise 实例生成以后，可以用 then 方法分别指定 Resolved 状态和 Reject 状态的回调函数。

- then 方法可以链式使用。
  ```javascript
  po
    .then(step1)
    .then(step2)
    .then(step3)
    .then(
      console.log,
      console.error
    );
  ```

### 例子：

使用 Promise 对象操作 ajax：
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

## 遍历器 (Iterator) 与 for-of 循环

[深入浅出 ES6（二）：迭代器和 for-of 循环](http://www.infoq.com/cn/articles/es6-in-depth-iterators-and-the-for-of-loop)

### 遍历方法的发展

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

### for-of 循环

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

### 遍历器

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

## 生成器（Generator）

## 异步请求

## 模块（Module）

## Refer Links

[ES6 新特性](http://es6katas.org/)

[阮一峰 - ECMAScript 6 标准入门](http://es6.ruanyifeng.com/)

[ES6 简单入门分享](http://blog.csdn.net/longwenxia1234/article/details/59831509)
