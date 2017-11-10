- [CSS 计数器](#css-%E8%AE%A1%E6%95%B0%E5%99%A8)
  - [相关属性](#%E7%9B%B8%E5%85%B3%E5%B1%9E%E6%80%A7)
    - [counter-reset](#counter-reset)
    - [counter-increment](#counter-increment)
  - [应用实例](#%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B)
    - [使用单个计数器](#%E4%BD%BF%E7%94%A8%E5%8D%95%E4%B8%AA%E8%AE%A1%E6%95%B0%E5%99%A8)
    - [使用嵌套计数器](#%E4%BD%BF%E7%94%A8%E5%B5%8C%E5%A5%97%E8%AE%A1%E6%95%B0%E5%99%A8)
    - [复选框计数器](#%E5%A4%8D%E9%80%89%E6%A1%86%E8%AE%A1%E6%95%B0%E5%99%A8)
  - [Refer Links](#refer-links)

# CSS 计数器

CSS 计数器是 CSS2.1 中自动计数编号部分的实现。

## 相关属性

CSS 计数器的值通过使用 counter-reset 和 counter-increment 操作，在 content 上应用 counter() 或 counters() 函数来显示在页面上。

### counter-reset

counter-reset 属性设置某个选择器出现次数的计数器的值。默认为 0。

利用这个属性，计数器可以设置或重置为任何值，可以是正值或负值。如果没有提供 number，则默认为 0。

NOTE：如果使用 "display: none"，则无法重置计数器。如果使用 "visibility: hidden"，则可以重置计数器。

### counter-increment

counter-increment 属性设置某个选取器每次出现的计数器增量。默认增量是 1。

利用这个属性，计数器可以递增（或递减）某个值，这可以是正值或负值。如果没有提供 number 值，则默认为 1。

NOTE：如果使用了 "display: none"，则无法增加计数。如使用 "visibility: hidden"，则可增加计数。

## 应用实例

### 使用单个计数器

使用 CSS 计数器之前，必须重置一个值，默认是 0。

使用 **counter()** 函数来给元素增加计数器。 

```css
body {
  counter-reset: section;           /* 重置计数器成 0 */
}
h3:before {
  counter-increment: section;      /* 增加计数器值 */
  content: "Section " counter(section) ": "; /* 显示计数器 */
}
```

```html
<h3>Introduction</h3>
<h3>Body</h3>
<h3>Conclusion</h3>
```

效果：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/7/6ee47ba39e035e695eafdbff7b699c4d.jpg)

### 使用嵌套计数器

CSS 计数器对创建有序列表特别有用，因为在子元素中会自动创建一个 CSS 计数器的实例。使用 **counters()** 函数，在不同级别的嵌套计数器之间可以插入字符串。

```css
ol {
  counter-reset: section;                /* 为每个 ol 元素创建新的计数器实例 */
  list-style-type: none;
}
li:before {
  counter-increment: section;            /* 只增加计数器的当前实例 */
  content: counters(section, ".") " ";   /* 为所有计数器实例增加以“.”分隔的值 */
}
```

```html
<ol>
  <li>item</li>          <!-- 1     -->
  <li>item               <!-- 2     -->
    <ol>
      <li>item</li>      <!-- 2.1   -->
      <li>item</li>      <!-- 2.2   -->
      <li>item           <!-- 2.3   -->
        <ol>
          <li>item</li>  <!-- 2.3.1 -->
          <li>item</li>  <!-- 2.3.2 -->
        </ol>
        <ol>
          <li>item</li>  <!-- 2.3.1 -->
          <li>item</li>  <!-- 2.3.2 -->
          <li>item</li>  <!-- 2.3.3 -->
        </ol>
      </li>
      <li>item</li>      <!-- 2.4   -->
    </ol>
  </li>
  <li>item</li>          <!-- 3     -->
  <li>item</li>          <!-- 4     -->
</ol>
<ol>
  <li>item</li>          <!-- 1     -->
  <li>item</li>          <!-- 2     -->
</ol>
```

效果：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/7/73f2b13765a45f963ce60bf93da5940d.jpg)

### 复选框计数器

动态计算 checkbox 选中的数量，一般用 JavaScript 实现，但利用 CSS 的伪元素（结合：checked 和 counter、before/after）可以更简洁的实现：


```css
.choose {
  counter-reset: fruit;
}
.choose input:checked {
  counter-increment: fruit;
}
.cout:before {
  content: counter(fruit);
}
```

```html
<div class="choose">
  <input type="checkbox">苹果
  <input type="checkbox">香蕉
  <input type="checkbox">梨子
  <input type="checkbox">葡萄
  <input type="checkbox">西瓜
  <input type="checkbox">椰子
</div>
<p>你选中了<span class="count"></span>种水果</p>
```

效果：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/7/7e8747f62ac52ea671b89d78f9def810.jpg)


## Refer Links

https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Counters

http://www.w3school.com.cn/cssref/pr_gen_counter-increment.asp

http://www.w3school.com.cn/cssref/pr_gen_counter-reset.asp

https://juejin.im/post/5a0029a45188254dd935cc40?utm_medium=fe&utm_source=weixinqun
