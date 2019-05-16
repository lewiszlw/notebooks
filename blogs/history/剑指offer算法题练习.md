---
title: 剑指offer算法题练习
date: 2017-07-29 10:24:54
tags: [Java,Algorithm,算法]
categories: Algorithms
---
最近准备面试，在看《剑指offer》，顺便记录下自己做的题。
<!--more-->
# 赋值运算符函数
# 实现Singleton模式
**题目**：设计一个类，我们生成该类的一个实例。

**思路**：
解法一（懒汉式）：设置构造函数为private，定义静态变量，初始为null，定义静态方法用来获取此类的对象，需在方法内判断此类的对象是否存在，若已存在则返回该对象，若不存在则new一个对象返回，延迟加载，避免内存浪费。此法适合单线程，若要多线程环境下使用，给静态方法加上synchronized，但每次获取实例都很耗时。
解法二（饿汉式）：定义静态变量，初始化为类的对象。没有加锁，执行效率提高，但无法延迟加载，当单例模式类较多，会使得系统启动变慢，也占用大量内存。
解法三（双重检验锁）：定义的静态变量加上volatile，定义的类方法使用synchronized锁住判断语句代码块。适合多线程。
解法四（静态内部类）：将静态变量定义在静态内部类中，并初始化为类的对象。延迟加载。

**代码**：[实现Singleton模式](https://github.com/lewiszlw/Coding-Interviews/tree/master/src/com/lewiszlw/chapter2)

# 二维数组的查找
**题目**：在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

**思路**：利用数组每行和每列递增的特点，从数组右上角开始找，如果比查找数字大，则查找数字不在当前行；如果比查找数字小，则查找数字不在当前列。

**代码**：[二维数组的查找（_03FindInPartiallySortedMatrix.java）](https://github.com/lewiszlw/Coding-Interviews/blob/master/src/com/lewiszlw/chapter2/_03FindInPartiallySortedMatrix.java)

# 替换空格
**题目**：请实现一个函数，把字符串中的每个空格替换成“%20”。

**思路**：
思路一：创建一个StringBuilder对象，依次遍历字符串，遇到空格用“%20”替换。
思路二：将字符串转换为数组，依然遍历。

**代码**：[替换空格](https://github.com/lewiszlw/Coding-Interviews/blob/master/src/com/lewiszlw/chapter2/_04ReplaceBlank.java)


# 从尾到头打印链表
**题目**：输入一个链表的头结点，从尾到头反过来打印出每个结点的值。

**思路**：
思路一：遍历链表，将值记录到一个数组中。
思路二：反转链表，然后遍历打印出来。
思路三：利用栈的特点，将结点依次压栈，然后依次弹栈。

**代码**：[从尾到头打印链表](https://github.com/lewiszlw/Coding-Interviews/blob/master/src/com/lewiszlw/chapter2/_05PrintListReversingly.java)




--------------
> 《剑指offer——名企面试官精讲典型编程题》
