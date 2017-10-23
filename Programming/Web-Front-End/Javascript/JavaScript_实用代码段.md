- [JavaScript 实用代码段](#javascript-%E5%AE%9E%E7%94%A8%E4%BB%A3%E7%A0%81%E6%AE%B5)
  - [防止网页被 iframe 引用](#%E9%98%B2%E6%AD%A2%E7%BD%91%E9%A1%B5%E8%A2%AB-iframe-%E5%BC%95%E7%94%A8)
  - [页面跳转](#%E9%A1%B5%E9%9D%A2%E8%B7%B3%E8%BD%AC)
  - [使 input 输入框不能编辑](#%E4%BD%BF-input-%E8%BE%93%E5%85%A5%E6%A1%86%E4%B8%8D%E8%83%BD%E7%BC%96%E8%BE%91)

# JavaScript 实用代码段

## 防止网页被 iframe 引用

http://www.ruanyifeng.com/blog/2008/10/anti-frameset_javascript_codes.html

http://www.ruanyifeng.com/blog/2010/08/anti-frameset_javascript_codes_continued.html

使用 iframe 可以将别的网站嵌入当前页面中。
1. 为什么反对这种做法？
　　1）故意屏蔽了被嵌入网页的网址，侵犯了原作者的著作权，以及访问者的知情权；
　　2）大量业者使用的是不可见框架，使得框架网页与被嵌入的网页视觉上完全相同，欺骗性极高；
　　3）不良业者在被嵌入网页的上方或周围附加广告（甚至病毒和木马），不仅破坏原作者的设计意图和形象，而且属于侵权利用他人资源的谋利行为；
　　4）如果访问者在框架内部，从一个网页点击到另一个网页，浏览器的地址栏是不变的，这是很差的用户体验，并且访问者会将这种体验归咎于原网页的作者。
2. 如果确有必要，将他人的网页嵌入自己的框架，那么应该同时满足以下三个条件：（即参考 Google 的做法）
　　A. 在框架网页的醒目位置，清楚地说明该网页使用了框架技术，并明确列出原网页的 URL 网址。
　　B. 在框架网页的醒目位置，向访问者提供"移除框架"的功能。
　　C. 不得附加任何广告或恶意代码。

3. 防止网页被嵌入的 JavaScript 代码：
```javascript
// 只要将其放入网页源码的头部，那些流氓就没有办法使用你的网页了
try{
　　top.location.hostname;// 浏览器的同源政策不允许 222.com 的网页操作 111.com 的网页，因此若不同域名，这里会抛出权限异常

　　if (top.location.hostname != window.location.hostname) {// 适配 chrome 的代码
　　　　top.location.href =window.location.href;
　　}
}
catch(e){
　　top.location.href = window.location.href;// 检测到不同域名，即被恶意引用，将 top 对象的网址自动导向被嵌入网页的网址
}
```

## 页面跳转

http://blog.csdn.net/ithomer/article/details/7861313 

## 使 input 输入框不能编辑

http://blog.sina.com.cn/s/blog_69e220850100pyg2.html

1. 使用JavaScript实现

使输入框无法获得焦点，不能编辑表单可以获得值，但可以复制。

```html
<input type="text" value="fisker" onfocus="this.blur()"/>
```

2. 使用input标签的readonly属性

输入框只读。不能编辑，同样表单可以获得值，也可以复制。

```html
<input type="text" value="fisker" readonly />
```

3. 使用input标签的disabled属性

输入框灰色，不能编辑，可以用JS改变或获得其值，但提交时并不提交该值。

```html
<input type="text" value="fisker" disabled />
```


