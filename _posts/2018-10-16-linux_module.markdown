---
layout: post
title:  "Centos 执行内核代码"
date:   2018-10-16 00:00:00 +0000
categories: linux
---

### 创建文件夹，然后新建文件hello.c

```c
#include<linux/init.h>
#include<linux/module.h>

static int __init hello_init(void)
{
   printk("hello world module init!\n");
   return 0;
}

static void __exit hello_exit(void)
{
   printk("hello world module exit!\n");
}

module_init(hello_init);
module_exit(hello_exit);

MODULE_LICENSE("Dual BSD/GPL");
MODULE_AUTHOR("YU ZHI HUI");
```

### 创建编译文件Makefile

```makefile
obj-m:=hello.o

KERDIR=/lib/modules/$(shell uname -r)/build

CURDIR=$(shell pwd)

all:
        make -C $(KERDIR) M=$(CURDIR) modules

clean:
        make -C $(KERDIR) M=$(CURDIR) clean
```

### 编译 

```shell
[root@ip-172-31-39-231 kernel]# make
make -C /lib/modules/2.6.32-754.6.3.el6.x86_64/build M=/root/kernel modules
make[1]: Entering directory `/usr/src/kernels/2.6.32-754.6.3.el6.x86_64'
  CC [M]  /root/kernel/hello.o
  Building modules, stage 2.
  MODPOST 1 modules
  CC      /root/kernel/hello.mod.o
  LD [M]  /root/kernel/hello.ko.unsigned
  NO SIGN [M] /root/kernel/hello.ko
make[1]: Leaving directory `/usr/src/kernels/2.6.32-754.6.3.el6.x86_64'
```
### 模块操作常用命令

 - 加载模块命令：insmod hello.ko
 - 查看模块命令：lsmod
 - 卸载模块命令：rmmod hello
 + 查看打印信息
    + 监听日志命令：tail -f /var/log/messages
    + 模块消息查看命令：dmesg
 - 清理消息命令为：dmesg -c

#### Missing Kernel-headers
遇到以下错误

```shell
[root@ip-172-31-39-231 kernel]# make
make -C /lib/modules/2.6.32-754.3.5.el6.x86_64/build M=/root/kernel modules
make: *** /lib/modules/2.6.32-754.3.5.el6.x86_64/build: No such file or directory.  Stop.
make: *** [all] Error 2
```

解决办法：  
yum install kernel-devel  
yum install kernel  
然后重启

检查是否已经修复：   
ls /usr/src/kernels/$(uname -r)
