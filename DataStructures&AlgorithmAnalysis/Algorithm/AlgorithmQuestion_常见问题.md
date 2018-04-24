- [常见问题](#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
	- [1. 素数判断](#1-%E7%B4%A0%E6%95%B0%E5%88%A4%E6%96%AD)
	- [2. 进制转换](#2-%E8%BF%9B%E5%88%B6%E8%BD%AC%E6%8D%A2)
		- [2.1. 二进制转化为十进制](#21-%E4%BA%8C%E8%BF%9B%E5%88%B6%E8%BD%AC%E5%8C%96%E4%B8%BA%E5%8D%81%E8%BF%9B%E5%88%B6)
		- [2.2. 十进制转二进制](#22-%E5%8D%81%E8%BF%9B%E5%88%B6%E8%BD%AC%E4%BA%8C%E8%BF%9B%E5%88%B6)
		- [2.3. 十进制转八进制](#23-%E5%8D%81%E8%BF%9B%E5%88%B6%E8%BD%AC%E5%85%AB%E8%BF%9B%E5%88%B6)
		- [2.4. 十进制转换为任意进制](#24-%E5%8D%81%E8%BF%9B%E5%88%B6%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BB%BB%E6%84%8F%E8%BF%9B%E5%88%B6)
		- [2.5. 任意进制转换成任意进制](#25-%E4%BB%BB%E6%84%8F%E8%BF%9B%E5%88%B6%E8%BD%AC%E6%8D%A2%E6%88%90%E4%BB%BB%E6%84%8F%E8%BF%9B%E5%88%B6)
	- [3. 回文数判断](#3-%E5%9B%9E%E6%96%87%E6%95%B0%E5%88%A4%E6%96%AD)
	- [4. 全排列问题 Permutation](#4-%E5%85%A8%E6%8E%92%E5%88%97%E9%97%AE%E9%A2%98-permutation)
	- [5. 重叠子问题](#5-%E9%87%8D%E5%8F%A0%E5%AD%90%E9%97%AE%E9%A2%98)
		- [5.1. 记忆化搜索（自顶向下解决问题）](#51-%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2%EF%BC%88%E8%87%AA%E9%A1%B6%E5%90%91%E4%B8%8B%E8%A7%A3%E5%86%B3%E9%97%AE%E9%A2%98%EF%BC%89)
		- [5.2. 动态规划（自下向上解决问题）](#52-%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%88%E8%87%AA%E4%B8%8B%E5%90%91%E4%B8%8A%E8%A7%A3%E5%86%B3%E9%97%AE%E9%A2%98%EF%BC%89)
	- [6. Refer Links](#6-refer-links)

# 常见问题

## 1. 素数判断

```cpp
int isPrime(int a)
{
	if(a==1)
		return 0;
	else if(a==2)
		return 1;
	else
	{
		int b=2;
		for(;b<a;)
		{
			if(!(a%b))
			{
				return 0;
				break;
			}
			else b++;

		}
		if(b==a)
			return 1;
	}
}
```

## 2. 进制转换

### 2.1. 二进制转化为十进制

```cpp
// 二进制转化为十进制
#include<iostream>
using namespace std;
int f(int n)
{
    int a=0;
    for(int i=0;n;i++)// 要从右到左用二进制的每个数去乘以 2 的相应次方求和；
    {
         int t=n%10;
         a+=t<<i;   // 左移 i 位，即：t 乘以 2 的 i 次方
         n/=10;
    }
    return a;
}
```

### 2.2. 十进制转二进制

```cpp
// 十进制转二进制   
#include<iostream>   
using namespace std;  

void printbinary(const unsigned int val)  
{   
    for(int i = 16; i >= 0; i--)  
    {  
        if(val & (1 << i))  
            cout << "1";  
        else  
            cout << "0";  
    }  
}  
  
int main()  
{  
    printbinary(1024);  
    return 0;  
}  
```

### 2.3. 十进制转八进制

```cpp
// 十进制转八进制   
#include <iostream>   
#include <vector>   
using namespace std;  
  
int main()  
{  
    cout<<"input a number:"<<endl;  
    int d;  
    vector<int> vec;  
  
    cin>>d;  
    while (d)  
    {  
        vec.push_back(d%8);  
        d=d/8;  
    }  
  
    cout<<"the result is:"<<endl;  
    for(vector<int>::iterator ip=vec.end()-1;ip>=vec.begin();)  
    {  
        cout<<*ip--;  
    }  
    cout<<endl;  
      
    return 0;  
}  
```

```cpp
// 通过库函数实现八进制、十六进制输出
#include <iostream>   
using namespace std;  
  
int main()  
{  
    int test=64;  
    cout<<"DEC:"<<test<<endl;  
    cout<<"OCT:"<<oct<<test<<endl;// 八进制   
    cout<<"HEX:"<<hex<<test<<endl;// 十六进制   
  
    return 0;  
}  
```

### 2.4. 十进制转换为任意进制

```cpp
// 十进制转换为任意进制 
#include <iostream>   
using namespace std;  
  
int main()  
{  
    long n;  
    int p,c,m=0,s[100];  
    cout<<"输入要转换的数字："<<endl;  
    cin>>n;  
    cout<<"输入要转换的进制："<<endl;  
    cin>>p;  
  
    cout<<"("<<n<<")10="<<"(";  
  
    while (n!=0)// 数制转换，结果存入数组 s[m]   
    {  
        c=n%p;  
        n=n/p;  
        m++;s[m]=c;   // 将余数按顺序存入数组 s[m] 中   
    }  
  
    for(int k=m;k>=1;k--)// 输出转换后的序列   
    {  
        if(s[k]>=10) // 若为十六进制等则输出相对应的字母   
            cout<<(char)(s[k]+55);  
        else         // 否则直接输出数字   
            cout<<s[k];  
    }  
  
    cout<<")"<<p<<endl;  
  
    return 0;  
}  
```

### 2.5. 任意进制转换成任意进制

```cpp
// 递归实现任意进制转换成任意进制 
#include<iostream> 
using namespace std; 
int conservation[100];  // 保存结果的数组 
int number;             // 数组中保存结果实际的位数
int change(int base,int jinzhi)  
{  
    //base 为基数，jinzhi 为想要转化的进制   
    int a;  
    int b;  
    a=base%jinzhi;  
    b=base/jinzhi;  
    conservation[number++]=a;  
    if(b!=0)   return change(b,jinzhi);  
    else return 0;  
}  

int main()  
{  
    int base,jinzhi;  
    cout<<"input base and jinzhi:";  
    cin>>base>>jinzhi;  
    change(base,jinzhi);  
    for(int i=number-1;i>=0;i--)  
      cout<<conservation[i];  
    cout<<endl;  
    return 0;  
}
```

## 3. 回文数判断

```cpp
int isPalindrome(int a)
{
	int m=a,b=0,s=0;
	while(m)
	{
		b=m%10;
		m/=10;
		s=s*10+b;
	}
	if(s==a)
		return 1;
	else 
		return 0;
}
```

## 4. 全排列问题 Permutation

- 回溯
	```cpp
	#include <iostream>
	#include <vector>
	using namespace std;

	// https://leetcode.com/problems/permutations/description/
	// 时间复杂度: O(n^n)，空间复杂度: O(n)
	class Solution {

	private:
			vector<vector<int>> res;
			vector<bool> used;

			// p中保存了一个有index-1个元素的排列。
			// 向这个排列的末尾添加第index个元素, 获得一个有index个元素的排列
			void generatePermutation(const vector<int>& nums, int index, vector<int>& p){
					if(index == nums.size()){
							res.push_back(p);
							return;
					}

					for(int i = 0 ; i < nums.size() ; i ++)
							if(!used[i]){
									used[i] = true;
									p.push_back(nums[i]);
									generatePermutation(nums, index + 1, p );
									p.pop_back();
									used[i] = false;
							}

					return;
			}

	public:
			vector<vector<int>> permute(vector<int>& nums) {
					res.clear();
					if(nums.size() == 0)
							return res;

					used = vector<bool>(nums.size(), false);
					vector<int> p;
					generatePermutation(nums, 0, p);

					return res;
			}
	};

	int main() {
			int nums[] = {1, 2, 3};
			vector<int> vec(nums, nums + sizeof(nums)/sizeof(int) );

			vector<vector<int>> res = Solution().permute(vec);
			for(int i = 0 ; i < res.size() ; i ++){

					for(int j = 0 ; j < res[i].size() ; j ++)
							cout << res[i][j] << " ";
					cout << endl;
			}

			return 0;
	}
	```

## 5. 重叠子问题

如 Fibonacci 数列问题

https://loggerhead.me/posts/qiu-fibonacci-shu-lie-de-n-chong-suan-fa.html

### 5.1. 记忆化搜索（自顶向下解决问题）

### 5.2. 动态规划（自下向上解决问题）

## 6. Refer Links