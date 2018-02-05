- [JavaScript 面向对象](#javascript-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1)
  - [1. 基础概念](#1-%E5%9F%BA%E7%A1%80%E6%A6%82%E5%BF%B5)
    - [1.1. 原型](#11-%E5%8E%9F%E5%9E%8B)
    - [1.2. Prototype 对象](#12-prototype-%E5%AF%B9%E8%B1%A1)
      - [1.2.1. prototype 的相关方法](#121-prototype-%E7%9A%84%E7%9B%B8%E5%85%B3%E6%96%B9%E6%B3%95)
    - [1.3. `__proto__`属性](#13-proto%E5%B1%9E%E6%80%A7)
  - [2. 对象创建 - 封装](#2-%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA---%E5%B0%81%E8%A3%85)
    - [2.1. 直接声明创建对象](#21-%E7%9B%B4%E6%8E%A5%E5%A3%B0%E6%98%8E%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1)
    - [2.2. 使用构造函数创建对象](#22-%E4%BD%BF%E7%94%A8%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1)
    - [2.3. 使用 Object.create 创建对象](#23-%E4%BD%BF%E7%94%A8-objectcreate-%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1)
    - [2.4. 使用 class 关键字创建对象](#24-%E4%BD%BF%E7%94%A8-class-%E5%85%B3%E9%94%AE%E5%AD%97%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1)
  - [3. 对象继承](#3-%E5%AF%B9%E8%B1%A1%E7%BB%A7%E6%89%BF)
    - [3.1. JavaScript 中的继承特性](#31-javascript-%E4%B8%AD%E7%9A%84%E7%BB%A7%E6%89%BF%E7%89%B9%E6%80%A7)
    - [3.2. 内置对象的默认继承](#32-%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1%E7%9A%84%E9%BB%98%E8%AE%A4%E7%BB%A7%E6%89%BF)
    - [3.3. 基于构造函数的继承](#33-%E5%9F%BA%E4%BA%8E%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E7%9A%84%E7%BB%A7%E6%89%BF)
    - [3.4. 基于 prototype 对象的继承](#34-%E5%9F%BA%E4%BA%8E-prototype-%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%BB%A7%E6%89%BF)
      - [3.4.1. 将子类构造函数的 prototype 属性指向父类的实例](#341-%E5%B0%86%E5%AD%90%E7%B1%BB%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E7%9A%84-prototype-%E5%B1%9E%E6%80%A7%E6%8C%87%E5%90%91%E7%88%B6%E7%B1%BB%E7%9A%84%E5%AE%9E%E4%BE%8B)
      - [3.4.2. 将子类构造函数的 prototype 属性指向父类构造函数的 prototype 属性](#342-%E5%B0%86%E5%AD%90%E7%B1%BB%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E7%9A%84-prototype-%E5%B1%9E%E6%80%A7%E6%8C%87%E5%90%91%E7%88%B6%E7%B1%BB%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E7%9A%84-prototype-%E5%B1%9E%E6%80%A7)
      - [3.4.3. 利用空对象作为中介](#343-%E5%88%A9%E7%94%A8%E7%A9%BA%E5%AF%B9%E8%B1%A1%E4%BD%9C%E4%B8%BA%E4%B8%AD%E4%BB%8B)
    - [3.5. 基于拷贝的继承](#35-%E5%9F%BA%E4%BA%8E%E6%8B%B7%E8%B4%9D%E7%9A%84%E7%BB%A7%E6%89%BF)
    - [3.6. 使用 Object.create 实现继承](#36-%E4%BD%BF%E7%94%A8-objectcreate-%E5%AE%9E%E7%8E%B0%E7%BB%A7%E6%89%BF)
    - [3.7. 使用 class 关键字实现继承](#37-%E4%BD%BF%E7%94%A8-class-%E5%85%B3%E9%94%AE%E5%AD%97%E5%AE%9E%E7%8E%B0%E7%BB%A7%E6%89%BF)
    - [3.8. NOTE](#38-note)
  - [4. Refer Links](#4-refer-links)

# JavaScript 面向对象

Javascript 是一种基于对象（object-based）的语言，在 JavaScript 中对象包括了狭义上的对象和广义上的对象：
- 狭义上的对象：指的是原生对象 Object，所有对象都会继承 Object.prototype 上的所有属性（如 hasOwnProperty，etc）。
- 广义上的对象：在 JavaScript 上，一切皆对象，也就是说包括数组、函数等都是广义上的对象。

## 1. 基础概念

### 1.1. 原型

### 1.2. Prototype 对象

Javascript 规定，每一个构造函数都有一个私有属性`prototype`，指向另一个对象 (prototype 对象)。这个对象的所有属性和方法，都会被构造函数的实例继承。

所有实例对象需要共享的属性和方法，都放在这个对象里面；那些不需要共享的属性和方法（即私有属性和私有方法），就放在构造函数里面。实例对象一旦创建，将自动引用 prototype 对象的属性和方法。也就是说，实例对象的属性和方法，分成两种，一种是本地的，另一种是引用的。

prototype 对象默认会包含两个属性：constructor 和`__proto__`，其中`__proto__`有部分浏览器不支持。

当我们的代码需要一个属性的时候，Javascript 的引擎会先看当前的这个对象中是否有这个属性，如果没有的话，就会查找他的 Prototype 对象是否有这个属性，一直继续下去，直到找到或是直到没有 Prototype 对象。

#### 1.2.1. prototype 的相关方法

为了配合 prototype 属性，Javascript 定义了一些辅助方法，帮助我们使用它。

- isPrototypeOf()

  这个方法用来判断，某个 proptotype 对象和某个实例之间的关系。
  ```javascript
  alert(Cat.prototype.isPrototypeOf(cat1)); //true
  alert(Cat.prototype.isPrototypeOf(cat2)); //true
  ```

- hasOwnProperty()

  每个实例对象都有一个 hasOwnProperty() 方法，用来判断某一个属性到底是本地属性，还是继承自 prototype 对象的属性。
  ```javascript
  alert(cat1.hasOwnProperty("name")); // true
  alert(cat1.hasOwnProperty("type")); // false
  ```

- in 运算符

  in 运算符可以用来判断，某个实例是否含有某个属性，不管是不是本地属性。
  ```javascript
  alert("name" in cat1); // true
  alert("type" in cat1); // true
  ```
  in 运算符还可以用来遍历某个对象的所有属性。
  ```javascript
  for (var prop in cat1) { 
    alert("cat1["+prop+"]="+cat1[prop]); 
  }
  ```

### 1.3. `__proto__`属性

prototype 是原型 / 构造函数的属性，而`__proto__`是实例对象的属性，指向原型 / 构造函数的 prototype 属性对象。

NOTE：

- 并不是所有的对象原型链上都有 Object.prototype
		
  例：
  ```javascript
  var obj=Object.create(null);

  obj.__proto__//undefined
  obj.toString()//undefined
  ```
	
- 并不是所有的对象都有 prototype 属性

  例：
  ```javascript
  function abc() {}
  abc.prototype//abc{}
  var bined = abc.bind(null);

  typeof binded//"function"
  binded.prototype//undefined
  ```

## 2. 对象创建 - 封装

JavaScript 中创建对象有多种方法：

### 2.1. 直接声明创建对象

```javscript
var o = {
  a: 1,
  b: 2,
  c: function () {
    //...
  }
};
```

缺点：
- 一是如果多生成几个实例，写起来就非常麻烦；
- 二是实例与原型之间，没有任何办法，可以看出有什么联系。

### 2.2. 使用构造函数创建对象

为了解决从原型对象生成实例的问题，Javascript 提供了一个构造函数（Constructor）模式。

构造器其实就是一个普通的函数，但是内部使用了 this 变量。当使用 new 操作符来作用这个函数时，它就可以被称为构造方法（构造函数），函数会创建对象实例并将 this 变量绑定在实例对象上。

当使用 new 构造对象时：
```
var o = new Foo();
```
JavaScript 实际上执行的是：
```javascript
var o = new Object();
o.__proto__ = Foo.prototype;
Foo.call(o);
```

为了和一般的函数对象有所区别，**构造函数的首字母一般都大写**。

```javascript
function Cat(name,color) {
  // 实例属性
  this.name = name;
　this.color = color;
}
// 原型属性
// 所有实例的 type 属性和 eat() 方法占同一块内存
Cat.prototype.type = "猫科动物";
Cat.prototype.eat = function(){alert("吃老鼠")};

// 使用 new 创建实例对象
var cat1 = new Cat("大毛","黄色");
var cat2 = new Cat("二毛","黑色");

// 这时 cat1 和 cat2 会自动含有一个 constructor 属性，指向它们的构造函数：
alert(cat1.constructor == Cat); //true
alert(cat2.constructor == Cat); //true

// 可使用 instanceof 运算符，验证原型对象与实例对象之间的关系
alert(cat1 instanceof Cat); //true
alert(cat2 instanceof Cat); //true

// 使用原型属性
alert(cat1.type); // 猫科动物
cat1.eat(); // 吃老鼠
alert(cat2.type); // 猫科动物
cat2.eat(); // 吃老鼠
alert(cat1.eat == cat2.eat); //true，证明 prototype 对象是被共享的
```

### 2.3. 使用 Object.create 创建对象

ECMAScript 5 中引入了一个新方法：Object.create()。可以调用这个方法来创建一个新对象。调用 create 方法时传入的第一个参数（必须传入）为要创建的新对象的原型：

```javascript
// 创建一个普通对象
// 等同于 var o = {};
var o = Object.create(Object.prototype);// a ---> Object.prototype ---> null

// NOTE：
var a = Object.create(null);// d ---> null
console.log(a.hasOwnProperty); // undefined, 因为 d 没有继承 Object.prototype
```

### 2.4. 使用 class 关键字创建对象

ECMAScript6 引入了一套新的关键字用来实现 class。**使用基于类语言的开发人员会对这些结构感到熟悉，但它们是不同的。JavaScript 仍然基于原型**。这些新的关键字包括 class, constructor，static，extends 和 super。

```javascript
"use strict";

class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}

var Polygon = new Polygon(2, 2);
```

## 3. 对象继承

### 3.1. JavaScript 中的继承特性

- 继承属性

  ```javascript
  // 让我们假设我们有一个对象 o, 其有自己的属性 a 和 b：
  // {a: 1, b: 2}
  // o 的原型 o.__proto__有属性 b 和 c：
  // {b: 3, c: 4}
  // 最后，o.__proto__.__proto__ 是 null.
  // 这就是原型链的末尾，即 null，
  // 根据定义，null 没有__proto__.
  // 综上，整个原型链如下：
  // {a:1, b:2} ---> {b:3, c:4} ---> null

  console.log(o.a); // 1
  // a 是 o 的自身属性吗？是的，该属性的值为 1

  console.log(o.b); // 2
  // b 是 o 的自身属性吗？是的，该属性的值为 2
  // o.__proto__上还有一个'b'属性，但是它不会被访问到。这种情况称为"属性遮蔽 (property shadowing)".

  console.log(o.c); // 4
  // c 是 o 的自身属性吗？不是，那看看 o.__proto__上有没有。
  // c 是 o.__proto__的自身属性吗？是的，该属性的值为 4

  console.log(o.d); // undefined
  // d 是 o 的自身属性吗？不是，那看看 o.__proto__上有没有。
  // d 是 o.__proto__的自身属性吗？不是，那看看 o.__proto__.__proto__上有没有。
  // o.__proto__.__proto__为 null，停止搜索，
  // 没有 d 属性，返回 undefined
  ```

- 继承方法

  JavaScript 并没有其他基于类的语言所定义的“方法”。在 JavaScript 里，任何函数都可以添加到对象上作为对象的属性。函数的继承与其他的属性继承没有差别，包括上面的“属性覆盖”（这种情况相当于其他语言的方法重写）。

  当继承的函数被调用时，this 指向的是当前继承的对象，而不是继承的函数所在的原型对象。

  ```javascript
  var o = {
    a: 2,
    m: function(){
      return this.a + 1;
    }
  };

  console.log(o.m()); // 3
  // 当调用 o.m 时，'this'指向了 o.

  var p = Object.create(o);
  // p 是一个对象，p.__proto__是 o.

  p.a = 4; // 创建 p 的自身属性 a.
  console.log(p.m()); // 5
  // 调用 p.m 时，'this'指向 p. 
  // 又因为 p 继承 o 的 m 函数
  // 此时的'this.a' 即 p.a，即 p 的自身属性 'a'
  ```

### 3.2. 内置对象的默认继承

```javascript
var o = {a: 1};

// o 这个对象继承了 Object.prototype 上面的所有属性
// 所以可以这样使用 o.hasOwnProperty('a').
// hasOwnProperty 是 Object.prototype 的自身属性。
// Object.prototype 的原型为 null。
// 原型链如下：
// o ---> Object.prototype ---> null

var a = ["yo", "whadup", "?"];

// 数组都继承于 Array.prototype 
// (indexOf, forEach 等方法都是从它继承而来).
// 原型链如下：
// a ---> Array.prototype ---> Object.prototype ---> null

function f(){
  return 2;
}

// 函数都继承于 Function.prototype
// (call, bind 等方法都是从它继承而来):
// f ---> Function.prototype ---> Object.prototype ---> null
```

### 3.3. 基于构造函数的继承

使用 call 或 apply 方法，将父对象的构造函数绑定在子对象上：
```javascript
function Animal(){
  this.species = "动物";
}

function Cat(name,color){
　　Animal.apply(this, arguments);
　　this.name = name;
　　this.color = color;
}

var cat1 = new Cat("大毛","黄色");
alert(cat1.species); // 动物
```

优点：

- 子类实例可以得到父类的引用属性，但分别占不同的内存空间，不会相互影响

- 创建子类实例时，可以向父类构造函数传参


缺点：

- 无法实现函数复用，每个子类实例都持有一个新的fun函数，效率较低


### 3.4. 基于 prototype 对象的继承

#### 3.4.1. 将子类构造函数的 prototype 属性指向父类的实例

```javascript
function Animal(){
  this.species = "动物";
}

function Cat(name,color){
　　this.name = name;
　　this.color = color;
}

Cat.prototype = new Animal();// 将子类构造函数的 prototype 属性指向父类的实例
Cat.prototype.constructor = Cat;// 将 Cat.prototype 对象的 constructor 值改回 Cat, 避免继承链的紊乱，此步不可少
var cat1 = new Cat("大毛","黄色");
alert(cat1.species); // 动物
```

优点：
- 简单易于实现

缺点：
- 每次创建子类都需要先创建父类的实例，浪费内存，效率较低。
- 创建子类实例时，无法向父类构造函数传参。
- 修改sub1.arr后sub2.arr也会变，因为来自原型对象的引用属性是所有实例共享的。


#### 3.4.2. 将子类构造函数的 prototype 属性指向父类构造函数的 prototype 属性

这种方式是对上一种方式的改进。

Animal 对象中，可以将不变的属性都直接写入 Animal.prototype。因此，我们可以直接让 Cat() 跳过 Animal()，直接继承 Animal.prototype。

```javascript
function Animal(){ }
Animal.prototype.species = "动物";

Cat.prototype = Animal.prototype;// 将子类构造函数的 prototype 属性指向父类构造函数的 prototype 属性
Cat.prototype.constructor = Cat;// 存在问题：实际上把 Animal.prototype 对象的 constructor 属性也改掉了！
var cat1 = new Cat("大毛","黄色");
alert(cat1.species); // 动物
```

优点是效率比较高（不用执行和建立 Animal 的实例了），比较省内存。

缺点是 Cat.prototype 和 Animal.prototype 现在指向了同一个对象，那么任何对 Cat.prototype 的修改，都会反映到 Animal.prototype。这导致了**这种方式不可被采用**。

#### 3.4.3. 利用空对象作为中介

这种方式是前两种方式的结合，同时克服了前两种方式的缺点。

```javascript
var F = function(){};
F.prototype = Animal.prototype;
Cat.prototype = new F();//F 是空对象，所以几乎不占内存，基本上不用考虑效率问题
Cat.prototype.constructor = Cat;
// 修改 Cat 的 prototype 对象，就不会影响到 Animal 的 prototype 对象
alert(Animal.prototype.constructor); // Animal
```

封装上述操作：
```javascript
function extend(Child, Parent) {
　　var F = function(){};
　　F.prototype = Parent.prototype;
　　Child.prototype = new F();
　　Child.prototype.constructor = Child;
　　// 为子对象设一个 uber 属性，这个属性直接指向父对象的 prototype 属性。（uber 是一个德语词，意思是"向上"、"上一层"。）这等于在子对象上打开一条通道，可以直接调用父对象的方法。
    Child.uber = Parent.prototype;
}

// 使用
extend(Cat,Animal);
var cat1 = new Cat("大毛","黄色");
alert(cat1.species); // 动物
```

### 3.5. 基于拷贝的继承

思路：把父对象的所有属性和方法，拷贝进子对象，从而实现继承的效果。

```javascript
function Animal(){}
Animal.prototype.species = "动物";

// 将父对象的 prototype 对象中的属性，一一拷贝给 Child 对象的 prototype 对象
function extend2(Child, Parent) {
　　var p = Parent.prototype;
　　var c = Child.prototype;
　　for (var i in p) {
　　　　c[i] = p[i];
　　　　}
　　c.uber = p;
}

// 使用
extend2(Cat, Animal);
var cat1 = new Cat("大毛","黄色");
alert(cat1.species); // 动物
```

### 3.6. 使用 Object.create 实现继承

ECMAScript 5 的新方法 Object.create() 可以在创建对象时指定新对象的原型：
```javascript
var a = {a: 1}; 
// a ---> Object.prototype ---> null

var b = Object.create(a);
// b ---> a ---> Object.prototype ---> null
console.log(b.a); // 1 （继承而来)

var c = Object.create(b);
// c ---> b ---> a ---> Object.prototype ---> null

var d = Object.create(null);
// d ---> null
console.log(d.hasOwnProperty); // undefined, 因为 d 没有继承 Object.prototype
```

### 3.7. 使用 class 关键字实现继承

ECMAScript6 引入了一套新的关键字用来实现 class，包括 class, constructor，static，extends 和 super，可以通过这些语法糖实现继承：

```javascript
"use strict";

class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}

class Square extends Polygon {
  constructor(sideLength) {
    super(sideLength, sideLength);
  }
  get area() {
    return this.height * this.width;
  }
  set sideLength(newLength) {
    this.height = newLength;
    this.width = newLength;
  }
}

var square = new Square(2);
```

### 3.8. NOTE

- 要注意代码中的原型链的长度，并在必要时将其分解，以避免潜在的性能问题。
  
  在原型链上查找属性比较耗时，对性能有副作用，这在性能要求苛刻的情况下很重要。另外，试图访问不存在的属性时会遍历整个原型链。

  当你执行：
  ```javscript
  o.someProp;
  ```
  它会检查 o 是否具有 someProp 属性。如果没有，它会查找 Object.getPrototypeOf(o).someProp，如果仍旧没有，它会继续查找 Object.getPrototypeOf(Object.getPrototypeOf(o)).someProp。直到在原型链上找到 someProp 属性或者`Object.getPrototypeOf(xxxx) === null`。

- 永远不要扩展原生对象的原型 (eg: Object.prototype)，除非是为了兼容新的 JavaScript 特性 (eg: Array.forEach).

  这种技术被称为猴子补丁并且会破坏封装。尽管一些流行的框架（如 Prototype.js）在使用该技术，但仍然没有足够好的理由使用附加的非标准方法来混入内置原型。

## 4. Refer Links

[MDN Web Docs - 继承与原型链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

[阮一峰 - Javascript 面向对象编程（一）](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_encapsulation.html)

[阮一峰 - Javascript 面向对象编程（二）](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)

[阮一峰 - Javascript 面向对象编程（三）](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html)

[阮一峰 - Javascript 继承机制的设计思想](http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html )

[重新理解 JS 的 6 种继承方式](http://www.ayqy.net/blog/%E9%87%8D%E6%96%B0%E7%90%86%E8%A7%A3js%E7%9A%846%E7%A7%8D%E7%BB%A7%E6%89%BF%E6%96%B9%E5%BC%8F/)


<!-- TODO: 三个视频都很重要，都要详细看然后记笔记 
http://www.imooc.com/video/7053

http://www.imooc.com/video/7054

http://www.imooc.com/video/7055

http://www.imooc.com/video/7680-->
