
# TODO

## 1. C++ 模板不支持分离编译

https://zhuanlan.zhihu.com/p/31111908

因为 Eigen 采用模板方式实现，由于模板函数不支持分离编译，所以只能提供源码而不是动态库的方式供用户使用，不过这也也更方面用户使用和研究。

https://blog.csdn.net/houjixin/article/details/8093701

## 2. 各种编译器

http://eigen.tuxfamily.org/index.php?title=Main_Page

Eigen is being successfully used with the following compilers:
- [GCC](http://gcc.gnu.org/), version 4.8 and newer. Older versions of gcc might work as well but they are not tested anymore.
- [MSVC](http://en.wikipedia.org/wiki/Visual_C%2B%2B) (Visual Studio), 2012 and newer. Be aware that enabling IntelliSense (/FR flag) is known to trigger some internal compilation errors. The old 3.2 version of Eigen supports MSVC 2010, and the 3.1 version supports MSVC 2008.
- [Intel C++ compiler](http://en.wikipedia.org/wiki/Intel_C%2B%2B_Compiler). Enabling the `-inline-forceinline` option is highly recommended.
- [LLVM/CLang++](http://clang.llvm.org/cxx_status.html), version 3.4 and newer. (The 2.8 version used to work fine, but it is not tested with up-to-date versions of Eigen)
- XCode 7 and newer. Based on LLVM/CLang.
- [MinGW](http://en.wikipedia.org/wiki/Mingw), recent versions. Based on GCC.
- QNX's QCC compiler.

Regarding performance, Eigen performs best with compilers based on GCC or LLVM/Clang.

## 3. Refer Links
