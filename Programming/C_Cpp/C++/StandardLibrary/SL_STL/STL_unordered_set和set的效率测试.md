- [STL unordered_set 和 set 的效率测试](#stl-unordered_set-和-set-的效率测试)
  - [1. 背景](#1-背景)
  - [2. 测试环境](#2-测试环境)
  - [3. 测试过程](#3-测试过程)
  - [4. 测试结果](#4-测试结果)
  - [5. 原理分析](#5-原理分析)
  - [6. Refer Links](#6-refer-links)

# STL unordered_set 和 set 的效率测试

## 1. 背景

set 采用红黑树实现，find 操作的平均时间效率为 O(lg n)，unordered_set 采用哈希表实现，find 操作的平均时间效率为 O(1)。也就是说，随着数据量增大，set 的 find 操作耗时逐渐增加，而 unordered_set 可以维持稳定。那么在小数据量的情况下，对于 find 操作 set 是否能比 unordered_set 更快呢？

## 2. 测试环境

- Ubuntu Windows Subsystem
- C++ 11
- Realse Build

## 3. 测试过程

1.	创建一个长度为 N 的 int 数组，其每个元素数值范围为 1~1000000000，并将该数组分别填充到 unordered_set 和 set 中。
1.	对 unordered_set 执行 50000000 次以递增 int 值为参数的 find 调用，统计其耗时。
1.	对 set 执行 50000000 次以递增 int 值为参数的 find 调用，统计其耗时。
1.	重复 1、2、3 步骤 1000 次，取平均值。

```cpp
#include <iostream>
#include <map>
#include <unordered_map>
#include <set>
#include <unordered_set>
#include "sys/time.h"
#include "sys/unistd.h"

using namespace std;

const int CALL_TIMES = 50000000;

const int REPEAT_TIMES = 50000000;

long getCurrentTime()
{
    struct timeval tv{};
    gettimeofday(&tv, NULL);
    return tv.tv_sec * 1000000 + tv.tv_usec;

}

string randStr(const int len)
{
    string str;
    srand(time(NULL));
    for (int i = 0; i < len; i++)
    {
        switch ((rand() % 3))
        {
            case 1:
                str += 'A' + rand() % 26;
                break;
            case 2:
                str += 'a' + rand() % 26;
                break;
            default:
                str += '0' + rand() % 10;
                break;
        }
    }
    return str;
}

void test_random_string(long N)
{
    unordered_set<string> str_umap;
    set<string> str_map;

    srand(time(NULL));
    int L = 1;
    int R = 100;

    string key[N];
    for (int i = 0; i < N; i++)
    {
        key[i] = randStr(rand() % (R - L + 1) + L);
    }

    for (int i = 0; i < N; i++)
    {
        str_umap.insert(key[i]);
        str_map.insert(key[i]);
    }

    cout << "[unorder_map] ";
    long totalTime = 0;
    long startTime = getCurrentTime();
    for (int t = 0; t < CALL_TIMES; t++)
    {
        static int i = 0;
        str_umap.find(key[i++]);
        if (i == N)
        {
            i = 0;
        }
    }
    long endTime = getCurrentTime();
    totalTime = endTime - startTime;
    cout << " Total Time: "
         << totalTime
         << " ns" << endl;

    cout << "[map] ";
    totalTime = 0;
    startTime = getCurrentTime();
    for (int t = 0; t < CALL_TIMES; t++)
    {
        static int i = 0;
        str_map.find(key[i++]);
        if (i == N)
        {
            i = 0;
        }
    }
    endTime = getCurrentTime();
    totalTime = endTime - startTime;
    cout << " Total Time: "
         << totalTime
         << " ns" << endl;
}

void test_random_int(long N)
{
    unordered_set<int> int_umap;
    set<int> int_map;

    //int_umap.max_load_factor(0.1);
    srand(time(NULL));
    int L = 1;
    int R = 1000000000;

    int key[N];
    for (int i = 0; i < N; i++)
    {
        key[i] = rand() % (R - L + 1) + L;
    }

    for (int i = 0; i < N; i++)
    {
        int_umap.insert(key[i]);
        int_map.insert(key[i]);
    }

    ////////////////////////////////////////////

    cout << "[unorder_map] ";
    long totalTime = 0;
    long startTime = getCurrentTime();
    for (int t = 0; t < CALL_TIMES; t++)
    {
        static int i = 0;
        int_umap.find(key[i++]);
        if (i == N)
        {
            i = 0;
        }
    }
    long endTime = getCurrentTime();
    totalTime = endTime - startTime;
    cout << " Total Time: "
         << totalTime
         << " ns" << endl;
    ////////////////////////////////////////////

    cout << "[map] ";
    totalTime = 0;
    startTime = getCurrentTime();
    for (int t = 0; t < CALL_TIMES; t++)
    {
        static int i = 0;
        int_map.find(key[i++]);
        if (i == N)
        {
            i = 0;
        }
    }
    endTime = getCurrentTime();
    totalTime += endTime - startTime;
    cout << " Total Time: "
         << totalTime
         << " us" << endl;

    ////////////////////////////////////////////

}

void test(long N)
{
    cout << "----------------- N = " << N << " -----------------" << endl;
//    cout << "---------- test random string -----------" << endl;
//    test_random_string(N);

    cout << "---------- test random int -----------" << endl;
    test_random_int(N);

    cout << "--------------------------------------------\n\n";
}

void test_cmp_and_mod()
{
    srand(time(NULL));
    int L = 1;
    int R = 100000000;

    long startTime = 0;
    long endTime = 0;
    long times = 10000000;

    int a = rand() % (R - L + 1) + L;
    int b = rand() % (R - L + 1) + L;
    int d = 9;

    startTime = getCurrentTime();
    for (int i = 0; i < times; i++)
    {
        int c = i % b;
        d &= c; // 避免编译器优化
    }
    endTime = getCurrentTime();
    cout << "MOD: " << endTime - startTime << " us" << endl;

    startTime = getCurrentTime();
    for (int i = 0; i < times; i++)
    {
        int c = i > b;
        d &= c; // 避免编译器优化
    }
    endTime = getCurrentTime();
    cout << "CMP: " << endTime - startTime << " us" << d << endl; // d 避免编译器优化

}

int main() {

//    test_cmp_and_mod();
//    return 0;
    test(50);
    test(100);
    test(150);
    test(200);
    test(250);
    test(300);
    test(400);
    test(500);
    test(600);

    return 0;
}
```

## 4. 测试结果

| 容器 size | unordered_set 调用 5 千万次 find 平均耗时 | set 调用 5 千万次 find 平均耗时 |
| :------: | :----------------------------------: | :------------------------: |
| 50       | 613638   us                          | 245483   us                |
| 100      | 517543   us                          | 279136   us                |
| 150      | 533324   us                          | 310197   us                |
| 200      | 495034   us                          | 363108   us                |
| 250      | 553043 us                            | 462667 us                  |
| 300      | 563446 us                            | 842430 us                  |
| 400      | 579653   us                          | 1463373   us               |
| 500      | 527805   us                          | 1823121   us               |
| 600      | 542060   us                          | 2160208   us               |

从测试结果的数据中可知，**unordered_set 和 set 的查找性能分界线大致在 250~300 之间**：
- 当容器 size 小于分界线时，set 表现出更优的查找性能；
- 当容器 size 大于分界线时，unordered_set 表现出更优的查找性能。

## 5. 原理分析

首先结论是：**当数据量较小时，Hashtable 的常数级查找效率并不能弥补 MOD 操作相比 BST 中 CMP 操作的大量耗时，因此 set 的查找效率反而会更优。而当数据量超过了分界线，MOD 操作的耗时差距基本被数据结构弥补，因此 unordered_set 开始发挥查找优势**。

在较大数据量情况下，unordered_set 查找速度比 set 更快，原理是 unordered_set 使用哈希表实现，其查找操作的平均时间效率为 O(1)；而 set 使用 BST 实现，其查找的平均时间效率为 O(lg n)。

在小数据量情况下，考虑到 unordered_set 执行查找操作时，哈希计算用到了取模运算，set 执行查找操作时，会进行数值大小的比较运算，而取模运算的 MOD 指令执行速度是比较运算的 CMP 指令慢得多。

进行进一步测试，分别执行 10000000 次 % 运算和 > 运算，得到以下结果：

| 运算     | >       | %        |
| -------- | ------- | -------- |
| 平均耗时 | 2750 us | 23765 us |

可见，MOD 运算的时间基本是 CMP 运算的 8 倍左右。

## 6. Refer Links

[编程语言中运算符的运算效率](https://blog.csdn.net/morgerton/article/details/64918560)