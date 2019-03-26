- [OpenCV 使用：基本操作](#opencv-使用基本操作)
  - [1. 命名空间](#1-命名空间)
  - [2. 基本数据结构](#2-基本数据结构)
    - [2.1. 点表示：Point](#21-点表示point)
    - [2.2. 颜色表示：Scalar](#22-颜色表示scalar)
    - [2.3. 矩形表示：Rect](#23-矩形表示rect)
    - [2.4. 基础图像容器 Mat](#24-基础图像容器-mat)
    - [2.5. ROI(Region of Interesting)](#25-roiregion-of-interesting)
  - [3. 图像载入 / 显示 / 输出](#3-图像载入--显示--输出)
    - [3.1. 函数](#31-函数)
    - [3.2. 示例](#32-示例)
  - [4. 视频载入 / 播放](#4-视频载入--播放)
  - [5. Refer Links](#5-refer-links)

# OpenCV 使用：基本操作

## 1. 命名空间

标配：
```cpp
#include <opencv2/core/core.hpp>
#include<opencv2/highgui/highgui.hpp>

using namespace cv;
```

## 2. 基本数据结构

### 2.1. 点表示：Point

用法：
```cpp
Point point；
point.x = 10;
point.y = 8;
```
或者：`Point point = point(10,8);` 都表示为有 x=10 和 y=8 定位的 2D 点。

### 2.2. 颜色表示：Scalar

`Scalar(a,b,c)` 表示定义的 RGB 值为：红色分量为 c，绿色分量为 b，蓝色分量为 a。这里取决于 opnecv 和 matlab 不同储存三通道颜色方式，matlab 为 RGB 顺序，opnecv 为 BGR，也可以理解为两者相反。

### 2.3. 矩形表示：Rect

Rect 类的成员变量有 x，y，width，height 表示左上角点的坐标和矩形宽和高。其成员函数有：
- Size() 返回值为 Size（尺寸大小)
- area() 返回矩形的面积
- contains(Point) 判断点是否在矩形内
- inside(Rect) 判断矩形是否在该矩形内
- tl() 返回左上角点坐标
- br() 返回右下角点坐标

```cpp
Rect rect = rect1 & rect2; 两矩形交集
Rect rect = rect1 | tect2; 两矩形并集
Rect rectShift = rect + point; 矩形平移
Rect rectScale = rect + size; 矩形缩放
```

注：opnecv 和 matlab 储存三通道颜色的方式不同，matlab 为 RGB 顺序，opnecv 为 BGR，要注意区分。

### 2.4. 基础图像容器 Mat

http://www.developersite.org/904-110092-OpenCV

Mat 类是 OpenCV 最基本的一个数据类型，它可以表示一个多维的多通道的数组。Mat 常用来存储图像，包括单通道二维数组——灰度图，多通道二维数组——彩色图。当然也可以用来存储点云，直方图等等，对于高维的数组可以考虑存储在 SparseMat 中。

构造方法：
- Mat(nrows, ncols, type[, fillValue]), 例如：
  ```cpp
  // 构建 3×2 的 4 通道 8 位矩阵，每个元素初始值为（1,2,3,4）
  Mat M(3,2,CV_8UC4,Scalar(1,2,3,4));
  ```
- M.create(nrows,ncols,type)，例如，
  ```cpp
  // 构建 100×100 的 10 通道 8·位矩阵
  M.create(100,100,CV_8UC(10))
  ```
- 构建多维的矩阵，
  ```cpp
  // 构建一个 100×100×100 的 8 位三维矩阵
  int sz[] = {100,100,100}
  Mat Cube(3, sz, CV_32F, Scalar::all(0))
  ```
  Mat 的各项属性
  ```cpp
  A.total() // 元素的个数
  A.elemSize() // 元素的大小，如果是 8UC3 的话，返回 3*sizeof(uchar)
  A.elemSize1() // 如果是 8UC3 的话，返回 sizeof(uchar)
  A.type() // 元素的数据类型
  A.depth()// 元素的位数
  A.channels()// 矩阵的通道数
  A.step1() // 矩阵的每一行元素的个数，A.step/A.elemSize1
  A.size() // 矩阵的尺寸
  // 注意以下是成员变量不是成员函数
  A.step // 矩阵的一行的字节数
  A.rows // 矩阵的行数，即高
  A.cols // 矩阵的列数，即宽
  ```
  单独对矩阵的某一行某一列进行操作：
  ```cpp
  // 第 4 行加上第 6 行的 3 倍赋值给第 4 行
  M.row(3) = M.row(3) + M.row(5)*3;

  // 把第 8 列拷贝到第 2 列，通过 M.col(1) = M.col(7) 是不起作用的，因为 A.row() 返回的只是矩阵的头，以上操作仅仅相当于两个指针的操作，所指内存其实是没有发生变化的，应该使用 copyTo 函数，该函数是值的传递：
  Mat M1 = M.col(1);
  M.col(7).copyTo(M1);
  // 把第 j 行复制到第 i 行
  A.row(j).copyTo(A(i))
  ```
  获取矩阵元素：
  ```cpp
  M.at<double>(i,j)
  // 访问单通道图像的像素
  src.at<uchar>(j,i)
  // 访问三通道图像的像素
  src.at<Vec3b>(j,i)[channel]
  ```
  设置矩阵的值：
  ```cpp
  A.setTo(s)// 把 A 中所有的值赋值为 s
  ```

### 2.5. ROI(Region of Interesting)

```cpp
// 定义一个 Mat 类型并给其设定 ROI 区域
Mat imageROI;
// 方法一
imageROI=image(Rect(500,250,logo.cols,logo.rows));
// 方法二
imageROI=srcImage3(Range(250,250+logoImage.rows),Range(200,200+logoImage.cols));
```
定义 ROI 区域有两种方法：

- 第一种是使用 cv:Rect. 顾名思义，cv::Rect 表示一个矩形区域。指定矩形的左上角坐标（构造函数的前两个参数）和矩形的长宽（构造函数的后两个参数）就可以定义一个矩形区域。

- 另一种定义 ROI 的方式是指定感兴趣行或列的范围（Range）。Range 是指从起始索引到终止索引（不包括终止索引）的一连段连续序列。cv::Range 可以用来定义 Range。

## 3. 图像载入 / 显示 / 输出

cv::Mat 类是用于保存图像以及其他矩阵数据的数据结构。默认情况下，其尺寸为 0，我们也可以指定初始尺寸，比如，比如定义一个 Mat 类对象，就要写 `cv::Mat pic(320,640,cv::Scalar(100));`。

### 3.1. 函数

读取 / 载入图像到内存中：
```cpp
Mat imread(const string& filename, intflags=1);
```
- 第一个参数，const string& 类型的 filename，填我们需要载入的图片路径名，直接写文件名在项目根目录下查找，也可写绝对路径 / 相对路径 (“../1.jpg”)。
- 第二个参数，int 类型的 flags，为载入标识，它指定一个加载图像的颜色类型：
  - flags > 0 返回一个 3 通道的彩色图像。
  - flags = 0 返回灰度图像。
  - flags < 0 返回包含 Alpha 通道的加载的图像。

可以看到它自带缺省值 1，所以有时候这个参数在调用时我们可以忽略。

创建窗口：
```cpp
void namedWindow(const string& winname,int flags=WINDOW_AUTOSIZE );
```
- 第一个参数，const string& 型的 name，即填被用作窗口的标识符的窗口名称。
- 第二个参数，int 类型的 flags ，窗口的标识，可以填如下的值：
  - WINDOW_NORMAL 设置了这个值，用户便可以改变窗口的大小（没有限制）。
  - WINDOW_AUTOSIZE 如果设置了这个值，窗口大小会自动调整以适应所显示的图像，并且不能手动改变窗口大小。
  - WINDOW_OPENGL 如果设置了这个值的话，窗口创建的时候便会支持 OpenGL。

在窗口上创建滑动条：
```cpp
int createTrackbar(const String& trackbarname, const String& winname, int* value, int count, TrackbarCallback onChange = 0, void* userdata = 0);
```
参数说明：
- trackbarname: 所创建的跟踪条的名字。
- winname: 跟踪条所依附的窗口的名字。
- value: 可选的指向整型变量的指针，整型变量的值对应于滑动条的位置。初始创建时，滑动条的值就是这个整型变量的值。
- count: 滑动条最大的值。最小值总是为 0。
- onChange: 指向回调函数的指针，每次滚动条改变位置时，这个函数就会被调用。这个函数的原型应该为：void Foo(int value, void* userdata)；其中第一个参数是跟踪条的位置对应的值，第二个参数是用户数据（见下一个参数）。如果回调为空，表示没有回调函数被调用，仅仅 value 会有变化。
- userdata: void*类型的 userdata，默认值 0。这个参数是用户传给回调函数的数据，用来处理轨迹条事件。如果使用的第三个参数 value 实参是全局变量的话，完全可以不去管这个 userdata 参数。

获取当前轨迹条的位置
```cpp
int getTrackbarPos(conststring& trackbarname, conststring& winname);
```
- 第一个参数，const string& 类型的 trackbarname，表示轨迹条的名字。
- 第二个参数，const string& 类型的 winname，表示轨迹条的父窗口的名称。

在窗口中显示图像：
```cpp
void imshow(const string& winname, InputArray mat);
```
- 第一个参数，const string& 类型的 winname，填需要显示的窗口标识名称。
- 第二个参数，InputArray 类型的 mat，填需要显示的图像。

将内存中的图像输出到外存：
```cpp
bool imwrite(const string& filename,InputArray img, const vector<int>& params=vector<int>() );
```
- 第一个参数，const string& 类型的 filename，填需要写入的文件名就行了，带上后缀，比如，“123.jpg”这样。
- 第二个参数，InputArray 类型的 img，一般填一个 Mat 类型的图像数据就行了。
- 第三个参数，const vector<int>& 类型的 params，表示为特定格式保存的参数编码，它有默认值 vector<int>()，所以一般情况下不需要填写，需要填写时有：
  - JPEG 格式图片，参数为 0-100，表示图像质量，默认为 95。
  - PNG 格式图片，参数为 0-9，表示压缩级别，高值则表示更小尺寸和压缩时间更长。
  - PPM,PGM,PBM 格式时，表示二进制格式标志，参数 0 或 1，默认为 1。

### 3.2. 示例

例 1 显示图片
```cpp
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>

using namespace cv;

int main()
{
// 读入一张图片（图片）
Mat img = imread("1.jpg");
// 创建一个名为 "Pic"窗口
namedWindow("Pic");
// 在窗口中显示图片
imshow("Pic", img);
// 等待 6000 ms 后窗口自动关闭
waitKey(6000);
}
```

例 2 使用滑动条改变阈值进行二值化操作
```cpp
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace cv;
using namespace std;

void onChangeTrackBar(int pos, void * usrdata)// 定义回调函数
{
	// 强制类型转换
	Mat src = *(Mat*)(usrdata);
	Mat dst;
	// 二值化，pos 参数即为阈值
	threshold(src, dst, pos, 255, 0);
	imshow("win", dst);
}

int main()
{
	Mat img = imread("3.jpg");
	Mat grayImg;
	cvtColor(img, grayImg, COLOR_RGB2GRAY);// 先将图片灰度化
	namedWindow("win");// 创建窗口
	imshow("win", grayImg);// 显示图片
	createTrackbar("trackBar", "win1", 0, 255, onChangeTrackBar, &grayImg);// 创建滑动条
	waitKey(100000);

}
```

## 4. 视频载入 / 播放

## 5. Refer Links

[【OpenCV】入门教程](http://blog.csdn.net/column/details/opencv-tutorial.html)
