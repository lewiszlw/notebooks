---
title: Python装饰器总结
date: 2017-11-24 10:39:05
tags: [Python,装饰器]
categories: Python
---
本质上，Python装饰器就是一个返回函数的高阶函数。形式与Java注解很相似，但Python装饰器是通过函数式编程来实现，而Java注解是通过反射来实现。而且两者功能也不同，Python的装饰器会改变被装饰对象的行为和性质，而Java的注解只是元数据，即一种描述数据的数据，它本身不改变被注解对象的行为和性质，只有理解这些注解的处理器才会改变行为。
<!--more-->
## 装饰器简介
装饰器本质上是一个Python函数，它可以让其他函数在不需要做任何代码变动的前提下增加额外功能，装饰器的返回值也是一个函数对象。它经常用于有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景。装饰器就是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量与函数功能本身无关的雷同代码并继续重用。概况地讲，装饰器的作用就是为已经存在的对象添加额外的功能。

## 简单装饰器
```python
def log(func):
    def wrapper(*args, **kwargs):
        print('%s function start' % func.__name__)
        result = func(*args, **kwargs)
        print('%s function end' % func.__name__)
        return result
    return wrapper
 
@log
def say1(words):
    print(words)

@log
def say2(words):
	return words
 
say1('hello')	# 此时say1和say2指向的是wrapper函数（__name__为'wrapper'）

data = say2('hello')
print(data)
# 均打印:
# say function start
# hello
# say function end
```

log函数对象就是一个装饰器，它接受一个函数作为参数，并返回一个函数。@符号是装饰器的语法糖，把@log放到say()函数的定义处,相当于执行了语句：

```python
say = log(say)
```

由于log()是一个decorator，返回一个函数，所以，原来的say()函数仍然存在，只是现在同名的say变量指向了新的函数，于是调用say()将执行新函数，即在log()函数中返回的wrapper()函数。
wrapper()函数的参数定义是(*args, **kwargs)，因此，wrapper()函数可以接受任意参数的调用。在wrapper()函数内，首先打印函数开始执行日志，再紧接着调用原始函数，然后打印函数执行结束日志，最后返回原始函数返回值。

## 带参数的装饰器
Python装饰器还可以有更大的灵活性，它可以允许我们提供相应的参数。
```python
def log(level):
    def decorator(func):
        def wrapper(*args, **kwargs):
            if level == 'warning':
                print('waring! %s is called' % func.__name__)
            elif level == 'error':
                print('error! %s is called' % func.__name__)
            else:
                print('%s is called' % func.__name__)
            return func(*args, **kwargs)
        return wrapper
    return decorator


@log('error')
def say(words):
    print(words)

say('hello')
# 打印：
# error! say is called
# hello

f = say
print(f.__name__)
# 打印：
# wrapper
```
它实际上是对原有装饰器的一个函数封装，并返回一个装饰器。我们可以将它理解为一个含有参数的闭包。当我们使用@log('error')调用的时候，Python能够发现这一层的封装，并把参数传递到装饰器环境中。
和两层嵌套的decorator相比，三层嵌套的效果相当于：
```python
say = log('error')(say)
```

**注意：**
使用装饰器极大地复用了代码，但是有一个缺点就是原函数的元信息不见了，比如函数的\__name\__、docstring、参数列表，这样可能会造成某些依赖函数元信息的代码执行出错。好在我们有**functools.wraps**，wraps本身也是一个装饰器，它能把原函数的元信息拷贝到装饰器中，所以完整的decorator的写法如下：
```python
import functools

def log(func):
	@functools.wraps(func)
    def wrapper(*args, **kwargs):
        print('%s function was called.' % func.__name__)
        return func(*args, **kwargs)
    return wrapper
```

## 类式装饰器
相比函数装饰器，类式装饰器具有灵活度大、高内聚、封装性等优点。使用类式装饰器可以依靠内部的\__call\__方法，当使用@形式将装饰器附加到函数上时，就会调用此方法。
### 不带参数的类式装饰器
```python
class log(object):

    def __init__(self, func):
        self._func = func

    def __call__(self, *args, **kwargs):
        print('start')
        return self._func(*args, **kwargs)

@log
def say(words):
    return words

res = say('hello')
print(res)

res = say('world')
print(res)

# 打印：
# start
# hello
# start
# world
```
在装饰阶段，\__init\__ 函数执行，在被装饰的方法被调用的时候，\__call\__ 执行。

### 带参数的类式装饰器
```python
class log(object):

    def __init__(self, level):
        self._level = level

    def __call__(self, func):
        print('__call__()方法执行')
        def decorator(*args, **kwargs):
            if self._level == 'warning':
                print('warning! function was called')
            elif self._level == 'error':
                print('error! function was called')
            return func(*args, **kwargs)
        return decorator


@log('error')
def say(words):
    return words

res = say('hello')
print(res)

res = say('world')
print(res)

# 打印：
# __call__()方法执行
# error! function was called
# hello
# error! function was called
# world

```
\__call\__只会在装饰阶段被调用一次。

## 内置装饰器
1. @staticmethod
类静态方法，其跟成员方法的区别是没有self参数，并且可以在类不进行实例化的情况下调用
2. @classmethod
与成员方法的区别在于所接收的第一个参数不是self（类实例的指针），而是cls（当前类）
3. @property
属性，表示可以通过类实例访问的信息

## 装饰器顺序
```python
@a
@b
@c
def f():
```

等效于：

```python
f = a(b(c(f)))
```


-------------------------
> [如何理解Python装饰器？ - 知乎](https://www.zhihu.com/question/26930016)
[装饰器 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014318435599930270c0381a3b44db991cd6d858064ac0000)

