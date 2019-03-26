- [OpenCV 使用：绘图操作](#opencv-使用绘图操作)
  - [1. 圆形](#1-圆形)
  - [2. 椭圆](#2-椭圆)
  - [3. 直线](#3-直线)
  - [4. 矩形](#4-矩形)
  - [5. 插入文本](#5-插入文本)
  - [6. Refer Links](#6-refer-links)

# OpenCV 使用：绘图操作

OpenCV 提供了一些基本的绘图操作，比如画圆，画椭圆，画线，画矩形，在图像里插入文字等功能。

## 1. 圆形

画圆使用的是 circle 函数，必须提供的参数是：画在出的圆显示在哪里，圆心，半径，以及画线的颜色。

例：
```cpp
// 圆心
Point center = Point(255,255);
// 半径
int r = 100;
// 承载图像
Mat picture(500,500,CV_8UC3,Scalar(255,255,255));
// 参数为：承载的图像、圆心、半径、颜色、粗细、线型，其中可以通过把线的粗细设置为 -1 来画实心的图形。
circle(picture,center,r,Scalar(0,0,0));
imshow("底板",picture);
```

## 2. 椭圆

画椭圆的使用的是 ellipse 函数。

例：
```cpp
// 参数为：承载的图像、圆心、长短轴、径向夹角（水平面到长轴的夹角）、起始角度（长轴到起始边沿的夹角）、结束角度（长轴到结束点的夹角）、倾斜的矩形（可选项）、颜色、粗细、线性、偏移
ellipse(picture,center,Size( 250, 100 ),0,30,240,Scalar(0,0,0));
```

## 3. 直线

画直线使用的是 line 函数。

例：
```cpp
// 画线
Point a = Point (600,600);
// 参数为：承载的图像、起始点、结束点、颜色、粗细、线型
//“画板”是 500*500 的，而这里把结束点设为了（600,600），这样做的不会报错，绘图的结果是根据画板的大小裁剪掉显示不出来的部分
line(picture,a,center,Scalar(255,0,0));
imshow("底板",picture);
```

## 4. 矩形

画矩形使用的是 rectangle 函数，需要知道的左上和右下角（与 cv::rect 定义的矩形不一样，rect 是左上角点和矩形长宽）。

例：
```cpp
// 画矩形
// 参数为：承载的图像、顶点、对角点、颜色（这里是蓝色）、粗细、大小
rectangle(picture,a,center,Scalar(255,0,0));
imshow("底板",picture);
```

## 5. 插入文本

在图像里插入文字使用 putText 函数。

例：
```cpp
// 插入文字
// 参数为：承载的图片，插入的文字，文字的位置（文本框左下角），字体，大小，颜色
// 这里文字为蓝色，因为 OpenCV 中三彩色通道的顺序是 BGR 而不是 RGB
string words= "good luck";
putText( picture, words, Point( picture.rows/2,picture.cols/4),CV_FONT_HERSHEY_COMPLEX, 1, Scalar(255, 0, 0) );
imshow("底板",picture);
```

## 6. Refer Links
