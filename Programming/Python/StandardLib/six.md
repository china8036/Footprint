
SIX 是用于 python2 与 python3 兼容的库，它存在的目的是为了拥有无需修改即可在 Python 2 和 Python 3 上同时工作的代码。话虽这么说，但是这并不代表在 Python 3 中引用该库就可以轻松地跑 Python 2 的代码。

实际上，SIX 是重定义了在 python2 和 3 中有差异的函数，例如 dict 的获取全部键值函数：在 Python2 中是 `.iterkeys()` 在 Python3 中是 `.keys()`，而在 SIX 中是 `six.iterkeys(dict)`（当然对应版本的原函数也能够使用）。

也就是说，离开了 SIX 库的话你写的代码不论在 Python2 还是 Python3 中都无法运行。因此不是急于追求兼容性的话并不需要使用这个库。

https://openingsource.org/980/

https://www.kawabangga.com/posts/2360