- [C++ 无锁并发 (Lock Free Concurrent)](#c-无锁并发-lock-free-concurrent)
  - [1. Refer Links](#1-refer-links)

# C++ 无锁并发 (Lock Free Concurrent)

<!-- TODO: -->

- GCC CAS

  GCC4.1+ 版本中支持 CAS 的原子操作（完整的原子操作可参看 [GCC Atomic Builtins](http://gcc.gnu.org/onlinedocs/gcc-4.1.1/gcc/Atomic-Builtins.html)）
  ```cpp
  bool __sync_bool_compare_and_swap (type *ptr, type oldval type newval, ...)
  type __sync_val_compare_and_swap (type *ptr, type oldval type newval, ...)
  ```

- Windows CAS

  在 Windows 下，你可以使用下面的 Windows API 来完成 CAS（完整的 Windows 原子操作可参看 MSDN 的 [InterLocked Functions](http://msdn.microsoft.com/en-us/library/windows/desktop/ms686360(v=vs.85).aspx#interlocked_functions)）：
  ```cpp
  InterlockedCompareExchange (__inout LONG volatile *Target,
                                  __in LONG Exchange,
                                  __in LONG Comperand);
  ```

- C++11 CAS

  C++11 STL 中的 atomic 类的函数可以让你跨平台（完整的 C++11 的原子操作可参看 [Atomic Operation Library](http://en.cppreference.com/w/cpp/atomic)）:
  ```cpp
  template< class T >
  bool atomic_compare_exchange_weak( std::atomic* obj,
                                    T* expected, T desired );
  template< class T >
  bool atomic_compare_exchange_weak( volatile std::atomic* obj,
                                    T* expected, T desired );
  ```

- C++ Boost CAS
  ```cpp

  ```

## 1. Refer Links

TODO:

[C++ 的无锁数据结构在工业界有真正的应用吗？](https://www.zhihu.com/question/52629893)
