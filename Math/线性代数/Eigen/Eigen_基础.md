- [Eigen](#eigen)
  - [1. 基本概念](#1-基本概念)
    - [1.1. Requirements](#11-requirements)
    - [1.2. Compiler support](#12-compiler-support)
  - [2. 安装配置](#2-安装配置)
  - [3. 模块划分](#3-模块划分)
  - [4. 使用](#4-使用)
    - [4.1. 基本操作](#41-基本操作)
    - [4.2. 运算操作](#42-运算操作)
      - [4.2.1. 矩阵 / 向量加减法](#421-矩阵--向量加减法)
      - [4.2.2. 矩阵 / 向量数乘](#422-矩阵--向量数乘)
      - [4.2.3. 矩阵乘法](#423-矩阵乘法)
      - [4.2.4. 向量点乘、叉乘](#424-向量点乘叉乘)
    - [4.3. 其他操作](#43-其他操作)
  - [5. 综合示例](#5-综合示例)
  - [6. Refer Links](#6-refer-links)

# Eigen

## 1. 基本概念

> Eigen is a C++ template library for linear algebra: matrices, vectors, numerical solvers, and related algorithms.

[Eigen](http://eigen.tuxfamily.org) 是一个用于线性代数、矩阵和向量运算、几何变换、数值求解器和相关算法的高级 C++ 库。SLAM 中常用的 ceres，深度学习常用的 tensorflow 都是基于 Eigen 开发的。

### 1.1. Requirements

Eigen doesn't have any dependencies other than the C++ standard library.

We use the CMake build system, but only to build the documentation and unit-tests, and to automate installation. If you just want to use Eigen, you can use the header files right away. There is no binary library to link to, and no configured header file. Eigen is a pure template library defined in the headers.

### 1.2. Compiler support

Eigen is standard C++98 and so should theoretically be compatible with any compliant compiler.

Eigen is being successfully used with the following compilers:
- [GCC](http://gcc.gnu.org/), version 4.8 and newer. Older versions of gcc might work as well but they are not tested anymore.
- [MSVC](http://en.wikipedia.org/wiki/Visual_C%2B%2B) (Visual Studio), 2012 and newer. Be aware that enabling IntelliSense (/FR flag) is known to trigger some internal compilation errors. The old 3.2 version of Eigen supports MSVC 2010, and the 3.1 version supports MSVC 2008.
- [Intel C++ compiler](http://en.wikipedia.org/wiki/Intel_C%2B%2B_Compiler). Enabling the `-inline-forceinline` option is highly recommended.
- [LLVM/CLang++](http://clang.llvm.org/cxx_status.html), version 3.4 and newer. (The 2.8 version used to work fine, but it is not tested with up-to-date versions of Eigen)
- XCode 7 and newer. Based on LLVM/CLang.
- [MinGW](http://en.wikipedia.org/wiki/Mingw), recent versions. Based on GCC.
- QNX's QCC compiler.

Regarding performance, Eigen performs best with compilers based on GCC or LLVM/Clang.

## 2. 安装配置

http://eigen.tuxfamily.org/dox/GettingStarted.html

Eigen 用源码的方式提供给用户使用，在使用时只需要包含 Eigen 的头文件即可进行使用。

P.S. 之所以采用这种方式，是因为 Eigen 采用模板方式实现，由于模板函数不支持分离编译，所以只能提供源码而不是动态库的方式供用户使用，不过这也也更方面用户使用和研究。

## 3. 模块划分

http://eigen.tuxfamily.org/dox/group__QuickRefPage.html

为了应对不同的需求，Eigen 库被分为多个功能模块，每个模块都有自己相对应的头文件。在 Eigen 库的使用过程中，需要根据使用的具体模块包含不同的头文件。

| Module      | Header file                 | Contents                                               |
| ----------- | --------------------------- | ------------------------------------------------------ |
| Core        | #include<Eigen/Core>        | Matrix 和 Array 类，基础的线性代数运算和数组操作          |
| Geometry    | #include<Eigen/Geometry>    | 旋转、平移、缩放、2 维和 3 维的各种变换                   |
| LU          | #include<Eigen/LU>          | 求逆，行列式，LU 分解                                   |
| Cholesky    | #include <Eigen/Cholesky>   | LLT 和 LDLT Cholesky 分解                                 |
| Householder | #include<Eigen/Householder> | 豪斯霍尔德变换，用于线性代数运算                       |
| SVD         | #include<Eigen/SVD>         | SVD 分解                                                |
| QR          | #include<Eigen/QR>          | QR 分解                                                 |
| Eigenvalues | #include<Eigen/Eigenvalues> | 特征值，特征向量分解                                   |
| Sparse      | #include<Eigen/Sparse>      | 稀疏矩阵的存储和一些基本的线性运算                     |
| 稠密矩阵    | #include<Eigen/Dense>       | 包含了 Core/Geometry/LU/Cholesky/SVD/QR/Eigenvalues 模块 |
| 矩阵        | #include<Eigen/Eigen>       | 包括 Dense 和 Sparse（整合库)                              |

P.S. 通过以下 2 个文本文件可包含所有 Eigen 库文件。
```cpp
#include <Eigen/Sparse>
#include <Eigen/Dense>
```

## 4. 使用

### 4.1. 基本操作

在 Eigen 中，所有矩阵和向量均为 Matrix 模板类的对象，向量是矩阵的行（或列）为 1 是的特殊情况。

- 矩阵

  Matrix 类有六个模板参数，其中三个有默认值，因此只要学习三个参数就足够了：
  ```cpp
  /* 强制性的三参数模板的原型 （三个参数分别表示：标量的类型，编译时的行，编译时的列) */
  Matrix<typename Scalar, int RowsAtCompileTime, int ColsAtCompileTime>
  ```
  - 静态模板

    Eigen 用 typedef 定义了很多静态矩阵模板，即行和列在编译阶段就确定好的矩阵模板。例如：Matrix4f 表示 4×4 的 floats 矩阵
    ```cpp
    typedef Matrix<float, 4, 4> Matrix4f;
    ```

  - 动态模板

    Eigen 的动态矩阵可以在运行时确定大小：
    ```cpp
    typedef Matrix<double, Dynamic, Dynamic> MatrixXd;
    typedef Matrix<int, Dynamic, 1> VectorXi;
    /* 也可使用‘行’固定‘列’动态的矩阵 */
    Matrix<float, 3, Dynamic>
    ```

- 向量

  向量是矩阵的特殊情况，也是用矩阵定义的：
  ```cpp
  typedef Matrix<float, 3, 1> Vector3f;
  typedef Matrix<int, 1, 2> RowVector2i;
  ```

- 构造函数

  可以使用默认的构造函数，不执行动态分配内存，也没有初始化矩阵参数：
  ```cpp
  Matrix3f a;   // a 是 3-by-3 矩阵，包含未初始化的 float[9] 数组
  MatrixXf b;   // b 是动态矩阵，当前大小为 0-by-0, 没有为数组的系数分配内存
  /* 矩阵的第一个参数表示“行”，数组只有一个参数。根据跟定的大小分配内存，但不初始化 */
  MatrixXf a(10,15);      // a 是 10-by-15 阵，分配了内存，没有初始化
  VectorXf b(30);         // b 是动态矩阵，当前大小为 30, 分配了内存，没有初始化
  /* 对于给定的矩阵，传递的参数无效 */
  Matrix3f a(3,3);
  /* 对于维数最大为 4 的向量，可以直接初始化 */
  Vector2d a(5.0, 6.0);
  Vector3d b(5.0, 6.0, 7.0);
  Vector4d c(5.0, 6.0, 7.0, 8.0);
  ```

- 初始化
  ```cpp
  Matrix3f m;
  m << 1, 2, 3,   4, 5, 6,   7, 8, 9;
  cout << m;
  ```

- 元素访问

  系数都是从 0 开始，矩阵默认按列存储。
  ```cpp
  MatrixXd m(2, 2);
  m(0, 0) = 3;
  m(1, 0) = 2.5;
  m(0, 1) = -1;
  m(1, 1) = m(1, 0) + m(0, 1);
  cout << "Here is the matrix m:" << endl;
  cout << m << endl;
  VectorXd v(2);
  v(0) = 4;
  v[1] = v[0] - 1;     //operator[] 在 vectors 中重载，意义和 () 相同
  cout << "Here is the vector v:" << endl;
  cout << v << endl;
  ```

- resize

  可以用 rows(), cols() and size() 改变现有矩阵的大小。这些类方法返回行、列、系数的数值。也可以用 resize() 来改变动态矩阵的大小。

e.g. 1
```cpp
#include <iostream>
#include <Eigen/Dense>

using namespace std;
using Eigen::MatrixXd;

int main()
{
  MatrixXd m(2,2);//MatrixXd 表示是任意尺寸的矩阵 ixj, m(2,2) 代表一个 2x2 的方块矩阵
  m(0,0) = 3;// 代表矩阵元素 a11
  m(1,0) = 2.5;//a21
  m(0,1) = -1;//a12
  m(1,1) = m(1,0) + m(0,1);//a22=a21+a12
  cout << m << endl;// 输出矩阵 m
}
```

e.g. 2
```cpp
#include <iostream>
#include <Eigen/Dense>

using namespace Eigen;
using namespace std;

int main()
{
  MatrixXd m = MatrixXd::Random(3,3);// 定义 3x3 的随机矩阵 m，各元素取值范围是 [-1，1]；相当于 Matrix3d m = Matrix3d::Random()

  m = (m + MatrixXd::Constant(3,3,1.2)) * 50;//"MatrixXd::Constant(3,3,1.2)"定义各元素为 1.2 的 3-by-3 常数矩阵，m 的取值范围变为 [10，110]
  cout << "m =" << endl << m << endl;
  VectorXd v(3);// 定义维度为 3 的列向量 v
  v << 1, 2, 3;// 赋值于 v
  cout << "m * v =" << endl << m * v << endl;
}
```

### 4.2. 运算操作

Eigen 中是重载了 +、-、*等基本运算符的，基本的加减乘，只要矩阵维数合乎数理，是可以直接进行操作的。

#### 4.2.1. 矩阵 / 向量加减法

e.g.
```cpp
#include <iostream>
#include <Eigen/Dense>
using namespace std;
using namespace Eigen;
int main()
{
  Matrix2d a;
  a << 1, 2,
       3, 4;
  MatrixXd b(2,2);
  b << 2, 3,
       1, 4;
  cout << "a + b =\n" << a + b << endl;// 矩阵加法
  cout << "a - b =\n" << a - b << endl;// 矩阵减法
  cout << "Doing a += b;" << endl;
  a += b;//a = a + b，同时重新赋值 a
  cout << "Now a =\n" << a << endl;

  Vector3d v(1,2,3);
  Vector3d w(1,0,0);
  cout << "-v + w - v =\n" << -v + w - v << endl;// 向量加减法
}
```

#### 4.2.2. 矩阵 / 向量数乘

e.g.
```cpp
#include <iostream>
#include <Eigen/Dense>
using namespace std;
using namespace Eigen;
int main()
{
  Matrix2d a;
  a << 1, 2,
       3, 4;
  Vector3d v(1,2,3);
  cout << "a * 2.5 =\n" << a * 2.5 << endl;// 矩阵数乘
  cout << "0.1 * v =\n" << 0.1 * v << endl;// 向量数乘
  cout << "Doing v *= 2;" << endl;
  v *= 2;// 向量数乘，从新赋值 v
  cout << "Now v =\n" << v << endl;
}
```

#### 4.2.3. 矩阵乘法

e.g.
```cpp
#include <iostream>
#include <Eigen/Dense>
using namespace std;
using namespace Eigen;
int main()
{
  Matrix2d m;
  m << 1, 2,
       3, 4;
  Vector2d u(-1,1), v(2,0);

  cout << "Here is m*m:\n" << m*m << endl;
  cout << "Here is m*u:\n" << m*u << endl;
  cout << "Here is u^T*m:\n" << u.transpose()*m << endl;
  cout << "Here is u^T*v:\n" << u.transpose()*v << endl;
  cout << "Here is u*v^T:\n" << u*v.transpose() << endl;
}
```

#### 4.2.4. 向量点乘、叉乘

e.g.
```cpp
#include <iostream>
#include <Eigen/Dense>
using namespace std;
using namespace Eigen;
int main()
{
  Vector3d v(1,2,3);
  Vector3d w(0,1,2);

  cout << "Dot product: " << v.dot(w) << endl;// 向量点乘
  cout << "Cross product:\n" << v.cross(w) << endl;// 向量叉乘
}
```

### 4.3. 其他操作

e.g.
```cpp
#include <iostream>
#include <Eigen/Dense>
using namespace Eigen;
using namespace std;
int main()
{
  Matrix3d m;
  m   << 1, 2, 3,
         1, 2, 1,
	       0, 2, 4;

  cout << "Here is m.trace():         " << m.trace() << endl;// 迹
  cout << "Here is m.transpose():     " << m.transpose() << endl;// 转置
  cout << "Here is m.conjugate():     " << m.conjugate() << endl;// 共轭
  cout << "Here is m.inverse():       " << m.inverse() << endl;// 逆
  cout << "Here is m.determinant():   " << m.determinant() << endl;// 行列式
  cout << "Here is m.sum():           " << m.sum() << endl;// 所有元素之和
  cout << "Here is m.prod():          " << m.prod() << endl;// 所有元素之积
  cout << "Here is m.mean():          " << m.mean() << endl;// 元素的平均数
  cout << "Here is m.minCoeff():      " << m.minCoeff() << endl;// 最小元素
  cout << "Here is m.maxCoeff():      " << m.maxCoeff() << endl;// 最大元素
}
```

## 5. 综合示例

```cpp
#include <iostream>
#include <Eigen/Dense>

//using Eigen::MatrixXd;
using namespace Eigen;
using namespace Eigen::internal;
using namespace Eigen::Architecture;

using namespace std;

int main()
{

#pragma region one_d_object

	cout<<"*******************1D-object****************"<<endl;

	Vector4d v1;
	v1<< 1,2,3,4;
	cout<<"v1=\n"<<v1<<endl;

	VectorXd v2(3);
	v2<<1,2,3;
	cout<<"v2=\n"<<v2<<endl;

	Array4i v3;
	v3<<1,2,3,4;
	cout<<"v3=\n"<<v3<<endl;

	ArrayXf v4(3);
	v4<<1,2,3;
	cout<<"v4=\n"<<v4<<endl;

#pragma endregion

#pragma region two_d_object

	cout<<"*******************2D-object****************"<<endl;
	//2D objects:
	MatrixXd m(2,2);

	//method 1
	m(0,0) = 3;
	m(1,0) = 2.5;
	m(0,1) = -1;
	m(1,1) = m(1,0) + m(0,1);

	//method 2
	m<<3,-1,
		2.5,-1.5;
	cout <<"m=\n"<< m << endl;

#pragma endregion

#pragma region Comma_initializer

	cout<<"*******************Initialization****************"<<endl;

	int rows=5;
	int cols=5;
	MatrixXf m1(rows,cols);
	m1<<( Matrix3f()<<1,2,3,4,5,6,7,8,9 ).finished(),
		MatrixXf::Zero(3,cols-3),
		MatrixXf::Zero(rows-3,3),
		MatrixXf::Identity(rows-3,cols-3);
	cout<<"m1=\n"<<m1<<endl;

#pragma endregion

#pragma region Runtime_info

	cout<<"*******************Runtime Info****************"<<endl;

	MatrixXf m2(5,4);
	m2<<MatrixXf::Identity(5,4);
	cout<<"m2=\n"<<m2<<endl;

	MatrixXf m3;
	m3=m1*m2;
	cout<<"m3.rows()="<<m3.rows()<<"  ;  "
		     <<"m3.cols()="<< m3.cols()<<endl;

	cout<<"m3=\n"<<m3<<endl;

#pragma endregion

#pragma region Resizing

	cout<<"*******************Resizing****************"<<endl;

	//1D-resize
	v1.resize(4);
	cout<<"Recover v1 to 4*1 array : v1=\n"<<v1<<endl;

	//2D-resize
	m.resize(2,3);
	m.resize(Eigen::NoChange, 3);
	m.resizeLike(m2);
	m.resize(2,2);

#pragma endregion

#pragma region Coeff_access

	cout<<"*******************Coefficient access****************"<<endl;

	float tx=v1(1);
	tx=m1(1,1);
	cout<<endl;

#pragma endregion

#pragma  region Predefined_matrix

	cout<<"*******************Predefined Matrix****************"<<endl;

	//1D-object
	typedef  Matrix3f   FixedXD;
	FixedXD x;

	x=FixedXD::Zero();
	x=FixedXD::Ones();
	x=FixedXD::Constant(tx);//tx is the value
	x=FixedXD::Random();
	cout<<"x=\n"<<x<<endl;

	typedef ArrayXf Dynamic1D;
	// 或者 typedef VectorXf Dynamic1D
	int size=3;
	Dynamic1D xx;
	xx=Dynamic1D::Zero(size);
	xx=Dynamic1D::Ones(size);
	xx=Dynamic1D::Constant(size,tx);
	xx=Dynamic1D::Random(size);
	cout<<"xx=\n"<<x<<endl;

	//2D-object
	typedef MatrixXf Dynamic2D;
	Dynamic2D y;
	y=Dynamic2D::Zero(rows,cols);
	y=Dynamic2D::Ones(rows,cols);
	y=Dynamic2D::Constant(rows,cols,tx);//tx is the value
	y=Dynamic2D::Random(rows,cols);

#pragma endregion

#pragma region Arithmetic_Operators

	cout<<"******************* Arithmetic_Operators****************"<<endl;

	//add & sub
	MatrixXf m4(5,4);
	MatrixXf m5;
	m4=m2+m3;
	m3-=m2;

	//product
	m3=m1*m2;

	//transposition
	m5=m4.transpose();
	//m5=m.adjoint();// 伴随矩阵

	//dot product
	double xtt;
	cout<<"v1=\n"<<v1<<endl;
	v2.resize(4);
	v2<<VectorXd::Ones(4);
	cout<<"v2=\n"<<v2<<endl;

	cout<<"*************dot product*************"<<endl;
	xtt=v1.dot(v2);
	cout<<"v1.*v2="<<xtt<<endl;

	//vector norm

	cout<<"*************matrix norm*************"<<endl;
	xtt=v1.norm();
	cout<<"norm of v1="<<xtt<<endl;
	xtt=v1.squaredNorm();
	cout<<"SquareNorm of v1="<<xtt<<endl;

#pragma endregion

cout<<endl;
}
```

## 6. Refer Links

[Eigen Document](http://eigen.tuxfamily.org/dox/)

[C++ 矩阵处理工具——Eigen](https://blog.csdn.net/abcjennifer/article/details/7781936)

[Eigen 库使用](https://blog.csdn.net/wokaowokaowokao12345/article/details/53397488)

[强大的 C++ 矩阵处理库 -EIGEN](http://fangda.me/2018/03/17/%E5%BC%BA%E5%A4%A7%E7%9A%84C-%E7%9F%A9%E9%98%B5%E5%A4%84%E7%90%86%E5%BA%93-Eigen/)

[知乎：Eigen 矩阵运算库在实际项目中的使用情况如何？](https://www.zhihu.com/question/21289223)

TODO:

[Eigen 库的使用技巧（一) 基本数据结构及其操作](https://www.aintk.xyz/post/2018-09-17-eigenusage/)

[Eigen 库的使用技巧（二) 求解线性代数问题](https://www.aintk.xyz/post/2018-09-18-eigenusage2/)

[Eigen 库的使用技巧（三) 求解几何问题](https://www.aintk.xyz/post/2018-09-18-eigenusage3/)

[侯凯：Eigen 教程系列](https://zzk.cnblogs.com/s/blogpost?Keywords=blog%3Ahoukai%20eigen&pageindex=1)