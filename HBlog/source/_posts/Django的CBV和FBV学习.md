---
title: Django的CBV和FBV学习
date: 2019-10-14 15:01:44
categories: [学习笔记,Python,Django]
---



**CBV（class base views）**：在视图里使用类处理请求，即采用面向对象的方法写视图文件。

如果我们要写一个处理GET方法的view，用函数写的话是下面这样。

```python
from django.http import HttpResponse
  
def my_view(request):
     if request.method == 'GET':
            return HttpResponse('OK')
```

如果用class-based view写的话，就是下面这样

<!--more-->

```python
from django.http import HttpResponse
from django.views import View
  
class MyView(View):
      def get(self, request):
            return HttpResponse('OK')
```



CBV的执行流程中，浏览器向服务器端发送请求，服务器端的urls.py根据请求匹配url，找到要执行的视图类，执行dispatch方法区分出POST请求还是GET请求，执行views.py对应类中的POST方法或GET方法。



Django的url是将一个请求分配给可调用的函数的，而不是一个class。针对这个问题，class-based view提供了一个`as_view()`静态方法（也就是类方法），调用这个方法，会创建一个类的实例，然后通过实例调用`dispatch()`方法，`dispatch()`方法会根据request的method的不同调用相应的方法来处理request（如`get() `,` post()`等）。这些方法和function-based view差不多了，要接收request，得到一个response返回。如果方法没有定义，会抛出HttpResponseNotAllowed异常。



url实例：

```python
# urls.py
from django.conf.urls import url
from myapp.views import MyView
  
urlpatterns = [
     url(r'^index/$', MyView.as_view()),
]
```



类的属性可以通过两种方法设置，第一种是常见的Python的方法，可以被子类覆盖。

```python
from django.http import HttpResponse
from django.views import View
  
class GreetingView(View):
    name = "yuan"
    def get(self, request):
         return HttpResponse(self.name)
  
# You can override that in a subclass
  
class MorningGreetingView(GreetingView):
    name= "alex"
```

第二种方法，也可以在url中指定类的属性：

在url中设置类的属性Python

```python
urlpatterns = [
   url(r'^index/$', GreetingView.as_view(name="egon")),
]
```



**FBV（function base views）** 就是在视图里使用函数处理请求。

```python
thisAtc = Article.objects.all().order_by('-created_time')
		ccccuser = User.objects.get(username='张三')
		article = ccccuser.article_set.all()
		print(article)
		for i in article:
			print(i.author)
```

在之前django的学习中，我们一直使用的是这种方式。

Python是一个面向对象的编程语言，如果只用函数来开发，有很多面向对象的优点就错失了（继承、封装、多态）。所以Django在后来加入了Class-Based-View。可以让我们用类写View。这样做的优点主要下面两种：

1. 提高了代码的复用性，可以使用面向对象的技术，比如Mixin（多继承）。
2. 可以用不同的函数针对不同的HTTP方法处理，而不是通过很多if判断，提高代码可读性。



> #### 使用Mixin**
>
> 我觉得要理解django的class-based-view（以下简称cbv），首先要明白django引入cbv的目的是什么。在django1.3之前，generic   view也就是所谓的通用视图，使用的是function-based-view（fbv），亦即基于函数的视图。有人认为fbv比cbv更pythonic，窃以为不然。
>
> python的一大重要的特性就是面向对象。而cbv更能体现python的面向对象。
>
> cbv是通过class的方式来实现视图方法的。class相对于function，更能利用多态的特定，因此更容易从宏观层面上将项目内的比较通用的功能抽象出来。关于多态，可以理解为一个东西具有多种形态（的特性）。cbv的实现原理通过看django的源码就很容易明白，大体就是由url路由到这个cbv之后，通过cbv内部的dispatch方法进行分发，将get请求分发给cbv.get方法处理，将post请求分发给cbv.post方法处理，其他方法类似。
>
> 怎么利用多态呢？cbv里引入了mixin的概念。Mixin就是写好了的一些基础类，然后通过不同的Mixin组合成为最终想要的类。
>
> 所以，理解cbv的基础是，理解Mixin。Django中使用Mixin来重用代码，一个View Class可以继承多个Mixin，但是只能继承一个View（包括View的子类），推荐把View写在最右边，多个Mixin写在左边。