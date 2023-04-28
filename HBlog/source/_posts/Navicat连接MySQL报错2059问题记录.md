---
title: Navicat连接MySQL报错2059问题记录
date: 2019-11-14 14:50:53
categories: [学习笔记,MySQL]
---

使用Navicat Premium 连接MySQL时出现如下错误：

![报错](http://q5u20aot3.bkt.clouddn.com/ArticleImgs/mysql2059.png)

#### 原因

> mysql8 之前的版本中加密规则是mysql_native_password,而在mysql8之后,加密规则是caching_sha2_password

<!--more-->

#### 解决



更改加密规则：

```mysql
mysql -u root -p  #登录数据库

ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; #更改加密方式

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'; #更新用户密码

FLUSH PRIVILEGES; #刷新权限
```

再重新连接即可。

