
- 定义无穷大
  ```cpp
  #define INF 0x3f3f3f3f // 比 0x7fffffff 更适用
  ```

- 将数组的每个元素都定义为无穷大
  ```cpp
  int a[20];
  memset(a, 0x3f, sizeof(a));
  ```

- 定义圆周率
  ```cpp
  const double PI = acos(-1, 0);
  ```




