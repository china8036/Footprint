<!-- toc -->
# Web 前端开发最佳实践-笔记 #

> 《Web 前端开发最佳实践》 党建 著

## HTML5 最佳实践 ##

> ？把这些整合到各个笔记里边去，只留下webapp最佳实践



## CSS3 最佳实践 ##




## JavaScript 最佳实践 ##





## 移动 Webapp 前端开发 ##

- 使用合适的图片显示兼容方案
一个简单有效的方法是设置一个全局样式：
```css
img {
	max-width: 100%;
}
```
该样式使得所有图片最大宽度不超过其容器的宽度，从而避免了图片因超出其容器宽度而被遮挡的问题，保证了图片的显示效果；   
更好的做法是使用 HTML5 中提供的设置响应式图片的 srcset 属性；

- 利用移动设备的便利性，使用 mailto 超链接

- 不要使用 `<iframe>` 标签，谨慎使用 `<table>` 标签
尽量使用`<ul>`、`<ol>`等列表控件代替`<table>`来显示数据；



- 使用内联图片代替小图片
	- 在性能和网络带宽都有局限的移动web开发中，应该尽量少用图片，最好使用CSS样式来美化页面，特别是CSS3的新特性更是可以被大量使用。

	- 可利用web fonts来制作各种字体或图标，如第三方类库Font Awesome、Bootstrap Glyphicons等，涵盖了页面中常用的上千种图标，开发者只需将这些图标作为一种特殊的字体使用，通过设置字体的大小和颜色便可更改图标的样式，例：
	```html
	<span class="glyphicon glyphicon-search"></span>
	```

	- 除了使用 Web Fonts 替换页面部分图片，可以将一些小尺寸的图片转换为内联图片在页面中展现，即将图片内容转换为 Base64 格式，并借助 Data URLs 技术将图片内容直接写入 CSS。其原理是减少了加载图片的 HTTP 请求数量，从而加快了页面的展现速度。内联图片可以是 CSS 背景图，也可以是页面的图片内容；
	例：在 CSS 中使用内联图片
	```css
	li {
		background: 
		url(data:image/gif;base64,R012DASKJhkjhkdasd3124e242kjhKBNKJDSAF9231ELKNHJ///asdasdadwerfoih90312423NRBF2JKCH498/das/dasd32r553olkhjLIH23)
		no-repeat
		left center;
	}
	```
	例：在 HTML 中使用内联图片
	```css
	<img alt="star" src="data:image/git;base64, RET3214IOJ23ID2dojwexxx" />
	```

- 不要使用 :hover 伪类设置悬停效果
在移动 Web 开发中，由于无法准则预测伪类被触发的时机，应避免使用 :hover 伪类设置悬停效果，若为了体现当前元素处于激活状态，可采用 :active 伪类来进行相应的设置；

- 在不需要选中文字的区域禁用文字选中功能
实现方法：
```css
-webkit-user-select: none;
-moz-user-select: none;
-ms-user-select: none;
user-select: none;
```