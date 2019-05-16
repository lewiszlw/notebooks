---
title: JavaScript简史
date: 2016-10-25 23:58:26
tags: JavaScript
categories: [读书笔记,JavaScriptDOM编程艺术]
---
## JavaScript的起源
JavaScript是NETscape公司与Sun公司合作开发的。
JavaScript是一种`脚本语言`，通常只能通过web浏览器去完成一些操作而不能像普通意义上的程序那样独立运行。
<!--more-->
## DOM
什么是DOM？简单的说，DOM是一套对文档的内容进行抽象和概念化的方法。
例如，当用“汽车”、“房子”和“树”等名词来称呼日常生活环境中的事物时，我们可以百分之百地肯定对方知道我们说的是什么，这是因为人们对这些名词所代表的东西有着同样的认识。
## 浏览器战争
### DHTML
DHTML是“Dynamic HTML”（动态HTML）的简称。DHTML并不是一项新技术，而是描述HTML、CSS和JavaScript技术组合的术语。
DHTML背后的含义是：
- 利用HTML把网页标记为各种元素； 
- 利用CSS设置元素样式和它们的显示位置;
- 利用JavaScript实时地操控页面和改变样式。  

**但这个术语与其说是一个技术性词语，不如说是一个市场营销噱头。** 
### 浏览器之间的冲突
NETscape公司的DOM使用了专有元素，这些元素称为层（layer）。需这样引用：
```
document.layers['myelement']
```
  而在微软公司的DOM中这个元素必须像这样引用：
```
doocument.all['myelement']
```
## 制定标准
NETscape、微软和其他一些浏览器制造商抛开彼此的敌意而和W3C一起制定新的标准，并与1998年10月完成了“第1级DOM”（DOM Level1）。
### 浏览器以外的考虑
DOM是一种API（应用程序接口）。简单地说，**API就是一组已经得到有关各方共同认可的基本约定**。类似的例如莫尔斯码、国际时区。
W3C对DOM的定义是：
“`一个与系统平台和编程语言无关的接口，程序和脚本可以通过这个接口动态地访问和修改文档的内容、结构和样式。`”
