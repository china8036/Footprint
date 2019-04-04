- [Matlab Script：VC++ 混合编程](#matlab-scriptvc-混合编程)
	- [1. 混编意义](#1-混编意义)
	- [2. 在 matlab 中调用 VC/VC++ 程序](#2-在-matlab-中调用-vcvc-程序)
	- [3. 在 VS 中调用 matlab 程序](#3-在-vs-中调用-matlab-程序)
		- [3.1. 调用 matlab 引擎](#31-调用-matlab-引擎)
			- [3.1.1. 配置](#311-配置)
			- [3.1.2. 基本操作](#312-基本操作)
			- [3.1.3. 实例](#313-实例)
		- [3.2. 将 matlab 函数编译为 DLL](#32-将-matlab-函数编译为-dll)
	- [4. Refer Links](#4-refer-links)

# Matlab Script：VC++ 混合编程

## 1. 混编意义

许多底层的代码都依赖于 C/C++ 语言，如果要使用 MATLAB 进行实时的数据处理，那么必然是使用 C/C++（从操作系统内核、套接字，或者设备）获取数据，再调用 MATLAB 进行处理。

MATLAB 具有着很高的计算性能，一些算法用 MATLAB 很容易实现，而用 C++ 很难实现，因此如果想在 C++ 中调用 MATLAB 编写的函数，可以将该函数编译成库文件，之后在 C++ 中对其进行调用。

## 2. 在 matlab 中调用 VC/VC++ 程序

[在 MATLAB 中调用封装好的 C++ 函数的方法](https://blog.csdn.net/codersadis/article/details/48266905)

## 3. 在 VS 中调用 matlab 程序

### 3.1. 调用 matlab 引擎

[C/C++ VS 中调用 matlab 函数的方法](https://blog.csdn.net/guyuealian/article/details/73743654)

#### 3.1.1. 配置

在 VS2010 中，右键项目 - 属性

1. 在 VC++ 目录中添加包含目录：`R2012a\extern\include`

    ![image](http://img.cdn.firejq.com/jpg/2019/4/4/d28b2e7d88be038b20cafd5fde2aff17.jpg)

1. 在 VC++ 目录中添加库目录：`R2012a\extern\lib\win64\microsoft`

    ![image](http://img.cdn.firejq.com/jpg/2019/4/4/7c8016a214b2ee50cdf24c7776ef286b.jpg)

1. 在链接器中添加附加依赖项：`libeng.lib`, `libmx.lib` 和 `libmat.lib`

    ![image](http://img.cdn.firejq.com/jpg/2019/4/4/55f18eb818e19b70a9569d669fb4ab11.jpg)

    P.S. 附加依赖项也可通过在代码中显式声明来引用：
    ```cpp
    // import necessary lib
    #pragma comment(lib, "libeng.lib")
    #pragma comment(lib, "libmx.lib")
    #pragma comment(lib, "libmat.lib")
    ```

1. 在系统环境变量的路径中添加：`...\MATLAB\R2012b\bin` 和 `...\MATLAB\R2012b\bin\win64`（否则运行时会出现错误 `计算机中丢失 libeng.dll`）。

NOTE: 以上配置中，引用路径中的 win64 或 win32 要与项目的启动配置平台保持一致（如果引用的 MATLAB 路径是 x64，那么配置管理器中一定要选中 x64），否则会出现 `error LNK2019: 无法解析的外部符号` 系列错误。

#### 3.1.2. 基本操作

```cpp
// 打开 matlab 引擎
Engine *ep;
if (!(ep = engOpen("\0")))
{
    fprintf(stderr, "\nCan't start MATLAB engine\n");
    return EXIT_FAILURE;
}
// 将数据传入引擎使用
engPutVariable();
// matlab 的指令作为参数进行操作
engEvalString(ep, "matlab 指令");
// 计算操作等完成后需要清理操作，清理 mxCreateDoubleMatrix 生成的变量
mxDestroyArray();
// 关闭引擎
engClose();
```

#### 3.1.3. 实例

e.g. 通过 matlab 引擎执行单句 matlab 代码
```cpp
// CMatlab.cpp : 定义控制台应用程序的入口点。
//
//#include "stdafx.h"
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include "engine.h"		// add header file

// import necessary lib
//#pragma comment(lib, "libeng.lib")
//#pragma comment(lib, "libmx.lib")
//#pragma comment(lib, "libmat.lib")

int main(void)
{
	Engine *ep;

	// open engine
	if (!(ep = engOpen("\0")))
	{
		fprintf(stderr, "\nCan't start MATLAB engine\n");
		return EXIT_FAILURE;
	}

	// generate variables
	int Nsample = 50;
	const double PI = 3.1415926;
	double *t = new double[Nsample] ;

	for(int i = 0; i < Nsample; i++)
	{
		t[i] = i * 2 * PI / Nsample;
	}

	mxArray *T = NULL, *result = NULL;
	T = mxCreateDoubleMatrix(1, Nsample, mxREAL);
	memcpy((void *)mxGetPr(T), (void *)t, Nsample*sizeof(t[0]));

	engPutVariable(ep, "T", T);			// put data to engine

	// execute matlab operations
	engEvalString(ep, "Y=sin(T);");
	engEvalString(ep, "plot(T,Y);");
	engEvalString(ep, "title('y=sin(t)');");
	engEvalString(ep, "xlabel('t');");
	engEvalString(ep, "ylabel('y');");

	printf("Hit return to continue\n");
	fgetc(stdin);

	// clean operation(don't forget!!!)
	mxDestroyArray(T);
	engEvalString(ep, "close;");

	// close engine
	engClose(ep);

	return EXIT_SUCCESS;
}
```

e.g. 通过 matlab 引擎执行 matlab 脚本文件

[C++ 调用 Matlab 引擎及 Eigen 配置](https://www.cnblogs.com/scut-linmaojiang/p/4429816.html)

[C++ 调用 matlab 实例](https://www.cnblogs.com/scut-linmaojiang/p/4435524.html)

### 3.2. 将 matlab 函数编译为 DLL

[matlab 函数编译成库供 C++ 调用（非常详细）](https://blog.csdn.net/w_b_h/article/details/74570852)

这种方法将 MATLAB 函数编译成静态库和动态库，并在 C++ 中进行调用。

1. 在 Matlab 的 command window 输入：`mex -setup`

    ![image](http://img.cdn.firejq.com/jpg/2019/4/4/5625c9e11c6bb96fbe8549c27c94dd0a.jpg)

1. 在 Matlab 的 command window 输入：`mbuild -setup`

    ![image](http://img.cdn.firejq.com/jpg/2019/4/4/3c3c4284b1d79450b5c6c8fd95be70ab.jpg)

1. 对 matlab 脚本中的函数进行编译：
    ```
    mcc -W cpplib:pc -T link:lib phasecong
    ```
    其中，`pc` 表示自定义的生成的库的名称，`phasecong` 表示的是要编译的函数名（注意函数名后面不要加 `.m`后缀）。在编译时，当前文件夹一定要为函数所在的文件夹。

1. 编译完成后，在该文件夹下就会生成 `.h` `.cpp` `.lib` `.dll` 这四个文件。

e.g.
```cpp
// pcproj.cpp : 定义控制台应用程序的入口点。

#include "stdafx.h"
#include "pc.h"
#include <iostream>
#include <fstream>

using namespace std;

//#pragma comment(lib, ".\\lib\\pc.lib")
//#pragma comment(lib, ".\\lib\\libmx.lib")
//#pragma comment(lib, ".\\lib\\libmat.lib")
#pragma comment(lib, ".\\lib\\mclmcrrt.lib")
#pragma comment(lib, ".\\lib\\mclmcr.lib")

#define WIDTH 512
#define HEIGHT 512

int _tmain(int argc, _TCHAR* argv[])
{
	//mclInitializeApplication(NULL, 0);
	//mclmcrInitialize();

	UINT8 *img_befor = new UINT8[WIDTH*HEIGHT];
	DOUBLE *img_after = new DOUBLE[WIDTH*HEIGHT];

	int data;
	double ddata;
	FILE *fp;

	errno_t err = fopen_s(&fp, "pixel.txt", "r");
	if (err!=0){
		cout << "open pic fail!" << endl;
		return 0;
	}

	for (int i = 0; i < WIDTH; i++){
		for (int j = 0; j < HEIGHT; j++){
			fscanf_s(fp, "%d", &data);
			img_befor[i*WIDTH + j] = data;
		}
	}

	fclose(fp);
	fp = NULL;

	pcInitialize();

	mwArray img_input_array(HEIGHT, WIDTH, mxUINT8_CLASS, mxREAL);
	mwArray img_output_array(HEIGHT, WIDTH, mxDOUBLE_CLASS, mxREAL);
	int nargout = 1;

	img_input_array.SetData(img_befor, HEIGHT*WIDTH);

	phasecong(nargout,img_output_array,img_input_array);

	img_output_array.GetData(img_after, HEIGHT*WIDTH);

	pcTerminate();

	// 将结果写到文件
	err = fopen_s(&fp, "pixel_after.txt", "w");
	if (err != 0){
		cout << "open pic_after fail!" << endl;
	}
	else{
		// 写文件
		for (int i = 0; i < WIDTH; i++){
			for (int j = 0; j < HEIGHT; j++){
				ddata = img_after[i*WIDTH + j];
				fprintf(fp, "%.4f ", ddata);
			}
			fprintf(fp, "\n");
		}
	}
	fclose(fp);
	fp = NULL;

	return 0;
}
```

## 4. Refer Links

[MATLAB 和 VS2010 的混合编程需要注意的问题](https://blog.csdn.net/codersadis/article/details/44495157)