- [CSS 动画](#css-%E5%8A%A8%E7%94%BB)
  - [1. CSS Transition](#1-css-transition)
    - [1.1. transition](#11-transition)
    - [1.2. transition-delay](#12-transition-delay)
    - [1.3. transition-timing-function](#13-transition-timing-function)
      - [1.3.1. cubic-bezier(x, y, x, y)](#131-cubic-bezierx-y-x-y)
      - [1.3.2. steps()](#132-steps)
    - [1.4. NOTE](#14-note)
  - [2. CSS Animation](#2-css-animation)
    - [2.1. @keyframes](#21-keyframes)
    - [2.2. animation](#22-animation)
      - [2.2.1. animation-fill-mode](#221-animation-fill-mode)
    - [2.3. 实例](#23-%E5%AE%9E%E4%BE%8B)
  - [3. Browser Compatibility](#3-browser-compatibility)
  - [4. CSS 动画实践](#4-css-%E5%8A%A8%E7%94%BB%E5%AE%9E%E8%B7%B5)
    - [4.1. 实践例子](#41-%E5%AE%9E%E8%B7%B5%E4%BE%8B%E5%AD%90)
    - [4.2. CSS 动画于 JavaScript 动画的比较](#42-css-%E5%8A%A8%E7%94%BB%E4%BA%8E-javascript-%E5%8A%A8%E7%94%BB%E7%9A%84%E6%AF%94%E8%BE%83)
  - [5. Refer Links](#5-refer-links)

# CSS 动画

动画是使元素从一种样式逐渐变化为另一种样式的效果。

## 1. CSS Transition

Transitions：用于**实现简单的动画，只有起始两帧过渡**。**多用于页面的交互操作**，使交互效果更生动活泼。

Transition 的变换基于连续变化的属性，例如，transition 对 opacity（取连续值）有效，而对 display（取离散值）无效。

- 哪些属性适合做动画？

  不是所有的 CSS 属性都适合添加动画，原因在于性能问题。因此，动画改变的属性应该只引起浏览器重绘而不会引起重排。例如，opacity 不改变布局，只引起浏览器重绘，因此适合添加动画。而 font-size、padding、margin 等属性，由于改变了布局，将会引起浏览器的重新布局，不适合添加动画。


### 1.1. transition

语法：
> transition: property duration timing-function delay;

transition 属性是一个简写属性，用于设置四个过渡属性：transition-property、transition-duration、transition-timing-function、transition-delay。

例：
```css
img{
  transition: 1s 1s height ease;
}
```
可以单独定义成各个属性：
```css
img{
  transition-property: height;
  transition-duration: 1s;
  transition-delay: 1s;
  transition-timing-function: ease;
}
```

- 可以指定 transition 适用的属性，比如只适用于 height：
  ```css
  img{
    transition: 1s height;
  }
  ```
  这样，只有 height 的变化需要 1 秒实现，其他变化（主要是 width）依然瞬间实现。

- 可以同时指定多个属性，**用逗号分隔**:
  ```css
  img{
    transition: 1s height, 1s width;
  }
  ```
  这样，height 和 width 的变化是同时进行的（效果跟不指定它们没有差别）

- [在变化某个目标属性时，前提是浏览器已经得知这个属性需要加动画，否则动画无法执行](https://www.zhihu.com/question/36375965)。

### 1.2. transition-delay

transition-delay 属性规定在过渡效果开始之前需要等待的时间，以秒或毫秒计。默认值为 0

例：
```css
img {
  transition: 1s;
  transition-delay: 1s;
}
```
上边代码产生在过渡效果开始前等待 1 秒的效果。

可通过 transition-delay，让 height 先发生变化，等结束以后，再让 width 发生变化：
```css
img{
  transition: 1s height, 1s 1s width;
}
```
上边代码指定，width 在 1 秒之后，再开始变化，也就是延迟（delay）1 秒。

### 1.3. transition-timing-function

transition-timing-function 属性规定过渡效果的速度曲线，即过渡动画的速度。默认值是 ease。

语法：
> transition-timing-function: linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier(n,n,n,n);

| 值                             | 描述                                       |
| ----------------------------- | ---------------------------------------- |
| linear                        | 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。 |
| ease(DEFAULT)                 | 规定慢速开始，然后变快，然后慢速结束的过渡效果（等于 cubic-bezier(0.25,0.1,0.25,1)）。 |
| ease-in                       | 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。 |
| ease-out                      | 规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。 |
| ease-in-out                   | 规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。 |
| cubic-bezier(*n*,*n*,*n*,*n*) | 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。 可以使用[工具网站](http://cubic-bezier.com/) 定制。|

例：
```css
img{
  transition: 1s height;
  transition-time-function: cubic-bezier(.83,.97,.05,1.44);
}
```
可简写为：
```css
img{
  transition: 1s height cubic-bezier(.83,.97,.05,1.44);
}
```

#### 1.3.1. cubic-bezier(x, y, x, y)

http://joveyzheng.com/2016/03/16/css-cubic-bezier/

timing-function 属性的取值中，支持使用 cubic-bezier 函数，自定义[三次贝塞尔曲线](http://cubic-bezier.com)-- 即速度变化曲线：

![image](http://img.cdn.firejq.com/jpg/2017/11/4/de8db216df9f16f33170b67a94403338.jpg)

- 横轴表示时间轴，纵轴表示速度大小；

- 坐标
  - P0：默认值 (0, 0)
  - P1：动态取值 (x1, y1)
  - P2：动态取值 (x2, y2)
  - P3：默认值 (1, 1)

  需要关注的是 P1 和 P2 两点的取值，而其中 X 轴的取值范围是 0 到 1，当取值超出范围时 cubic-bezier 将失效；Y 轴的取值没有规定，当然也毋须过大。

  最直接的理解是，将以一条直线放在范围只有 1 的坐标轴中，并从中间拿出两个点来拉扯（X 轴的取值区间是 [0, 1]，Y 轴任意），最后形成的曲线就是动画的速度曲线。

#### 1.3.2. steps()

https://idiotwu.me/understanding-css3-timing-function-steps/

timing-function 属性的取值中，还支持使用 steps() 函数。

steps 函数指定了一个阶跃函数，第一个参数指定了时间函数中的间隔数量（必须是正整数，可使用[动画帧数计算器](http://tid.tenpay.com/labs/css3_keyframes_calculator.html) 计算），即把动画分为 n 步阶段性展示；第二个参数可选，接受 start 和 end 两个值，指定在每个间隔的起点或是终点发生阶跃变化，默认为 end。

例：
- steps(3, start)

  ![image](http://img.cdn.firejq.com/jpg/2017/11/4/fe52b0a39db06512e4c1e5733846d241.jpg)

  steps() 第一个参数将动画分割成三段。当指定跃点为 start 时，动画在每个计时周期的起点发生阶跃（即图中空心圆 → 实心圆）。由于第一次阶跃发生在第一个计时周期的起点处（0s），所以我们看到的第一步动画（初态）就为 1/3 的状态，因此在视觉上动画的过程为 1/3 → 2/3 → 1。

- steps(3, end)

  ![image](http://img.cdn.firejq.com/jpg/2017/11/4/ec5d83197347b743a264dbc5a2583e4c.jpg)

  当指定跃点为 end，动画则在每个计时周期的终点发生阶跃（即图中空心圆 → 实心圆）。由于第一次阶跃发生在第一个计时周期结束时（1s），所以我们看到的初态为 0% 的状态；而在整个动画周期完成处（3s），虽然发生阶跃跳到了 100% 的状态，但同时动画结束，所以 100% 的状态不可视。因此在视觉上动画的过程为 0 → 1/3 → 2/3（回忆一下数电里的异步清零，当所有输出端都为高电平的时候触发清零，所以全为高电平是暂态）。

### 1.4. NOTE

- 目前，各大浏览器（包括 IE 10）都已经支持无前缀的 transition，所以 transition 已经可以很安全地不加浏览器前缀

- 不是所有的 CSS 属性都支持 transition，完整的列表查看[这里](http://oli.jp/2010/css-animatable-properties/)，以及具体的[效果](http://leaverou.github.io/animatable/)

- transition 需要明确知道，开始状态和结束状态的具体数值，才能计算出中间状态。比如，height 从 0px 变化到 100px，transition 可以算出中间状态。但是，transition 没法算出 0px 到 auto 的中间状态，也就是说，如果开始或结束的设置是 height: auto，那么就不会产生动画效果。类似的情况还有，display: none 到 block，background: url(foo.jpg) 到 url(bar.jpg) 等等。

- transition 需要事件触发，所以没法在网页加载时自动发生。

- transition 是一次性的，不能重复发生，除非一再触发。

- transition 只能定义开始状态和结束状态，不能定义中间状态，也就是说只有两个状态。

- 一条 transition 规则，只能定义一个属性的变化，不能涉及多个属性。

## 2. CSS Animation

针对 transition 的多个局限性，CSS 提出了 Animation。

Keyframes animation：用于**实现较为复杂的动画**，**一般关键帧较多**。

### 2.1. @keyframes

@keyframes 用于定义动画的各个关键帧的状态。

创建动画的原理是，将一套 CSS 样式逐渐变化为另一套样式。在动画过程中，开发者能够多次改变这套 CSS 样式。

- 语法：

  > @keyframes animationname {keyframes-selector {css-styles;}}

  - animationname 表示定义动画的名称。

  - keyframes-selector 表示动画时长的百分比，以百分比定义，可以使用关键词 "from" 和 "to"-- 等价于 0% 和 100%（0% 是动画的开始时间，100% 动画的结束时间）。

    如果省略某个状态，浏览器会自动推算中间状态。因此可通过增加关键帧状态，来使动画变化更加丰富。

    注：为了获得最佳的浏览器支持，应始终定义 0% 和 100% 选择器。

  - css-styles 为一个或多个合法的 CSS 样式属性。

  例：
  ```css
  @keyframes mymove {
    0%   {top:0px; left:0px; background:red;}
    25%  {top:0px; left:100px; background:blue;}
    50%  {top:100px; left:100px; background:yellow;}
    75%  {top:100px; left:0px; background:green;}
    100% {top:0px; left:0px; background:red;}
  }
  ```

- @keyframes 定义中，可把多个相同的关键帧状态写在一行：
  ```css
  @keyframes pound {
    from，to { transform: none; }
    50% { transform: scale(1.2); }
  }
  ```

- 浏览器从一个状态向另一个状态过渡，是平滑过渡。使用 steps 函数可以实现分步过渡：

  例：
  ```css
  div:hover {
    animation: 1s rainbow infinite steps(10);
  }
  ```
  例：[打字机动画](http://dabblet.com/gist/1745856)

### 2.2. animation

语法：

> animation: name duration timing-function delay iteration-count direction;

animation 是除了 animation-play-state 属性，所有动画属性的简写属性，具体包括：

| 属性                                       | 描述                             |
| ---------------------------------------- | ------------------------------ |
| [animation-name](http://www.w3school.com.cn/cssref/pr_animation-name.asp) | 规定 @keyframes 动画的名称。 设置为 none 规定无动画效果（可用于覆盖来自级联的动画）          |
| [animation-duration](http://www.w3school.com.cn/cssref/pr_animation-duration.asp) | 规定动画完成一个周期所花费的秒或毫秒。默认是 0，意味着没有动画效果。    |
| [animation-timing-function](http://www.w3school.com.cn/cssref/pr_animation-timing-function.asp) | 规定动画的速度曲线。默认是 "ease"。          |
| [animation-delay](http://www.w3school.com.cn/cssref/pr_animation-delay.asp) | 规定动画何时开始。默认是 0。                |
| [animation-iteration-count](http://www.w3school.com.cn/cssref/pr_animation-iteration-count.asp) | 规定动画被播放的次数。默认是 1。设置为 infinite 表示无限次播放。              |
| [animation-direction](http://www.w3school.com.cn/cssref/pr_animation-direction.asp) | 规定动画是否在下一周期逆向地播放。默认是 "normal"，即动画正常播放。设置为 alternate，则动画会在奇数次数（1、3、5 等等）正常播放，而在偶数次数（2、4、6 等等）向后播放。 |
| [animation-play-state](http://www.w3school.com.cn/cssref/pr_animation-play-state.asp) | 规定动画是否正在运行或暂停。默认是 "running"，表示动画正在播放。设置为 paused，表示动画已暂停播放。在 JavaScript 中使用该属性，可以在播放过程中暂停动画。   |
| [animation-fill-mode](http://www.w3school.com.cn/cssref/pr_animation-fill-mode.asp) | 规定动画在播放之前或之后，其动画效果是否可见，其属性值是由逗号分隔的一个或多个填充模式关键词。                 |

例：
```css
div:hover {
  animation: 1s 1s rainbow linear 3 forwards normal;
}
```
可以分解成各个单独的属性：
```css
div:hover {
  animation-name: rainbow;
  animation-duration: 1s;
  animation-timing-function: linear;
  animation-delay: 1s;
  animation-fill-mode: forwards;
  animation-direction: normal;
  animation-iteration-count: 3;
}
```

#### 2.2.1. animation-fill-mode

animation-fill-mode 属性规定动画在播放之前或之后，其动画效果是否可见。

动画结束以后，会立即从结束状态跳回到起始状态。如果想让动画保持在结束状态，就需要使用 animation-fill-mode 属性。

| 值         | 描述                                       |
| --------- | ---------------------------------------- |
| none      | 不改变默认行为，即动画结束后回到动画还没开始的状态。                                 |
| forwards  | 当动画完成后，让动画停留在结束状态。           |
| backwards | 动画结束后，让动画回到第一帧的状态。 |
| both      | 根据 animation-direction（见后）轮流应用 forwards 和 backwards 规则。                           |

### 2.3. 实例

- [心脏跳动](http://lea.verou.me/css-4d/#heart-demo)
  ```css
  @keyframes pound {
    from { transform: none; }
    to { transform: scale(1.2); }
  }

  .heart {
    animation: pound .3s infinite;
  }
  ```

- [打字机动画](http://dabblet.com/gist/1745856)
  ```css
  /**
  * Typing animation with pure CSS.
  * Works best in browsers supporting the ch unit.
  */

  @keyframes typing { from { width: 0; } }
  @keyframes blink-caret { 50% { border-color: transparent; } }

  h1 {
    font: bold 200% Consolas, Monaco, monospace;
    border-right: .1em solid;
    width: 16.5em; /* fallback */
    width: 30ch; /* # of chars */
    margin: 2em 1em;
    white-space: nowrap;
    overflow: hidden;
    animation: typing 20s steps(30, end), /* # of steps = # of chars */
              blink-caret .5s step-end infinite alternate;
  }
  ```

## 3. Browser Compatibility

目前：

- IE 10 和 Firefox（>= 16）支持没有前缀的 animation，而 chrome 不支持，所以必须使用 webkit 前缀：
  例：
  ```css
  animation: mymove 5s linear 2s infinite alternate;
  /* Firefox: */
  -moz-animation: mymove 5s linear 2s infinite alternate;
  /* Safari and Chrome: */
  -webkit-animation: mymove 5s linear 2s infinite alternate;
  /* Opera: */
  -o-animation: mymove 5s linear 2s infinite alternate;
  ```

- 所有浏览器都不支持 @keyframes 规则，Firefox 支持替代的 @-moz-keyframes 规则，Opera 支持替代的 @-o-keyframes 规则，Safari 和 Chrome 支持替代的 @-webkit-keyframes 规则：
  例：
  ```css
  @keyframes mymove
  {
  from {top:0px;}
  to {top:200px;}
  }

  @-moz-keyframes mymove /* Firefox */
  {
  from {top:0px;}
  to {top:200px;}
  }

  @-webkit-keyframes mymove /* Safari 和 Chrome */
  {
  from {top:0px;}
  to {top:200px;}
  }

  @-o-keyframes mymove /* Opera */
  {
  from {top:0px;}
  to {top:200px;}
  }
  ```

## 4. CSS 动画实践

需求中常见的 css3 动画一般有**补间动画 / 关键帧动画**和**逐帧动画**两种：

- 补间动画 / 关键帧动画

  常用于实现位移、颜色（透明度）、大小、旋转、倾斜等变化。一般有 Transitions 和 Keyframes animation 两种方法实现补间动画（平滑动画）。

- 逐帧动画

  当 animation 的 timing-function 使用 steps 时，可实现没有补间（平滑过度），只有关键帧之间的跳跃的动画效果。

  逐帧动画可用于 loading 动画，但更多的用于 Sprite 精灵动画（人物运动）。精灵动画把所有帧都放在一起，通过 CSS3 的 animation 控制 background-position。

使用 CSS 实现动画效果的优缺点：
- 优点：
  - 简单、高效
  - 声明式的
  - 不依赖于主线程，采用硬件加速（GPU）
  - 简单的控制 keyframe animation 播放和暂停

- 缺点：
  - 不能动态修改或定义动画内容
  - 不同的动画无法实现同步
  - 多个动画彼此无法堆叠

### 4.1. 实践例子

https://aotu.io/notes/2016/01/04/css3-animation/index.html

https://aotu.io/notes/2016/05/06/Guide-To-Tween-Animation/index.html

https://webdesign.tutsplus.com/tutorials/a-beginners-introduction-to-css-animation--cms-21068

### 4.2. CSS 动画于 JavaScript 动画的比较

[Javascript 可以比 CSS transition 性能更好](http://zencode.in/19.CSS-vs-JS%E5%8A%A8%E7%94%BB%EF%BC%9A%E8%B0%81%E6%9B%B4%E5%BF%AB%EF%BC%9F.html)

## 5. Refer Links

http://www.w3school.com.cn/css3/css3_transition.asp

http://www.w3school.com.cn/css3/css3_animation.asp

http://www.ruanyifeng.com/blog/2014/02/css_transition_and_animation.html

https://idiotwu.me/understanding-css3-timing-function-steps/

[CSS3 热身实战——过渡与动画（实现炫酷下拉，手风琴，无缝滚动）](http://web.jobbole.com/91973/)