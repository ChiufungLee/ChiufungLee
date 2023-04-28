---
title: Linux中wget、yum与apt-get的用法及区别
categories: [学习笔记,Linux]
comments: true
toc: false
date: 2020-02-26 14:20:46
---

一般来说著名的linux系统基本上分两大类：

- RedHat系列：Redhat、Centos、Fedora等
- Debian系列：Debian、Ubuntu等



###### RedHat 系列

1. 常见的安装包格式 rpm包,安装rpm包的命令是“rpm -参数”
2. 包管理工具 yum
3. 支持tar包

<!--more-->

yum可以用于运作rpm包，例如在Centos系统上对某个软件的管理：

```shell
# 安装：
yum install

# 卸载：
yum remove

# 更新：
yum update
```



###### Debian系列

1. 常见的安装包格式 deb包,安装deb包的命令是“dpkg -参数”

2. 包管理工具 apt-get

3. 支持tar包

   

apt-get可以用于运作deb包，例如在Ubuntu系统上对某个软件的管理：

```shell
# 安装：
apt-get install

# 卸载：
apt-get remove

# 更新：
apt-get update
```



wget不是安装方式，它是一种下载工具，类似于迅雷。通过HTTP、HTTPS、FTP三个最常见的TCP/IP协议下载，并可以使用HTTP代理，名字是`World Wide Web`与`get`的结合。如果要下载一个软件,可以直接运行：`wget 下载地址`，如：

```shell
# 安装Python
wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz
```

如果当前ubuntu未安装wget，可按下列操作进行安装和检查是否安装成功：

```shell
sudo apt-get update 

sudo apt-get install wget

wget --version
```


原文链接：https://blog.csdn.net/abcwoabcwo/article/details/80244989