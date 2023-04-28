---
title: Python爬虫之Requests库的使用
categories: [学习笔记,Python,爬虫]
comments: true
toc: false
date: 2020-02-22 18:02:05
---

#### HTTP协议

​	HTTP，Hypertext Transfer Protocol, 超文本传输协议，是一个基于“请求与响应”模式的、无状态的应用层*（作用于TCP协议之上）*协议。

![1584508422463](/assets/img/atc_img/1584508422463.png)

<!--more-->

###### PATCH和PUT的区别

PATCH用于局部更新，而PUT提交时要将所有字段一并提交，未提交字段则被删除。使用PATCH可以节省网络带宽。

![1584716704672](/assets/img/atc_img/1584716704672.png)



#### Requests库的使用

Requests是用python语言基于urllib编写的，采用的是Apache2 Licensed开源协议的HTTP库。

###### Requests库主要方法 

|        方法        |                      说明                      |
| :----------------: | :--------------------------------------------: |
| requests.request() |     构造一个请求，支撑以下各方法的基础方法     |
|  requests.head()   |   获取HTML网页头信息的方法，对应于HTTP的HEAD   |
|   requests.get()   |     获取HTML网页的主要方法，对应HTTP的GET      |
|  requests.post()   | 向HTML网页提交POST请求的方法，对应于HTTP的POST |
|   requests.put()   |  向HTML网页提交PUT请求的方法，对应于HTTP的PUT  |
|  requests.patch()  | 向HTML网页提交局部修改请求，对应于HTTP的PATCH  |
| requests.delete()  |   向HTML页面提交删除请求，对应于HTTP的DELETE   |



###### Response对象的属性

|        属性         |                       说明                       |
| :-----------------: | :----------------------------------------------: |
|    r.status_code    | HTTP请求的返回状态，200表示连接成功，404表示失败 |
|       r.text        |  HTTP响应内容的字符串形式，即url对应的页面内容   |
|     r.encoding      |      从HTTP  header中猜测响应内容的编码方式      |
| r.apparent_encoding |         从内容中分析出响应内容的编码方式         |
|      r.content      |             HTTP响应内容的二进制形式             |

注意：若header中不存在charset，则r.encoding认为编码为ISO-8859-1。



###### Requests库的异常

|           异常            |                    说明                     |
| :-----------------------: | :-----------------------------------------: |
| requests.ConnectionError  | 网络连接错误异常，如DNS查询失败、拒绝连接等 |
|    requests.HTTPError     |                HTTP错误异常                 |
|   requests.URLRequired    |                 URL缺失异常                 |
| requests.TooManyRedirects |     超过最大重定向次数，产生重定向异常      |
|  requests.ConnectTimeout  |           连接远程服务器超时异常            |
|     requests.Timeout      |          请求URL超时，产生超时异常          |
|                           |                                             |
|    r.raise_for_status     |     如果不是200，产生request.HTTPError      |



###### Requests库方法介绍

###### 1、requests.request(method, url, **kwargs)

params: 字典或字节序列

data: 字典、字节序列或文件对象，重点作为向服务器提交资源时使用

json: JSON格式的数据

headers:  字典，HTTP定制头

cookies: 从HTTP协议解析cookie 

auth: 元组，支持HTTP认证功能

files: 字典类型，向服务器传输文件

timeout: 设定超时时间，秒为单位

<span style="color:red">proxies: 字典类型，设定访问代理服务器，可以增加登录认证（隐藏爬虫源地址，防止逆追踪）</span>

allow_redirects: True/False, 默认True，重定向开关

stream: True/False, 默认True，获取内容立即下载开关

verify: True/False, 默认True，认证SSL证书开关

cert: 本地SSL证书路径





```python
# requests库爬取网页通用代码框架
try:
    r = requests.get(url, timeout=30)
    r.raise_for_status()
    r.encoding = r.apparent_encoding
    return r.text
except:
    return "产生异常"

```



###### 网络爬虫的尺寸

![1584721695183](/assets/img/atc_img/1584721695183.png)





网络爬虫的限制：

- 来源审查：判断User-Agent进行限制

  检查来访HTTP协议头的User-Agent域，只响应浏览器或友好爬虫的访问

- 发布公告：Robots协议（网络爬虫排除标准）

  告知所有爬虫网站的爬取策略，要求爬虫遵守

  形式：在网站根目录下robots.txt文件



> robots.txt是一种存放于网站根目录下的ASCII编码的文本文件，它通常告诉网络搜索引擎的漫游器（又称网络蜘蛛），此网站中的哪些内容是不应被搜索引擎的漫游器获取的，哪些是可以被漫游器获取的。
>
> 因为一些系统中的URL是大小写敏感的，所以robots.txt的文件名应统一为小写。
>
> robots.txt应放置于网站的根目录下。
>
> 如果想单独定义搜索引擎的漫游器访问子目录时的行为，那么可以将自定的设置合并到根目录下的robots.txt，或者使用robots元数据（Metadata，又称元数据）。
>
> robots.txt协议并不是一个规范，而只是约定俗成的，所以并不能保证网站的隐私。
>
> 注意robots.txt是用字符串比较来确定是否获取URL，所以目录末尾有与没有斜杠“/”表示的是不同的URL。robots.txt允许使用类似"Disallow: *.gif"这样的通配符。



