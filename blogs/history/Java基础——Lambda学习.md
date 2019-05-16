---
title: Java基础——Lambda学习
date: 2018-08-08 21:49:06
tags: [Java, Lambda]
categories: Java
---
Java8新特性——Lambda表达式。
<!--more-->
# 语法格式
Java中的lambda表达式语法
```
(parameters) -> expression
或
(parameters) ->{ statements; }
```

# 实质
Java中lambda表达式使用类型推断（type inference）技术

定义接口
```java
public interface IFlower {
    String getColor(String flowerName);
}
```

编写Lambda表达式
```java
IFlower flower = (String name) -> {if(name == "rose") return "red"; else return "unkonwn";};
System.out.println(flower.getColor("rose"));   //打印：red
```

编译器知道IFlower接口只有一个方法getColor()，所以getColor()方法肯定对应(String name) -> {if(name == "rose") return "red"; else return "unkonwn";}，而且getColor()方法只有一个String类型的参数，所以(String name) -> {if(name == "rose") return "red"; else return "unkonwn";}中的String name就是这个参数了。

因此，接口要求必须是函数式接口，如果其中有两个方法则lambda表达式会编译错误。

# 参考资料
http://www.runoob.com/java/java8-lambda-expressions.html
http://www.cnblogs.com/feichexia/archive/2012/11/15/Java8\_LambdaExpression.html
