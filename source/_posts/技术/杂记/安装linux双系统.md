---
title: 安装Linux双系统
date: 2019-06-02 10:00
top: 0
categories: 
  - 技术
  - 杂记
---
### win7下安装linux双系统
---
- [参考网址1](http://blog.csdn.net/coderjyf/article/details/51241919)
- [参考网址2](https://jingyan.baidu.com/article/76a7e409bea83efc3b6e1507.html)
- [进入linux后，无法进入windows，解决方法](http://www.jb51.net/article/110288.htm)

#### 个人安装注意小结
- 使用U盘安装双系统，安装在硬盘分区，如果不想用双系统了，直接格式化该硬盘分区
- 安装unbantu过程中，注意分区
- 注意划分/boot分区
- **重要的一点是在安装启动引导设备选择前面划分的/boot盘**
- 安装完linux后，**拔出U盘**，在进行重启进入win7，在用easyBCD修改开机方式即可