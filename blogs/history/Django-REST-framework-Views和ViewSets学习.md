---
title: Django-REST-framework Views和ViewSets学习
date: 2017-12-12 10:18:36
tags: [Python, Django, Django-REST-framework]
categories: Python
---
REST-framework作为Django的扩展框架，利用它我们能够快速地开发RESTful接口。
<!--more-->

## 基于类的视图
REST框架提供了一个APIView类，它是Django View类的子类
- 使用REST-framework框架的Request和Response
- 传入的请求将被认证，并且在将请求分派给处理程序方法之前将运行适当的permission等检查
```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import authentication, permissions
from django.contrib.auth.models import User

class ListUsers(APIView):
    authentication_classes = (authentication.TokenAuthentication,)
    permission_classes = (permissions.IsAdminUser,)

    def get(self, request, format=None):
        usernames = [user.username for user in User.objects.all()]
        return Response(usernames)

	def post(self, request, *args, **kwargs):
        ...
```

### API策略属性
这些属性是执行视图前后进行一些切面操作：
- renderer\_classes
- parser\_classes
- permission\_classes
- authentication\_classes
- content\_negotiation\_class

### dispatch()调度过程
dispatch源码：
```python
    def dispatch(self, request, *args, **kwargs):
        """
        `.dispatch()` is pretty much the same as Django's regular dispatch,
        but with extra hooks for startup, finalize, and exception handling.
        """
        self.args = args
        self.kwargs = kwargs
        request = self.initialize_request(request, *args, **kwargs)
        self.request = request
        self.headers = self.default_response_headers  # deprecate?

        try:
            self.initial(request, *args, **kwargs)

            # Get the appropriate handler method
            if request.method.lower() in self.http_method_names:
                handler = getattr(self, request.method.lower(),
                                  self.http_method_not_allowed)
            else:
                handler = self.http_method_not_allowed

            response = handler(request, *args, **kwargs)

        except Exception as exc:
            response = self.handle_exception(exc)

        self.response = self.finalize_response(request, response, *args, **kwargs)
        return self.response
```
1. 调用initialize\_request()方法确保传递给处理程序方法的请求对象是一个实例Request，而不是通常的Django HttpRequest。会将策略属性传入进去。
```python
    def initialize_request(self, request, *args, **kwargs):
        """
        Returns the initial request object.
        """
        parser_context = self.get_parser_context(request)

        return Request(
            request,
            parsers=self.get_parsers(),
            authenticators=self.get_authenticators(),
            negotiator=self.get_content_negotiator(),
            parser_context=parser_context
        )
```
2. 调用initial()方法，会进行一些权限校验等等。
```python
    def initial(self, request, *args, **kwargs):
        """
        Runs anything that needs to occur prior to calling the method handler.
        """
        self.format_kwarg = self.get_format_suffix(**kwargs)

        # Ensure that the incoming request is permitted
        self.perform_authentication(request)
        self.check_permissions(request)
        self.check_throttles(request)

        # Perform content negotiation and store the accepted info on the request
        neg = self.perform_content_negotiation(request)
        request.accepted_renderer, request.accepted_media_type = neg

        # Determine the API version, if versioning is in use.
        version, scheme = self.determine_version(request, *args, **kwargs)
        request.version, request.versioning_scheme = version, scheme
```
3. 根据请求方法执行相应函数。
```python
response = handler(request, *args, **kwargs)
```
4. handle\_exeception()。处理程序方法抛出的任何异常都将传递给此方法，该方法将返回一个Response实例，或者重新引发异常。
5. 执行finalize\_response()确保Response从处理程序方法返回的任何对象将被呈现为正确的内容类型，由内容negotiation确定。

## 基于函数的视图
利用`@api_view()`装饰器可以开发简单的视图函数：
```python
from rest_framework.decorators import api_view

@api_view()
def hello_world(request):
    return Response({"message": "Hello, world!"})
```
注：默认情况下，只有GET方法会被接受。其他方法将返回“405 方法不允许”。可以指定允许的方法，如@api_view(['GET', 'POST']) 。

**API策略装饰器：**
- @renderer_classes(...)
- @parser_classes(...)
- @authentication_classes(...)
- @throttle_classes(...)
- @permission_classes(...)
```python
from rest_framework.decorators import api_view, throttle_classes
from rest_framework.throttling import UserRateThrottle

class OncePerDayUserThrottle(UserRateThrottle):
        rate = '1/day'

@api_view(['GET'])
@throttle_classes([OncePerDayUserThrottle])
def view(request):
    return Response({"message": "Hello for today! See you tomorrow!"})
```

## 通用视图
基于类的视图的关键好处之一是它们允许你构建可重用行为的方式。REST框架通过提供一些预先构建的视图来提供常用的模式，从而充分利用了这一点。
### GenericAPIView
这个类扩展了REST框架的APIView类，为标准的列表视图和细节视图添加了通常需要的行为。提供的每个具体通用视图都是通过GenericAPIView与一个或多个mixin类相结合来构建的。

常用属性：
- queryset：用于从视图返回对象的查询集
- serializer\_class：用于验证、序列化输出和反序列化输入的序列化器
- filter\_backends：用于过滤查询集的过滤器后端类的列表

常用方法：
- get\_queryset(self)：返回应该用于列表视图的查询集，默认返回queryset属性指定的查询集
- get\_object(self)：返回应该用于详细视图的对象实例。默认使用lookup\_field参数来过滤基本查询集
- filter\_queryset(self, queryset)：给定一个查询集，使用任何后端正在使用的过滤器，返回一个新的查询集
- get\_serializer\_class(self)：返回应该用于序列化程序的类。默认返回serializer_class属性

### Mixins
mixin类提供用于提供基本视图行为的操作。
#### ListModelMixin
- 提供一个.list(request, *args, **kwargs)实现列出查询集的方法
- 如果查询集已填充，则返回一个200 OK响应，并将查询集的序列化表示形式作为响应的主体。响应数据可以可选地分页
```python
    def list(self, request, *args, **kwargs):
        queryset = self.filter_queryset(self.get_queryset())

        page = self.paginate_queryset(queryset)
        if page is not None:
            serializer = self.get_serializer(page, many=True)
            return self.get_paginated_response(serializer.data)

        serializer = self.get_serializer(queryset, many=True)
        return Response(serializer.data)
```

#### CreateModelMixin
- 提供一种.create(request, *args, **kwargs)实现创建和保存新模型实例的方法
- 如果创建了一个对象，则会返回一个201 Created响应，并将对象的序列化表示形式作为响应的主体。如果该表示包含一个名为的键url，则Location响应的标题将填充该值
```python
    def create(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        self.perform_create(serializer)
        headers = self.get_success_headers(serializer.data)
        return Response(serializer.data, status=status.HTTP_201_CREATED, headers=headers)

    def perform_create(self, serializer):
        serializer.save()

    def get_success_headers(self, data):
        try:
            return {'Location': data[api_settings.URL_FIELD_NAME]}
        except (TypeError, KeyError):
            return {}
```

#### RetrieveModelMixin
- 提供一个.retrieve(request, *args, **kwargs)方法，实现在响应中返回现有的模型实例
```python
    def retrieve(self, request, *args, **kwargs):
        instance = self.get_object()
        serializer = self.get_serializer(instance)
        return Response(serializer.data)
```

#### UpdateModelMixin
- 提供一个.update(request, *args, **kwargs)方法，实现更新和保存现有的模型实例
- 还提供了.partial_update(request, *args, **kwargs)一种与该update方法类似的方法，只是所有更新字段都是可选的
```python
    def update(self, request, *args, **kwargs):
        partial = kwargs.pop('partial', False)
        instance = self.get_object()
        serializer = self.get_serializer(instance, data=request.data, partial=partial)
        serializer.is_valid(raise_exception=True)
        self.perform_update(serializer)
        return Response(serializer.data)

    def perform_update(self, serializer):
        serializer.save()

    def partial_update(self, request, *args, **kwargs):
        kwargs['partial'] = True
        return self.update(request, *args, **kwargs)
```

#### DestroyModelMixin
提供一个.destroy(request, *args, **kwargs)方法，实现现有模型实例的删除
```python
    def destroy(self, request, *args, **kwargs):
        instance = self.get_object()
        self.perform_destroy(instance)
        return Response(status=status.HTTP_204_NO_CONTENT)

    def perform_destroy(self, instance):
        instance.delete()
```

### 具体视图类
#### CreateAPIView
- 用于创建模型实例
- 提供一个post方法处理
- 继承：GenericAPIView，CreateModelMixin
```python
class CreateAPIView(mixins.CreateModelMixin,
                    GenericAPIView):

    """
    Concrete view for creating a model instance.
    """
    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)
```

#### ListAPIView
- 用于读取模型实例的集合
- 提供一个get方法处理
- 继承：GenericAPIView，ListModelMixin
```python
class ListAPIView(mixins.ListModelMixin,
                  GenericAPIView):
    """
    Concrete view for listing a queryset.
    """
    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)
```

#### RetrieveAPIView
- 用于读取单个模型实例
- 提供一个get方法处理
- 继承：GenericAPIView，RetrieveModelMixin
```python
class RetrieveAPIView(mixins.RetrieveModelMixin,
                      GenericAPIView):
    """
    Concrete view for retrieving a model instance.
    """
    def get(self, request, *args, **kwargs):
        return self.retrieve(request, *args, **kwargs)
```

#### DestroyAPIView
- 用于删除单个模型实例
- 提供一个delete方法处理
- 继承：GenericAPIView，DestroyModelMixin
```python
class DestroyAPIView(mixins.DestroyModelMixin,
                     GenericAPIView):

    """
    Concrete view for deleting a model instance.
    """
    def delete(self, request, *args, **kwargs):
        return self.destroy(request, *args, **kwargs)
```

#### UpdateAPIView
- 用于更新单个模型实例
- 提供put和patch方法处理
- 继承：GenericAPIView，UpdateModelMixin
```python
class UpdateAPIView(mixins.UpdateModelMixin,
                    GenericAPIView):

    """
    Concrete view for updating a model instance.
    """
    def put(self, request, *args, **kwargs):
        return self.update(request, *args, **kwargs)

    def patch(self, request, *args, **kwargs):
        return self.partial_update(request, *args, **kwargs)
```

#### ListCreateAPIView
- 用于读取、创建模型实例的集合
- 提供get和post方法处理
- 继承：GenericAPIView，ListModelMixin，CreateModelMixin
```python
class ListCreateAPIView(mixins.ListModelMixin,
                        mixins.CreateModelMixin,
                        GenericAPIView):
    """
    Concrete view for listing a queryset or creating a model instance.
    """
    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)

    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)
```

#### RetrieveUpdateAPIView
- 用于读取、更新单个模型实例
- 提供get、put和patch方法处理
- 继承：GenericAPIView，RetrieveModelMixin，UpdateModelMixin
```python
class RetrieveUpdateAPIView(mixins.RetrieveModelMixin,
                            mixins.UpdateModelMixin,
                            GenericAPIView):
    """
    Concrete view for retrieving, updating a model instance.
    """
    def get(self, request, *args, **kwargs):
        return self.retrieve(request, *args, **kwargs)

    def put(self, request, *args, **kwargs):
        return self.update(request, *args, **kwargs)

    def patch(self, request, *args, **kwargs):
        return self.partial_update(request, *args, **kwargs)
```

#### RetrieveDestroyAPIView
- 用于读取、删除单个模型实例
- 提供get和delete方法处理
- 继承：GenericAPIView，RetrieveModelMixin，DestroyModelMixin
```python
class RetrieveDestroyAPIView(mixins.RetrieveModelMixin,
                             mixins.DestroyModelMixin,
                             GenericAPIView):
    """
    Concrete view for retrieving or deleting a model instance.
    """
    def get(self, request, *args, **kwargs):
        return self.retrieve(request, *args, **kwargs)

    def delete(self, request, *args, **kwargs):
        return self.destroy(request, *args, **kwargs)
```

#### RetrieveUpdateDestroyAPIView
- 用于读取、更新和删除单个模型实例
- 提供get、put、patch和delete方法处理
- 继承：GenericAPIView，RetrieveModelMixin，UpdateModelMixin，DestroyModelMixin
```python
class RetrieveUpdateDestroyAPIView(mixins.RetrieveModelMixin,
                                   mixins.UpdateModelMixin,
                                   mixins.DestroyModelMixin,
                                   GenericAPIView):
    """
    Concrete view for retrieving, updating or deleting a model instance.
    """
    def get(self, request, *args, **kwargs):
        return self.retrieve(request, *args, **kwargs)

    def put(self, request, *args, **kwargs):
        return self.update(request, *args, **kwargs)

    def patch(self, request, *args, **kwargs):
        return self.partial_update(request, *args, **kwargs)

    def delete(self, request, *args, **kwargs):
        return self.destroy(request, *args, **kwargs)
```

## ViewSets
Django-REST-framework允许将一组相关视图的逻辑组合在一个类中，称为一个 ViewSet。ViewSet类是一种基于类的View，不提供像.get()或者.post这样的方法处理程序，而是提供诸如.list()和.create()的操作。

### 简单视图集
例如：
```python
from django.contrib.auth.models import User
from django.shortcuts import get_object_or_404
from myapps.serializers import UserSerializer
from rest_framework import viewsets
from rest_framework.response import Response

class UserViewSet(viewsets.ViewSet):
    """
    A simple ViewSet for listing or retrieving users.
    """
    def list(self, request):
        queryset = User.objects.all()
        serializer = UserSerializer(queryset, many=True)
        return Response(serializer.data)

    def retrieve(self, request, pk=None):
        queryset = User.objects.all()
        user = get_object_or_404(queryset, pk=pk)
        serializer = UserSerializer(user)
        return Response(serializer.data)
```

配置url：
```python
# 分成两个单独的视图
user_list = UserViewSet.as_view({'get': 'list'})
user_detail = UserViewSet.as_view({'get': 'retrieve'})

# 通常作法：用路由器注册ViewSet，自动生成urlconf
from myapp.views import UserViewSet
from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register(r'users', UserViewSet, base_name='user')
urlpatterns = router.urls
```

### 标记额外路由行为
REST-framework包含的默认路由器将为标准的create / retrieve / update / destroy风格操作提供路由，如下所示：
```python
class UserViewSet(viewsets.ViewSet):
    """
    Example empty viewset demonstrating the standard
    actions that will be handled by a router class.

    If you're using format suffixes, make sure to also include
    the `format=None` keyword argument for each action.
    """

    def list(self, request):
        pass

    def create(self, request):
        pass

    def retrieve(self, request, pk=None):
        pass

    def update(self, request, pk=None):
        pass

    def partial_update(self, request, pk=None):
        pass

    def destroy(self, request, pk=None):
        pass
```

如果需要路由到特别的方法，可以使用`@detail_route`或`@list_route`装饰器。
```python
# url为：^user/{pk}/set_password/$
@detail_route(methods=['post'], permission_classes=[IsAdminOrIsSelf])
def set_password(self, request, pk=None):
    ...

# url为：^user/get_password/$
@list_route(method=['GET'])
def get_password(self, request):
    ...


# urls.py
from myapp.views import UserViewSet
from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register(r'users', UserViewSet, base_name='user')
urlpatterns = router.urls
```

### ViewSet基类
1. GenericViewSet
  - 继承ViewSetMixin和GenericAPIView
  - 提供默认设置的get\_object和get\_queryset等方法
2. ModelViewSet
  - 继承CreateModelMixin、RetrieveModelMixin、UpdateModelMixin、DestroyModelMixin、ListModelMixin和GenericViewSet
  - 提供.list()，.retrieve()， .create()，.update()，.partial_update()，和.destroy()操作
3. ReadOnlyModelViewSet
  - 继承RetrieveModelMixin、ListModelMixin和GenericViewSet
  - 只提供一些只读操作：.retrieve()和.list()


-------------
> [Home - Django REST framework](http://www.django-rest-framework.org/)
