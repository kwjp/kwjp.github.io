---
layout: post
title:  "CentOs6 搭建ShadowSocks Server"
date:   2018-09-20 00:00:00 +0000
categories: ShadowSocks
---
## 1 安装ShadowSocks
```
yum install python-setuptools
easy_install pip
pip install shadowsocks
```

## 2 配置ShadowSocks    
vim /etc/shadowsocks.json，根据应用模式分别写入如下内容  

### 2.1单用户模式  

```json
{
  "server": "0.0.0.0",
  "server_port": 8989,
  "password": "xxxx",
  "method": "aes-256-cfb"
}
```
### 2.2多用户模式  
```json
{
    "server":"0.0.0.0",
    "port_password":{
    "443":"xxxx",
    "444":"xxxx",
    "888":"xxxx",
    "8989":"xxxx"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```

## 3 停止和启动ShadowSocks服务  
ssserver -c /etc/shadowsocks.json -d start   
ssserver -c /etc/shadowsocks.json -d stop  

## 4 centos端口相关
### 4.1 开放端口  
/sbin/iptables -I INPUT -p tcp --dport 443 -j ACCEPT  
/sbin/iptables -I INPUT -p tcp --dport 444 -j ACCEPT  
/sbin/iptables -I INPUT -p tcp --dport 888 -j ACCEPT  
/sbin/iptables -I INPUT -p tcp --dport 8888 -j ACCEPT  
/sbin/iptables -I INPUT -p tcp --dport 8989 -j ACCEPT  

### 4.2 查看当前打开的端口  
/etc/init.d/iptables status  

## 5 FQA
收集的一些安装过程中的常见问题

### 5.1 easy_install pip时报错
问题描述：centos6自带python，但版本号是2.6.6，而pip要求python环境在2.7以上，所以需要先升级一下服务器的python版本  
解决办法：[Centos6.X升级Python为2.7](https://kwjp.github.io/centos/2018/09/22/upgrade_python27.html)  

### 5.2 unxz时报错
问题描述：安装python2.7是需要先下载源码，再解压、编译的，python源码压缩包是.tar.xz格式的，其解压需要用到指令unxz  
解决办法：需要在服务器上安装xz。  

- 下载
```
wget https://jaist.dl.sourceforge.net/project/lzmautils/xz-5.2.4.tar.gz
```
如果wget不到说明下载地址有变动，去这里查看[最新下载地址](https://sourceforge.net/projects/lzmautils/files/latest/download)  

- 解压、编译
```
# tar -zxvf xz-5.2.4.tar.gz
# cd xz-5.2.4
# mkdir /opt/software/zx
# ./configure --prefix=/opt/software/zx     #指定安装目录
# make && make install    #编译并安装
# ln -s /opt/software/zx /usr/local/bin/xz     #建立软链接
```

- 配置环境变量  
vim /etc/profile，在 export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL 一行的上面添加如下内容:
```
export XZ_HOME=/opt/software/zx
export PATH=$XZ_HOME/bin:$PATH
```
source /etc/profile 使修改生效

- 校验
```
[root@ip-172-31-39-231 ~]# xz -V
xz (XZ Utils) 4.999.9beta
```

### 5.3 configure: error: no acceptable C compiler found in $PATH
问题描述：缺少c编译器  
解决办法：安装gcc  
```
yum install gcc
```


### 5.4 ShadowSocks客户端500错误
问题描述：同一台服务器，在日本可正常连接，在国内连接后打开google时始终报500错误  
原因分析：ip被墙了，可能是太多人在东京的服务器上装ss了。  
解决办法：把服务器搬到美国  

 - 登陆vultr后台，并Take Snapshot
 - Deploy new server
 - Server Location选择Los Angeles United States
 - Server Type选择Snapshot，并选中第一步的备份

### 5.5 服务器重启后，无法连接

- 重新执行iptables端口开放语句
- 手动启动ssserver

### 5.6 bash: lsb_release: command not found
问题描述:在aws买的centos后，想看centos的版本号，执行lsb_release -a时报如下错误  
解决办法1:安装lsb_release ,执行命令 
> yum install -y redhat-lsb

解决办法2:cat /etc/redhat-release

# 6.参考文献  
[使用ShadowSocks科学上网及突破公司内网](http://www.devtalking.com/articles/shadowsocks-guide/)




