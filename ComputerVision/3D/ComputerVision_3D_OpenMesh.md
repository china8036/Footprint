- [计算机视觉 - 三维模型：OpenMesh](#计算机视觉---三维模型openmesh)
	- [1. 基本概念](#1-基本概念)
	- [2. 安装配置](#2-安装配置)
		- [2.1. 源码编译安装](#21-源码编译安装)
		- [2.2. 二进制安装包安装](#22-二进制安装包安装)
	- [3. 基本操作](#3-基本操作)
		- [3.1. 迭代器 (Iterator)](#31-迭代器-iterator)
		- [3.2. 循环器 (Circulator)](#32-循环器-circulator)
		- [3.3. 读写文件](#33-读写文件)
	- [4. Refer Links](#4-refer-links)

# 计算机视觉 - 三维模型：OpenMesh

## 1. 基本概念

> OpenMesh is a generic and efficient library that offers data structures for representing and manipulating polygonal meshes. It is a powerful tool for handling polygonal meshes.

[OpenMesh](https://www.openmesh.org/) 是一个提供了用于表示和操作多边形网格数据结构的通用且高效率的库，用户可以根据应用的需要定制网格类型，还可以提供自定义的表示点、边和面的数据结构或可以方便地使用 OpenMesh 中预定义的结构。

OpenMesh 使用半边数据结构 (Halfedge Data Structure) 来存储和管理网格元素（点、边、面）和它们之间的连接关系。

OpenMesh 的实现和内部结构，使得其具有以下特点和功能：
- 处理一般的多边形网格，而不仅限于三角网格
- 顶点、半边、边和面的显示表示
- 顶点 1-ring 领域的快速访问
- 处理非流形顶点
- 更改标量、坐标等的数据类型，默认为 float
- 为网格元素（点、边、面）增加自定义的属性，或开关预定义标准属性

e.g.
```cpp
#include <iostream>

// −−−−−−−−−−−−−−−−−−−− OpenMesh
#include <OpenMesh/Core/IO/MeshIO.hh>
#include <OpenMesh/Core/Mesh/PolyMesh_ArrayKernelT.hh>

using namespace std;

// 先定义一个 MyMesh 类
typedef OpenMesh::TriMesh_ArrayKernelT<> MyMesh;
...

// 定义个 MyMesh 类型的变量
MyMesh mesh;
...

// 当要用到元素循环器 / 迭代器时，代码通常是这样的
for (MyMesh::FaceIter f_it = mesh.faces_begin(); f_it != mesh.faces_end(); ++f_it)
{
    for (MyMesh::FaceVertexIter fv_it = mesh.fv_iter(*f_it); fv_it.is_valid(); ++fv_it)
    {
        // do something with fv_it
    }
}
...

// 访问一个点位置数据或法向
auto pos = mesh.point(vh); // vh 是某个顶点的句柄
auto pos = mesh.normal(vh); // vh 是某个顶点的句柄
...

// 访问一个函数，也要通过。操作
bool flag = mesh.is_boundary(fh);// vh 是某个面的句柄
```

## 2. 安装配置

OpenMesh 支持在以下平台和编译器中使用：
- Linux (gcc and clang)
- Windows
	- Microsoft Visual Studio 2015,2017
	- MinGW
- MacOS X

### 2.1. 源码编译安装

[OpenMesh Document: Compiling OpenMesh](https://www.openmesh.org/media/Documentations/OpenMesh-Doc-Latest/a04067.html)

### 2.2. 二进制安装包安装

在 Windows 平台上，最简单的安装方式是使用针对 Visual Studio 编译的二进制安装包进行安装（以下例子使用静态链接文件，可省去配置系统环境变量的步骤）：

![image](http://img.cdn.firejq.com/jpg/2019/4/11/d5a2446e1784bda35b8abe95528b35d8.jpg)

NOTE：选择二进制安装包时，VS 的版本需要与实际开发使用的 VS 版本相对应，否则在运行代码时会出现很多编译器错误。

项目配置过程：
1. 添加包含目录和库目录：

		![image](http://img.cdn.firejq.com/jpg/2019/4/10/467129f0840336ac5669a6ee5b5e559b.jpg)

		![image](http://img.cdn.firejq.com/jpg/2019/4/10/0b85bcf2d58e18ccc5e18925e4ef1b1f.jpg)

1. 添加附加依赖项：

		```
		OpenMeshCored.lib
		OpenMeshToolsd.lib
		```

		![image](http://img.cdn.firejq.com/jpg/2019/4/10/c86e579ca8de1fbd3525023d1159ddb6.jpg)

1. 添加预处理器定义：

		```
		_USE_MATH_DEFINES
		OM_STATIC_BUILD
		```

		![image](http://img.cdn.firejq.com/jpg/2019/4/10/094224d0de8cedecc8d7b6fa85c33977.jpg)

## 3. 基本操作

### 3.1. 迭代器 (Iterator)

https://www.openmesh.org/media/Documentations/OpenMesh-Doc-Latest/a04080.html

> The mesh provides linear iterators (that enumerate vertices, halfedges, edges, and faces). These can be used to easily navigate through a mesh.
>
> All iterators are defined in the namespace OpenMesh::Iterators. They are template classes that expect a mesh as template argument to be fully specified. You should use the iterator types provided by the mesh itself, i.e. MyMesh::VertexIter instead of OpenMesh::Iterators::VertexIterT<MyMesh>.
>
> When using iterators, use the pre-increment operation (++it) for efficiency reasons.

Iterator 用于遍历 Mesh 中的所有点、面、边或半边。

e.g.
```cpp
MyMesh mesh;
// iterate over all vertices
for (MyMesh::VertexIter v_it=mesh.vertices_begin(); v_it!=mesh.vertices_end(); ++v_it)
   ...; // do something with *v_it, v_it->, or *v_it
// iterate over all halfedges
for (MyMesh::HalfedgeIter h_it=mesh.halfedges_begin(); h_it!=mesh.halfedges_end(); ++h_it)
   ...; // do something with *h_it, h_it->, or *h_it
// iterate over all edges
for (MyMesh::EdgeIter e_it=mesh.edges_begin(); e_it!=mesh.edges_end(); ++e_it)
   ...; // do something with *e_it, e_it->, or *e_it
// iterator over all faces
for (MyMesh::FaceIter f_it=mesh.faces_begin(); f_it!=mesh.faces_end(); ++f_it)
   ...; // do something with *f_it, f_it->, or *f_it
```

### 3.2. 循环器 (Circulator)

> OpenMesh also provides so called Circulators that provide means to enumerate items adjacent to another item of the same or another type.

Circulator 用于访问 Mesh 中一个元素的所有近邻元素，如对于一个顶点，可通过 Circulator 访问该顶点的近邻点 (1-ring 领域)、近邻面、近邻表、近邻入射半边和近邻出射半边。

The circulators around a vertex are:
- VertexVertexIter: iterate over all neighboring vertices.
- VertexIHalfedgeIter: iterate over all incoming halfedges.
- VertexOHalfedgeIter: iterate over all outgoing halfedges.
- VertexEdgeIter: iterate over all incident edges.
- VertexFaceIter: iterate over all adjacent faces.

The circulators around a face are:
- FaceVertexIter: iterate over the face's vertices.
- FaceHalfedgeIter: iterate over the face's halfedges.
- FaceEdgeIter: iterate over the face's edges.
- FaceFaceIter: iterate over all edge-neighboring faces.

Other circulators:
- HalfedgeLoopIter: iterate over a sequence of Halfedges. (all Halfedges over a face or a hole)

OpenMesh 允许用户指定循环器遍历邻域的顺序，只要在标准循环器名称的 Iter 前添加 CW（顺时针）或 CCW（逆时针）即可，如：
```cpp
VertexVertexIter vvit = mesh.vv_iter(some_vertex_handle);          // fastest (clock or counterclockwise)
VertexVertexCWIter vvcwit = mesh.vv_cwiter(some_vertex_handle);    // clockwise
VertexVertexCCWIter vvccwit = mesh.vv_ccwiter(some_vertex_handle); // counter-clockwise
```

e.g.
```cpp
MyMesh mesh;
// (linearly) iterate over all vertices
for (MyMesh::VertexIter v_it=mesh.vertices_sbegin(); v_it!=mesh.vertices_end(); ++v_it)
{
  // circulate around the current vertex
  for (MyMesh::VertexVertexIter vv_it=mesh.vv_iter(*v_it); vv_it.is_valid(); ++vv_it)
  {
    // do something with e.g. mesh.point(*vv_it)
  }
}
```

e.g.
```cpp
// Enumerating all halfedges adjacent to a certain face (the inner halfedges)
MyMesh mesh;
...
// Assuming faceHandle contains the face handle of the target face
MyMesh::FaceHalfedgeIter fh_it = mesh.fh_iter(faceHandle);
for(; fh_it.is_valid(); ++fh_it) {
    std::cout << "Halfedge has handle " << *fh_it << std::endl;
}
```

### 3.3. 读写文件

https://www.openmesh.org/media/Documentations/OpenMesh-Doc-Latest/a04079.html

OpenMesh 对网格数据文件的读写功能封装在命名空间 OpenMesh::MeshIO 中，因此需要引入头文件 `#include <OpenMesh/Core/IO/MeshIO.hh>`，其中主要通过 IOManager 来进行网格数据文件的读写操作：

![image](http://img.cdn.firejq.com/jpg/2019/3/22/16c4b96abcf0b91e7cb28a9a5ed040f0.jpg)

IOManager uses the filename extension to determine which reader/writer to use. I.e. if passing "inputmesh.obj" as filename parameter, the OBJ-File reader/writer will be used to parse/write the file.

- 读取网格文件
	```cpp
	MyMesh mesh;

	if (!OpenMesh::IO::read_mesh(mesh, "some input file"))
	{
		std::cerr << "read error\n";
		exit(1);
	}
	```

- 写入网格文件
	```cpp
	MyMesh mesh;

	// do something with your mesh ...
	if (!OpenMesh::IO::write_mesh(mesh, "some output file"))
	{
		std::cerr << "write error\n";
		exit(1);
	}
	```

	e.g.
	```cpp
	#include <iostream>

	// −−−−−−−−−−−−−−−−−−−− OpenMesh
	#include <OpenMesh/Core/IO/MeshIO.hh>
	#include <OpenMesh/Core/Mesh/PolyMesh_ArrayKernelT.hh>

	using namespace std;

	typedef OpenMesh::PolyMesh_ArrayKernelT<> MyMesh;

	int main() {
		MyMesh mesh ;
		MyMesh::VertexHandle vhandle[8];
		vhandle[0] = mesh.add_vertex(MyMesh::Point(-1, -1, 1) );
		vhandle[1] = mesh.add_vertex(MyMesh::Point (1 , -1, 1) );
		vhandle[2] = mesh.add_vertex(MyMesh::Point (1 , 1 , 1) );
		vhandle[3] = mesh.add_vertex(MyMesh::Point(-1, 1 , 1) );
		vhandle[4] = mesh.add_vertex(MyMesh::Point(-1, -1, -1));
		vhandle[5] = mesh.add_vertex(MyMesh::Point (1 , -1, -1));
		vhandle[6] = mesh.add_vertex(MyMesh::Point (1 , 1 , -1));
		vhandle[7] = mesh.add_vertex(MyMesh::Point(-1, 1 , -1));

		// generate ( quadrilateral ) faces
		std::vector<MyMesh::VertexHandle> face_vhandles;
		face_vhandles.clear() ;
		face_vhandles.push_back(vhandle[0]);
		face_vhandles.push_back(vhandle[1]);
		face_vhandles.push_back(vhandle[2]);
		face_vhandles.push_back(vhandle[3]);
		mesh.add_face(face_vhandles);
		face_vhandles.clear();
		face_vhandles.push_back(vhandle[7]);
		face_vhandles.push_back(vhandle[6]);
		face_vhandles.push_back(vhandle[5]);
		face_vhandles.push_back(vhandle[4]);
		mesh.add_face(face_vhandles);
		face_vhandles.clear();
		face_vhandles.push_back(vhandle[1]);
		face_vhandles.push_back(vhandle[0]);
		face_vhandles.push_back(vhandle[4]);
		face_vhandles.push_back(vhandle[5]);
		mesh.add_face(face_vhandles);
		face_vhandles.clear();
		face_vhandles.push_back(vhandle[2]);
		face_vhandles.push_back(vhandle[1]);
		face_vhandles.push_back(vhandle[5]);
		face_vhandles.push_back(vhandle[6]);
		mesh.add_face (face_vhandles);
		face_vhandles.clear();
		face_vhandles.push_back(vhandle[3]);
		face_vhandles.push_back(vhandle[2]);
		face_vhandles.push_back(vhandle[6]);
		face_vhandles.push_back(vhandle[7]);
		mesh.add_face(face_vhandles);
		face_vhandles.clear();
		face_vhandles.push_back(vhandle[0]);
		face_vhandles.push_back(vhandle[3]);
		face_vhandles.push_back(vhandle[7]);
		face_vhandles.push_back(vhandle[4]);
		mesh.add_face(face_vhandles);

		// write mesh to output.obj
		try
		{
			if (!OpenMesh::IO::write_mesh(mesh, "output.off")) {
				std::cerr << "Cannot write mesh to file 'output.off'" << std::endl;
				return 1;
			}
		} catch (std::exception& x) {
			std::cerr << x.what() << std::endl;
			return 1;
		}
		return 0;
	}
	```

## 4. Refer Links

[OpenMesh Document](https://www.openmesh.org/media/Documentations/OpenMesh-Doc-Latest/index.html)

TODO:

[OpenMesh 入门教程中文版](https://blog.csdn.net/chaojiwudixiaofeixia/article/details/51815266)

[OpenMesh 的进阶用法](https://blog.csdn.net/chaojiwudixiaofeixia/article/details/79882155)
