

https://blog.csdn.net/kwsy2008/article/details/52227892


在 C/C++ 程序调试中经常用到的几个宏：`__FILE__`、`__FUNCTION__`、`__LINE__`，事实上在 python 中也有类似的：

```python
import sys
def get_cur_info():
    print sys._getframe().f_code.co_filename # 当前文件名，可以通过__file__获得
    print sys._getframe().f_code.co_name # 当前函数名
    print sys._getframe().f_lineno # 当前行号
```