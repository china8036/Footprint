- [OpenCV 使用：图像处理](#opencv-使用图像处理)
  - [1. 掩膜操作](#1-掩膜操作)
  - [2. 亮度 / 对比度调节](#2-亮度--对比度调节)
    - [2.1. 算子](#21-算子)
    - [2.2. 理论依据](#22-理论依据)
    - [2.3. 访问像素](#23-访问像素)
    - [2.4. 示例](#24-示例)
  - [3. 边缘检测 (Edge Detection)](#3-边缘检测-edge-detection)
    - [3.1. 操作步骤](#31-操作步骤)
    - [3.2. Canny 算子](#32-canny-算子)
    - [3.3. Sobel 算子](#33-sobel-算子)
    - [3.4. Laplace 算子](#34-laplace-算子)
    - [3.5. scharr 滤波器](#35-scharr-滤波器)
    - [3.6. Structured forest 算法](#36-structured-forest-算法)
  - [4. 轮廓检测 (Contour Detection)](#4-轮廓检测-contour-detection)
    - [4.1. findContours 函数（检测轮廓）](#41-findcontours-函数检测轮廓)
    - [4.2. approxPolyDP 函数（点集逼近）](#42-approxpolydp-函数点集逼近)
    - [4.3. drawContours 函数（绘制轮廓）](#43-drawcontours-函数绘制轮廓)
    - [4.4. 获取轮廓的外接图形](#44-获取轮廓的外接图形)
  - [5. 图像分割 (Image Segmentation)](#5-图像分割-image-segmentation)
    - [5.1. 轮廓检测、边缘检测、图像分割三者联系](#51-轮廓检测边缘检测图像分割三者联系)
    - [5.2. 阈值分割](#52-阈值分割)
      - [5.2.1. 固定阈值化函数 threshould()](#521-固定阈值化函数-threshould)
      - [5.2.2. 自适应阈值化函数：adaptiveThreshold()](#522-自适应阈值化函数adaptivethreshold)
      - [5.2.3. 示例](#523-示例)
    - [5.3. 区域分割](#53-区域分割)
    - [5.4. 边缘分割](#54-边缘分割)
    - [5.5. 直方图法](#55-直方图法)
    - [5.6. 前后景分割](#56-前后景分割)
    - [5.7. 通道分割 / 混合](#57-通道分割--混合)
    - [5.8. 特定理论](#58-特定理论)
  - [6. 图像混合](#6-图像混合)
  - [7. 噪声](#7-噪声)
    - [7.1. 高斯噪声](#71-高斯噪声)
    - [7.2. 椒盐噪声 / 脉冲噪声](#72-椒盐噪声--脉冲噪声)
    - [7.3. 降噪 / 去噪方法](#73-降噪--去噪方法)
      - [7.3.1. 均值滤波](#731-均值滤波)
      - [7.3.2. 中值滤波](#732-中值滤波)
      - [7.3.3. 小波变换](#733-小波变换)
      - [7.3.4. 比较](#734-比较)
  - [8. 滤波](#8-滤波)
    - [8.1. opencv 线性滤波 API](#81-opencv-线性滤波-api)
      - [8.1.1. 方框滤波——boxblur 函数](#811-方框滤波boxblur-函数)
      - [8.1.2. 均值滤波——blur 函数](#812-均值滤波blur-函数)
      - [8.1.3. 高斯滤波——GaussianBlur 函数](#813-高斯滤波gaussianblur-函数)
    - [8.2. 非线性滤波](#82-非线性滤波)
      - [8.2.1. 中值滤波——medianBlur 函数](#821-中值滤波medianblur-函数)
      - [8.2.2. 双边滤波——bilateralFilter 函数](#822-双边滤波bilateralfilter-函数)
    - [8.3. 极大值 / 极小值 / 中值滤波](#83-极大值--极小值--中值滤波)
      - [8.3.1. 极大值滤波](#831-极大值滤波)
      - [8.3.2. 极小值滤波（与极大值滤波相反）](#832-极小值滤波与极大值滤波相反)
      - [8.3.3. 中点滤波](#833-中点滤波)
      - [8.3.4. 中值滤波](#834-中值滤波)
      - [8.3.5. 加权中值滤波（中值滤波的改进）](#835-加权中值滤波中值滤波的改进)
  - [9. 漫水填充](#9-漫水填充)
  - [10. 尺寸调整](#10-尺寸调整)
    - [10.1. resize 函数](#101-resize-函数)
    - [10.2. 图像金字塔](#102-图像金字塔)
    - [10.3. 两者区别](#103-两者区别)
  - [11. Refer Links](#11-refer-links)

# OpenCV 使用：图像处理

## 1. 掩膜操作

http://www.jianshu.com/p/c6d1c01c900b

## 2. 亮度 / 对比度调节

http://blog.csdn.net/poem_qianmo/article/details/21479533

### 2.1. 算子

一般的图像处理算子都是一个函数，它接受一个或多个输入图像，并产生输出图像。下式给出了算子的一般形式：

![image](http://img.cdn.firejq.com/jpg/2019/3/25/ad6506cae64a954444150e2f34c59f20.jpg)

图像亮度和对比度的调整操作，其实属于图像处理变换中比较简单的一种——点操作（pointoperators）。点操作有一个特点，仅仅根据输入像素值（有时可加上某些全局信息或参数），来计算相应的输出像素值。这类算子包括亮度（brightness）和对比度（contrast）调整，以及颜色校正（colorcorrection）和变换（transformations）。

### 2.2. 理论依据

亮度和对比度调节的理论公式：

![image](http://img.cdn.firejq.com/jpg/2019/3/25/83e56b4e336b4dacedbe14bb123bcfb9.jpg)

其中：
- 参数 f(i, j) 表示源图像像素。
- 参数 g(I, j) 表示输出图像像素。
- 参数 a（需要满足 a>0）被称为增益（gain），常常被用来控制图像的对比度。
- 参数 b 通常被称为偏置（bias），常常被用来控制图像的亮度。

### 2.3. 访问像素

为了执行  这个运算，我们需要访问图像的每一个像素。因为是对 GBR 图像进行运算，每个像素有三个值（G、B、R），所以我们必须分别访问它们（PS:OpenCV 中的图像存储模式为 GBR）。以下是访问像素的代码片段，三个 for 循环解决问题：

```cpp
// 三个 for 循环，执行运算 new_image(i,j) =a*image(i,j) + b
for(int y = 0; y < image.rows; y++ )
{
       for(int x = 0; x < image.cols; x++ )
       {
              for(int c = 0; c < 3; c++ )
              {
                     new_image.at<Vec3b>(y,x)[c]= saturate_cast<uchar>( (g_nContrastValue*0.01)*(image.at<Vec3b>(y,x)[c] ) + g_nBrightValue );
              }
       }
}
/*
为了访问图像的每一个像素，我们使用这样的语法： image.at<Vec3b>(y,x)[c]，其中，y 是像素所在的行， x 是像素所在的列， c 是 R、G、B（对应 0、1、2）其中之一。
因为我们的运算结果可能超出像素取值范围（溢出），还可能是非整数（如果是浮点数的话），所以我们要用 saturate_cast 对结果进行转换，以确保它为有效值。
这里的 a 也就是对比度，一般为了观察的效果，取值为 0.0 到 3.0 的浮点值，但是我们的轨迹条一般取值都会整数，所以在这里我们可以，将其代表对比度值的 nContrastValue 参数设为 0 到 300 之间的整型，在最后的式子中乘以一个 0.01，这样就可以完成轨迹条中 300 个不同取值的变化。所以在式子中，我们会看到 saturate_cast<uchar>( (g_nContrastValue*0.01)*(image.at<Vec3b>(y,x)[c] ) + g_nBrightValue ) 中的 g_nContrastValue*0.01
*/
```

### 2.4. 示例

```cpp
#include <opencv2/core/core.hpp>
#include<opencv2/highgui/highgui.hpp>
#include"opencv2/imgproc/imgproc.hpp"
#include <iostream>
using namespace std;
using namespace cv;

static void ContrastAndBright(int, void *);

int g_nContrastValue; // 对比度值
int g_nBrightValue;  // 亮度值
Mat g_srcImage,g_dstImage;

int main(  )
{
       // 读入用户提供的图像
       g_srcImage= imread( "pic1.jpg");
              if(!g_srcImage.data ) { printf("Oh，no，读取 g_srcImage 图片错误~！\n"); return false; }
       g_dstImage= Mat::zeros( g_srcImage.size(), g_srcImage.type() );

       // 设定对比度和亮度的初值
       g_nContrastValue=80;
       g_nBrightValue=80;

       // 创建窗口
       namedWindow("【效果图窗口】", 1);

       // 创建轨迹条
       createTrackbar("对比度：", "【效果图窗口】",&g_nContrastValue,300,ContrastAndBright );
       createTrackbar("亮   度：","【效果图窗口】",&g_nBrightValue,200,ContrastAndBright );

       // 调用回调函数
       ContrastAndBright(g_nContrastValue,0);
       ContrastAndBright(g_nBrightValue,0);

       // 输出一些帮助信息
       cout<<endl<<"\t 嗯。好了，请调整滚动条观察图像效果~\n\n"
                     <<"\t 按下“q”键时，程序退出~!\n"

       // 按下“q”键时，程序退出，不按“q“则一直循环
   	   while(char(waitKey(1)) != 'q') {}
           return 0;
}

//     描述：改变图像对比度和亮度值的回调函数
static void ContrastAndBright(int, void *)
{
       // 创建窗口
       namedWindow("【原始图窗口】", 1);

       // 改变亮度和对比度的三个 for 循环，执行运算 g_dstImage(i,j) =a*g_srcImage(i,j) + b
       for(int y = 0; y < g_srcImage.rows; y++ )
       {
              for(int x = 0; x < g_srcImage.cols; x++ )
              {
                     for(int c = 0; c < 3; c++ )
                     {
                            g_dstImage.at<Vec3b>(y,x)[c]= saturate_cast<uchar>( (g_nContrastValue*0.01)*(g_srcImage.at<Vec3b>(y,x)[c] ) + g_nBrightValue );
                     }
              }
       }

       // 显示图像
       imshow("【原始图窗口】", g_srcImage);
       imshow("【效果图窗口】", g_dstImage);
}
```

## 3. 边缘检测 (Edge Detection)

http://blog.csdn.net/poem_qianmo/article/details/25560901

### 3.1. 操作步骤

1. 滤波：边缘检测的算法主要是基于图像强度的一阶和二阶导数，但导数通常对噪声很敏感，因此必须采用滤波器来改善与噪声有关的边缘检测器的性能。常见的滤波方法主要有高斯滤波，即采用离散化的高斯函数产生一组归一化的高斯核。
1. 增强：增强边缘的基础是确定图像各点邻域强度的变化值。增强算法可以将图像灰度点邻域强度值有显著变化的点凸显出来。在具体编程实现时，可通过计算梯度幅值来确定。
1. 检测：经过增强的图像，往往邻域中有很多点的梯度值比较大，而在特定的应用中，这些点并不是我们要找的边缘点，所以应该采用某种方法来对这些点进行取舍。实际工程中，常用的方法是通过阈值化方法来检测。

最优边缘检测的三个主要评价标准：
- 低错误率：标识出尽可能多的实际边缘，同时尽可能的减少噪声产生的误报。
- 高定位性：标识出的边缘要与图像中的实际边缘尽可能接近。
- 最小响应：图像中的边缘只能标识一次，并且可能存在的图像噪声不应标识为边缘。

### 3.2. Canny 算子

Canny 边缘检测算子是 John F.Canny 于 1986 年开发出来的一个多级边缘检测算法。Canny 边缘检测算法以 Canny 的名字命名，被很多人推崇为当今最优的边缘检测的算法。

下文出现的 soble 边缘检测是基于单一阈值的，我们不能兼顾到低阈值的丰富边缘和高阈值时的边缘缺失这两个问题。而 canny 算子则很好的弥补了这一不足，从目前看来，**canny 边缘检测在做图像轮廓提取方面是最优秀的边缘检测算法**。

canny 边缘检测采用双阈值值法，高阈值用来检测图像中重要的、显著的线条、轮廓等，而低阈值用来保证不丢失细节部分，低阈值检测出来的边缘更丰富，但是很多边缘并不是我们关心的。最后采用一种查找算法，将低阈值中与高阈值的边缘有重叠的线条保留，其他的线条都删除。

```cpp
void Canny(InputArray image,OutputArray edges, double threshold1, double threshold2, int apertureSize=3,bool L2gradient=false);
```
参数说明：
- 第一个参数，InputArray 类型的 image，输入图像，即源图像，填 Mat 类的对象即可，且需为单通道 8 位图像。
- 第二个参数，OutputArray 类型的 edges，输出的边缘图，需要和源图片有一样的尺寸和类型。
- 第三个参数，double 类型的 threshold1，第一个滞后性阈值。
- 第四个参数，double 类型的 threshold2，第二个滞后性阈值。
	需要注意的是，阈值 1 和阈值 2 两者之中的小者用于边缘连接，而大者用来控制强边缘的初始段，推荐的高低阈值比在 2:1 到 3:1 之间。
- 第五个参数，int 类型的 apertureSize，表示应用 Sobel 算子的孔径大小，其有默认值 3。
- 第六个参数，bool 类型的 L2gradient，一个计算图像梯度幅值的标识，有默认值 false。

例：最简单的 Canny 用法
```cpp
// 载入原始图
Mat src = imread("1.jpg");  // 工程目录下应该有一张名为 1.jpg 的素材图
Canny(src, src, 3, 9);
imshow("【效果图】Canny 边缘检测", src);
```

高阶 Canny 用法：先转成灰度图，降噪，再使用 canny，最后将得到的边缘作为掩码，拷贝原图到效果图上，得到彩色的边缘图
```cpp
Mat dst,edge,gray;
dst.create( src1.size(), src1.type() );
cvtColor( src1, gray, CV_BGR2GRAY );
blur( gray, edge, Size(3,3) );
Canny( edge, edge, 3, 9);
imshow("【效果图】Canny 边缘检测 2", edge);

// 将 g_dstImage 内的所有元素设置为 0
dst = Scalar::all(0);
// 使用 Canny 算子输出的边缘图 g_cannyDetectedEdges 作为掩码，来将原图 g_srcImage 拷到目标图 g_dstImage 中
src1.copyTo( dst, edge);
// 显示效果图
imshow("【效果图】Canny 边缘检测 3", dst);
```

![image](http://img.cdn.firejq.com/jpg/2019/3/25/e330a4e1e0706446fa28a6b44a346fe7.jpg)

### 3.3. Sobel 算子

Sobel 算子是一个主要用作边缘检测的离散微分算子 (discrete differentiation operator)。 Sobel 算子结合了高斯平滑和微分求导，用来计算图像灰度函数的近似梯度。在图像的任何一点使用此算子，将会产生对应的梯度矢量或是其法矢量。

Sobel 函数使用扩展的 Sobel 算子，来计算一阶、二阶、三阶或混合图像差分：
```cpp
void Sobel (InputArray src, OutputArray dst, int ddepth, int dx, int dy, int ksize=3, double scale=1, double delta=0, int borderType=BORDER_DEFAULT );
```
参数说明：
- 第一个参数，InputArray 类型的 src，为输入图像，填 Mat 类型即可。
- 第二个参数，OutputArray 类型的 dst，即目标图像，函数的输出参数，需要和源图片有一样的尺寸和类型。
- 第三个参数，int 类型的 ddepth，输出图像的深度，支持如下 src.depth() 和 ddepth 的组合：
  - 若 src.depth() = CV_8U, 取 ddepth =-1/CV_16S/CV_32F/CV_64F
  - 若 src.depth() = CV_16U/CV_16S, 取 ddepth =-1/CV_32F/CV_64F
  - 若 src.depth() = CV_32F, 取 ddepth =-1/CV_32F/CV_64F
  - 若 src.depth() = CV_64F, 取 ddepth = -1/CV_64F
- 第四个参数，int 类型 dx，x 方向上的差分阶数。
- 第五个参数，int 类型 dy，y 方向上的差分阶数。
- 第六个参数，int 类型 ksize，有默认值 3，表示 Sobel 核的大小；必须取 1，3，5 或 7。
- 第七个参数，double 类型的 scale，计算导数值时可选的缩放因子，默认值是 1，表示默认情况下是没有应用缩放的。我们可以在文档中查阅 getDerivKernels 的相关介绍，来得到这个参数的更多信息。
- 第八个参数，double 类型的 delta，表示在结果存入目标图（第二个参数 dst）之前可选的 delta 值，有默认值 0。
- 第九个参数， int 类型的 borderType，我们的老朋友了（万年是最后一个参数），边界模式，默认值为 BORDER_DEFAULT。这个参数可以在官方文档中 borderInterpolate 处得到更详细的信息。

```cpp
void convertScaleAbs(InputArray src, OutputArray dst, double alpha=1, double beta=0)
// 线性变换转换输入数组元素成 8 位无符号整型
// 对于每个输入数组的元素函数 convertScaleAbs 进行三次操作依次是：缩放，得到一个绝对值，转换成无符号 8 位类型
// 为了能够用图像显示，提供一个直观的图形，我们利用该方法，将 -256 — 255 的导数值，转成 0 — 255 的无符号 8 位类型
```
参数说明：
- src – 输入数组。
- dst – 输出数组。
- alpha – 可选缩放比例常数。
- beta – 可选叠加到结果的常数。

```cpp
void addWeighted(InputArray src1, double alpha, InputArray src2, double beta, double gamma, OutputArray dst, intdtype=-1)
// 计算两个矩阵的加权和
// 利用 convertScaleAbs 和 addWeighted，我们可以对梯度进行一个可以用图像显示的近似表达
```
参数说明：
- src1 – 第一个输入数组。
- alpha – 第一个数组的加权系数。
- src2 – 第二个输入数组，必须和第一个数组拥有相同的大小和通道。
- beta – 第二个数组的加权系数。
- dst – 输出数组，和第一个数组拥有相同的大小和通道。
- gamma – 对所有和的叠加的常量。
- dtype – 输出数组中的可选的深度，当两个数组具有相同的深度，此系数可设为 -1，意义等同于选择与第一个数组相同的深度。

例：
```cpp
#include <opencv2/opencv.hpp>
#include<opencv2/highgui/highgui.hpp>
#include<opencv2/imgproc/imgproc.hpp>
using namespace cv;
int main( )
{
    //【0】创建 grad_x 和 grad_y 矩阵
    Mat grad_x, grad_y, abs_grad_x, abs_grad_y, dst;

    //【1】载入原始图
    Mat src = imread("1.jpg");  // 工程目录下应该有一张名为 1.jpg 的素材图

    //【2】显示原始图
    imshow("【原始图】sobel 边缘检测", src);

    //【3】求 X 方向梯度
    Sobel( src, grad_x, CV_16S, 1, 0, 3, 1, 1, BORDER_DEFAULT );
    convertScaleAbs( grad_x, abs_grad_x );
    imshow("【效果图】 X 方向 Sobel", abs_grad_x);

    //【4】求 Y 方向梯度
    Sobel( src, grad_y, CV_16S, 0, 1, 3, 1, 1, BORDER_DEFAULT );
    convertScaleAbs( grad_y, abs_grad_y );
    imshow("【效果图】Y 方向 Sobel", abs_grad_y);

    //【5】合并梯度（近似)
    addWeighted( abs_grad_x, 0.5, abs_grad_y, 0.5, 0, dst );
    imshow("【效果图】整体方向 Sobel", dst);

    waitKey(0);
    return 0;
}
```

![image](http://img.cdn.firejq.com/jpg/2019/3/25/78b65e376f06ad2934ec8f739674756e.jpg)

### 3.4. Laplace 算子

Laplacian 算子是 n 维欧几里德空间中的一个二阶微分算子，定义为梯度 grad() 的散度 div().

由于 Laplacian 使用了图像梯度，它内部的代码其实是调用了 Sobel 算子的。

让一幅图像减去它的 Laplacian 可以增强对比度。

Laplacian 函数可以计算出图像经过拉普拉斯变换后的结果。

```cpp
void Laplacian(InputArray src,OutputArray dst, int ddepth, int ksize=1, double scale=1, double delta=0, intborderType=BORDER_DEFAULT );
```
参数说明：
- 第一个参数，InputArray 类型的 image，输入图像，即源图像，填 Mat 类的对象即可，且需为单通道 8 位图像。
- 第二个参数，OutputArray 类型的 edges，输出的边缘图，需要和源图片有一样的尺寸和通道数。
- 第三个参数，int 类型的 ddept，目标图像的深度。
- 第四个参数，int 类型的 ksize，用于计算二阶导数的滤波器的孔径尺寸，大小必须为正奇数，且有默认值 1。
- 第五个参数，double 类型的 scale，计算拉普拉斯值的时候可选的比例因子，有默认值 1。
- 第六个参数，double 类型的 delta，表示在结果存入目标图（第二个参数 dst）之前可选的 delta 值，有默认值 0。
- 第七个参数， int 类型的 borderType，边界模式，默认值为 BORDER_DEFAULT。这个参数可以在官方文档中 borderInterpolate() 处得到更详细的信息。

例：
```cpp
int main( )
{
    //【0】变量的定义
    Mat src,src_gray,dst, abs_dst;
    //【1】载入原始图
    src = imread("1.jpg");  // 工程目录下应该有一张名为 1.jpg 的素材图
    //【2】显示原始图
imshow("【原始图】图像 Laplace 变换", src);

    //【3】使用高斯滤波消除噪声
    GaussianBlur( src, src, Size(3,3), 0, 0, BORDER_DEFAULT );
    //【4】转换为灰度图
    cvtColor( src, src_gray, CV_RGB2GRAY );

    //【5】使用 Laplace 函数
    Laplacian( src_gray, dst, CV_16S, 3, 1, 0, BORDER_DEFAULT );
    //【6】计算绝对值，并将结果转换成 8 位
    convertScaleAbs( dst, abs_dst );

    //【7】显示效果图
    imshow( "【效果图】图像 Laplace 变换", abs_dst );
    waitKey(0);
    return 0;
}
```

![image](http://img.cdn.firejq.com/jpg/2019/3/25/8f3684061969f93319ed9e5053b124c4.jpg)

### 3.5. scharr 滤波器

scharr 滤波器在 OpenCV 中主要是配合 Sobel 算子的运算而存在的。

```cpp
void Scharr(InputArray src, OutputArray dst, int ddepth, int dx, int dy, double scale=1, double delta=0, intborderType=BORDER_DEFAULT );
```

参数说明：
- 第一个参数，InputArray 类型的 src，为输入图像，填 Mat 类型即可。
- 第二个参数，OutputArray 类型的 dst，即目标图像，函数的输出参数，需要和源图片有一样的尺寸和类型。
- 第三个参数，int 类型的 ddepth，输出图像的深度，支持如下 src.depth() 和 ddepth 的组合：
  - 若 src.depth() = CV_8U, 取 ddepth =-1/CV_16S/CV_32F/CV_64F
  - 若 src.depth() = CV_16U/CV_16S, 取 ddepth =-1/CV_32F/CV_64F
  - 若 src.depth() = CV_32F, 取 ddepth =-1/CV_32F/CV_64F
  - 若 src.depth() = CV_64F, 取 ddepth = -1/CV_64F
- 第四个参数，int 类型 dx，x 方向上的差分阶数。
- 第五个参数，int 类型 dy，y 方向上的差分阶数。
- 第六个参数，double 类型的 scale，计算导数值时可选的缩放因子，默认值是 1，表示默认情况下是没有应用缩放的。
- 第七个参数，double 类型的 delta，表示在结果存入目标图（第二个参数 dst）之前可选的 delta 值，有默认值 0。
- 第八个参数， int 类型的 borderType，我们的老朋友了（万年是最后一个参数），边界模式，默认值为 BORDER_DEFAULT。

使用 Scharr 滤波器运算符计算 x 或 y 方向的图像差分。其实它的参数变量和 Sobel 基本上是一样的，除了没有 ksize 核的大小
```cpp
Scharr(src, dst, ddepth, dx, dy, scale, delta, borderType);
// 等价于
Sobel(src, dst, ddepth, dx, dy, CV_SCHARR,scale, delta, borderType);
```

例：
```cpp
int main( )
{
    //【0】创建 grad_x 和 grad_y 矩阵
    Mat grad_x, grad_y;
    Mat abs_grad_x, abs_grad_y,dst;

    //【1】载入原始图
    Mat src = imread("1.jpg");  // 工程目录下应该有一张名为 1.jpg 的素材图

    //【2】显示原始图
    imshow("【原始图】Scharr 滤波器", src);

    //【3】求 X 方向梯度
    Scharr( src, grad_x, CV_16S, 1, 0, 1, 0, BORDER_DEFAULT );
    convertScaleAbs( grad_x, abs_grad_x );
    imshow("【效果图】 X 方向 Scharr", abs_grad_x);

    //【4】求 Y 方向梯度
    Scharr( src, grad_y, CV_16S, 0, 1, 1, 0, BORDER_DEFAULT );
    convertScaleAbs( grad_y, abs_grad_y );
    imshow("【效果图】Y 方向 Scharr", abs_grad_y);

    //【5】合并梯度（近似)
    addWeighted( abs_grad_x, 0.5, abs_grad_y, 0.5, 0, dst );

    //【6】显示效果图
    imshow("【效果图】合并梯度后 Scharr", dst);

    waitKey(0);
    return 0;
}
```

### 3.6. Structured forest 算法

## 4. 轮廓检测 (Contour Detection)

轮廓（Contours），指的是有相同颜色或者密度，连接所有连续点的一条曲线。检测轮廓的工作对形状分析和物体检测与识别都非常有用。

在轮廓检测之前，首先要对图片进行二值化或者 Canny 边缘检测。在 OpenCV 中，寻找的物体是白色的，而背景必须是黑色的，因此图片预处理时必须保证这一点。

http://blog.csdn.net/corcplusplusorjava/article/details/20536251

OpenCV 里提取目标轮廓的函数是 findContours，它的输入图像是一幅二值图像，输出的是每一个连通区域的轮廓点的集合：vector<vector<Point>>。外层 vector 的 size 代表了图像中轮廓的个数，里面 vector 的 size 代表了轮廓上点的个数。

### 4.1. findContours 函数（检测轮廓）

```cpp
void findContours(InputOutputArray image, OutputArrayOfArrays contours, int mode, int method, Point offset=Point());// 重载函数一

void findContours( InputOutputArray image, OutputArrayOfArrays contours, OutputArray hierarchy, int mode, int method, Point offset=Point());// 重载函数二
```
参数说明：
- image：输入图像必须为一个 2 值单通道图像；
- contours：检测的轮廓数组，每一个轮廓用一个 point 类型的 vector 表示；
- hiararchy：和轮廓个数相同，每个轮廓 contours[i] 对应 4 个 hierarchy 元素 hierarchy[i](0) ~ hierarchy[i](3)，分别表示后一个轮廓、前一个轮廓、父轮廓、内嵌轮廓的索引编号，如果没有对应项，该值设置为负数。
- mode：表示轮廓的检索模式
  - CV_RETR_EXTERNAL 表示只检测外轮廓
  - CV_RETR_LIST 检测的轮廓不建立等级关系
  - CV_RETR_CCOMP 建立两个等级的轮廓，上面的一层为外边界，里面的一层为内孔的边界信息。如果内孔内还有一个连通物体，这个物体的边界也在顶层。
  - CV_RETR_TREE 建立一个等级树结构的轮廓。具体参考 contours.c 这个 demo
- method：为轮廓的近似办法
  - CV_CHAIN_APPROX_NONE 存储所有的轮廓点，相邻的两个点的像素位置差不超过 1，即 max（abs（x1-x2），abs（y2-y1））==1
  - CV_CHAIN_APPROX_SIMPLE 压缩水平方向，垂直方向，对角线方向的元素，只保留该方向的终点坐标，例如一个矩形轮廓只需 4 个点来保存轮廓信息
  - CV_CHAIN_APPROX_TC89_L1，CV_CHAIN_APPROX_TC89_KCOS 使用 teh-Chinl chain 近似算法
- offset：代表轮廓点的偏移量，可以设置为任意值。对 ROI 图像中找出的轮廓，并要在整个图像中进行分析时，这个参数还是很有用的。比如你要从图像的（100, 0）开始进行轮廓检测，那么就传入（100, 0）。

### 4.2. approxPolyDP 函数（点集逼近）

```cpp
void approxPolyDP(InputArray curve, OutputArray approxCurve, double epsilon, bool closed);
```
参数说明：
- InputArray curve：输入的点集
- OutputArray approxCurve：输出的点集，当前点集是能最小包容指定点集的。draw 出来即是一个多边形；
- double epsilon：指定的精度，也即是原始曲线与近似曲线之间的最大距离。
- bool closed：若为 true, 则说明近似曲线是闭合的，它的首位都是相连，反之，若为 false，则断开。

注：findContours 后的轮廓信息 contours 可能过于复杂不平滑，可以用 approxPolyDP 函数对该多边形曲线做适当近似。

### 4.3. drawContours 函数（绘制轮廓）

```cpp
void drawContours(InputOutputArray image, InputArrayOfArrays contours, int contourIdx, const Scalar& color, int thickness=1, int lineType=8, InputArray hierarchy=noArray(), int maxLevel=INT_MAX, Point offset=Point() )
```
参数说明：
- InputOutputArray image：要绘制轮廓的图像
- InputArrayOfArrays contours：所有输入的轮廓，每个轮廓被保存成一个 point 向量
- int contourIdx：指定要绘制轮廓的编号，如果是负数，则绘制所有的轮廓
- const Scalar& color：绘制轮廓所用的颜色
- int thickness=1：绘制轮廓的线的粗细，如果是负数，则轮廓内部被填充
int lineType=8：绘制轮廓的线的连通性
- InputArray hierarchy=noArray()：关于层级的可选参数，只有绘制部分轮廓时才会用到
- int maxLevel=INT_MAX：绘制轮廓的最高级别，这个参数只有 hierarchy 有效的时候才有效
  - maxLevel=0，绘制与输入轮廓属于同一等级的所有轮廓即输入轮廓和与其相邻的轮廓
  - maxLevel=1, 绘制与输入轮廓同一等级的所有轮廓与其子节点。
  - maxLevel=2，绘制与输入轮廓同一等级的所有轮廓与其子节点以及子节点的子节点
- Point offset=Point()：表示偏移量，如果传入（10，20），那绘制将从图像的（10，20）处开始

注：findContours() 运行的时候，这个图像会被直接涂改，因此如果是将来还有用的图像，应该复制之后再传给 findContours()。

例：
```cpp
Mat image = imread("3.jpg");
cvtColor(image, image, COLOR_RGB2GRAY);// 灰度化
threshold(image, image, 20, 255, CV_THRESH_BINARY);// 二值化

// 获取轮廓不包括轮廓内的轮廓
//vector< vector<Point> > contours;
//Mat result(image.size(), CV_8U, Scalar(255));
//findContours(image, contours, CV_RETR_EXTERNAL, CV_CHAIN_APPROX_NONE);
//drawContours(result, contours, -1, Scalar(0), 2);
//imshow("resultImage", result);

// 获取所有轮廓包括轮廓内的轮廓
vector< vector<Point> > allContours;
Mat allContoursResult(image.size(), CV_8UC3, Scalar(255));
findContours(image, allContours, CV_RETR_LIST, CV_CHAIN_APPROX_NONE);
drawContours(allContoursResult, allContours, -1, Scalar(0), 2);
imshow("allContours", allContoursResult);

waitKey(0);
return 0;
```

### 4.4. 获取轮廓的外接图形

- 获取包围对象的垂直矩阵

  ```cpp
  cv::Rect r0= cv::boundingRect(cv::Mat(contours[0]));  // 在轮廓中获取外接矩形
  cv::rectangle(result,r0,cv::Scalar(0),2);  // 根据得到的矩形，绘制矩形
  ```

- 获取包围对象的最小圆

  ```cpp
  cv::Rect r0= cv::boundingRect(cv::Mat(contours[0]));
  cv::rectangle(result,r0,cv::Scalar(0),2);
  ```

- 获取包围对象的多边形

  ```cpp
  std::vector<cv::Point> poly;
  cv::approxPolyDP(cv::Mat(contours[2]),poly, 5, true); // yes it is a closed shape
  ```

- 获得包围对象的凸包

  ```cpp
  std::vector<cv::Point> hull;
  cv::convexHull(cv::Mat(contours[3]),hull);
  ```

- 获取各个对象的质心

  ```cpp
  itc = contours.begin();
  while (itc!=contours.end()) {
  // compute all moments
  cv::Moments mom= cv::moments(cv::Mat(*itc++));
  // draw mass center
  cv::circle(result,
  // position of mass center converted to integer
  cv::Point(mom.m10/mom.m00,mom.m01/mom.m00),  2,cv::Scalar(0),2); // draw black dot
  }
  ```

例：
```cpp
Mat image = imread("3.jpg");
cvtColor(image, image, COLOR_RGB2GRAY);// 灰度化
threshold(image, image, 20, 255, CV_THRESH_BINARY);// 二值化

// 获取所有轮廓包括轮廓内的轮廓
vector< vector<Point> > allContours;
Mat allContoursResult(image.size(), CV_8U, Scalar(255));
findContours(image, allContours, CV_RETR_LIST, CV_CHAIN_APPROX_NONE);
drawContours(allContoursResult, allContours, -1, Scalar(0), 2);
imshow("allContours", allContoursResult);
// 绘制每一个轮廓的外接矩形
Mat result(image.size(), CV_8U, Scalar(255));
for (int i = 0; i < allContours.size(); i++)
{
Rect r = boundingRect(cv::Mat(allContours[i]));
rectangle(result, r, Scalar(0), 2);
}
imshow("result", result);
```

## 5. 图像分割 (Image Segmentation)

图像分割定义：http://baike.baidu.com/item/%E5%9B%BE%E5%83%8F%E5%88%86%E5%89%B2#2_4

必看 http://dsqiu.iteye.com/blog/1669891

### 5.1. 轮廓检测、边缘检测、图像分割三者联系

http://wangmurong.org.cn/2017/03/01/2015-10-21-edge-detection-segmentation-contour-detection/

在图像处理中，轮廓检测 (Contour Detection), 边缘检测 (Edge Detection)，图像分割 (Image Segmentation) 三个概念联系十分紧密，但从具体概念来说并不一样。下面是这三个概念的直观定义：
- 图像分割是指将数字图像分割成多个图像小块（即像素集合，如超像素) 的过程。
- 边缘检测是检测数字图像中明暗变化剧烈或者说是不变化不连续的像素点。
- 轮廓检测是指检测图像中的对象边界。

图像分割和边缘检测从定义就可以很好的区分，轮廓检测和边缘检测的区别是在语义层次上，轮廓检测更偏向于关注上层语义，主要目标是为了确定图像中的语义对象的边界；而边缘检测只关注的是图像中像素点的变化。另外，从结果的表现形式来看，轮廓检测得到的轮廓一定是封闭的，而边缘检测得到的边缘与图像分割得到的分割边界通常都不是封闭的。图像的轮廓检测通常可以先检测边缘，再将检测到的边缘进行进一步处理，得到图像的轮廓。图像分割也需要先对边缘进行检测，因此可以说边缘检测是图像分割与轮廓检测的基础性工作。

### 5.2. 阈值分割

阈值分割法是一种基于区域的图像分割技术。其基本原理是：通过设定不同的特征阈值，把图像象素点分为若干类。

根据图像阈值化算法所依据的信息源，可将阈值化方法分为五类：
- 基于聚类的方法：数据聚类中，总的数据集被划分为属性相似的子类，例如将灰度级聚类成为两部分：前景物体部分和背景部分。
- 基于直方图的方法：在直方图的峰、谷和直方图的圆滑曲线上进行分析。
- 基于熵的方法：熵方法将区域分为背景区域和前景区域，前景区域通常是物体部分（在一些热红外图像中，背景部分是物体) 。该方法是通过最小化一个熵函数来实现的，交叉熵函数包含了介于原图和其二值图像之间的保留信息。
- 基于空间方法：使用概率密度函数模型，考虑全局范围内的像素之间的相似关系。
- 基于自适应方法：局部方法不能决定单一的阈值，自适应阈值依赖于局部图像特点。

#### 5.2.1. 固定阈值化函数 threshould()

```cpp
double threshold(InputArray src, OutputArray dst, double thresh, double max_value, int threshold_type)
```
参数说明：
- 第一个参数表示输入图像，必须为单通道灰度图；
- 第二个参数表示输出的边缘图像，为单通道黑白图；
- 第三个参数表示阈值；
- 第四个参数表示设定的最大灰度值（该参数运用在二进制与反二进制阈值操作中）；
- 第五个参数表示阈值运算类型：
  ```
  /* Threshold types */
  enum
  {
  CV_THRESH_BINARY      	=0,  二进制阈值化，参数值为 0，像素值大于阈值设为 255，反正设为 0
  CV_THRESH_BINARY_INV  =1,  反二进制阈值化，参数值为 1，大于阈值设为 0，反之设为 255
      CV_THRESH_TRUNC       	=2,  截断阈值化，参数值为 2，大于阈值的像素点不进行任何改变，其余灰度值全部变为 0
      CV_THRESH_TOZERO      	=3,  阈值化为 0，参数值为 3，大于阈值的像素设为 0，其余不做任何改变
      CV_THRESH_TOZERO_INV  =4,  反阈值化为 0，参数值为 4，小于阈值的像素设为 0，其余不做任何改变
      CV_THRESH_MASK        	=7,
      CV_THRESH_OTSU        	=8,  使用 OTSU 阈值分割算法，参数中的阈值不再起作用（可直接设为 0）
  };
  ```

#### 5.2.2. 自适应阈值化函数：adaptiveThreshold()

```cpp
void adaptiveThreshold( InputArray src, OutputArray dst, double maxValue, int adaptiveMethod, int thresholdType, int blockSize, double C );
```
参数说明：
- src- 输入图像。
- dst- 输出图像。
- max_value- 使用 CV_THRESH_BINARY 和 CV_THRESH_BINARY_INV 的最大值。
adaptive_method- 自适应阈值算法使用：CV_ADAPTIVE_THRESH_MEAN_C 或 CV_ADAPTIVE_THRESH_GAUSSIAN_C .
- threshold_type- 取阈值类型：必须是 CV_THRESH_BINARY 或者 CV_THRESH_BINARY_INV.
- block_size- 用来计算阈值的象素邻域大小：3, 5, 7, ...
- param1- 与方法有关的参数。对方法 CV_ADAPTIVE_THRESH_MEAN_C 和 CV_ADAPTIVE_THRESH_GAUSSIAN_C， 它是一个从均值或加权均值提取的常数，尽管它可以是负数。对方法 CV_ADAPTIVE_THRESH_MEAN_C，先求出块中的均值，再减掉 param1。对方法 CV_ADAPTIVE_THRESH_GAUSSIAN_C ，先求出块中的加权和 (gaussian)，再减掉 param1。当 block_size 比较小的时候，相当于提取边缘。

注意：threshold 函数输入图像可以是 8 位也可以是 32 位，即可以是灰度图也可以为彩色图，但 adaptiveThreshold 输入图像必须是 8 位，即必须是灰度图。

#### 5.2.3. 示例

https://www.cnblogs.com/pengkunfan/p/3526091.html

- 全局固定阈值；
  ```cpp
  // 全局二值化
  int th = 100;
  Mat global;
  threshold(image, global, th, 255, 0);// 最后一个参数从 0~8 指定了不同的阈值操作方法
  ```
- 局部自适应阈值；
  ```cpp
  // 局部二值化
  int blockSize = 25;
  int constValue = 10;
  Mat local;
  adaptiveThreshold(image, local, 255, CV_ADAPTIVE_THRESH_MEAN_C, CV_THRESH_BINARY_INV, blockSize, constValue);
  ```
- OTSU（大津二值化算法)；
  ```cpp
  // 运用 OTSU 二值化
  threshold(image, local, 0, 255, CV_THRESH_OTSU);
  ```

### 5.3. 区域分割

### 5.4. 边缘分割

http://www.jianshu.com/p/4ffdf060fe57

### 5.5. 直方图法

https://mayfly.gitbooks.io/opencv/content/zhi_fang_tu_fa.html

### 5.6. 前后景分割

http://blog.csdn.net/u011630458/article/details/45723895

### 5.7. 通道分割 / 混合

分割 RGB 图像为若干单通道图像：
```cpp
void split(const Mat& mtx, vector<Mat>& mv)
```
（对偶运算）将若干单通道图像合并：
```cpp
void merge(const vector<Mat>& mv, OutputArray dst)
```

通过滑动条操作分割多通道图像 http://blog.csdn.net/qq_23968185/article/details/51272361

### 5.8. 特定理论

## 6. 图像混合

http://blog.csdn.net/poem_qianmo/article/details/20911629

## 7. 噪声

### 7.1. 高斯噪声

高斯噪声指噪声服从高斯分布，即某个强度的噪声点个数最多，离这个强度越远噪声点个数越少，且这个规律服从高斯分布。高斯噪声是一种加性噪声，即噪声直接加到原图像上，因此可以用线性滤波器滤除。

### 7.2. 椒盐噪声 / 脉冲噪声

椒盐噪声是由图像传感器，传输信道，解码处理等产生的黑白相间的亮暗点噪声。椒盐噪声类似把椒盐撒在图像上，因此得名，是一种在图像上出现很多白点或黑点的噪声，如电视里的雪花噪声等。椒盐噪声可以认为是一种逻辑噪声，用线性滤波器滤除的结果不好，一般采用中值滤波器滤波可以得到较好的结果。

椒盐噪声是指两种噪声，一种是盐噪声（salt noise）盐 = 白色 (255)，另一种是胡椒噪声（pepper noise），椒 = 黑色 (0)。前者是高灰度噪声，后者属于低灰度噪声。一般两种噪声同时出现，呈现在图像上就是黑白杂点。对于彩色图像，也有可能表现为在单个像素 BGR 三个通道随机出现的 255 或 0。

opencv 添加椒盐噪声 API:
```cpp
void salt(cv::Mat image, int n)  // 添加盐噪声
void pepper(cv::Mat image, int n)  // 添加椒噪声
```
参数说明：
- image — 输入图像（输出图像）灰度或彩色模式
- n — 噪点个数

例：
```cpp
// 盐噪声
void salt(cv::Mat image, int n) {

    int i,j;
    for (int k=0; k<n/2; k++) {

        // rand() is the random number generator
        i = std::rand()%image.cols; // % 整除取余数运算符，rand=1022,cols=1000,rand%cols=22
        j = std::rand()%image.rows;

        if (image.type() == CV_8UC1) { // gray-level image

            image.at<uchar>(j,i)= 255; //at 方法需要指定 Mat 变量返回值类型，如 uchar 等

        } else if (image.type() == CV_8UC3) { // color image

            image.at<cv::Vec3b>(j,i)[0]= 255; //cv::Vec3b 为 opencv 定义的一个 3 个值的向量类型
            image.at<cv::Vec3b>(j,i)[1]= 255; //[] 指定通道，B:0，G:1，R:2
            image.at<cv::Vec3b>(j,i)[2]= 255;
        }
    }
}

// 椒噪声
void pepper(cv::Mat image, int n) {

    int i,j;
    for (int k=0; k<n; k++) {

        // rand() is the random number generator
        i = std::rand()%image.cols; // % 整除取余数运算符，rand=1022,cols=1000,rand%cols=22
        j = std::rand()%image.rows;

        if (image.type() == CV_8UC1) { // gray-level image

            image.at<uchar>(j,i)= 0; //at 方法需要指定 Mat 变量返回值类型，如 uchar 等

        } else if (image.type() == CV_8UC3) { // color image

            image.at<cv::Vec3b>(j,i)[0]= 0; //cv::Vec3b 为 opencv 定义的一个 3 个值的向量类型
            image.at<cv::Vec3b>(j,i)[1]= 0; //[] 指定通道，B:0，G:1，R:2
            image.at<cv::Vec3b>(j,i)[2]= 0;
        }
    }
}
```

注意：若想要分通道加入椒盐噪声，只需要在 `image.at<cv::Vec3b>(j,i)[0]` 或 [1]、[2] 三个通道随机加入 255 或 0 即可。

### 7.3. 降噪 / 去噪方法

#### 7.3.1. 均值滤波

均值滤波也称为线性滤波，其采用的主要方法为邻域平均法。其基本原理是用均值替代原图像中的各个像素值，即对待处理的当前像素点（x,y）, 选择一个模板，该模板由其近邻的若干像素组成，求模板中所有像素的均值，再把该均值赋予当前像素点 (x,y), 作为处理后图像在该点上的灰度 g(x,y)。

这种算法简单，处理速度快，主要缺点：降低噪声的同时使图像产生模糊，特别是在边缘和细节处。而且邻域越大，在去噪能力增强的同时模糊程度越严重。

#### 7.3.2. 中值滤波

中值滤波是基于排序统计理论的一种能有效抑制噪声的非线性信号处理技术。

其实现原理如下：将某个像素邻域中的像素按灰度值进行排序，然后选择该序列的中间值作为输出的像素值，让周围像素灰度值的差比较大的像素改取与周围的像素值接近的值，从而可以消除孤立的噪声点。利用中值滤波算法可以很好地对图像进行平滑处理。

这种算法简单，时间复杂度低，但其对点、线和尖顶多的图像不宜采用中值滤波。很容易自适应化。

#### 7.3.3. 小波变换

小波变换是一种窗口大小固定但其形状可改变的时频局部化分析方法。

小波变换利用非均匀的分辨率，即在低频段用高的频率分辨率和低的时间分辨率（宽的分析窗口）；而在高频段利用低的频率分辨率和高的时间分辨率（窄的分析窗口），这样就能有效地从信号（如语言、图像等）中提取信息，较好地解决了时间和频率分辨率的矛盾。对于一副图像，我们关心的是它的低频分量，因为低频分量是保持信号特性的重要部分，高频分量则仅仅起到提供信号细节的作用，而且噪声也大多属于高频信息。这样，利用小波变换，噪声信息大多集中在次低频、次高频、以及高频子块中，特别是高频子块，几乎以噪声信息为主，为此，将高频子块置为零，对次低频和次高频子块进行一定的抑制，则可以达到一定的噪声去除效果。

#### 7.3.4. 比较

对于降低椒盐噪声，中值滤波效果比均值滤波效果好。原因：
- 椒盐噪声是幅值近似相等但随机分布在不同的位置上，图像中有干净点也有污染点。
- 中值滤波是选择适当的点来代替污染点的值，所以处理效果好。
- 因为噪声的均值不为零，所以均值滤波不能很好地去除噪声点。

对于高斯噪声，均值滤波效果比中值滤波效果好。原因：
- 高斯噪声是幅值近似正态分布，但分布在每点像素上。
- 因为图像中的每点都是污染点，所以中值滤波选不到合适的干净点。
- 因为正态分布的均值为零，所以均值滤波可以削弱噪声

## 8. 滤波

http://blog.csdn.net/poem_qianmo/article/details/22745559

图像滤波，即在尽量保留图像细节特征的条件下对目标图像的噪声进行抑制，是图像预处理中不可缺少的操作，其处理效果的好坏将直接响到后续图像处理和分析的有效性和可靠性。（滤波就是要去除没用的信息，保留有用的信息，可能是低频，也可能是高频）

滤波的目的有两个：一是抽出对象的特征作为图像识别的特征模式；另一个是为适应图像处理的要求，消除图像数字化时所混入的噪声。

对滤波处理的要求有两条：一是不能损坏图像的轮廓及边缘等重要信息；二是使图像清晰视觉效果好。

图像的滤波方法很多，主要可以分为以下几类
- 频率域 / 空间域滤波
  - 频率域滤波

    频率域法的处理是在图像的某种变换域内，对图像的变换系数值进行运算，然后通过逆变换获得增强图像。这是一种间接的图像滤波方法。是将图像从空间或时间域转换到频率域，再利用变换系数反映某些图像特征的性质进行图像滤波的方法。

    傅立叶变换是一种常用的变换。在傅立叶变换域，频谱的直流分量正比于图像的平均亮度，噪声对应于频率较高的区域，图像实体位于频率较低的区域。图像在变换具有的这些内在特性可被用于图像滤波。可以构造一个低通滤波器，使低频分量顺利通过而有效地阻于高频分量，即可滤除图像的噪声，再经过反变换来取得平滑的图像。

  - 空间域滤波

    空间滤波方法是一类直接的滤波方法，它在处理图像时直接对图像灰度作运算。

    一类是拟合图像的方法，包括 n 阶多项式拟合、离散正交多项式拟合、二次曲面拟合等多种方法；另一类是平滑图像的方法，包括领域平均法、中值滤波法、梯度倒数加权法、选择式掩模法等。

- 线性滤波

  线性滤波器的原始数据与滤波结果是一种算术运算，即用加减乘除等运算实现，如 (1) 均值滤波器（模板内像素灰度值的平均值）、(2) 高斯滤波器（高斯加权平均值）等。由于线性滤波器是算术运算，有固定的模板，因此滤波器的转移函数是可以确定并且是唯一的（转移函数即模板的傅里叶变换）。

  线性滤波器经常用于剔除输入信号中不想要的频率或者从许多频率中选择一个想要的频率。

  常见的线性滤波器
  - 高通滤波器

    边缘提取与增强。边缘区域的灰度变换加大，也就是频率较高。所以，对于高通滤波，边缘部分将被保留，非边缘部分将被过滤，即只允许高频率通过；

    注意：高通滤波即是锐化处理；

  - 低通滤波器

    边缘平滑，边缘区域将被平滑过渡。即只允许低频率通过；

    注意：低通滤波即是 “平滑处理“（smoothing）也称“模糊处理”（bluring），空间域图像平滑方法主要用低通卷积滤波、中值滤波等；频率域图像平滑常用的低通滤波器有低通梯形滤波器、低通高斯滤波器、低通指数滤波器、巴特沃思低通滤波器等；

  - 带通滤波器

    允许一定范围频率通过；

  - 带阻滤波器

    阻止一定范围频率通过并且允许其它频率通过；

  - 全通滤波器

    允许所有频率通过、仅仅改变相位关系；

  - 陷波滤波器（Band-stop filter）

    阻止一个狭窄频率范围通过的特殊带阻滤波器；

### 8.1. opencv 线性滤波 API

#### 8.1.1. 方框滤波——boxblur 函数

方框滤波（box Filter）被封装在一个名为 boxblur 的函数中，即 boxblur 函数的作用是使用方框滤波器（box filter）来模糊一张图片，从 src 输入，从 dst 输出。

```
void boxFilter(InputArray src,OutputArray dst, int ddepth, Size ksize, Point anchor=Point(-1,-1), boolnormalize=true, int borderType=BORDER_DEFAULT )
```
参数说明：
- 第一个参数，InputArray 类型的 src，输入图像，即源图像，填 Mat 类的对象即可。该函数对通道是独立处理的，且可以处理任意通道数的图片，但需要注意，待处理的图片深度应该为 CV_8U, CV_16U, CV_16S, CV_32F 以及 CV_64F 之一。
- 第二个参数，OutputArray 类型的 dst，即目标图像，需要和源图片有一样的尺寸和类型。
- 第三个参数，int 类型的 ddepth，输出图像的深度，-1 代表使用原图深度，即 src.depth()。
- 第四个参数，Size 类型（对 Size 类型稍后有讲解）的 ksize，内核的大小。一般这样写 Size( w,h ) 来表示内核的大小 ( 其中，w 为像素宽度， h 为像素高度)。Size（3,3）就表示 3x3 的核大小，Size（5,5）就表示 5x5 的核大小
- 第五个参数，Point 类型的 anchor，表示锚点（即被平滑的那个点），注意他有默认值 Point(-1,-1)。如果这个点坐标是负值的话，就表示取核的中心为锚点，所以默认值 Point(-1,-1) 表示这个锚点在核的中心。
- 第六个参数，bool 类型的 normalize，默认值为 true，一个标识符，表示内核是否被其区域归一化（normalized）了。
- 第七个参数，int 类型的 borderType，用于推断图像外部像素的某种边界模式。有默认值 BORDER_DEFAULT，我们一般不去管它。

注意：
- 当 normalize=true 的时候，方框滤波就变成了我们熟悉的均值滤波。也就是说，均值滤波是方框滤波归一化（normalized）后的特殊情况。其中，归一化就是把要处理的量都缩放到一个范围内，比如 (0,1)，以便统一处理和直观量化。而非归一化（Unnormalized）的方框滤波用于计算每个像素邻域内的积分特性，比如密集光流算法（dense optical flow algorithms）中用到的图像倒数的协方差矩阵（covariance matrices of image derivatives）
- 如果我们要在可变的窗口中计算像素总和，可以使用 integral() 函数。

例：
```cpp
// 载入原图并创建同样尺寸的目标图
Mat image=imread("2.jpg");
Mat out=Mat::zeros(image.size(), image.type());
// 进行均值滤波操作
boxFilter(image, out, -1, Size(5, 5));
```
效果：

![image](http://img.cdn.firejq.com/jpg/2019/3/25/536e43690483f4c4ec1f43932ae27a30.jpg)

#### 8.1.2. 均值滤波——blur 函数

均值滤波 / 归一化滤波，是最简单的一种滤波操作，输出图像的每一个像素是核窗口内输入图像对应像素的像素的平均值 ( 所有像素加权系数相等)，其实说白了它就是归一化后的方框滤波。

blur 函数内部中其实就是调用了一下 boxFilter。

均值滤波是典型的线性滤波算法，主要方法为邻域平均法，即用一片图像区域的各个像素的均值来代替原图像中的各个像素值。

均值滤波本身存在着固有的缺陷，即它不能很好地保护图像细节，在图像去噪的同时也破坏了图像的细节部分，从而使图像变得模糊，不能很好地去除噪声点。

```cpp
void blur(InputArray src, OutputArraydst, Size ksize, Point anchor=Point(-1,-1), int borderType=BORDER_DEFAULT )
```
参数说明：
- 第一个参数，InputArray 类型的 src，输入图像，即源图像，填 Mat 类的对象即可。该函数对通道是独立处理的，且可以处理任意通道数的图片，但需要注意，待处理的图片深度应该为 CV_8U, CV_16U, CV_16S, CV_32F 以及 CV_64F 之一。
- 第二个参数，OutputArray 类型的 dst，即目标图像，需要和源图片有一样的尺寸和类型。比如可以用 Mat::Clone，以源图片为模板，来初始化得到如假包换的目标图。
- 第三个参数，Size 类型（对 Size 类型稍后有讲解）的 ksize，内核的大小。一般这样写 Size( w,h ) 来表示内核的大小 ( 其中，w 为像素宽度， h 为像素高度)。Size（3,3）就表示 3x3 的核大小，Size（5,5）就表示 5x5 的核大小
- 第四个参数，Point 类型的 anchor，表示锚点（即被平滑的那个点），注意他有默认值 Point(-1,-1)。如果这个点坐标是负值的话，就表示取核的中心为锚点，所以默认值 Point(-1,-1) 表示这个锚点在核的中心。
- 第五个参数，int 类型的 borderType，用于推断图像外部像素的某种边界模式。有默认值 BORDER_DEFAULT，我们一般不去管它。

例：
```cpp
// 载入原图并创建同样尺寸的目标图
Mat image=imread("2.jpg");
Mat out=Mat::zeros(image.size(), image.type());
// 进行均值滤波操作
blur(image, out, Size(7, 7));
```

#### 8.1.3. 高斯滤波——GaussianBlur 函数

阮一峰讲高斯模糊 http://www.ruanyifeng.com/blog/2012/11/
gaussian_blur.html

高斯滤波是一种线性平滑滤波，适用于消除高斯噪声，广泛应用于图像处理的减噪过程。通俗的讲，高斯滤波就是对整幅图像进行加权平均的过程，每一个像素点的值，都由其本身和邻域内的其他像素值经过加权平均后得到。高斯滤波的具体操作是：用一个模板（或称卷积、掩模）扫描图像中的每一个像素，用模板确定的邻域内像素的加权平均灰度值去替代模板中心像素点的值。

高斯模糊技术生成的图像，其视觉效果就像是经过一个半透明屏幕在观察图像，这与镜头焦外成像效果散景以及普通照明阴影中的效果都明显不同。从数学的角度来看，图像的高斯模糊过程就是图像与正态分布做卷积。由于正态分布又叫作高斯分布，所以这项技术就叫作高斯模糊。

图像与圆形方框模糊做卷积将会生成更加精确的焦外成像效果。由于高斯函数的傅立叶变换是另外一个高斯函数，所以高斯模糊对于图像来说就是一个低通滤波操作。

高斯滤波器是一类根据高斯函数的形状来选择权值的线性平滑滤波器，对于抑制服从正态分布的噪声非常有效。

高斯滤波尝尝被称为最有效的滤波器，但实际运行效率不高。

GaussianBlur 函数的作用是用高斯滤波器来模糊一张图片，对输入的图像 src 进行高斯滤波后用 dst 输出。它将源图像和指定的高斯核函数做卷积运算，并且支持就地过滤（In-placefiltering）：
```cpp
void GaussianBlur(InputArray src,OutputArray dst, Size ksize, double sigmaX, double sigmaY=0, intborderType=BORDER_DEFAULT )
```

参数说明：
- 第一个参数，InputArray 类型的 src，输入图像，即源图像，填 Mat 类的对象即可。它可以是单独的任意通道数的图片，但需要注意，图片深度应该为 CV_8U,CV_16U, CV_16S, CV_32F 以及 CV_64F 之一。
- 第二个参数，OutputArray 类型的 dst，即目标图像，需要和源图片有一样的尺寸和类型。比如可以用 Mat::Clone，以源图片为模板，来初始化得到如假包换的目标图。
- 第三个参数，Size 类型的 ksize 高斯内核的大小。其中 ksize.width 和 ksize.height 可以不同，但他们都必须为正数和奇数。或者，它们可以是零的，它们都是由 sigma 计算而来。
- 第四个参数，double 类型的 sigmaX，表示高斯核函数在 X 方向的的标准偏差。
- 第五个参数，double 类型的 sigmaY，表示高斯核函数在 Y 方向的的标准偏差。若 sigmaY 为零，就将它设为 sigmaX，如果 sigmaX 和 sigmaY 都是 0，那么就由 ksize.width 和 ksize.height 计算出来。

  为了结果的正确性着想，最好是把第三个参数 Size，第四个参数 sigmaX 和第五个参数 sigmaY 全部指定到。

- 第六个参数，int 类型的 borderType，用于推断图像外部像素的某种边界模式。有默认值 BORDER_DEFAULT，我们一般不去管它。

例：
```cpp
// 载入原图并创建同样尺寸的目标图
Mat image=imread("2.jpg");
Mat out=Mat::zeros(image.size(), image.type());
// 进行高斯滤波操作
GaussianBlur(image, out, Size( 5, 5 ), 0, 0 );
```

### 8.2. 非线性滤波

线性滤波器中两个信号之和的响应与他们各自响应之和相等。换句话说，每个像素的输出值是一些输入像素的加权和，线性滤波器易于构造，并且易于从频率响应角度来进行分析。但在噪声是散粒噪声而不是高斯噪声，如椒盐噪声、脉冲噪声等，即图像偶尔会出现很大的值的时候，用高斯滤波器对图像进行模糊的话，噪声像素是不会被去除的，它们只是转换为更为柔和但仍然可见的散粒。这种情况下，使用邻域像素的非线性滤波也许会得到更好的效果。

非线性滤波器的原始数据与滤波结果是一种逻辑关系，即用逻辑运算实现，如最大值滤波器、最小值滤波器、中值滤波器等，是通过比较一定邻域内的灰度值大小来实现的，没有固定的模板，因而也就没有特定的转移函数（因为没有模板作傅里叶变换），另外，膨胀和腐蚀也是通过最大值、最小值滤波器实现的。

非线性滤波算子常见的有五种，这五种滤波算子对不同的图像都会有不同的作用，其中最常用的是中值滤波，因为它的效果最好且信息损失的最少。

#### 8.2.1. 中值滤波——medianBlur 函数

中值滤波（Median filter）是一种典型的非线性滤波技术，基本思想是用像素点邻域灰度值的中值来代替该像素点的灰度值，该方法在去除脉冲噪声、椒盐噪声的同时又能保留图像边缘细节。

中值滤波是基于排序统计理论的一种能有效抑制噪声的非线性信号处理技术，其基本原理是把数字图像或数字序列中一点的值用该点的一个邻域中各点值的中值代替，让周围的像素值接近的真实值，从而消除孤立的噪声点，对于斑点噪声（speckle noise）和椒盐噪声（salt-and-pepper noise）来说尤其有用，因为它不依赖于邻域内那些与典型值差别很大的值。中值滤波器在处理连续图像窗函数时与线性滤波器的工作方式类似，但滤波过程却不再是加权运算。

中值滤波在一定的条件下可以克服常见线性滤波器如最小均方滤波、方框滤波器、均值滤波等带来的图像细节模糊，而且对滤除脉冲干扰及图像扫描噪声非常有效，也常用于保护边缘信息，保存边缘的特性使它在不希望出现边缘模糊的场合也很有用，是非常经典的平滑噪声处理方法。

中值滤波与均值滤波器比较：
1.	中值滤波器优势：在均值滤波器中，由于噪声成分被放入平均计算中，所以输出受到了噪声的影响，但是在中值滤波器中，由于噪声成分很难选上，所以几乎不会影响到输出。因此同样用 3x3 区域进行处理，中值滤波消除的噪声能力更胜一筹。中值滤波无论是在消除噪声还是保存边缘方面都是一个不错的方法。
2.	中值滤波器劣势：中值滤波花费的时间是均值滤波的 5 倍以上。

中值滤波在一定条件下，可以克服线性滤波器（如均值滤波等）所带来的图像细节模糊，而且对滤除脉冲干扰即图像扫描噪声最为有效。在实际运算过程中并不需要图像的统计特性，也给计算带来不少方便。但是对一些细节多，特别是线、尖顶等细节多的图像不宜采用中值滤波。

medianBlur 函数使用中值滤波器来平滑（模糊）处理一张图片，从 src 输入，而结果从 dst 输出。

且对于多通道图片，每一个通道都单独进行处理，并且支持就地操作（In-placeoperation）。
```cpp
void medianBlur(InputArray src,OutputArray dst, int ksize)
```
参数说明：
- 第一个参数，InputArray 类型的 src，函数的输入参数，填 1、3 或者 4 通道的 Mat 类型的图像；当 ksize 为 3 或者 5 的时候，图像深度需为 CV_8U，CV_16U，或 CV_32F 其中之一，而对于较大孔径尺寸的图片，它只能是 CV_8U。
- 第二个参数，OutputArray 类型的 dst，即目标图像，函数的输出参数，需要和源图片有一样的尺寸和类型。我们可以用 Mat::Clone，以源图片为模板，来初始化得到如假包换的目标图。
- 第三个参数，int 类型的 ksize，孔径的线性尺寸（aperture linear size），注意这个参数必须是大于 1 的奇数，比如：3，5，7，9 等。

例：
```cpp
// 载入原图并创建同样尺寸的目标图
Mat image=imread("2.jpg");
Mat out=Mat::zeros(image.size(), image.type());
// 进行中值滤波操作
medianBlur( image, out, 7);
```

#### 8.2.2. 双边滤波——bilateralFilter 函数

双边滤波（Bilateral filter）是一种非线性的滤波方法，是结合图像的空间邻近度和像素值相似度的一种折衷处理，同时考虑空域信息和灰度相似性，达到保边去噪的目的。具有简单、非迭代、局部的特点。

双边滤波器的好处是可以做边缘保存（edge preserving），一般过去用的维纳滤波或者高斯滤波去降噪，都会较明显地模糊边缘，对于高频细节的保护效果并不明显。

```cpp
void bilateralFilter(InputArray src, OutputArraydst, int d, double sigmaColor, double sigmaSpace, int borderType=BORDER_DEFAULT)
```

参数说明：
- 第一个参数，InputArray 类型的 src，输入图像，即源图像，需要为 8 位或者浮点型单通道、三通道的图像。
- 第二个参数，OutputArray 类型的 dst，即目标图像，需要和源图片有一样的尺寸和类型。
- 第三个参数，int 类型的 d，表示在过滤过程中每个像素邻域的直径。如果这个值我们设其为非正数，那么 OpenCV 会从第五个参数 sigmaSpace 来计算出它来。
- 第四个参数，double 类型的 sigmaColor，颜色空间滤波器的 sigma 值。这个参数的值越大，就表明该像素邻域内有更宽广的颜色会被混合到一起，产生较大的半相等颜色区域。
- 第五个参数，double 类型的 sigmaSpace 坐标空间中滤波器的 sigma 值，坐标空间的标注方差。他的数值越大，意味着越远的像素会相互影响，从而使更大的区域足够相似的颜色获取相同的颜色。当 d>0，d 指定了邻域大小且与 sigmaSpace 无关。否则，d 正比于 sigmaSpace。
- 第六个参数，int 类型的 borderType，用于推断图像外部像素的某种边界模式。注意它有默认值 BORDER_DEFAULT。

例：
```cpp
// 载入原图并创建同样尺寸的目标图
Mat image=imread("2.jpg");
Mat out=Mat::zeros(image.size(), image.type());
// 进行双边滤波操作
bilateralFilter( image, out, 25, 25*2, 25/2 );
```

### 8.3. 极大值 / 极小值 / 中值滤波

#### 8.3.1. 极大值滤波

极大值滤波就是选取像素点领域的最大值作为改点的像素值，有效率去了灰度值比较低的噪声，也可作为形态学里面的膨胀操作。

极大值滤波可以表示为： Maximum(A)=max[A(x+i,y+j)]      (x,y) 属于 M

注：（x+i,y+j) 是定义在图像上的坐标，(i,j) 是定义在模板 M 上的坐标。M 即为运算的模板。

#### 8.3.2. 极小值滤波（与极大值滤波相反）

#### 8.3.3. 中点滤波

中点滤波常用于去除图像中的短尾噪声，例如高斯噪声和均匀分布噪声。终点滤波器的输出时给定窗口内灰度的极大值和极小值的平均值；

Midpoint(A)=(max[A(x+i,y+j)]+min[A(x+i,y+j)])/2          (x,y) 属于 M

注：（x+i,y+j) 是定义在图像上的坐标，(i,j) 是定义在模板 M 上的坐标。M 即为运算的模板。

#### 8.3.4. 中值滤波

中值滤波可以消除图像中的长尾噪声，例如负指数噪声和椒盐噪声。在消除噪声时，中值滤波对图像噪声的模糊极小（受模板大小的影响），中值滤波实质上是用模板内所包括像素灰度的中值来取代模板中心像素的灰度。中值滤波在消除图像内椒盐噪声和保持图像的空域细节方面，其性能优于均值滤波。

Median(A)=Median[A(x+i,y+j)]              (x,y) 属于 M

注：（x+i,y+j) 是定义在图像上的坐标，(i,j) 是定义在模板 M 上的坐标。M 即为运算的模板。

#### 8.3.5. 加权中值滤波（中值滤波的改进）

加权中值滤波是在中值滤波的基础上加以改进，其性能在一定程度上优于中值滤波。

## 9. 漫水填充

漫水填充法是一种用特定的颜色填充联通区域，通过设置可连通像素的上下限以及连通方式来达到不同的填充效果的方法。漫水填充经常被用来标记或分离图像的一部分以便对其进行进一步处理或分析，也可以用来从输入图像获取掩码区域，掩码会加速处理过程，或只处理掩码指定的像素点，操作的结果总是某个连续的区域。

函数原型：
```cpp
int floodFill(InputOutputArray image, Point seedPoint, Scalar newVal, Rect* rect=0, Scalar loDiff=Scalar(), Scalar upDiff=Scalar(), int flags=4 )

int floodFill(InputOutputArray image, InputOutputArray mask, Point seedPoint,Scalar newVal, Rect* rect=0, Scalar loDiff=Scalar(), Scalar upDiff=Scalar(), int flags=4 )
```
参数说明：
- 第一个参数，InputOutputArray 类型的 image, 输入 / 输出 1 通道或 3 通道，8 位或浮点图像，具体参数由之后的参数具体指明。
- 第二个参数， InputOutputArray 类型的 mask，这是第二个版本的 floodFill 独享的参数，表示操作掩模，。它应该为单通道、8 位、长和宽上都比输入图像 image 大两个像素点的图像。第二个版本的 floodFill 需要使用以及更新掩膜，所以这个 mask 参数我们一定要将其准备好并填在此处。需要注意的是，漫水填充不会填充掩膜 mask 的非零像素区域。例如，一个边缘检测算子的输出可以用来作为掩膜，以防止填充到边缘。同样的，也可以在多次的函数调用中使用同一个掩膜，以保证填充的区域不会重叠。另外需要注意的是，掩膜 mask 会比需填充的图像大，所以 mask 中与输入图像 (x,y) 像素点相对应的点的坐标为 (x+1,y+1)。
- 第三个参数，Point 类型的 seedPoint，漫水填充算法的起始点。
- 第四个参数，Scalar 类型的 newVal，像素点被染色的值，即在重绘区域像素的新值。
- 第五个参数，Rect*类型的 rect，有默认值 0，一个可选的参数，用于设置 floodFill 函数将要重绘区域的最小边界矩形区域。
- 第六个参数，Scalar 类型的 loDiff，有默认值 Scalar( )，表示当前观察像素值与其部件邻域像素值或者待加入该部件的种子像素之间的亮度或颜色之负差（lower brightness/color difference）的最大值。
- 第七个参数，Scalar 类型的 upDiff，有默认值 Scalar( )，表示当前观察像素值与其部件邻域像素值或者待加入该部件的种子像素之间的亮度或颜色之正差（lower brightness/color difference）的最大值。
- 第八个参数，int 类型的 flags，操作标志符，此参数包含三个部分：
  - 低八位（第 0~7 位）用于控制算法的连通性，可取 4 (4 为缺省值) 或者 8。如果设为 4，表示填充算法只考虑当前像素水平方向和垂直方向的相邻点；如果设为 8，除上述相邻点外，还会包含对角线方向的相邻点。
  - 高八位部分（16~23 位）可以为 0 或者如下两种选项标识符的组合：
    - FLOODFILL_FIXED_RANGE - 如果设置为这个标识符的话，就会考虑当前像素与种子像素之间的差，否则就考虑当前像素与其相邻像素的差。也就是说，这个范围是浮动的。
    - FLOODFILL_MASK_ONLY - 如果设置为这个标识符的话，函数不会去填充改变原始图像 （也就是忽略第三个参数 newVal), 而是去填充掩模图像（mask）。这个标识符只对第二个版本的 floodFill 有用，因第一个版本里面压根就没有 mask 参数。
  - 中间八位部分，上面关于高八位 FLOODFILL_MASK_ONLY 标识符中已经说的很明显，需要输入符合要求的掩码。Floodfill 的 flags 参数的中间八位的值就是用于指定填充掩码图像的值的。但如果 flags 中间八位的值为 0，则掩码会用 1 来填充。
  而所有 flags 可以用 or 操作符连接起来，即“|”。

例：
```cpp
Mat src = imread("1.jpg");
Rect ccomp;
floodFill(srcImage,Point(50,300),Scalar(155,255,55),&ccomp,Scalar(20,20,20),Scalar(20,20,20));
imshow("漫水填充图",srcImage);
//Point(50,300) 是 Point 类型的，表示漫水填充的起始点；Scalar(155,255,55) 表示像素点被染色的值，即在重绘去油像素的新值；&ccomp 是 Rect*类型的，有默认值为 0，用于设定 floodfill 函数将要重绘区域的最小边界矩形区域；第五个参数 Scalar(20,20,20) 表示当前观察像素值与其部件邻域像素值或者待加入该部件的种子像素之间的亮度或颜色之负差的最大值；第六个参数 Scalar(20,20,20) 表示当前观察像素值与其部件邻域像素值或者待加入该部件的种子像素之间的亮度或颜色之正差的最大值；
```

## 10. 尺寸调整

我们经常会将某种尺寸的图像转换为其他尺寸的图像，如果要放大或缩小图片的尺寸，opencv 里有两种方法：
- resize 函数。这是最直接的方式。
- pyrUp( )、pyrDown( ) 函数。即图像金字塔相关的两个函数，对图像进行向上采样，向下采样的操作。

另外，pyrUp、pyrDown 在 OpenCV 的 imgproc 模块中的 Image Filtering 子模块里。而 resize 在 imgproc 模块的 Geometric Image Transformations 子模块里。

### 10.1. resize 函数

resize( ) 为 OpenCV 中专职调整图像大小的函数。

此函数将源图像精确地转换为指定尺寸的目标图像。如果源图像中设置了 ROI（Region Of Interest ，感兴趣区域），那么 resize( ) 函数会对源图像的 ROI 区域进行调整图像尺寸的操作，来输出到目标图像中。若目标图像中已经设置 ROI 区域，不难理解 resize( ) 将会对源图像进行尺寸调整并填充到目标图像的 ROI 中。

函数原型：
```cpp
void resize(InputArray src,OutputArray dst, Size dsize, double fx=0, double fy=0, int interpolation=INTER_LINEAR )
```
参数说明：
- 第一个参数，InputArray 类型的 src，输入图像，即源图像，填 Mat 类的对象即可。
- 第二个参数，OutputArray 类型的 dst，输出图像，当其非零时，有着 dsize（第三个参数）的尺寸，或者由 src.size() 计算出来。
- 第三个参数，Size 类型的 dsize，输出图像的大小；如果它等于零 。
- 第四个参数，double 类型的 fx，沿水平轴的缩放系数，有默认值 0。
- 第五个参数，double 类型的 fy，沿垂直轴的缩放系数，有默认值 0。
- 第六个参数，int 类型的 interpolation，用于指定插值方式，默认为 INTER_LINEAR（线性插值）。可选的插值方式如下：
  - INTER_NEAREST - 最近邻插值
  - INTER_LINEAR - 线性插值（默认值）
  - INTER_AREA - 区域插值（利用像素区域关系的重采样插值）
  - INTER_CUBIC –三次样条插值（超过 4×4 像素邻域内的双三次插值）
  - INTER_LANCZOS4 -Lanczos 插值（超过 8×8 像素邻域的 Lanczos 插值）
  若要缩小图像，一般情况下最好用 CV_INTER_AREA 来插值，而若要放大图像，一般情况下最好用 CV_INTER_LINEAR（效率较高，速度较快，推荐使用）。

例：
```cpp
resize(srcImage,dstImage,Size(),0.5,0.5);// 缩小为一半
resize(srcImage,dstImage,Size(),2,2);// 放大 2 倍
resize(srcImage,dstImage,Size(srcImage.cols*3,srcImage.rows*3));// 放大 3 倍
```

### 10.2. 图像金字塔

图像金字塔是图像中多尺度表达的一种，最主要用于图像的分割，是一种以多分辨率来解释图像的有效但概念简单的结构。

将一层一层的图像比喻成金字塔，层级越高，则图像越小，分辨率越低。金字塔的底部是待处理图像的高分辨率表示，而顶部是低分辨率的近似。

![image](http://img.cdn.firejq.com/jpg/2019/3/25/eb0faa3ee4129de31033be4b0794d4f7.jpg)

一般情况下有两种类型的图像金字塔常常出现在文献和以及实际运用中。他们分别是：
- 高斯金字塔 (Gaussianpyramid): 用来向下采样，主要的图像金字塔
- 拉普拉斯金字塔 (Laplacianpyramid): 用来从金字塔低层图像重建上层未采样图像，在数字图像处理中也即是预测残差，可以对图像进行最大程度的还原，配合高斯金字塔一起使用。

两者的简要区别：高斯金字塔用来向下降采样图像，而拉普拉斯金字塔则用来从金字塔底层图像中向上采样重建一个图像。

当图像向金字塔的上层移动时，尺寸和分辨率就降低。OpenCV 中，从金字塔中上一级图像生成下一级图像的可以用 PryDown。而通过 PryUp 将现有的图像在每个维度都放大两遍。

图像金字塔中的向上和向下采样分别通过 OpenCV 函数 pyrUp 和 pyrDown 实现。

即：
- 对图像向上采样：pyrUp 函数。
- 对图像向下采样：pyrDown 函数。

这里的向下与向上采样，是对图像的尺寸而言的（和金字塔的方向相反），向上就是图像尺寸加倍，向下就是图像尺寸减半。而如果我们按上图中演示的金字塔方向来理解，金字塔向上图像其实在缩小，这样刚好是反过来了。

需要注意的是，PryUp 和 PryDown 不是互逆的，即 PryUp 不是降采样的逆操作。这种情况下，图像首先在每个维度上扩大为原来的两倍，新增的行（偶数行）以 0 填充。然后给指定的滤波器进行卷积（实际上是一个在每个维度都扩大为原来两倍的过滤器）去估计“丢失”像素的近似值。

函数原型：
```cpp
void pyrUp(InputArray src, OutputArraydst, const Size& dstsize=Size(), int borderType=BORDER_DEFAULT )
```
参数说明：
- 第一个参数，InputArray 类型的 src，输入图像，即源图像，填 Mat 类的对象即可。
- 第二个参数，OutputArray 类型的 dst，输出图像，和源图片有一样的尺寸和类型。
- 第三个参数，const Size& 类型的 dstsize，输出图像的大小；有默认值 Size()，即默认情况下，由 Size（src.cols*2，src.rows*2）来进行计算。
- 第四个参数，int 类型的 borderType，边界模式，一般我们不用去管它。

```cpp
void pyrDown(InputArray src,OutputArray dst, const Size& dstsize=Size(), int borderType=BORDER_DEFAULT)
```
参数说明：
- 第一个参数，InputArray 类型的 src，输入图像，即源图像，填 Mat 类的对象即可。
- 第二个参数，OutputArray 类型的 dst，输出图像，和源图片有一样的尺寸和类型。
- 第三个参数，const Size& 类型的 dstsize，输出图像的大小；有默认值 Size()，即默认情况下，由 Size Size((src.cols+1)/2, (src.rows+1)/2) 来进行计算。
- 第四个参数，int 类型的 borderType，边界模式，一般我们不用去管它。

例：
```cpp
pyrUp(srcImage,dstImage,Size(srcImage.cols*2,srcImage.rows*2));// 放大 2 倍
pyrDown(srcImage,dstImage,Size(srcImage.cols/2,srcImage.rows/2));// 缩小 2 倍
```

### 10.3. 两者区别

图像金字塔操作即 pyrUp、pyrDown 函数仅仅进行金字塔中相邻层次的图像采样，即每调用一次函数，只能使目标图像尺寸变为原来的 2 倍或 1/2，且长宽比基本不变，适合用来做长宽比不变的图像尺寸缩放操作，另外，图像金字塔非常重要的一个应用就是用语图像分割——首先建立一个图像金字塔，然后对 Gi 和 Gi+1 的像素直接依照对应的关系，建立起“父与子”的关系，而快速初始分割可以先在金字塔高层的低分辨率图像上完成，然后逐层对分割进行优化。

resize() 函数可一次性将图像变为任意不为 0 的尺寸，适合用来做归一化的图像尺寸缩放操作。

## 11. Refer Links
