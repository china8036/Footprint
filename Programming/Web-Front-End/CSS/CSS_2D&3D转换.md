- [CSS 2D 和 3D 转换](#css-2d-%E5%92%8C-3d-%E8%BD%AC%E6%8D%A2)
  - [CSS3 属性](#css3-%E5%B1%9E%E6%80%A7)
  - [2D 转换](#2d-%E8%BD%AC%E6%8D%A2)
    - [平移变换：translate()](#%E5%B9%B3%E7%A7%BB%E5%8F%98%E6%8D%A2%EF%BC%9Atranslate)
      - [translateX(n)](#translatexn)
      - [translateY(n)](#translateyn)
    - [旋转变换：rotate()](#%E6%97%8B%E8%BD%AC%E5%8F%98%E6%8D%A2%EF%BC%9Arotate)
    - [缩放变换：scale()](#%E7%BC%A9%E6%94%BE%E5%8F%98%E6%8D%A2%EF%BC%9Ascale)
      - [scaleX(n)](#scalexn)
      - [scaleY(n)](#scaleyn)
    - [翻转变换：skew()](#%E7%BF%BB%E8%BD%AC%E5%8F%98%E6%8D%A2%EF%BC%9Askew)
      - [skewX(angle)](#skewxangle)
      - [skewY(angle)](#skewyangle)
    - [matrix()](#matrix)
  - [3D 转换](#3d-%E8%BD%AC%E6%8D%A2)
    - [3D 平移转换：translate3d(x,y,z)](#3d-%E5%B9%B3%E7%A7%BB%E8%BD%AC%E6%8D%A2%EF%BC%9Atranslate3dxyz)
      - [translateZ(z)](#translatezz)
    - [3D 缩放转换：scale3d(x,y,z)](#3d-%E7%BC%A9%E6%94%BE%E8%BD%AC%E6%8D%A2%EF%BC%9Ascale3dxyz)
      - [scaleZ(z)](#scalezz)
    - [3D 旋转转换：rotate3d(x,y,z,angle)](#3d-%E6%97%8B%E8%BD%AC%E8%BD%AC%E6%8D%A2%EF%BC%9Arotate3dxyzangle)
      - [rotateZ(angle)](#rotatezangle)
    - [matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n)](#matrix3dnnnnnnnnnnnnnnnn)
  - [Refer Links](#refer-links)

# CSS 2D 和 3D 转换

转换是使元素改变形状、尺寸和位置的一种效果。

## CSS3 属性

- transform
  
  用法：
  > transform: none|transform-functions;

  通过 `transform` 属性，可使元素进行 2D 或 3D 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。

  默认值为 none。

- transform-origin

  用法：
  > transform-origin: x-axis y-axis z-axis;

  通过 `transform-origin` 属性，可设置元素转换的基点位置。2D 转换元素能够改变元素 x 和 y 轴，3D 转换元素还能改变其 Z 轴。

  默认值为 `50% 50% 0`，即元素的中心点，定位的坐标原点在元素的左上角

  例：`transform-origin: 50px 70px;` 则中心点位置有中间移到了距离左侧50像素，顶部70像素的地方：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/6/f783c1dff9c4a41421bdd82ebbf40231.jpg)
  

- transform-style	
 
  规定被嵌套元素如何在 3D 空间中显示。

- perspective	
 
  规定 3D 元素的透视效果。	

- perspective-origin	

  规定 3D 元素的底部位置。	

- backface-visibility	
  
  定义元素在不面对屏幕时是否可见。

## 2D 转换

### 平移变换：translate()

通过 translate(left, top) 方法，元素从其当前位置，根据给定的 left（x 坐标） 和 top（y 坐标） 位置参数进行平移。

例：通过 translate(50px,100px) 把元素从左侧移动 50 像素，从顶端移动 100 像素
```css
div {
  transform: translate(50px,100px);
  -ms-transform: translate(50px,100px);		/* IE 9 */
  -webkit-transform: translate(50px,100px);	/* Safari and Chrome */
  -o-transform: translate(50px,100px);		/* Opera */
  -moz-transform: translate(50px,100px);		/* Firefox */
}
```

#### translateX(n)	

定义 2D 转换，沿着 X 轴移动元素。

#### translateY(n)	

定义 2D 转换，沿着 Y 轴移动元素。

### 旋转变换：rotate()

通过 rotate(deg) 方法，元素顺时针旋转给定的角度。当角度为负值时，元素将逆时针旋转。

例：通过 rotate(30deg) 把元素顺时针旋转 30 度
```css
div {
  transform: rotate(30deg);
  -ms-transform: rotate(30deg);		/* IE 9 */
  -webkit-transform: rotate(30deg);	/* Safari and Chrome */
  -o-transform: rotate(30deg);		/* Opera */
  -moz-transform: rotate(30deg);		/* Firefox */
}
```

### 缩放变换：scale()

通过 scale(width, height) 方法，元素的尺寸会根据给定的宽度（X 轴）和高度（Y 轴）参数增加或减少。

例：通过 scale(2,4) 把宽度转换为原始尺寸的 2 倍，把高度转换为原始高度的 4 倍
```css
div {
  transform: scale(2,4);
  -ms-transform: scale(2,4);	/* IE 9 */
  -webkit-transform: scale(2,4);	/* Safari 和 Chrome */
  -o-transform: scale(2,4);	/* Opera */
  -moz-transform: scale(2,4);	/* Firefox */
}
```
#### scaleX(n)	

定义 2D 缩放转换，改变元素的宽度。

#### scaleY(n)	

定义 2D 缩放转换，改变元素的高度。

### 翻转变换：skew()

通过 skew(x, y) 方法，元素根据给定的水平线（X 轴）和垂直线（Y 轴）参数翻转给定的角度。

例：通过 skew(30deg,20deg) 围绕 X 轴把元素翻转 30 度，围绕 Y 轴翻转 20 度

```css
div {
  transform: skew(30deg,20deg);
  -ms-transform: skew(30deg,20deg);	/* IE 9 */
  -webkit-transform: skew(30deg,20deg);	/* Safari and Chrome */
  -o-transform: skew(30deg,20deg);	/* Opera */
  -moz-transform: skew(30deg,20deg);	/* Firefox */
}
```

#### skewX(angle)	

定义 2D 倾斜转换，沿着 X 轴。

#### skewY(angle)	

定义 2D 倾斜转换，沿着 Y 轴。

### matrix()

matrix(a,b,c,d,e,f) 方法把所有 2D 转换方法组合在一起，使用六个值的矩阵定义 2D 转换，本质上其它各种转换都是应用matrix()方法来实现的，只是类似于`transform:rotate`这种表现形式，我们更容易理解，记忆与上手。


- matrix方法中的6个参数对应的矩阵：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/6/ef8adbd97daab0e6ce809d54041c26f8.jpg)

  当对坐标为(x, y)的元素进行转换时，实际上会进行以下计算：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/6/aca1e4b2948caf6d29be96c2111c4b89.jpg)

  其中，ax+cy+e表示变换后的水平坐标，bx+dy+f表示变换后的垂直位置。

- 实例解析：
  ```css
  transform: matrix(1, 0, 0, 1, 30, 30); /* a=1, b=0, c=0, d=1, e=30, f=30 */
  ```
  根据这个矩阵偏移元素的中心点，假设是(0, 0)，即x=0, y=0。

  于是，变换后的x坐标就是`ax+cy+e=1*0+0*0+30=30`, y坐标就是`bx+dy+f=0*0+1*0+30=30`.

  于是，中心点坐标从(0, 0)变成了(30, 30)，即元素的所有像素都向右平移了30px，再向下平移了30px。

- 实际上：
  - `transform: matrix(1, 0, 0, 1, a, b);`就等同于`transform: translate(apx, bpx);`

    translate, rotate等方法都是需要单位的，而matrix方法e, f参数的单位可以省略。

  - `transform:matrix(sx, 0, 0, sy, 0, 0);`，等同于`transform: scale(sx, sy);`

  - 其余变换类似......


例：使用 matrix 方法将 div 元素旋转 30 度
```css
div {
  transform:matrix(0.866,0.5,-0.5,0.866,0,0);
  -ms-transform:matrix(0.866,0.5,-0.5,0.866,0,0);		/* IE 9 */
  -moz-transform:matrix(0.866,0.5,-0.5,0.866,0,0);	/* Firefox */
  -webkit-transform:matrix(0.866,0.5,-0.5,0.866,0,0);	/* Safari and Chrome */
  -o-transform:matrix(0.866,0.5,-0.5,0.866,0,0);		/* Opera */
}
```

## 3D 转换

部分浏览器会强制使用 GPU 渲染加速来实现 3D 转换效果。

### 3D 平移转换：translate3d(x,y,z)

#### translateZ(z)	

定义 3D 转换，只是用 Z 轴的值。

### 3D 缩放转换：scale3d(x,y,z)	

#### scaleZ(z)	

通过设置 Z 轴的值来定义 3D 缩放转换。

### 3D 旋转转换：rotate3d(x,y,z,angle)

#### rotateZ(angle)	

定义沿着 Z 轴的 3D 旋转。

### matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n)	

定义 3D 转换，使用 16 个值的 4x4 矩阵。

## Refer Links

http://www.w3school.com.cn/css3/css3_2dtransform.asp

http://www.w3school.com.cn/css3/css3_3dtransform.asp

http://www.zhangxinxu.com/wordpress/2012/06/css3-transform-matrix-%E7%9F%A9%E9%98%B5/
