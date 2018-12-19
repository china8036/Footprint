- [移动 Webapp 前端 基础概念](#%E7%A7%BB%E5%8A%A8-webapp-%E5%89%8D%E7%AB%AF-%E5%9F%BA%E7%A1%80%E6%A6%82%E5%BF%B5)
  - [Refer Links](#refer-links)

# 移动 Webapp 前端 基础概念

- 设备像素 / 物理像素（physical pixel）：设备能控制显示的最小单位，设备屏幕的物理像素，单位是 px，比如 iPhone6P 的 1920x1080px，iPhone6 的 750x1334px，即平常所说的分辨率对应的单位。

- 分辨率（Resolution）：一个物理概念，对于屏幕，分辨率一般表示屏幕上显示的物理像素总和。比如，我们说 iPhone6 屏幕分辨率是 750x1334px；
对于图像，概念等同于图像尺寸、图像大小、像素尺寸等等。比如，我们说 20x20px 的 icon。严格来说，图像分辨率的单位是 ppi（Pixels Per Inch），对于一个图片文件，其像素尺寸是一定的，可能含有来自相机的 meta 信息，比如分辨率 200ppi。

- CSS 像素：CSS 像素是 Web 编程的概念，独立于设备的用于逻辑上衡量像素的单位，也就是说我们在做网页时用到的 CSS 像素单位，是抽象的，而不是实际存在的。1 个 CSS 像素在不同设备上可能对应不同的物理像素数，这个比值是设备的属性（Device Pixel Ratio，设备像素比）。在 CSS 规范中，长度单位可以分为绝对单位和相对单位。**px 是一个相对单位，相对的是设备像素**（Device Pixels）。比如 iPhone5 使用的是 Retina 视网膜屏幕，用 2x2 的 Device Pixel 代表 1x1 的 CSS Pixel，所以设备像素数为 640x1136px，而 CSS 逻辑像素数为 320x568px。
  - chrome的移动模拟器中的尺寸值指的就是css像素：
    ![image](http://img.cdn.firejq.com/jpg/2017/10/17/e41def633412a81f2a7ded110a5753c5.jpg)

- 设备独立像素 / 密度无关像素：可以认为是计算机坐标系统中的一个点，这个点代表一个可以由程序使用并控制的虚拟像素（比如：CSS 像素，只是在 android 机中 CSS 像素就不叫”CSS 像素”了而是叫”设备独立像素”)，然后由相关系统转换为物理像素。一般指Google提出的用来适配Android海量奇怪屏幕的，Windows也有这个概念，但含义不同，IOS好像没有设备独立像素一说。

- 像素密度 (pixel density)/PPI(pixel per inch)：每英寸内有多少个像素点，PPI 的值越高，画质越好，也就是越细腻；
![image](http://img.cdn.firejq.com/jpg/2017/10/16/985b1cc2699110f189f234bb09120379.jpg)
![image](http://img.cdn.firejq.com/jpg/2017/10/16/f5e09d360064f92ea0f9d71062e7c268.jpg)

- 设备像素比 (dpr,device pixel ratio)：设备像素比 = 物理像素数 / 逻辑像素数（在某一方向上，x 方向或者 y 方向），如 dpr=2 时，1 个 CSS 像素由 4 个物理像素点组成；通过 JavaScript 中的 window.devicePixelRatio 来获取当前设备的设备像素比；


![image](http://img.cdn.firejq.com/jpg/2017/10/16/f61dab811efd1393e8c8c8bf4c207b7c.jpg)

注意：

通过 screen.width/height 属性获取的是设备独立像素值（CSS 像素），而不是设备的分辨率。在 pc 平台的 chrome 中 screen.width=1920，screen.height=1080（由于 pc 端设备像素和设备独立像素数值相等，因此在 PC 端很多人把这个值当成我们常说的屏幕分辨率）；在移动端环境中，使用 screen.width/height 可以获取移动设备的屏宽 / 高，如 Iphone 5s 下 screen.width =320、screen.height = 568；

所以不管是移动端还是 PC 端通过 screen.width/height 获取的这个值是设备独立像素（CSS 像素），而不是设备的屏幕分辨率，因为设备的屏幕分辨率对于 WEB 开发者来说是无法通过代码来获得的，是完全透明的。

## Refer Links
https://github.com/jawil/blog/issues/21

http://yunkus.com/physical-pixel-device-independent-pixels/

http://www.ayqy.net/blog/%E5%AE%8C%E5%85%A8%E7%90%86%E8%A7%A3px-dpr-dpi-dip/

https://github.com/jawil/blog/issues/21

https://lulua87.github.io/2017/08/29/How-does-FE-implement-Mockup/


