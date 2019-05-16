---
title: Gvim下Emmet安装及使用教程
date: 2016-11-12 14:38:52
tags: [Vim,Emmet,HTML,CSS]
categories: blog
---
# 前言
在编写html和css文件时，因为各种各样的标签写得让人头疼，上网看到这款插件，在使用后能极大地提高代码编写效率。Emmet的前身是Zen coding，它使用仿CSS选择器的语法来生成代码，大大提高了HTML/CSS代码编写的速度。
<!--more-->
# 安装教程
我没有选择先安装[vundle](https://github.com/VundleVim/Vundle.vim)、[pathogen](https://github.com/tpope/vim-pathogen)等插件管理器，而是直接安装Emmet vim插件。但是推荐先安装插件管理器再安装Emmet。
安装方法：
- 下载[Emmet.vim](https://github.com/mattn/emmet-vim)并解压；
- 将解压的文件里面plugin目录下文件复制到vim目录下plugin文件夹里（非vimfile文件夹）；
- 将解压的文件里面autoload目录下文件及文件夹复制到vim目录下autoload文件夹里。

# Emmet语法及常用缩写
输入缩写然后按`<c-y>,`，光标要在紧跟最后一个字符。
## HTML
### 模板
`html:5`和`!`生成
```html
<!DOCTYPE html>
<html lang="en">
<head>
        <meta charset="UTF-8">
        <title></title>
</head>
<body>
        
</body>
</html>
```
### \>：下级
`nav>ul>li`生成
```html
<nav>
        <ul>
                <li></li>
        </ul>
</nav>
```
### +：同级
`div+p+bq`生成
```html
<div></div>
<p></p>
<blockquote></blockquote>
```
### ^：上级
`div+div>p>span+em^bq`生成
```html
<div></div>
<div>
        <p>
                <span></span>
                <em></em>
        </p>
        <blockquote></blockquote>
</div>
```
`div+div>p>span+em^^bq`生成
```html
<div></div>
<div>
        <p>
                <span></span>
                <em></em>
        </p>
</div>
<blockquote></blockquote>
```
### ()：分组
`div>(header>ul>li*2>a)+footer>p`生成
```html
<div>
        <header>
                <ul>
                        <li><a href=""></a></li>
                        <li><a href=""></a></li>
                </ul>
        </header>
        <footer>
                <p></p>
        </footer>
</div>
```
`(div>dl>(dt+dd)*3)+footer>p`生成
```html
<div>
        <dl>
                <dt></dt>
                <dd></dd>
                <dt></dt>
                <dd></dd>
                <dt></dt>
                <dd></dd>
        </dl>
</div>
<footer>
        <p></p>
</footer>
```
### *：乘法
`ul>li*5`生成
```html
<ul>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
</ul>
```
### $：自增符号
`ul>li.item$*5`生成
```html
<ul>
        <li class="item1"></li>
        <li class="item2"></li>
        <li class="item3"></li>
        <li class="item4"></li>
        <li class="item5"></li>
</ul>
```
`h$[title=item$]{Header $}*3`生成
```html
<h1 title="item1">Header 1</h1>
<h2 title="item2">Header 2</h2>
<h3 title="item3">Header 3</h3>
```
`ul>li.item$@-*5`生成
```html
<ul>
        <li class="item5"></li>
        <li class="item4"></li>
        <li class="item3"></li>
        <li class="item2"></li>
        <li class="item1"></li>
</ul>                           //反向增
```
`ul>li.item$@3*5`生成
```html
<ul>
        <li class="item3"></li>
        <li class="item4"></li>
        <li class="item5"></li>
        <li class="item6"></li>
        <li class="item7"></li>
</ul>                          //从3开始
```
### {}：文本内容
`a{click me}`生成
```html
<a href="">click me</a>
```
`p>{click}+a{here}+{to continue}`生成
```html
<p>
        click
        <a href="">class</a>
        to continue
</p>
```
### ID和类属性
`#header`生成
```html
<div id="header"></div>
```
`.title`生成
```html
<div class="title"></div>
```
`form#search.wide`生成
```html
<form id="search" class="wide" action=""></form>
```
`p.class1.class2.class3`生成
```html
<p class="class1 class2 class3"></p>
```
### 自定义属性
`p[a="value1" b=value2 c='value3' d]`生成
```html
<p a="value1" b="value2" c="value3" d=""></p>
```
### 隐式标签
`.class`生成
```html
<div class="class"></div>
```
`em>.class`生成
```html
<em><span class="class"></span></em>
```
`ul>.class`生成
```html
<ul>
        <li class="class"></li>
</ul>
```
### 各类标签
例如：
`link`生成
```html
<link rel="stylesheet" href="">
```
`link:favicon`生成
```html
<link rel="shortcut icon" type="image/x-icon" href="favicon.ico">
```
## CSS
举例：
pos-->position:relative;
pos:s-->position:static;
pos:a-->position:absolute;
pos:r-->position:relative;
pos:f-->position:fixed;
# 附录
- [Emmet official site](http://emmet.io/)
- [Emmet所有缩写](http://docs.emmet.io/cheat-sheet/)

