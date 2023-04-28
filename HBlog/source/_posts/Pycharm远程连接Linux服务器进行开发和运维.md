---
title: Pycharm远程连接Linux服务器进行开发和运维
categories: [学习笔记,Python]
comments: true
toc: false
date: 2020-02-12 13:56:46
---

部署了Django项目，之前一直是本地修改，再上传到服务器，各种不爽和不方便！经过一番查询，发现pycharm可以远程连接。

**配置方法：**

<!--more-->

1. 打开pycharm,windows下连接linux服务器如图所示：

   路径：tools→Deployment→Configuration

![114.png](http://www.chenxm.cc/upload/article/2019/3/8/114_20190308110447_625.png)

2. 配置连接名字，type选择**sftp**,只能选择**sftp**

3. 配置连接服务器基本信息sftp host、username、password，填写信息完成之后.

4. 点击**Test SFTP connection**

   然后点击**Autodetect**

**![999.png](http://www.chenxm.cc/upload/article/2019/3/8/999_20190308110136_275.png)**

5. 点击Mappings—— 对接服务器代码存放路径和windows电脑代码存放路径。

   填写信息：Local path、Deployment path on server，

   然后点击**ok**按钮

[![122.png](http://www.chenxm.cc/upload/article/2019/3/8/122_20190308111855_756.png)](http://www.chenxm.cc/zb_users/upload/2018/08/201808201534762714104722.png)

6. 设置如何使得本地代码和服务器代码同步更新，如图，

   路径：Tools→Deployment→Options

![113.png](http://www.chenxm.cc/upload/article/2019/3/8/113_20190308110403_533.png)

找到 Upload changed files automatically to the default server 这一行，选择On Explicit save action(Ctrl+S)

![112.png](http://www.chenxm.cc/upload/article/2019/3/8/112_20190308110327_182.png)



**注意：**

如果发现使用pycharm上传文件上传成功，但是linux上不存在，请检查第5步，服务器存放路径，

1. 检查上传文件路径是否正确
2. 路径：Tools→Deployment→Sync with Deployed to xx





来源： http://www.chenxm.cc/article/628.html 