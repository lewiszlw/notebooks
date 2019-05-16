---
title: Linux下用命令行编译Java程序
date: 2017-01-10 12:44:42
tags: [Linux,Java]
categories: Linux
---
写这个主要是因为我用IDE（eclipse neon版本）编译运行一个Java程序时要在console用到EOF快捷键，在IDE上按下EOF快捷键没有反应，Google了下好像是bug，所以选择用命令行来运行下写的程序。可是遇到了许多问题，于是写篇博客记录下。
<!--more-->
# 编译命令
```
javac [options] sourcefile
```
javac命令用于编译Java源文件（.java），生成字节码文件（.class）。

option:
- -d 用于指定编译成的class文件的存放位置，缺省情况下不指定class文件的存放目录，编译的class文件将和源文件在同一目录下
- -classpath 可以简写成-cp，用于搜索编译所需的class文件，指出编译所用到的class文件的位置，如jar、zip或者其他包含class文件的目录，指定该选项会覆盖CLASSPATH的设定
- -sourcepath用于搜索编译所需的源文件（即java文件），指定要搜索的源文件的位置，如jar、zip或其他包含java文件的目录

sourcefile：源文件位置
# 执行命令
```
java [options] classfile [arguments]
```
java命令用于执行.class文件。

option：
命令行选项，一般用于 -classpath 指定要执行的文件所在的位置以及需要用到的类路径，包括jar、zip和class文件目录，会覆盖CLASSPATH的设定。

classfile：字节码文件位置

arguments：用于给main函数传递参数
# 举例
**创建hello工程文件文件夹**
- /bin 用于存放编译好的.class文件
- /lib 用于存放第三方库文件
- /src 用于存放Java源文件

例如编写好了test.java放在src/package1/下，需要引入第三方库lib1.jar和lib2.jar（存放在lib文件夹a）。

**编译Java源文件**
工程文件目录下执行：
```
javac -d bin -sourcepath src -cp lib/lib1.jar:lib/lib2.jar src/package1/test.java
```
注：多个包之间用“:”连接。如果依赖其他类文件，则需要加上-sourcepath src。

**执行.class文件**
```
java -cp bin:lib/lib1.jar:lib/lib2.jar package1.test
```

------------------
参考文章：
- [如何用javac 和java 编译运行整个Java工程](http://blog.csdn.net/huagong_adu/article/details/6929817)
- [Linux下编译使用命令行编译运行java程序（多个文件或者整个工程）](http://www.wangmingkuo.com/java/linux%E4%B8%8B%E7%BC%96%E8%AF%91%E4%BD%BF%E7%94%A8%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%BC%96%E8%AF%91%E8%BF%90%E8%A1%8Cjava%E7%A8%8B%E5%BA%8F%EF%BC%88%E5%A4%9A%E4%B8%AA%E6%96%87%E4%BB%B6%E6%88%96%E8%80%85/)
