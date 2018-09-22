---
layout: post
title:  "Centos6.X升级Python为2.7版本并安装Pip"
date:   2018-09-22 00:00:00 +0000
categories: markdown
---
### 背景
最终目标：安装ss  
环境：CentOs6.10，自带python2.6.6   
问题：安装ss需要pip，而pip需要python2.7以上版本
解决思路：下载python源代码，然后自行编译

###获取python2.7源代码

安装python是需要先下载源码，再解压、编译。
linux下安装xz命令


 - 下载
```
wget http://www.python.org/ftp/python/2.7.10/Python-2.7.10.tar.xz
```

 - 解压
```
unxz Python-2.7.10.tar.xz
tar -vxf Python-2.7.10.tar
```

官网下载的python源码压缩包是.tar.xz格式的，解压过程中如果提示找不到unxz命令，则需要安装xz，具体参考这篇文章:[linux下安装xz命令](https://blog.csdn.net/qq_21383435/article/details/79540117)

###编译
执行命令  
```
./configure   
make && make install
```

###配置
 - 备份Python2.6给yum用



