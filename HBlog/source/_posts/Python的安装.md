---
title: Python的安装
categories: [学习笔记,Python]
comments: true
toc: false
date: 2019-02-26 14:33:00
---

目前，Python有两个版本，一个是2.x版，一个是3.x版，这两个版本是不兼容的。而官方宣布， 于2020 年 1 月 1 日起，停止 Python 2.x 的更新。所以安装可以直接上Python 3.x版本的了。

##### Windows上安装Python

从[Python官网](https://www.python.org/)下载Python 最新版的安装程序，下载后双击运行并安装，安装时勾选`Add Python 3.5 to PATH`。

<!--more-->

![1582696081978](C:\Users\lzfdd\AppData\Roaming\Typora\typora-user-images\1582696081978.png)

等待安装完成后，打开命令提示符窗口，输入`python`，出现Python版本号则安装成功。（我这里更新过了所以版本号是3.7）

![1582696392014](C:\Users\lzfdd\AppData\Roaming\Typora\typora-user-images\1582696392014.png)



##### Linux上安装Python

> 前面说过，Linux系统分为两大类，这里基于RedHat系列的Centos 7.5系统安装。

###### 首先下载Python的安装包

```shell
wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz
```



###### 解压安装

```shell
# 解压tar包
tar xf Python-3.7.4.tgz 

# 进入解压后的包
cd Python-3.7.4

# 配置安装信息,我的安装路径为/usr/install/python3/
./configure --prefix=/usr/install/python3/

# 编译
make 

# 安装
make install
```



###### 创建软连接

Linux的软链接相当于windows里的快捷方式，创建后不用再配环境变量。

```shell
ln -s /usr/local/python3/bin/python3 /usr/bin/python3.7
```



###### 问题记录

- 出现：`zipimport.ZipImportError: can’t decompress data ` 错误

  

  原因：缺少zlib 的相关工具包

  解决：

  ```shell
  yum -y install zlib*
  ```

  

- 出现：`ModuleNotFoundError: No module named '_ctypes'` 错误

  原因：

  ​        Python3中有个内置模块叫`ctypes`，它是Python3的外部函数库模块，它提供兼容C语言的数据类型，并通过它调用Linux系统下的共享库(Shared library)，此模块需要使用CentOS7系统中外部函数库(Foreign function library)的开发链接库(头文件和链接库)。由于在CentOS7系统中没有安装外部函数库(libffi)的开发链接库软件包，所以在安装pip的时候就报了`ModuleNotFoundError: No module named '_ctypes'`的错误。

  解决：

  ​        其实很简单安装一下外部函数库(libffi)就可以了，操作步骤如下：

  ```shell
  # 使用yum install命令安装libffi-devel
  yum install libffi-devel -y
  
  # 命令重新编译并安装python
  make && make install  
  ```

  ######  

