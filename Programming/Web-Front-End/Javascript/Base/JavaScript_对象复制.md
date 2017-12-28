- [JavaScript 对象复制](#javascript-%E5%AF%B9%E8%B1%A1%E5%A4%8D%E5%88%B6)
  - [浅复制](#%E6%B5%85%E5%A4%8D%E5%88%B6)
  - [深复制](#%E6%B7%B1%E5%A4%8D%E5%88%B6)
  - [Refer Links](#refer-links)

# JavaScript 对象复制

JavaScript 中对象、数组、函数都是通过引用传递的，因此如果只是单纯的使用`=`赋值，仅仅是将对象的地址进行引用，并不是值的复制，修改一个对象的值会影响另一个对象的值。因此，若需要进行对象的复制，需要我们自己实现。

## 浅复制

所谓浅复制，指的是对对象中的每一项进行复制，但不对对象中的子对象的属性进行复制，而是直接引用该子对象。

实现：
```javascript
var shallowClone = function (obj) {
  if(typeof obj !== 'object') {
    return;
  } else {
    var newObj = {};
    for (var i in obj) {
      if (obj.hasOwnProperty(i)) {// 不复制原型链上的属性
        newObj[i] = obj[i];
      }
    }
    return newObj;
  }
};
```

测试：
```javascript
var person = {
  name: "firejq",
  info: {
    skill: 'code'
  }
};
var shallowPerson = clone(person);

person.name = "jq";// 只会改变 person.name，不会影响 shallowPerson.name
console.log(person);
console.log(shallowPerson);

person.info.skill = "say"; // 会影响 person 和 shallowPerson
console.log(person);
console.log(shallowPerson);
```

因为浅复制只会将对象的各个属性进行依次复制，并不会进行递归复制，而 JavaScript 存储对象都是存地址的，所以**浅复制会导致对象中的子对象，即 person.info 和 shallowPerson.info 指向同一块内存地址**，修改其中一项就会改变另外一项的值。

## 深复制

所谓深复制，指的是对对象的每一项进行复制，包括对象中的对象的每一项属性，都会被递归复制到新的对象上。这就不会存在浅复制中新旧对象的子对象指向同一个对象的问题。

需要注意的是，如果对象比较大，层级也比较多，深复制会带来性能上的问题。在遇到需要采用深复制的场景时，可以考虑有没有其他替代的方案。在实际的应用场景中，也是浅复制更为常用。

- 通过 JSON.parse(),JSON.stringfy() 实现

  ```javascript
  var deepClone = function (obj) {
    if (typeof obj !== 'object') {
      return;
    } else {
      return JSON.parse(JSON.stringify(obj));
    }
  }
  ```


- 通过递归实现

  ```javascript
  var deepClone = function (obj) {
    if (typeof obj !== 'object') {
      return;
    } else {
      var newobj = obj instanceof Array ? [] : {};// 判断是数组还是对象
      for(var i in obj) {
        newobj[i] = obj[i] instanceof Object ? arguments.callee(obj[i]) : obj[i];
      }
      return o;
    }

  }
  ```

- 整合实现方案

  ```javascript
  var deepClone = function(obj) {
    if(typeof obj !== 'object') {
      return;
    } else {
      var newobj = obj.constructor === Array ? [] : {};// 判断是数组还是对象
      if(window.JSON) {// 若支持 JSON 转换，采用 JSON 实现
        newobj = JSON.parse(JSON.stringify(obj)); // 序列化对象后再还原
      } else {// 若不支持 JSON 转换，采用递归实现
        for(var i in obj){
          newobj[i] = typeof obj[i] === 'object' ? deepClone(obj[i]) : obj[i]; 
        }
      }
      return newobj;
    }
  };
  ```

## Refer Links

https://www.zhihu.com/question/23031215

http://jerryzou.com/posts/dive-into-deep-clone-in-javascript/
