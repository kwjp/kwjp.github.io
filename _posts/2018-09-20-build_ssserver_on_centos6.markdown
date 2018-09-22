---
layout: post
title:  "CentOs6 搭建ss会遇到的问题和解决办法参考"
date:   2018-09-20 00:00:00 +0000
categories: ss
---
# 1.升级python版本
centos6自带python，但版本号是2.6.6，安装ShadowSocks需要使用pip，而pip要求python环境在2.7以上，所以需要先升级一下服务器的python版本，具体参考下面这篇文章  
[Centos6.X升级Python为2.7版本并安装Pip](https://blog.csdn.net/LoveCarpenter/article/details/74011641 "Centos")  

# 2.安装xz
安装python是需要先下载源码，再解压、编译的，python源码压缩包是.tar.xz格式的，其解压需要用到指令unxz，这需要在服务器上安装xz，具体参考下面的文章  
[linux下安装xz命令](https://blog.csdn.net/qq_21383435/article/details/79540117)

# 3.出错处理
在执行./configure的时候，可能会报如下错误
>configure: error: no acceptable C compiler found in $PATH

可以参照下面文章结局  
[configure: error: no acceptable C compiler found in $PATH 问题解决](http://blog.51cto.com/raulkang/573151)

# 4.配置ShadowSocks    
vim /etc/shadowsocks.json，根据应用模式分别写入如下内容  

## 4.1单用户模式  

```json
{
  "server": "0.0.0.0",
  "server_port": 8989,
  "password": "xxxx",
  "method": "aes-256-cfb"
}
```
## 4.2多用户模式  
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

# 5.停止和启动ShadowSocks服务  
ssserver -c /etc/shadowsocks.json -d start   
ssserver -c /etc/shadowsocks.json -d stop  

# 6.centos端口相关
## 6.1开放端口  
/sbin/iptables -I INPUT -p tcp --dport 443 -j ACCEPT  
/sbin/iptables -I INPUT -p tcp --dport 444 -j ACCEPT  
/sbin/iptables -I INPUT -p tcp --dport 888 -j ACCEPT  
/sbin/iptables -I INPUT -p tcp --dport 8888 -j ACCEPT  
/sbin/iptables -I INPUT -p tcp --dport 8989 -j ACCEPT  

## 6.2查看打开的端口  
/etc/init.d/iptables status  

## 6.3注意事项
重启后开放的端口会失效，需要重新开放端口

# 7.客户端500错误
网上查到了一下方式说是可以解决
[500错误](https://github.com/shadowsocks/shadowsocks/issues/1275)
其实是被GFW了，解决办法是，服务器备份一下，从东京转到洛杉矶即可

# 8.参考文献  
[使用ShadowSocks科学上网及突破公司内网](http://www.devtalking.com/articles/shadowsocks-guide/)
