---
title: Ajax学习笔记
categories: [学习笔记,Web前端,Ajax]
comments: true
toc: 1
date: 2019-10-24 12:50:08
---

#### ajax简介

创建快速动态网页的技术，使网页实现异步更新，即在无需重新加载整个网页的情况下，实现网页的局部更新。

> （Asynchronous JavaScript and XML, 异步的JavaScript和XML）

<!--more-->

##### XMLHttpRequest 对象

XMLHttpRequest 是 AJAX 的基础，用于在后台与服务器交换数据。

###### 创建XMLHttpRequest

```javascript
var xmlhttp;
if(window.XMLHttpRequest)		//判断浏览器是否支持XMLHttpRequest对象
    {
        xmlhttp = new XMLHttpRequest();		//创建XMLHttpRequest对象
    }
else		//IE5、6b不支持XMLHttpRequest对象，使用ActiveX对象
    {
        xmlhttp = new ActiveXObject("Microsoft.XMLHTTP")
    }
```

##### 将请求发送到服务器

```javascript
xmlhttp.open("GET","test1.txt",true);	//method,url,async
xmlhttp.send();
```



| 方法                         | 描述                                                         |
| :--------------------------- | :----------------------------------------------------------- |
| open(*method*,*url*,*async*) | 规定请求的类型、URL 以及是否异步处理请求。<br />method*：请求的类型：GET 或 POST*<br />url*：文件在服务器上的位置*<br />async：true（异步）或 false（同步） |
| send(*string*)               | 将请求发送到服务器。<br />*string*：仅用于 POST 请求         |



###### 请求方法

get：

```javascript
xmlhttp.open("GET","demo_get.asp",true);
xmlhttp.send();
```

post：如果需要像 HTML 表单那样 POST 数据，使用 setRequestHeader() 来添加 HTTP 头。然后在 send() 方法中规定想要发送的数据

```javascript
setRequestHeader(header,value)		//向请求添加 HTTP 头。header: 规定头的名称，value: 规定头的值

xmlhttp.open("POST","ajax_test.asp",true);
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xmlhttp.send("fname=Bill&lname=Gates");
```



> ###### 必须使用post情况
>
> 1. 无法使用缓存文件（更新服务器上的文件或数据库）
> 2. 向服务器发送大量数据（POST 没有数据量限制）
> 3. 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠



###### 异步

XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true：

```javascript
xmlhttp.open("GET","ajax_test.asp",true);
```

当 async = true 时，需要指定当服务器响应已做好被处理的准备时所执行的函数：

```javascript
xmlhttp.onreadystatechange = function()
{
    if(xmlhttp.readyState == 4 && xmlhttp.status == 200)
        {
            document.getElementById("Mydiv").innerHTML=xmlhttp.responseText;
        }
}
xmlhttp.open("GET","test1.txt",true);
xmlhttp.send();
```

##### 服务器响应

使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性获取响应

responseText：获得字符串类型的响应数据

```javascript
document.getElementById("mydiv").innerHTML=xmlhttp.responsetText;
```



responseXML：获取XML类型的响应数据

```javascript
xmlDoc=xmlhttp.responseXML;
txt="";
x=xmlDoc.getElementsByTagName("ARTIST");
for (i=0;i<x.length;i++)
  {
  txt=txt + x[i].childNodes[0].nodeValue + "<br />";
  }
document.getElementById("myDiv").innerHTML=txt;
```



##### onreadystatechange事件

每当readyState改变时，就会触发onreadystatechange事件

 XMLHttpRequest 对象的三个重要的属性：

| 属性               | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| onreadystatechange | 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。 |
| readyState         | 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。<br />0: 请求未初始化<br />1: 服务器连接已建立<br />2: 请求已接收<br />3: 请求处理中<br />4: 请求已完成，且响应已就绪 |
| status             | 200: "OK"<br />404: 未找到页面                               |



#### 页面跳转问题

1. form不能有action跳转
2. a 不能有href跳转



​          方法一：给form表单写一个onsubmit函数，在最后加一句：**return false;** 如果不加return false，提交页面时，整个页面会立马刷新，数据改变只是一闪而过；

​          方法二：表单不用`<input type="submit">`的方式，改为设置一个button按钮，让用户通过手动点击按钮来实现数据更改。



当使用ajax进行form表单提交时，只能提交text，radio等 [input] 对象的数据，对于文件流数据，serialize()并不起作用，需要用FormDada（）来提交，例如

```javascript
var data = new FormData();		//创建formdata对象，便于将文件传输到后端
	data.append("title",$('#title').val());
	data.append("description",$('#description').val());

	data.append("pic", document.getElementById("pic").files[0]);	
	//在formdata对象中添加(封装)文件对象

	data.append("tourl",$('#tourl').val())
	data.append("imgIndex",$('#imgIndex').val());

	$.ajax({
		type :"POST",
		url : '/banner/upload_handle/',
		data: data,
		processData:false,   //设置不对数据进行自处理，默认jquery会对上传的数据进行处理
		contentType:false,   //设置不添加请求头的内容类型
		dataType:"html",
		global:false,
		async: false,
		success: function(data) {
			$(".picList").empty();
			$(".picList").html(data);
			console.log(data);
		},
		error:function(){
			console.log("获取数据失败！");
		}
	});
```

