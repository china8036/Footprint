- [OpenCV 应用：验证码识别](#opencv-应用验证码识别)
  - [1. 预处理](#1-预处理)
    - [1.1. 灰度化](#11-灰度化)
      - [1.1.1. 数字图像常见灰度化方法](#111-数字图像常见灰度化方法)
      - [1.1.2. 颜色空间转换函数：cvtColor()](#112-颜色空间转换函数cvtcolor)
      - [1.1.3. 示例](#113-示例)
    - [1.2. 二值化](#12-二值化)
      - [1.2.1. 数字图像常见二值化方法](#121-数字图像常见二值化方法)
      - [1.2.2. 示例](#122-示例)
    - [1.3. 去噪](#13-去噪)
  - [2. 轮廓检测](#2-轮廓检测)
  - [3. 目标分割](#3-目标分割)
  - [4. 提取字符特征](#4-提取字符特征)
  - [5. 分类和学习](#5-分类和学习)
  - [6. Refer Links](#6-refer-links)

# OpenCV 应用：验证码识别

## 1. 预处理

图像预处理，将每一个文字图像分检出来交给识别模块识别，这一过程称为图像预处理。

图像预处理的主要目的是消除图像中无关的信息恢复有用的真实信息增强有关信息的可检测性和最大限度地简化数据从而改进特征抽取、图像分割、匹配和识别的可靠性。预处理过程一般有数字化、几何变换、归一化、平滑、复原和增强等步骤。

### 1.1. 灰度化

python 处理 https://segmentfault.com/a/1190000003755100

java 处理 https://my.oschina.net/u/656588/blog/624970

#### 1.1.1. 数字图像常见灰度化方法

https://www.cnblogs.com/finlay/p/3665302.html

- 分量法
　
  将彩色图像中的三分量的亮度作为三个灰度图像的灰度值，可根据应用需要选取一种灰度图像。

  ![image](http://img.cdn.firejq.com/jpg/2019/3/25/1387b4a0a74a25123eceb3d0a589af67.jpg)

- 最大值法
　
  将彩色图像中的三分量亮度的最大值作为灰度图的灰度值。

  ![image](http://img.cdn.firejq.com/jpg/2019/3/25/d19861ec06d00dfe9a153b3db2c0dbc7.jpg)

- 平均值法
　
  将彩色图像中的三分量亮度求平均得到一个灰度值。

  ![image](http://img.cdn.firejq.com/jpg/2019/3/25/dcf5b08e817c65c1c98b2424e863000d.jpg)

- 加权平均法
　
  根据重要性及其它指标，将三个分量以不同的权值进行加权平均。由于人眼对绿色的敏感最高，对蓝色敏感最低，因此，按下式对 RGB 三分量进行加权平均能得到较合理的灰度图像。

  ![image](http://img.cdn.firejq.com/jpg/2019/3/25/0cd0f86b5efc3a3a928fe28d987a2121.jpg)

　　
#### 1.1.2. 颜色空间转换函数：cvtColor()

```cpp
void cvtColor(InputArray src, OutputArray dst, int code, int dstCn=0
```
参数说明：
- src 和 dst 分别是待转的图像（src）和待转图像转换后的图像（dst）；
- code 是一个掩码，表示由 src 到 dst 之间是怎么转的，比如是彩色转为灰度，还是彩色 RGB 转为 HIS/HUS 模式，常用模式如下：
  - RGB 和 Gray 转换有：Opnecv2 版本的 CV_RGB2GRAY,CV_GRAY2RGB，Opencv3 版本的 COLOR_RGB2GRAY,COLOR_GRAY2RGB；
  - RGB 和 HSV 转换有：Opnecv2 版本的 CV_RGB2HSV,CV_BGR2HSV,CV_HSV2RGB,CV_HSV2BGR，Opencv3 版本的 COLOR_RGB2HSV,COLOR_BGR2HSV,COLOR_HSV2RGB,COLOR_HSV2BGR；
  （即对于颜色转换，Opnecv2 的 CV_前缀的宏命名规范被 Opnecv3 中的 COLOR_式的宏命名前缀所取代，另外，Opnecv 中默认的图片通道存储顺序是 BGR）
- dstCn 表示 dst 图像的波段数，这个值默认是 0，它可以从参数 code 中推断。

#### 1.1.3. 示例

```cpp
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace cv;
using namespace std;

int main()
{
	Mat img = imread("3.jpg");
	Mat newImg;
	cvtColor(img, newImg, COLOR_RGB2GRAY);// 灰度化
	namedWindow("win");
	imshow("win", newImg);
	waitKey(100000);

}
```

### 1.2. 二值化

python 处理 https://segmentfault.com/a/1190000003755115

#### 1.2.1. 数字图像常见二值化方法

http://www.jianshu.com/p/6efd324e8677

http://blog.csdn.net/jia20003/article/details/8074627

#### 1.2.2. 示例

```cpp
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace cv;
using namespace std;
int main()
{
	Mat img = imread("3.jpg");
	Mat grayImg;
	Mat twoValueImg;
	cvtColor(img, grayImg, COLOR_RGB2GRAY);// 灰度化
	threshold(grayImg, twoValueImg, 25, 255, CV_THRESH_BINARY);// 二值化

	namedWindow("win1");
	namedWindow("win2");

	imshow("win1", grayImg);
	imshow("win2", twoValueImg);
	waitKey(100000);
}
```

### 1.3. 去噪

## 2. 轮廓检测

## 3. 目标分割

## 4. 提取字符特征

## 5. 分类和学习

## 6. Refer Links
