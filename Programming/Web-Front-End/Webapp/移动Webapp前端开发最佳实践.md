- [移动 Webapp 前端开发最佳实践](#%E7%A7%BB%E5%8A%A8-webapp-%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5)
  - [善用webkit内核中的一些私有的meta标签](#%E5%96%84%E7%94%A8webkit%E5%86%85%E6%A0%B8%E4%B8%AD%E7%9A%84%E4%B8%80%E4%BA%9B%E7%A7%81%E6%9C%89%E7%9A%84meta%E6%A0%87%E7%AD%BE)
  - [去除iOS和Android中的输入URL的控件条](#%E5%8E%BB%E9%99%A4ios%E5%92%8Candroid%E4%B8%AD%E7%9A%84%E8%BE%93%E5%85%A5url%E7%9A%84%E6%8E%A7%E4%BB%B6%E6%9D%A1)
  - [iOS中禁止用户保存图片/复制图片](#ios%E4%B8%AD%E7%A6%81%E6%AD%A2%E7%94%A8%E6%88%B7%E4%BF%9D%E5%AD%98%E5%9B%BE%E7%89%87%E5%A4%8D%E5%88%B6%E5%9B%BE%E7%89%87)
  - [使用图片显示自适应方案](#%E4%BD%BF%E7%94%A8%E5%9B%BE%E7%89%87%E6%98%BE%E7%A4%BA%E8%87%AA%E9%80%82%E5%BA%94%E6%96%B9%E6%A1%88)
  - [利用移动设备的便利性，使用 mailto 超链接](#%E5%88%A9%E7%94%A8%E7%A7%BB%E5%8A%A8%E8%AE%BE%E5%A4%87%E7%9A%84%E4%BE%BF%E5%88%A9%E6%80%A7%EF%BC%8C%E4%BD%BF%E7%94%A8-mailto-%E8%B6%85%E9%93%BE%E6%8E%A5)
  - [不要使用 `<iframe>` 标签，谨慎使用 `<table>` 标签](#%E4%B8%8D%E8%A6%81%E4%BD%BF%E7%94%A8-iframe-%E6%A0%87%E7%AD%BE%EF%BC%8C%E8%B0%A8%E6%85%8E%E4%BD%BF%E7%94%A8-table-%E6%A0%87%E7%AD%BE)
  - [使用内联图片代替小图片](#%E4%BD%BF%E7%94%A8%E5%86%85%E8%81%94%E5%9B%BE%E7%89%87%E4%BB%A3%E6%9B%BF%E5%B0%8F%E5%9B%BE%E7%89%87)
  - [不要使用 :hover 伪类设置悬停效果](#%E4%B8%8D%E8%A6%81%E4%BD%BF%E7%94%A8-hover-%E4%BC%AA%E7%B1%BB%E8%AE%BE%E7%BD%AE%E6%82%AC%E5%81%9C%E6%95%88%E6%9E%9C)
  - [在不需要选中文字的区域禁用文字选中功能](#%E5%9C%A8%E4%B8%8D%E9%9C%80%E8%A6%81%E9%80%89%E4%B8%AD%E6%96%87%E5%AD%97%E7%9A%84%E5%8C%BA%E5%9F%9F%E7%A6%81%E7%94%A8%E6%96%87%E5%AD%97%E9%80%89%E4%B8%AD%E5%8A%9F%E8%83%BD)
  - [允许网页宽度自动调整](#%E5%85%81%E8%AE%B8%E7%BD%91%E9%A1%B5%E5%AE%BD%E5%BA%A6%E8%87%AA%E5%8A%A8%E8%B0%83%E6%95%B4)
  - [不使用绝对宽度](#%E4%B8%8D%E4%BD%BF%E7%94%A8%E7%BB%9D%E5%AF%B9%E5%AE%BD%E5%BA%A6)
  - [相对大小的字体](#%E7%9B%B8%E5%AF%B9%E5%A4%A7%E5%B0%8F%E7%9A%84%E5%AD%97%E4%BD%93)
  - [多使用流式布局（fluid grid）](#%E5%A4%9A%E4%BD%BF%E7%94%A8%E6%B5%81%E5%BC%8F%E5%B8%83%E5%B1%80%EF%BC%88fluid-grid%EF%BC%89)
  - [Refer Links](#refer-links)

# 移动 Webapp 前端开发最佳实践

## 善用webkit内核中的一些私有的meta标签

```html
<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" name="viewport">
<meta content="yes" name="apple-mobile-web-app-capable">
<meta content="black" name="apple-mobile-web-app-status-bar-style">
<meta content="telephone=no" name="format-detection">
<meta content="email=no" name="format-detection" />
```

第一个meta标签表示：强制让文档的宽度与设备的宽度保持1:1，并且文档最大的宽度比例是1.0，且不允许用户点击屏幕放大浏览；

第二个meta标签是iphone设备中的safari私有meta标签，它表示：允许全屏模式浏览；

第三个meta标签也是iphone的私有标签，它指定的iphone中safari顶端的状态条的样式；

第四个meta标签表示：告诉设备忽略将页面中的数字识别为电话号码；

第五个meta标签标识：在Android平台，禁用对邮箱地址的自动识别


## 去除iOS和Android中的输入URL的控件条

隐藏浏览器中输入URL的空间条，使webapp更加像nativeapp：
```javascript
setTimeout(scrollTo,0,0,0);
```
请注意，这句代码必须放在window.onload里才能够正常的工作，而且你的当前文档的内容高度必须是高于窗口的高度时，这句代码才能有效的执行。

## iOS中禁止用户保存图片/复制图片

为一个img标签指定`-webkit-touch-callout: none`会禁止设备弹出列表按钮，这样用户就无法保存/复制你的图片了。



## 使用图片显示自适应方案

使得图片能够自动缩放的一个简单有效的方法是设置一个全局样式：
```css
img {
	max-width: 100%;
}
```
该样式使得所有图片最大宽度不超过其容器的宽度，从而避免了图片因超出其容器宽度而被遮挡的问题，保证了图片的显示效果；   
更好的做法是使用 HTML5 中提供的设置响应式图片的 srcset 属性，即根据不同大小的屏幕，加载不同分辨率的图片。



## 利用移动设备的便利性，使用 mailto 超链接

## 不要使用 `<iframe>` 标签，谨慎使用 `<table>` 标签

尽量使用`<ul>`、`<ol>`等列表控件代替`<table>`来显示数据；

## 使用内联图片代替小图片
	
- 在性能和网络带宽都有局限的移动 web 开发中，应该尽量少用图片，最好使用 CSS 样式来美化页面，特别是 CSS3 的新特性更是可以被大量使用。

- 可利用 web fonts 来制作各种字体或图标，如第三方类库 Font Awesome、Bootstrap Glyphicons 等，涵盖了页面中常用的上千种图标，开发者只需将这些图标作为一种特殊的字体使用，通过设置字体的大小和颜色便可更改图标的样式，例：
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


## 不要使用 :hover 伪类设置悬停效果

在移动 Web 开发中，由于无法准则预测伪类被触发的时机，应避免使用 :hover 伪类设置悬停效果，若为了体现当前元素处于激活状态，可采用 :active 伪类来进行相应的设置；

## 在不需要选中文字的区域禁用文字选中功能
	
实现方法：
```css
user-select: none;
-webkit-user-select: none;
-moz-user-select: none;
-ms-user-select: none;
```


## 允许网页宽度自动调整

实现：
```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```
即：网页宽度默认等于屏幕宽度（width=device-width），原始缩放比例（initial-scale=1）为1.0，即网页初始大小占屏幕面积的100%。


## 不使用绝对宽度

由于网页会根据屏幕宽度调整布局，所以不能使用绝对宽度的布局，也不能使用具有绝对宽度的元素。这一条非常重要。

具体说，CSS代码不能指定像素宽度：
```css
width: xxx px;
```
只能指定百分比宽度：
```css
width: xx%;
```
或者
```css
width:auto;
```


## 相对大小的字体

字体也不能使用绝对大小（px），而只能使用相对大小（em）。
```css
body {
　font: normal 100% Helvetica, Arial, sans-serif;
}
```
上面的代码指定，字体大小是页面默认大小的100%，即16像素。

```css
h1 {
	font-size: 1.5em; 
}
```

## 多使用流式布局（fluid grid）

float的好处是，如果宽度太小，放不下两个元素，后面的元素会自动滚动到前面元素的下方，不会在水平方向overflow（溢出），避免了水平滚动条的出现。





## Refer Links

《Web 前端开发最佳实践》 党建 著

[css 自适应布局阮一峰](https://www.cnblogs.com/youxin/p/3343780.html)

