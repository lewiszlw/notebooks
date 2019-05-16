---
title: Django View视图层学习
date: 2017-12-06 14:44:12
tags: [Python, Django]
categories: Python
---
View视图层主要封装负责处理用户请求和返回响应的逻辑。
<!--more-->

## URL路由
对于高质量的Web 应用来说，使用简洁、优雅的URL 模式是一个非常值得重视的细节。Django 让你随心所欲设计你的URL，不受框架束缚。
### Django 如何处理一个请求
当一个用户请求Django 站点的一个页面，下面是Django 系统决定执行哪个Python 代码遵循的算法：
1. Django 决定要使用的根URLconf 模块。通常，这个值就是ROOT\_URLCONF 的设置，但是如果进来的HttpRequest 对象具有一个urlconf 属性（通过中间件request_processing 设置），则使用这个值来替换ROOT\_URLCONF 设置。
2. Django 加载该Python 模块并寻找可用的urlpatterns。它是django.conf.urls.url() 实例的一个Python 列表。
3. Django 依次匹配每个URL 模式，在与请求的URL 匹配的第一个模式停下来。
4. 一旦其中的一个正则表达式匹配上，Django 将导入并调用给出的视图，它是一个简单的Python 函数（或者一个基于类的视图）。视图将获得如下参数:
  - 一个HttpRequest 实例。
  - 如果匹配的正则表达式返回了没有命名的组，那么正则表达式匹配的内容将作为位置参数提供给视图。
  - 关键字参数由正则表达式匹配的命名组组成，但是可以被django.conf.urls.url()的可选参数kwargs覆盖。
5. 如果没有匹配到正则表达式，或者如果过程中抛出一个异常，Django 将调用一个适当的错误处理视图。

### 非命名组和命名组
1. 没有命名的正则表达式组（通过圆括号）来捕获URL 中的值并以位置 参数传递给视图：
```python
url(r'^articles/([0-9]{4})/$', views.year_archive)
```

2. 在Python 正则表达式中，命名正则表达式组的语法是**(?P&lt;name&gt;pattern)**，其中name 是组的名称，pattern 是要匹配的模式：
```python
url(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive)
```

匹配/分组算法：
1. 如果有命名参数，则使用这些命名参数，忽略非命名参数。
2. 否则，它将以位置参数传递所有的非命名参数。

### 包含其他URLconfs
```python
url(r'^community/', include('django_website.aggregator.urls'))

urlpatterns = [
    url(r'^(?P<page_slug>[\w-]+)-(?P<page_id>\w+)/', include([
        url(r'^history/$', views.history),
        url(r'^edit/$', views.edit),
        url(r'^discuss/$', views.discuss),
        url(r'^permissions/$', views.permissions),
    ])),
]
```
正则表达式没有包含$（字符串结束匹配符），但是包含一个末尾的斜杠。每当Django 遇到include()（django.conf.urls.include）时，它会去掉URL 中匹配的部分并将剩下的字符串发送给包含的URLconf 做进一步处理。

被包含的URLconf 会收到来自父URLconf 捕获的任何参数。

### 嵌套参数
```python
from django.conf.urls import url

urlpatterns = [
 	# 不推荐，会产生两个位置参数，如page-2和2
    url(r'blog/(page-(\d+)/)?$', blog_articles),

    # 推荐，产生一个关键字参数：page_number
    url(r'comments/(?:page-(?P<page_number>\d+)/)?$', comments),
]
```
第二个外围参数是一个不捕获的参数**(?:...)**。

作为一个经验的法则，当正则表达式需要一个参数但视图忽略它的时候，只捕获视图需要的值并使用非捕获参数。

### 反向解析URL
在使用Django 项目时，一个常见的需求是获得URL 的最终形式，以用于嵌入到生成的内容中（视图中和显示给用户的URL等）或者用于处理服务器端的导航（重定向等）。
在需要URL 的地方，对于不同层级，Django 提供不同的工具用于URL 反查：
1. 在模板中：使用url 模板标签。
2. 在Python 代码中：使用django.core.urlresolvers.reverse() 函数。
3. 在更高层的与处理Django 模型实例相关的代码中：使用get\_absolute\_url() 方法。

例如：
```python
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^articles/([0-9]{4})/$', views.year_archive, name='news-year-archive'),
]
```
你可以在模板的代码中使用下面的方法获得它们：
```html
<a href="{% url 'news-year-archive' 2012 %}">2012 Archive</a>

<ul>
{% for yearvar in year_list %}
<li><a href="{% url 'news-year-archive' yearvar %}">{{ yearvar }} Archive</a></li>
{% endfor %}
</ul>
```
在Python 代码中，这样使用：
```python
from django.core.urlresolvers import reverse
from django.http import HttpResponseRedirect

def redirect_to_year(request):
    year = 2006
    return HttpResponseRedirect(reverse('news-year-archive', args=(year,)))
```

### URL命名空间
URL 的命名空间使用':' 操作符指定。例如，管理站点应用的主页使用'admin:index'。它表示'admin' 的一个命名空间和'index' 的一个命名URL。

命名空间也可以嵌套。命名URL'sports:polls:index' 将在命名空间'polls'中查找'index'，而poll 定义在顶层的命名空间'sports' 中。
```python
# urls.py
from django.conf.urls import include, url
urlpatterns = [
    url(r'^author-polls/', include('polls.urls', namespace='author-polls', app_name='polls')),
    url(r'^publisher-polls/', include('polls.urls', namespace='publisher-polls', app_name='polls')),
]

# polls/urls.py
from django.conf.urls import url
from . import views
urlpatterns = [
    url(r'^$', views.IndexView.as_view(), name='index'),
    url(r'^(?P<pk>\d+)/$', views.DetailView.as_view(), name='detail'),
]
```

## 视图函数
一个视图函数，简称视图，是一个简单的Python 函数，它接受Web请求并且返回Web响应。响应可以是一张网页的HTML内容，一个重定向，一个404错误，一个XML文档，或者一张图片. . . 是任何东西都可以。无论视图本身包含什么逻辑，都要返回响应。代码写在哪里也无所谓，只要它在你的Python目录下面。

```python
from django.http import HttpResponse
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    html = "<html><body>It is now %s.</body></html>" % now
    return HttpResponse(html)
```

## 快捷函数
django.shortcuts 收集了“跨越” 多层MVC 的辅助函数和类。 换句话讲，这些函数/类为了方便，引入了可控的耦合。
### render
结合一个给定的模板和一个给定的上下文字典，并返回一个渲染后的 HttpResponse 对象。
```python
from django.shortcuts import render

def my_view(request):
    return render(request, 'myapp/index.html', {"foo": "bar"},
        content_type="application/xhtml+xml")
```

### redirect
为传递进来的参数返回HttpResponseRedirect 给正确的URL 。
1. 通过传递一个对象；将调用get_absolute_url() 方法来获取重定向的URL：
```python
from django.shortcuts import redirect
def my_view(request):
    object = MyModel.objects.get(...)
    return redirect(object)
```
2. 通过传递一个视图的名称，可以带有位置参数和关键字参数；将使用reverse() 方法反向解析URL：
```python
def my_view(request):
    return redirect('some-view-name', foo='bar')
```
3. 传递要重定向的一个硬编码的URL：
```python
def my_view(request):
    return redirect('/some/url/')
```
4. 也可以是一个完整的URL：
```python
def my_view(request):
    return redirect('http://example.com/')
```

注意：默认情况下，redirect() 返回一个临时重定向。以上所有的形式都接收一个permanent 参数；如果设置为True，将返回一个永久的重定向：
```python
def my_view(request):
    object = MyModel.objects.get(...)
    return redirect(object, permanent=True)
```

**301永久重定向和302临时重定向**：
1. 301重定向是永久的重定向，搜索引擎在抓取新内容的同时也将旧的网址替换为重定向之后的网址。
2. 302跳转是暂时的跳转，搜索引擎会抓取新的内容而保留旧的网址。因为服务器返回302代码，搜索引擎认为新的网址只是暂时的。

### get_object_or_404
在一个给定的模型管理器上调用get()，但是引发Http404 而不是模型的DoesNotExist 异常。
```python
from django.shortcuts import get_object_or_404
def my_view(request):
    my_object = get_object_or_404(MyModel, pk=1)
```

### get_list_or_404
返回一个给定模型管理器上filter() 的结果，并将结果映射为一个列表，如果结果为空则返回Http404。
```python
from django.shortcuts import get_list_or_404
def my_view(request):
    my_objects = get_list_or_404(MyModel, published=True)
```

## 视图装饰器
### require_http_methods(request_method_list)
限制视图只能服务规定的http方法。用法：
```python
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"])
def my_view(request):
    pass
```
注意，HTTP请求的方法名必须大写。

### require_GET()
只允许视图接受GET方法的装饰器。

### require_POST()
只允许视图接受POST方法的装饰器。

### require_safe()
只允许视图接受 GET 和 HEAD 方法的装饰器。 这些方法通常被认为是安全的，因为方法不该有请求资源以外的目的。


---------
> [Django 文档 — Django 1.8.2 中文文档](http://python.usyiyi.cn/documents/django_182/index.html)
