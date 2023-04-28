---
title: Django使用Paginator实现分页功能
categories: [学习笔记,Python,Django]
comments: true
toc: false
date: 2019-09-24 11:23:15
---

要使用Django实现分页器,必须从Django中导入Paginator模块

```python
from django.code.paginator import Paginator
```

下面贴出具体例子。

<!--more-->

`view.py`代码

```django
#导入render和HttpResponse模块
from django.shortcuts import render,HttpResponse

#导入Paginator,EmptyPage和PageNotAnInteger模块
from django.core.paginator import Paginator, EmptyPage, PageNotAnInteger

#从Django项目的应用中导入模块
from app01.models import *

def index(request):

    #获取Book数据表中的所有记录
    book_list=Book.objects.all()

    #生成paginator对象,定义每页显示10条记录
    paginator = Paginator(book_list, 10)

    #从前端获取当前的页码数,默认为1
    page = request.GET.get('page',1)
    
    #把当前的页码数转换成整数类型
    currentPage=int(page)

    try:
        print(page)
        book_list = paginator.page(page)#获取当前页码的记录
    except PageNotAnInteger:
        book_list = paginator.page(1)#如果用户输入的页码不是整数时,显示第1页的内容
    except EmptyPage:
        book_list = paginator.page(paginator.num_pages)#如果用户输入的页数不在系统的页码列表中时,显示最后一页的内容

    return render(request,"index.html",locals())
```



前端代码

```html
<!DOCTYPE html>
 3 <html lang="en">
 4 <head>
 5     <meta charset="UTF-8">
 6     <title>Title</title>
 7     <link rel="stylesheet" href="{% static 'bootstrap/css/bootstrap.css' %}">
 8 </head>
 9 <body>
10 <div class="container">
11     <h4>分页器</h4>
12     <ul>
13         #遍历boot_list中的所有元素
14         {% for book in book_list %}
15             #打印书籍的名称和价格
16             <li>{{ book.title }} {{ book.price }}</li>
17         {% endfor %}
18     </ul>
19     <ul class="pagination" id="pager">
20         {#上一页按钮开始#}
21         {# 如果当前页有上一页#}
22         {% if book_list.has_previous %}
23             {#  当前页的上一页按钮正常使用#}
24             <li class="previous"><a href="/?page={{ book_list.previous_page_number }}">上一页</a></li>
25         {% else %}
26             {# 当前页的不存在上一页时,上一页的按钮不可用#}
27             <li class="previous disabled"><a href="#">上一页</a></li>
28         {% endif %}
29         {#上一页按钮结束#}
30         {# 页码开始#}
31         {% for num in paginator.page_range %}
32 
33             {% if num == currentPage %}
34                 <li class="item active"><a href="/?page={{ num }}">{{ num }}</a></li>
35             {% else %}
36                 <li class="item"><a href="/?page={{ num }}">{{ num }}</a></li>
37 
38             {% endif %}
39         {% endfor %}
40         {#页码结束#}
41         {# 下一页按钮开始#}
42         {% if book_list.has_next %}
43             <li class="next"><a href="/?page={{ book_list.next_page_number }}">下一页</a></li>
44         {% else %}
45             <li class="next disabled"><a href="#">下一页</a></li>
46         {% endif %}
47         {# 下一页按钮结束#}
48     </ul>
49 </div>
50 </body>
51 </html>
```

