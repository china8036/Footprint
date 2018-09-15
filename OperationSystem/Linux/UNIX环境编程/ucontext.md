- [ucontext](#ucontext)
  - [1. 基本 API](#1-基本-api)
    - [1.1. structure](#11-structure)
    - [1.2. function](#12-function)
  - [2. 构建协程库](#2-构建协程库)
  - [3. Refer Links](#3-refer-links)

# ucontext

## 1. 基本 API

在类 System V 环境中，在头文件 ucontext.h 中定义了两个结构类型（mcontext_t 和 ucontext_t）和四个函数 getcontext(),setcontext(),makecontext(),swapcontext()，简单说来， getcontext 获取当前上下文，setcontext 设置当前上下文，swapcontext 切换上下文，makecontext 创建一个新的上下文，利用它们可以在一个进程中实现用户级的线程切换。

### 1.1. structure

- mcontext_t 类型与机器相关，并且不透明。
- ucontext_t 结构体则至少拥有以下成员：
  - `ucontext_t *uc_link`     pointer to the context that will be resumed when this context returns，当当前上下文运行终止时系统会恢复 uc_link 指向的上下文。
  - `sigset_t    uc_sigmask`  the set of signals that are blocked when this context is active，uc_sigmask 为该上下文中的阻塞信号集合。
  - `stack_t     uc_stack`    the stack used by this context，uc_stack 为该上下文中使用的栈。
  - `mcontext_t  uc_mcontext` a machine-specific representation of the saved context，uc_mcontext 保存的上下文的特定机器表示，包括调用线程的特定寄存器等。

### 1.2. function

- `int  getcontext(ucontext_t *ucp);`: get current user context，将当前的上下文保存到 ucp 中。
- `int  setcontext(const ucontext_t *ucp);`: set current user context，setcontext 的上下文 ucp 应该通过 getcontext 或者 makecontext 取得，如果调用成功则不返回。
  - 如果上下文是通过调用 getcontext() 取得，程序会继续执行这个调用。
  - 如果上下文是通过调用 makecontext() 取得，程序会调用 makecontext() 函数的第二个参数指向的函数，如果 func 函数返回，则恢复 makecontext() 第一个参数指向的上下文 context_t 中指向的 uc_link。如果 uc_link 为 NULL，则线程退出。
- `void makecontext(ucontext_t *ucp, (void *func)(), int argc, ...);`: manipulate user contexts，makecontext 修改通过 getcontext() 取得的上下文 ucp（这意味着调用 makecontext 前必须先调用 getcontext），然后将该上下文的栈空间设置为 ucp->stack，设置后继的上下文为 ucp->uc_link.
- `int swapcontext(ucontext_t *oucp, const ucontext_t *ucp);`: swap user context，保存当前上下文到 oucp 结构体中，然后激活 upc 上下文。

eg:
```cpp
#include <stdio.h>
#include <ucontext.h>
#include <unistd.h>
 
int main(int argc, const char *argv[]){
    ucontext_t context;
 
    getcontext(&context);
    puts("Hello world");
    sleep(1);
    setcontext(&context);
    return 0;
}
```
运行：
```
$ ./example 
Hello world
^C
```

## 2. 构建协程库

[ucontext- 人人都可以实现的简单协程库](https://blog.csdn.net/qq910894904/article/details/41911175)

## 3. Refer Links

[<ucontext.h> - The Open Group](http://pubs.opengroup.org/onlinepubs/7908799/xsh/ucontext.h.html)

[makecontext, swapcontext](http://pubs.opengroup.org/onlinepubs/7908799/xsh/makecontext.html)

[getcontext, setcontext](http://pubs.opengroup.org/onlinepubs/7908799/xsh/getcontext.html)

[ucontext- 人人都可以实现的简单协程库](https://blog.csdn.net/qq910894904/article/details/41911175)
