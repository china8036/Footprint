- [OpenCV 使用：形态学操作](#opencv-使用形态学操作)
  - [1. 图像膨胀 / 腐蚀](#1-图像膨胀--腐蚀)
    - [1.1. 膨胀](#11-膨胀)
    - [1.2. 腐蚀](#12-腐蚀)
  - [2. 高级形态学变换](#2-高级形态学变换)
    - [2.1. 开运算（Opening Operation）](#21-开运算opening-operation)
    - [2.2. 闭运算 (Closing Operation)](#22-闭运算-closing-operation)
    - [2.3. 形态学梯度](#23-形态学梯度)
    - [2.4. 顶帽 / 礼帽运算](#24-顶帽--礼帽运算)
    - [2.5. 黑帽运算](#25-黑帽运算)
    - [2.6. morphologyEx 调用膨胀 / 腐蚀操作](#26-morphologyex-调用膨胀--腐蚀操作)
  - [3. Refer Links](#3-refer-links)

# OpenCV 使用：形态学操作

http://blog.csdn.net/poem_qianmo/article/details/24599073

形态学操作就是基于形状的一系列图像处理操作。通过将“结构元素”作用于输入图像来产生输出图像。

获取结构元素 API
```cpp
Mat getStructuringElement(int shape, Size esize, Point anchor=Point(-1, -1))
```
Parameters:
- shape – The element shape：
enum { MORPH_RECT=0, MORPH_CROSS=1, MORPH_ELLIPSE=2 };
  - MORPH_RECT：矩形，值为 0；
  - MORPH_CROSS：十字形，值为 1；
  - MORPH_ELLIPSE：椭圆形，值为 2；
- esize – Size of the structuring element
- anchor – The anchor position within the element. The default value  (-1, -1) means that the anchor is at the center. 指定锚点位置。若不指定锚点位置，则默认锚点在内核中心位置。

形态学操作不光可以用在二值图，也可以用在灰度图，甚至可以用在彩色图。

## 1. 图像膨胀 / 腐蚀

http://blog.csdn.net/poem_qianmo/article/details/23710721

概念 http://www.developersite.org/904-152163-opencv

腐蚀与膨胀 (Erosion 与 Dilation) 是两个最基本的形态学操作；

腐蚀和膨胀是对白色部分（高亮部分）而言的，不是黑色部分。比如膨胀，就是将图像中的高亮区域扩大。

按数学方面来说，膨胀或者腐蚀操作就是将图像（或图像的一部分区域，我们称之为 A）与核（我们称之为 B）进行卷积。核可以是任何的形状和大小，它拥有一个单独定义出来的参考点，我们称其为锚点（anchorpoint）。多数情况下，核是一个小的中间带有参考点和实心正方形或者圆盘，其实，我们可以把核视为模板或者掩码。

### 1.1. 膨胀

膨胀就是求局部最大值的操作，核 B 与图形卷积，即计算核 B 覆盖的区域的像素点的最大值，并把这个最大值赋值给参考点指定的像素。这样就会使图像中的高亮区域逐渐增长。

![image](http://img.cdn.firejq.com/jpg/2019/3/25/e64205c5320394157bdad639f16a7e47.jpg)

### 1.2. 腐蚀

腐蚀就是求局部最小值的操作。随着核的移动，每次都取核覆盖区域的最小像素值，因此最终完成的效果是将高亮区域缩小。

![image](http://img.cdn.firejq.com/jpg/2019/3/25/b4fefba26209b45b3183c558827a83fe.jpg)

应用
- 消除噪声；
- 分割 (isolate) 独立的图像元素，以及连接 (join) 相邻的元素；
- 寻找图像中的明显的极大值区域或极小值区域；
- 可以求图像梯度或者图像中的小洞；

例：

原图：

![image](http://img.cdn.firejq.com/jpg/2019/3/25/5526312511651950bc2075344dd0a15b.jpg)

膨胀后：背景（白色) 膨胀，而黑色字母缩小了

![image](http://img.cdn.firejq.com/jpg/2019/3/25/83fc19ffffe1f76372cbb986cfcab74e.jpg)

腐蚀后：亮区（背景) 变细，而黑色区域（字母) 则变大了

![image](http://img.cdn.firejq.com/jpg/2019/3/25/189f74468b58e7ca68c8d3c8e47c2635.jpg)

## 2. 高级形态学变换

OpenCV 中使用 morphologyEx 函数，利用基本的膨胀和腐蚀技术，来执行更加高级形态学变换，如开闭运算，形态学梯度，“顶帽”、“黑帽”等；

```cpp
void morphologyEx(InputArray src,  OutputArray dst,  int op,  InputArray kernel,  Point anchor=Point(-1,-1),  int iterations=1,  int borderType=BORDER_CONSTANT,  const Scalar& borderValue=morphologyDefaultBorderValue());
```
参数说明：
- 第一个参数，InputArray 类型的 src，输入图像，即源图像，填 Mat 类的对象即可。图像位深应该为以下五种之一：CV_8U, CV_16U,CV_16S, CV_32F 或 CV_64F。
- 第二个参数，OutputArray 类型的 dst，即目标图像，函数的输出参数，需要和源图片有一样的尺寸和类型。
- 第三个参数，int 类型的 op，表示形态学运算的类型，可以是如下之一的标识符：
  ```
  MORPH_OPEN – 开运算（Opening operation）
  MORPH_CLOSE – 闭运算（Closing operation）
  MORPH_GRADIENT - 形态学梯度（Morphological gradient）
  MORPH_TOPHAT - “顶帽”（“Top hat”）
  MORPH_BLACKHAT - “黑帽”（“Black hat“）
  MORPH_ERODE – 腐蚀
  MORPH_DILATE - 膨胀
  ```
- 第四个参数，InputArray 类型的 kernel，形态学运算的内核。若为 NULL 时，表示的是使用参考点位于中心 3x3 的核。我们一般使用函数 getStructuringElement 配合这个参数的使用。
- 第五个参数，Point 类型的 anchor，锚的位置，其有默认值（-1，-1），表示锚位于中心。
- 第六个参数，int 类型的 iterations，迭代使用函数的次数，默认值为 1。
- 第七个参数，int 类型的 borderType，用于推断图像外部像素的某种边界模式。注意它有默认值 BORDER_ CONSTANT。
- 第八个参数，const Scalar& 类型的 borderValue，当边界为常数时的边界值，有默认值 morphologyDefaultBorderValue()，一般我们不用去管他。需要用到它时，可以看官方文档中的 createMorphologyFilter() 函数得到更详细的解释。

注意：对于多通道图像，每一个通道都是单独进行操作。

### 2.1. 开运算（Opening Operation）

开运算（Opening Operation），其实就是先腐蚀后膨胀的过程；

其数学表达式如下：

![image](http://img.cdn.firejq.com/jpg/2019/3/25/fcb49f33a2346e8457e49b7644d27b80.jpg)

开运算可以用来消除小物体、在纤细点处分离物体、平滑较大物体的边界的同时并不明显改变其面积。

例：
```cpp
Mat element = getStructuringElement(MORPH_RECT, Size(15, 15));
// 进行形态学操作
morphologyEx(image, image, MORPH_OPEN, element);
```

### 2.2. 闭运算 (Closing Operation)

先膨胀后腐蚀的过程称为闭运算 (Closing Operation)；
其数学表达式如下：

![image](http://img.cdn.firejq.com/jpg/2019/3/25/5d886dfb4eac3ac30064ece20064d144.jpg)

闭运算能够排除小型黑洞（黑色区域)。

例：
```cpp
// 定义核
Mat element = getStructuringElement(MORPH_RECT, Size(15, 15));
// 进行形态学操作
morphologyEx(image,image, MORPH_CLOSE, element);
```

### 2.3. 形态学梯度

形态学梯度（Morphological Gradient）为膨胀图与腐蚀图之差；

数学表达式如下：

![image](http://img.cdn.firejq.com/jpg/2019/3/25/f34e9dd4baf1e0b1fcee31c24efbbda8.jpg)

对二值图像进行这一操作可以将团块（blob）的边缘突出出来。我们可以用形态学梯度来保留物体的边缘轮廓

例：
```cpp
// 定义核
Mat element = getStructuringElement(MORPH_RECT, Size(15, 15));
// 进行形态学操作
morphologyEx(image,image, MORPH_GRADIENT, element);
```

### 2.4. 顶帽 / 礼帽运算

顶帽运算（Top Hat）又常常被译为”礼帽“运算。为原图像与上文刚刚介绍的“开运算“的结果图之差；

数学表达式如下：

![image](http://img.cdn.firejq.com/jpg/2019/3/25/038a7161b855154911f8e131ce544888.jpg)

因为开运算带来的结果是放大了裂缝或者局部低亮度的区域，因此，从原图中减去开运算后的图，得到的效果图突出了比原图轮廓周围的区域更明亮的区域，且这一操作和选择的核的大小相关。

顶帽运算往往用来分离比邻近点亮一些的斑块。当一幅图像具有大幅的背景的时候，而微小物品比较有规律的情况下，可以使用顶帽运算进行背景提取。

例：
```cpp
// 定义核
Mat element = getStructuringElement(MORPH_RECT, Size(15, 15));
// 进行形态学操作
morphologyEx(image,image, MORPH_TOPHAT, element);
```

### 2.5. 黑帽运算

黑帽（Black Hat）运算为”闭运算“的结果图与原图像之差；

数学表达式为：

![image](http://img.cdn.firejq.com/jpg/2019/3/25/8cd60d661ff1f7d36ae92a92061503b4.jpg)

黑帽运算后的效果图突出了比原图轮廓周围的区域更暗的区域，且这一操作和选择的核的大小相关。所以，黑帽运算用来分离比邻近点暗一些的斑块。

例：
```cpp
// 定义核
Mat element = getStructuringElement(MORPH_RECT, Size(15, 15));
// 进行形态学操作
morphologyEx(image,image, MORPH_BLACKHAT, element);
```

### 2.6. morphologyEx 调用膨胀 / 腐蚀操作

腐蚀
```cpp
// 定义核
Mat element = getStructuringElement(MORPH_RECT, Size(15, 15));
// 进行腐蚀操作
morphologyEx(image,image, MORPH_ERODE, element);
```
膨胀
```cpp
// 定义核
Mat element = getStructuringElement(MORPH_RECT, Size(15, 15));
// 进行膨胀操作
morphologyEx(image,image, MORPH_DILATE, element);
```

## 3. Refer Links
