- [OpenCV：安装](#opencv安装)
  - [1. 直接安装](#1-直接安装)
  - [2. 编译安装](#2-编译安装)
  - [3. Refer Links](#3-refer-links)

# OpenCV：安装

## 1. 直接安装

http://www.jianshu.com/p/076909c1f121

http://blog.csdn.net/poem_qianmo/article/details/19809337

## 2. 编译安装

http://www.itwendao.com/article/detail/154268.html

https://www.cnblogs.com/Geo-fortune/p/4822997.html

工具和环境：
- [opencv3.2 源码](https://sourceforge.net/projects/opencvlibrary/files/)
- [opencv_contrib-3.2.0 源码](https://github.com/opencv/opencv_contrib)
- cmake3.8
- Visual Studio 2017

目录结构：

![image](http://img.cdn.firejq.com/jpg/2019/3/25/2072934db7cd142ec10d2c3b0623ac27.jpg)

- 利用 CMake 编译 opencv 和 opencv_contrib 库

  1. 第一次编译

      打开 cmake-gui.exe，选择 D:\software\code\openCV\opencv3.2\sources 和 D:\software\code\openCV\build，勾选 grouped，点击 configure。

      NOTE：此时在弹出的选项框中若选择 vs2017，则只编译出 x86 的库；若选择 vs2017 win64，则只编译出 x64 的库。

      出现的问题：卡在 download ippicv_windows_20151201.zip，解决方法：关掉 gui 窗口同时在任务管理中结束 cmake 进程，手动下载 ippicv_windows_20151201.zip，然后放在 D:\software\code\openCV\opencv3.2\sources\3rdparty\ippicv\downloads\windows-04e81ce5d0e329c3fbc606ae32cad44d 下，下载地址 google 可得。

      编译成功后显示 Configuring done。

  1. 第二次编译

      在红色的选项框中找到 OPENCV_EXTRA_MODULES_PATH，将值设为 opencv_contrib/modules 的绝对路径，即 D:\software\code\openCV\opencv_contrib-3.2.0\modules，可以在红色区域内再次去掉 BUILD_OPENCV 和 WITH_CUDA 等有些硬件不支持的库，再次点击 configure；

      出现的问题：卡在 download protobuf-cpp-3.1.0.tar.gz，解决方法：手动下载，放在 D:\software\code\openCV\opencv_contrib-3.2.0\modules\dnn\.download\bd5e3eed635a8d32e2b99658633815ef\v3.1.0 即可，具体操作同上一步。

      编译成功后显示 Configuring done。

  1. 点击 generate，显示 Generating done，即 cmake 的编译操作完成。

- 利用 VS 编译 Debug 和 Release 库

  1. 在 vs2017 中 open 刚刚 build 目录下生成的 OpenCV.sln
  1. 在 CMake Target 中找到 INSTALL

      ![image](http://img.cdn.firejq.com/jpg/2019/3/25/ce124edbddd932f1edf6a0905d78ce6c.jpg)

      在 Debug 和 Release 的条件中分别右键选择 build，生成 debug 和 release 库，这个过程大概持续 10-20 分钟，此过程大量耗费 cpu 资源，最好不要同时进行其它占用 cpu 资源较多的操作。

      build 成功后，可以在 D:\software\code\openCV\build\install 文件夹中看到这几个文件目录：

      ![image](http://img.cdn.firejq.com/jpg/2019/3/25/a2a8aa40895de43a8701093897a31835.jpg)

- Windows 和 VS 中配置 Opencv3.2

  1. 将 D:\software\code\openCV\build\install\x86\vc15\bin 添加到系统环境变量。

  1. 在一个工程中打开 Property Manager，在 debug win32 下 add new properties。

  1. 在 include directories 中添加

      ![image](http://img.cdn.firejq.com/jpg/2019/3/25/681754875b58e8e49a5e3fc6b1bfa6df.jpg)

  1.	在 library directories 中添加

      ![image](http://img.cdn.firejq.com/jpg/2019/3/25/66a54f9d4f69b02662de5d0b10212c7f.jpg)

  1.	在 linker 的 input 中添加
      ```
      opencv_aruco320.lib
      opencv_bgsegm320.lib
      opencv_bioinspired320.lib
      opencv_calib3d320.lib
      opencv_ccalib320.lib
      opencv_core320.lib
      opencv_datasets320.lib
      opencv_dnn320.lib
      opencv_dpm320.lib
      opencv_face320.lib
      opencv_features2d320.lib
      opencv_flann320.lib
      opencv_fuzzy320.lib
      opencv_highgui320.lib
      opencv_imgcodecs320.lib
      opencv_imgproc320.lib
      opencv_line_descriptor320.lib
      opencv_ml320.lib
      opencv_objdetect320.lib
      opencv_optflow320.lib
      opencv_phase_unwrapping320.lib
      opencv_photo320.lib
      opencv_plot320.lib
      opencv_reg320.lib
      opencv_rgbd320.lib
      opencv_saliency320.lib
      opencv_shape320.lib
      opencv_stereo320.lib
      opencv_stitching320.lib
      opencv_structured_light320.lib
      opencv_superres320.lib
      opencv_surface_matching320.lib
      opencv_text320.lib
      opencv_tracking320.lib
      opencv_video320.lib
      opencv_videoio320.lib
      opencv_videostab320.lib
      opencv_xfeatures2d320.lib
      opencv_ximgproc320.lib
      opencv_xobjdetect320.lib
      opencv_xphoto320.lib
      ```
      和
      ```
      opencv_aruco320d.lib
      opencv_bgsegm320d.lib
      opencv_bioinspired320d.lib
      opencv_calib3d320d.lib
      opencv_ccalib320d.lib
      opencv_core320d.lib
      opencv_datasets320d.lib
      opencv_dnn320d.lib
      opencv_dpm320d.lib
      opencv_face320d.lib
      opencv_features2d320d.lib
      opencv_flann320d.lib
      opencv_fuzzy320d.lib
      opencv_highgui320d.lib
      opencv_imgcodecs320d.lib
      opencv_imgproc320d.lib
      opencv_line_descriptor320d.lib
      opencv_ml320d.lib
      opencv_objdetect320d.lib
      opencv_optflow320d.lib
      opencv_phase_unwrapping320d.lib
      opencv_photo320d.lib
      opencv_plot320d.lib
      opencv_reg320d.lib
      opencv_rgbd320d.lib
      opencv_saliency320d.lib
      opencv_shape320d.lib
      opencv_stereo320d.lib
      opencv_stitching320d.lib
      opencv_structured_light320d.lib
      opencv_superres320d.lib
      opencv_surface_matching320d.lib
      opencv_text320d.lib
      opencv_tracking320d.lib
      opencv_video320d.lib
      opencv_videoio320d.lib
      opencv_videostab320d.lib
      opencv_xfeatures2d320d.lib
      opencv_ximgproc320d.lib
      opencv_xobjdetect320d.lib
      opencv_xphoto320d.lib
      ```
      改进：
      - 在配置里直接写 `D:\OpenCV3.1\sources\newBulid\install\x64\vc14\lib\*.lib` 就可以了。
      - 在该目录下进入 cmd，运行 `dir /b *.lib > lib.txt` 即可得到所有文件名的列表（`/b` 使只输出文件名）。

      若报错：在计算机中找不到 `opencv_core320d.dll/opencv_highimg320.dll`
      - 解决方法一：检查是否有将 `D:\software\code\openCV\build64\install\x64\vc15\bin` 添加到了系统环境变量中。
      - 解决方法二：直接将 `D:\software\code\openCV\build64\install\x64\vc15\bin` 下的 dll 文件拷贝到`C:\Windows\System32` 下（32 位程序）/ `C:\Windows\SysWOW64 下（64 位程序）。

  1. 测试

      建立控制台项目，在 debug x64 或 release x64 模式下，添加现有属性文件（即上边配置好的配置文件），运行代码：
      ```cpp
      #include <opencv2/highgui/highgui.hpp>
      #include <opencv2/imgproc/imgproc.hpp>
      #include <opencv2/core/core.hpp>
      using namespace cv;

      int main() {
        VideoCapture cap(0);
        Mat frame;
        while (waitKey(30) != 27)
        {
          cap >> frame;
          imshow("原图", frame);
          // 灰度
          cvtColor(frame, frame, CV_BGR2GRAY);
          // 设置中值滤波器参数
          medianBlur(frame, frame, 7);
          // 拉普拉斯边缘检测

          Laplacian(frame, frame, CV_8U, 5);

          threshold(frame, frame, 127, 255, THRESH_BINARY_INV);
          imshow("素描风", frame);

        }
      }
      ```

## 3. Refer Links
