- [CSS 滤镜](#css-%E6%BB%A4%E9%95%9C)
  - [1. USAGE](#1-usage)
  - [2. EXAMPLE](#2-example)

# CSS 滤镜

https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter

CSS3 滤镜（filter）属提供的图形特效，像模糊，锐化或元素变色。过滤器通常被用于调整图片，背景和边界的渲染，但也可用于 video。

## 1. USAGE

```css
.example-1 {
  filter: blur(5px); 
  /* 毛玻璃效果：设置高斯模糊，参数表示屏幕上以多少像素融在一起，所以值越大越模糊；如果没有设定值，则默认是 0；这个参数可设置 css 长度值，但不接受百分比值 */
}

.example-2 {
  filter: grayscale(0.5); 
  /* 黑白图效果：将图像转换为灰度图像，值定义转换的比例。值为 100% 则完全转为灰度图像，值为 0% 图像无变化。默认值是 0 */
}

.example-3 {
  filter: sepia(0.5); 
  /* 怀旧效果：将图像转换为深褐色，值为 100% 则完全是深褐色的，值为 0% 图像无变化。 */
}

.example-4 {
  filter: brightness(3); 
  /* 亮度，如果值是 0%，图像会全黑。值是 100%，则图像无变化。其他的值对应线性乘数效果。值超过 100% 也是可以的，图像会比原来更亮。如果没有设定值，默认是 1。 */
}

.example-5 {
  filter: hue-rotate(180deg); 
  /* 给图像应用色相旋转，设定图像会被调整的色环角度值。值为 0deg，则图像无变化。该值虽然没有最大值，超过 360deg 的值相当于又绕一圈。默认值是 0deg */
}

.example-6 {
  filter: invert(1); 
  /* 反色，值定义转换的比例。100% 的价值是完全反转。值为 0% 则图像无变化。 */
}

.example-7 {
  filter: opacity(0.5); 
  /* 透明度，值为 0% 则是完全透明，值为 100% 则图像无变化。该函数与已有的 opacity 属性很相似，不同之处在于通过 filter，一些浏览器为了提升性能会提供硬件加速。 */
}

.example-8 {
  filter: saturate(5);
  /* 饱和度，值为 0% 则是完全不饱和，值为 100% 则图像无变化。 */
}

.example-9 {
  filter: contrast(0.5); 
  /* 对比度，值是 0% 的话，图像会全黑。值是 100%，图像不变。值可以超过 100%，意味着会运用更低的对比。若没有设置值，默认是 1。 */
}

.example-10 {
  filter: drop-shadow(10px 10px 5px rgba(0, 0, 0, 0.9)); 
  /* 设置阴影效果，该函数与已有的 box-shadow 属性很相似；不同之处在于，通过滤镜，一些浏览器为了更好的性能会提供硬件加速 */
}
```

可以组合任意数量的函数来控制渲染：

例：同时增强图像的对比度和亮度
```css
filter: contrast(175%) brightness(3%)
```

## 2. EXAMPLE

http://jsbin.com/vasiperusi/edit?html,css,output