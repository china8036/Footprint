- [MacOS 常用操作](#MacOS-常用操作)
  - [1. 显示隐藏文件](#1-显示隐藏文件)
  - [2. 刷新 DNS 缓存](#2-刷新-DNS-缓存)
  - [3. 文件名乱码](#3-文件名乱码)
  - [4. 读写 NTFS](#4-读写-NTFS)
  - [5. 与 Windows 配合使用](#5-与-Windows-配合使用)
  - [6. Refer Links](#6-Refer-Links)

# MacOS 常用操作

## 1. 显示隐藏文件

`Command+Shift+.` 可以显示隐藏文件、文件夹，再按一次，恢复隐藏。

## 2. 刷新 DNS 缓存

[Flushing your DNS cache in Mac OS X and Linux](https://help.dreamhost.com/hc/en-us/articles/214981288-Flushing-your-DNS-cache-in-Mac-OS-X-and-Linux)

- Mac OS X 12 (Sierra) and later:
  ```
  sudo killall -HUP mDNSResponder; sudo killall mDNSResponderHelper; sudo dscacheutil -flushcache
  ```

- Mac OS X 11 (El Capitan) and OS X 12 (Sierra):
  ```
  sudo killall -HUP mDNSResponder
  ```

- Mac OS X 10.10 (Yosemite), Versions 10.10.4+:
  ```
  sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
  ```

## 3. 文件名乱码

[下载的文件名总是「乱码」？这里有各平台的解决方法](https://sspai.com/post/44360)

## 4. 读写 NTFS

[macOS 读写 NTFS，及 EXT 分区格式化和挂载](https://zhih.me/macos-mount-ntfs-ext/)

## 5. 与 Windows 配合使用

[如何优雅地同时使用 mac 和 windows 工作？](https://www.jianshu.com/p/6b747fae1075)

[Mac To Win | 不完全迁移体验指北](https://sspai.com/post/45742)

[microsoft remote desktop 使用](https://jingyan.baidu.com/article/219f4bf7fe65e9de442d38b6.html)

## 6. Refer Links
