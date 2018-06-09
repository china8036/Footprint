- [Software Operation NOTE](#software-operation-note)
  - [apt](#apt)
    - [Kali 2017.2 更换国内源](#kali-20172-%E6%9B%B4%E6%8D%A2%E5%9B%BD%E5%86%85%E6%BA%90)

# Software Operation NOTE

## apt



### Kali 2017.2 更换国内源

https://www.cnblogs.com/dunitian/p/4712852.html

```
vim /etc/apt/sources.list
```
加入：
```
# deb cdrom:[Debian GNU/Linux 7.0 _Kali_ - Official Snapshot amd64 LIVE/INSTALL Binary 20150312-17:50]/ kali contrib main non-free

#deb cdrom:[Debian GNU/Linux 7.0 _Kali_ - Official Snapshot amd64 LIVE/INSTALL Binary 20150312-17:50]/ kali contrib main non-free

deb http://http.kali.org/kali moto main non-free contrib
deb-src http://http.kali.org/kali moto main non-free contrib

deb http://security.kali.org/ moto/updates main contrib non-free
deb-src http://security.kali.org/ moto/updates main contrib non-free

#中科大kali源
deb http://mirrors.ustc.edu.cn/kali kali main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali main non-free contrib
deb http://mirrors.ustc.edu.cn/kali-security kali/updates main contrib non-free

#新加坡kali源
deb http://mirror.nus.edu.sg/kali/kali/ kali main non-free contrib
deb-src http://mirror.nus.edu.sg/kali/kali/ kali main non-free contrib
deb http://security.kali.org/kali-security kali/updates main contrib non-free
deb http://mirror.nus.edu.sg/kali/kali-security kali/updates main contrib non-free
deb-src http://mirror.nus.edu.sg/kali/kali-security kali/updates main contrib non-free

#阿里云kali源
deb http://mirrors.aliyun.com/kali kali main non-free contrib
deb-src http://mirrors.aliyun.com/kali kali main non-free contrib
deb http://mirrors.aliyun.com/kali-security kali/updates main contrib non-free


#163 Kali源
deb http://mirrors.163.com/debian wheezy main non-free contrib 
deb-src http://mirrors.163.com/debian wheezy main non-free contrib 
deb http://mirrors.163.com/debian wheezy-proposed-updates main non-free contrib 
deb-src http://mirrors.163.com/debian wheezy-proposed-updates main non-free contrib 
deb-src http://mirrors.163.com/debian-security wheezy/updates main non-free contrib 
deb http://mirrors.163.com/debian-security wheezy/updates main non-free contrib
```