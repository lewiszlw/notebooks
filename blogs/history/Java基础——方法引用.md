---
title: Java基础——方法引用
date: 2018-09-13 20:25:23
tags: [Java]
categories: Java
---
方法引用（Method Reference）是对 lambda 表达式的一种转换表达，也称双冒号运算。
<!--more-->
# 语法格式

```java
className:: methodName
或
objectName:: methodName
```

# Demo

```java
List<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);
```

采用 forEach 方法遍历上述 list
```java
list.forEach(new Consumer<Integer>() {
    @Override
    public void accept(Integer i) {
        System.out.println(i);
    }
});
```

由于 Consumer 接口是函数式接口，可以使用 lambda 改写
```java
list.forEach(i -> System.out.println(i));
```

继续采用 Method Reference 改写
```java
list.forEach(System.out:: println);
```
