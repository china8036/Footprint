- [Linux 环境编程基础](#linux-%E7%8E%AF%E5%A2%83%E7%BC%96%E7%A8%8B%E5%9F%BA%E7%A1%80)
  - [Windows下进行Linux编程](#windows%E4%B8%8B%E8%BF%9B%E8%A1%8Clinux%E7%BC%96%E7%A8%8B)
    - [Cygwin](#cygwin)
    - [MinGW](#mingw)
      - [MSYS](#msys)
      - [MinGW 和 Cygwin 的区别](#mingw-%E5%92%8C-cygwin-%E7%9A%84%E5%8C%BA%E5%88%AB)
  - [自动化编译工具](#%E8%87%AA%E5%8A%A8%E5%8C%96%E7%BC%96%E8%AF%91%E5%B7%A5%E5%85%B7)
    - [Cmake](#cmake)

# Linux 环境编程基础

## Windows下进行Linux编程

### Cygwin

[cygwin](www.cygwin.com) 模拟 POSIX 系统，目的是提供运行于 Windows 平台的类 Unix 环境（以 GNU 工具为代表），可以让使用者在 windows 上执行 linux 的程序，为了达到这个目的，Cygwin 提供了一套抽象层 dll，用于将部分 Posix 调用转换成 Windows 的 API 调用，实现相关功能。

Cygwin 的目录结构基本照搬了 linux 的样子，但同时，也兼容了 Windows 的许多功能：大部分应用使用 Unix 风格的路径，Windows 的盘符通过类似挂载点的方式提供给 Cygwin 使用；Cygwin 中既可以运行 Cygwin 的应用（依赖模拟层），又可以运行 Windows 应用，而传递给应用的路径会经过它的模拟层变换，以此保证程序运行不会出错。

Cygwin 是运行于 Windows 平台的 POSIX“子系统”，提供 Windows 下的类 Unix 环境，并提供将部分 Linux 应用“移植”到 Windows 平台的开发环境的一套软件。

### MinGW

> MinGW, a contraction of "Minimalist GNU for Windows", is a minimalist development environment for native Microsoft Windows applications.

[MinGW](http://www.mingw.org/) 的作用是让开发者可以在 windows 操作系统上方便的使用 gcc 等 GNU 开发工具，是用于开发原生（32 位） Windows 应用的开发环境。它是一些头文件和端口库的集合，该集合允许人们在没有第三方动态链 接库的情况下使用 GCC（GNU Compiler C）产生 Windows32 程序。

MinGW 主要由 GNU binary utilities、GCC 和 GDB 组成。同时还包括一些必要的库，例如 libc（C Runtime），及专门用于 Win32 环境的 API 接口库。

MinGW 是极简主义者，不会尝试为 MS-Windows 上的 POSIX 应用程序部署提供 POSIX 运行时环境。如果你想在这个平台上部署 POSIX 应用程序，请考虑使用 Cygwin。

MinGW 被许多 Linux 上发展起来的开发工具选择为 Windows 版本的默认编译器，例如 CodeBlocks，例如 DevC++。

#### MSYS

> MSYS, a contraction of "Minimal SYStem", is a Bourne Shell command line interpreter system. Offered as an alternative to Microsoft's cmd.exe, this provides a general purpose command line environment, which is particularly suited to use with MinGW, for porting of many Open Source applications to the MS-Windows platform; a light-weight fork of Cygwin-1.3, it includes a small selection of Unix tools, chosen to facilitate that objective.

[MSYS](http://www.mingw.org/wiki/MSYS) 是 GNU 实用程序（如 bash，make，gawk 和 grep 等）的集合，允许构建依赖于传统 UNIX 工具的应用程序和程序。它旨在补充 MinGW 和 cmd shell 的缺陷。

由于 MinGW 本身仅代表工具链，而在 Windows 下配套的命令行工具不够齐全，因此，MinGW 开发者从曾经比较旧的 Cygwin 创建了一个分支，也用于提供类 Unix 环境。但与 Cygwin 的大而全不同，MSYS 是冲着小巧玲珑的目标去的，所以整套 MSYS 以及 MinGW，主要以基本的 Linux 工具为主，大小在 200M 左右，并且没有多少扩展能力。用于辅助 Windows 版 MinGW 进行命令行开发的配套软件包，提供了部分 Unix 工具以使得 MinGW 的工具使用起来方便一些。

#### MinGW 和 Cygwin 的区别

MinGW 和 Cygwin 同样是在 Windows 仲模拟 unix 环境，两者有什么区别？

- 从实现上说
  - Cygwin 是用一个 dll 模拟 linux 环境来“欺骗”应用程序，好像自己运行在 linux 环境下。
  - MinGW 是在编译时提供 linux 到 windows 必要代码的“翻译”转换，用到的还是 windows 运行时库。

- 从目标上说 
  - MinGW 是让 Windows 用户可以用上 GNU 工具，比如 GCC，有点像用 Linux 开发环境开发 Windows 程序。
  - Cygwin 提供完整的类 Unix 环境，Windows 用户不仅可以使用 GNU 工具，还可以调用 Unix 的系统函数，理论上 Linux 上的程序只要用 Cygwin 重新编译，就可以在 Windows 上运行。Cygwin 是运行在 Windows 下的，但是它使用的是 Unix 系统的函数和思想。

- 从能力上说
  - 如果程序只用到 C/C++ 标准库，可以用 MinGW 或 Cygwin 编译。
  - 如果程序还用到了 POSIX API，则只能用 Cygwin 编译。

- 从依赖上说 
  - 程序经 MinGW 编译后可以直接在 Windows 上面运行。
  - 程序经 Cygwin 编译后运行，需要依赖安装时附带的 cygwin1.dll。

- 联系：均提供了部分 Linux 下的应用，多跑在 Windows 上；MinGW 作为 Cygwin 下的软件包，可以在 Cygwin 上运行。



## 自动化编译工具

文本形式的代码到可执行文件生成无论在什么平台大致分为以下几个部分： 
- 用编辑器编写源代码，如.c文件。 
- 用编译器编译代码生成目标文件，如.o。 
- 用链接器连接目标代码生成可执行文件，如.exe和.out。 

Linux平台下，编译和链接一般通过gcc或g++来完成（g++默认链接c++库，所以一般情况下用gcc编译c文件，而g++编译cpp文件。当然g++也可以编译c文件，而gcc编译cpp文件则需要在后面加上参数-lstdc++，作用就是链接c++库），但是如果编译和链接的阶段如果源文件太多，一个一个编译时就会特别麻烦，因此就产生了自动化编译工具make，来批处理编译源文件。

make是一个自动化编译工具，通过make你可以使用一条命令实现整个项目的完全编译。但是你需要编写一个规则文件，make依据它来批处理编译，这个文件就是makefile。

对于一个大工程，编写makefile实在是件复杂的事，于是人们又想，为什么不设计一个工具，读入所有源文件之后，自动生成makefile呢，于是就出现了cmake工具，它能够输出各种各样的makefile或者project文件,从而帮助程序员减轻负担。但是随之而来也就是编写cmakelist文件，它是cmake所依据的规则。

原文件—cmakelist —cmake —makefile —make —生成可执行文件（make中则包含了多条链接以及gcc/g++编译语句）

### Cmake

> CMake is an open-source, cross-platform family of tools designed to build, test and package software. CMake is used to control the software compilation process using simple platform and compiler independent configuration files, and generate native makefiles and workspaces that can be used in the compiler environment of your choice. The suite of CMake tools were created by Kitware in response to the need for a powerful, cross-platform build environment for open-source projects such as ITK and VTK.

[CMake](https://cmake.org/) 是个跨平台的自动化建构系统，它用组态档控制建构过程（build process）的方式和 Unix 的 Make 相似，只是 CMake 的组态档取名为 CmakeLists.txt。Cmake 并不直接建构出最终的软件，而是产生标准的建构档（如 Unix 的 Makefile 或 Windows Visual C++ 的 projects/workspaces），然后再依一般的建构方式使用。这使得熟悉某个集成开发环境（IDE）的开发者可以用标准的方式建构他的软件， 这种可以使用各平台的原生建构系统的能力是 CMake 和 SCons 等其他类似系统的区别之处。
