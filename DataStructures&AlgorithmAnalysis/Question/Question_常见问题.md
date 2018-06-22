- [常见问题](#)
	- [1. 素数判断](#1)
	- [2. 进制转换](#2)
		- [2.1. 二进制转化为十进制](#21)
		- [2.2. 十进制转二进制](#22)
		- [2.3. 十进制转八进制](#23)
		- [2.4. 十进制转换为任意进制](#24)
		- [2.5. 任意进制转换成任意进制](#25)
	- [3. 回文数判断](#3)
	- [4. Refer Links](#4-refer-links)

# 常见问题

## 1. 素数判断

基于以下理论：
- 判断一个数是否为素数只需判断从 2 到 sqrt(n) 是否存在其约数。
- 除了 2 和 3 以外，所有素数都一定与 6 的倍数相邻。因此，在 5 到 sqrt(n) 中每 6 个数只判断 2 个约数即可，时间复杂度 O(sqrt(n)/3)。

```java
public boolean isPrime(int n) {
		if (n == 1)
				return false;
		if (n == 2 || n == 3)
				return true;
		if (n % 6 != 1 && n % 6 != 5)
				return false;
		for (int i = 5; i * i <= n; i += 6) 
				if (n % i == 0 || n % (i + 2) == 0)
						return false;
		return true;
}
```

## 2. 进制转换

### 2.1. 二进制转化为十进制

```cpp
// 二进制转化为十进制
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
```

### 2.3. 十进制转八进制

```cpp
// 十进制转八进制   
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

## 3. 回文判断

### 3.1. 回文数

```java
public boolean isPalindrome(int a) {
		int s = 0;
		for (int m = a, i = m % 10; m != 0; m /= 10, i = m % 10)
				s = s * 10 + i;
		return s == a ? true : false;
}
```

### 3.2. 回文串

```java
public boolean isPalindrome(String s) {
		for (int i = 0; i < s.length() / 2; i++)
				if (s.charAt(i) != s.charAt(s.length() - 1 - i))
						return false;
		return true;
}
```

## 4. Refer Links