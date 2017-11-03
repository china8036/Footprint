- [排序](#%E6%8E%92%E5%BA%8F)
  - [内部排序](#%E5%86%85%E9%83%A8%E6%8E%92%E5%BA%8F)
    - [比较排序](#%E6%AF%94%E8%BE%83%E6%8E%92%E5%BA%8F)
      - [插入排序](#%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)
        - [直接插入排序](#%E7%9B%B4%E6%8E%A5%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)
        - [折半插入排序](#%E6%8A%98%E5%8D%8A%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)
        - [二路插入排序](#%E4%BA%8C%E8%B7%AF%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)
        - [表插入排序](#%E8%A1%A8%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)
        - [希尔排序](#%E5%B8%8C%E5%B0%94%E6%8E%92%E5%BA%8F)
        - [二叉查找树排序](#%E4%BA%8C%E5%8F%89%E6%9F%A5%E6%89%BE%E6%A0%91%E6%8E%92%E5%BA%8F)
      - [交换排序](#%E4%BA%A4%E6%8D%A2%E6%8E%92%E5%BA%8F)
        - [冒泡排序](#%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F)
        - [快速排序 (Quicksort)](#%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F-quicksort)
          - [Refer](#refer)
          - [THOUGHT](#thought)
          - [CODING](#coding)
      - [选择排序](#%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F)
        - [简单选择排序](#%E7%AE%80%E5%8D%95%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F)
        - [堆排序](#%E5%A0%86%E6%8E%92%E5%BA%8F)
      - [归并 / 二路排序](#%E5%BD%92%E5%B9%B6-%E4%BA%8C%E8%B7%AF%E6%8E%92%E5%BA%8F)
    - [非比较排序 / 分布排序](#%E9%9D%9E%E6%AF%94%E8%BE%83%E6%8E%92%E5%BA%8F-%E5%88%86%E5%B8%83%E6%8E%92%E5%BA%8F)
      - [计数排序](#%E8%AE%A1%E6%95%B0%E6%8E%92%E5%BA%8F)
      - [桶排序](#%E6%A1%B6%E6%8E%92%E5%BA%8F)
      - [基数排序](#%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F)
  - [外部排序](#%E5%A4%96%E9%83%A8%E6%8E%92%E5%BA%8F)
  - [Refer Links](#refer-links)

# 排序

## 内部排序

### 比较排序

#### 插入排序

##### 直接插入排序

##### 折半插入排序

##### 二路插入排序

##### 表插入排序

##### 希尔排序

##### 二叉查找树排序

#### 交换排序

##### 冒泡排序

##### 快速排序 (Quicksort)

###### Refer

http://www.ruanyifeng.com/blog/2011/04/quicksort_in_javascript.html

https://www.cnblogs.com/pugang/archive/2012/06/27/2565093.html

https://segmentfault.com/a/1190000002651247 

###### THOUGHT

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/1/ccbfac79fd73c9576075e0c68e825fb1.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/1/4217d2063fb387554fe3d9eb2f548b29.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/1/95ede038b0ab6f6814caa487bf75b64a.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/1/f780531479e7f36555aeedd5a36d4018.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/1/d0e73028de873c60219a4c3e47f5fac0.jpg)

###### CODING

- CPP 实现：

  ```cpp
  #include <iostream>
  using namespace std;

  //Data swop function
  void Swap(int &p, int &q) {
    int temp = p;
    p = q;
    q = temp;
  }

  //Partition function
  int Partition(int a[], int p, int r) {
    int k = p, pivot = a[r];//tail node
    for (int i = p; i < r; i++) {
      if (a[i] <= pivot) {
        Swap(a[i], a[k++]);
      }
    }
    Swap(a[k], a[r]);
    return k;

    /*int i = p, j = r + 1;
    int x = a[p];//head node
    while (1) {
      while (a[++i] < x && i < r);
      while (a[--j] > x);
      if (i >= j) break;
      Swap(a[i], a[j]);
    }
    a[p] = a[j];
    a[j] = x;
    return j;*/
  }

  //Quick sort
  void Quick_sort(int a[], int p, int r) {
    if (p < r) {
      int pivot = Partition(a, p, r);
      Quick_sort(a, p, pivot - 1);
      Quick_sort(a, pivot + 1, r);
    }
  }

  int main() {
    int a[] = { 2, 8, 7, 1, 3, 5, 6, 4 };//array for testing
    Quick_sort(a, 0, 7);
    for (int i = 0; i < 8; i++) {
      cout << a[i] << ' ';
    }
  }
  ```

- JavaScript 实现：

  ```javascript
  var quickSort = function(arr) {
      // 检查数组的元素个数，如果小于等于 1，就返回
  　　if (arr.length <= 1) { return arr; }
      // 选择"基准"（pivot），并将其与原数组分离，再定义两个空数组，用来存放一左一右的两个子集
  　　var pivotIndex = Math.floor(arr.length / 2);
  　　var pivot = arr.splice(pivotIndex, 1)[0];
  　　var left = [];
  　　var right = [];
      // 开始遍历数组，小于"基准"的元素放入左边的子集，大于基准的元素放入右边的子集
  　　for (var i = 0; i < arr.length; i++){
  　　　　if (arr[i] < pivot) {
  　　　　　　left.push(arr[i]);
  　　　　} else {
  　　　　　　right.push(arr[i]);
  　　　　}
  　　}
      // 使用递归不断重复这个过程，就可以得到排序后的数组
  　　return quickSort(left).concat([pivot], quickSort(right));

  };
  ```

#### 选择排序

##### 简单选择排序

##### 堆排序

#### 归并 / 二路排序

<!-- TODO: PS：“快些选堆”：其中“快”指快速排序，“些”指希尔排序，“选”指选择排序，“堆”指堆排序，即这四种排序方法是不稳定的，其他自然都是稳定的。 -->

### 非比较排序 / 分布排序

#### 计数排序

#### 桶排序

#### 基数排序

<!-- TODO: 
内部排序算法总结 / 比较：
http://blog.csdn.net/hguisu/article/details/7776068
各自的空间、时间效能
各自的适用情况
从什么方面去优化的
还可以怎么优化
-->

## 外部排序

## Refer Links

http://blog.csdn.net/jnu_simba/article/details/9705111

http://blog.csdn.net/hguisu/article/details/7776068

http://www.codeceo.com/article/10-sort-algorithm-interview.html

https://www.toptal.com/developers/sorting-algorithms/

http://jsdo.it/norahiko/oxIy/fullscreen


