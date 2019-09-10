- [CSS overflow 相关](#css-overflow-%E7%9B%B8%E5%85%B3)
  - [1. overflow 基本属性值](#1-overflow-%E5%9F%BA%E6%9C%AC%E5%B1%9E%E6%80%A7%E5%80%BC)
  - [2. overflow-x 与 overflow-y](#2-overflow-x-%E4%B8%8E-overflow-y)
  - [3. overflow 与滚动条](#3-overflow-%E4%B8%8E%E6%BB%9A%E5%8A%A8%E6%9D%A1)
  - [4. overflow 与 BFC](#4-overflow-%E4%B8%8E-bfc)
  - [5. overflow 与 absolute 绝对定位](#5-overflow-%E4%B8%8E-absolute-%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D)
  - [6. 依赖于 overflow 的样式属性](#6-%E4%BE%9D%E8%B5%96%E4%BA%8E-overflow-%E7%9A%84%E6%A0%B7%E5%BC%8F%E5%B1%9E%E6%80%A7)
    - [6.1. resize](#61-resize)
    - [6.2. text-overflow: ellipsis](#62-text-overflow-ellipsis)
  - [7. Refer Links](#7-refer-links)

# CSS overflow 相关

## 1. overflow 基本属性值

- visible(default)
- hidden
- scroll
- auto
- inherit

## 2. overflow-x 与 overflow-y

若 overflow-x 与 overflow-y 值相同，则等同于 overflow；

若 overflow-x 与 overflow-y 值不同，且其中一个为 visible，另一个为 hidden/scroll/auto，则值为 visible 的 overflow-*的值会被强制重置为 auto；

因此，若要在 x 或 y 一个方向上设置 overflow 的值，需要将另一个方向上的 overflow 赋为 hidden/scroll/auto。

## 3. overflow 与滚动条

![image](http://img.cdn.firejq.com/jpg/2017/11/30/d20c4229a8e1aada9c6cc8ccb314a4fe.jpg)

- 滚动条出现的条件：
  - `overflow:auto/scroll;`
  - 内容尺寸大于容器尺寸，即“溢出”。

- 任何浏览器中，默认滚动条都来自于`<html>`而不是`<body>`

  ![image](http://img.cdn.firejq.com/jpg/2017/11/30/a184f34b974d48b4418572aeb992d8bf.jpg)

- JavaScript 获取滚动高度

  - Chrome：`document.documentElement.scrollTop`
  - 其它浏览器：`document.body.scrollTop`

  因此，可采用兼容写法：
  ```javascript
  var st = document.documentElement.scrollTop || document.body.scrollTop;
  ```

- 滚动条的宽度机制：滚动条会占用容器的可用宽度或可用高度。

  获取滚动条宽度：
  ```html
  <div class="box" style="width:400px;">
    <div id="in" class="in"></div>
  </div>
  ```
  ```javascript
  var wd = 400 - document.getElementById("in").clientWidth;
  ```
  在 IE7+/Chrome/FireFox 中，滚动条默认为 17px。

- `overflow:auto`的布局隐患：

  当出现滚动条时，滚动条会占用容器尺寸，导致原本的布局发发生改变；

  因此，实现该类布局时，应尽量采用自适应布局，或者预留滚动条的宽度。

- 垂直滚动条出现时占位导致的水平居中跳动问题解决
  ```css
  .container {
    width: 1150px;
    margin: 0 auto;
  }
  ```
  解决方案一：
  ```css
  .container {
    width: 1200px;
    padding-left: calc(100vw - 100%);
  }
  ```
  其中：100vw 即浏览器宽度，100% 即可用内容的宽度，为 padding-left 赋值后，无论滚动条有没有出现，容器的左、右都会有滚动条宽度的偏移，因此永远都是水平居中的，不会产生任何跳动。

  解决方案二：这种方法将滚动条始终显示，不美观，只适合 IE7- 等不支持方案一的浏览器使用
  ```css
  html {overflow-y: scroll;}
  ```

## 4. overflow 与 BFC

https://www.imooc.com/video/6459

当 overflow 属性值为 auto/scroll/hidden 时，会在元素上创建一个 BFC。

应用：
- 清除浮动
- 避免 margin 穿透问题
- 两栏自适应布局

## 5. overflow 与 absolute 绝对定位

https://www.imooc.com/video/6484

当元素正好在 absolute 绝对定位元素与包含块之间时，`overflow:hidden;`会失效：

```html
<body>
  <div style="overflow:hidden;">
    <img style="position:absolute;" src="xxx">
  </div>
</body>
```
此时，body 即为包含块，overflow 的元素正好位于 absolute 元素与包含块元素之间，因此，图片从容器中溢出而不是 hidden：
![image](http://img.cdn.firejq.com/jpg/2017/11/30/ecf4ede31b1ae9c9e5f57988b3b7ac97.jpg)

![image](http://img.cdn.firejq.com/jpg/2017/11/30/0a5bb735749f240a0e4c9dd1d26b044c.jpg)

解决方法：
- 将 overflow 元素自身设置为包含块
- 将 overflow 元素的子元素设置为包含块
- 任意合法 transform 声明当作包含块

## 6. 依赖于 overflow 的样式属性

### 6.1. resize

resize 属性使得元素尺寸可被拉伸，但要该属性生效，元素的 overflow 值不可是 visible。

### 6.2. text-overflow: ellipsis

`text-overflow: ellipsis`可使文本超过容器限制后以省略号表示，但要该属性生效，必须设置元素`overflow:hidden;`，才能生效。

## 7. Refer Links

https://www.imooc.com/learn/256