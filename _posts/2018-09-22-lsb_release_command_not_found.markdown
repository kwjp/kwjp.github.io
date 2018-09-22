---
layout: post
title:  "CentOS命令lsb_release: command not found解决办法"
date:   2018-09-22 00:00:00 +0000
categories: CentOs
---
### 问题描述
在aws买的centos后，想看centos的版本号，执行lsb_release -a时报如下错误
 > bash: lsb_release: command not found...

### 解决办法
安装lsb_release 执行命令 
> yum install -y redhat-lsb

### 验证
再次执行lsb_release -a，已经可以查看OS信息  
```
[root@ip-172-31-39-231 ~]# lsb_release -a
LSB Version:    :base-4.0-amd64:base-4.0-noarch:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:xxxx
Distributor ID: CentOS
Description:    CentOS release 6.10 (Final)
Release:    6.10
Codename:   Final
```
