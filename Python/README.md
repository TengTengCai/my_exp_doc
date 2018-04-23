[TOC]

# Python

## Python3 在Linux下的安装

通过wget，在官网上下载对应的源码。

```bash
[root@localhost local]# wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tar.xz
--2018-04-23 13:50:16--  https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tar.xz
正在解析主机 www.python.org (www.python.org)... 151.101.72.223, 2a04:4e42:36::223
正在连接 www.python.org (www.python.org)|151.101.72.223|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：17049912 (16M) [application/octet-stream]
正在保存至: “Python-3.6.5.tar.xz”
```

再通过unxz对Python-3.6.5.tar.xz进行解压，再对tar解归档

```bash
[root@localhost local]# unxz Python-3.6.5.tar.xz 
[root@localhost local]# ls
bin  games    lib    libexec           sbin   src
etc  include  lib64  Python-3.6.5.tar  share
[root@localhost local]# tar -xvf Python-3.6.5.tar 
```

在编译和安装前，需要对源码进行配置安装目录 安装目录默认是 /usr/local/bin/

```bash
[root@localhost Python3]# ./configure
```

开始编译安装

```bash
[root@localhost Python3]# make && make install
```

为python3设置软链接到/usr/bin/，添加软连接到环境变量中

```bash
[root@localhost ~]# ln -s /usr/local/bin/python3 /usr/bin/python3
[root@localhost ~]# ln -s /usr/local/bin/pip3 /usr/bin/pip3
```

python3在Linux中安装完成，启用python3时使用命令python3

```bash
[root@localhost ~]# python3
Python 3.6.5 (default, Apr 23 2018, 14:26:05) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-16)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

