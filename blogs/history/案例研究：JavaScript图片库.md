---
title: 案例研究：JavaScript图片库
date: 2016-11-01 14:13:23
tags: [JavaScript,DOM]
categories: [读书笔记,JavaScriptDOM编程艺术]
---
把图片发布到网上的办法很多。你可以简单地把所有图片放在一个网页，但是当图片数量过多，这个页面会变得过于庞大，我们必须面对这样一个事实：没有人愿意等待很长时间去下载一个网页。
因此，为每张图片分别创建一个网页的方案值得考虑。不过这一方法并非尽善尽美。首先，为每张图片分别创建一个网页的工作量很大。其次，每个网页上应该提供某种导航链接给出当前图片在整个图片库的位置，方便人们从当前图片转到其他图片。
<!--more-->
## 标记
第一项工作是为这些图片创建一个链接清单。
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8"/>
  <title>Image Gallery</title>
</head>
<body>
  <h1>Snapshots</h1>
  <ul>
    <li>
    <a href="images/fireworks.jpg" title="a firework display">fireworks</a>
    </li>
    <li>
    <a href="images/coffee.png" title="a cup of coffee">coffee</a>
    </li>
    <li>
    <a href="images/rose.jpg" title="a red rose">rose</a>
    </li>
    <li>
    <a href="images/bigben.jpeg" title="the famouse clock">bigben</a>
    </li>
  </ul>
</body>
</html>
```
将其保存在gallery.html文件，并把图片集中保存在目录images里。images目录和gallery.html文件放在同一目录下。
希望改进的几个地方：
- 当点击某个链接时，能留在这个网页而不是转到另一个窗口。
- 当点击某个链接时，能在这个网页上同时看到那张图片以及原有的图片清单。

实现方法：
- 通过增加一个“占位符”图片的办法在这个主页上为图片预留一个浏览区域。
- 在点击某个链接时，拦截这个网页的默认行为。
- 在点击某个链接时，把“占位符”图片换成这个链接相对应的图片。

“占位符”图片：
```html
<img id="placeholder" src="images/placeholder.jpg" alt="my image gallery"/>
```
## JavaScript
为了把“占位符”图片替换成想要查看的图片，需要改变它的src属性。
```javascript
function showPic(whichpic){
	var source = whichpic.getAttribute("href");
	var placeholder = document.getElementById("placeholder");
	placeholder.setAttribute("src",source);
	}
```
## 应用这个JavaScript函数
把这个函数保存在扩展名为.js的文件中，然后在gallery.html中引用这个脚本文件。但是我们还需要事件处理函数来调用这个showPic函数。
**事件处理函数**
事件处理函数的作用是，在特定事件发生时调用特定的JavaScript代码。
在图片库里，用户点击链接触发一个动作，因此需要使用onclick事件处理函数。当我把onclick嵌入到链接中时，需要把这个链接本身用作showPic函数的参数：使用this关键字。
事件处理函数的语法：
```javascript
event="javascript statements(s)"
```
但是仅这样，点击链接时showPic函数会调用，同时链接被点击的默认行为也会被调用。
事件处理函数的工作机制：在给某个元素添加事件处理函数后，一旦事件发生，相应的JavaScript代码就会得到执行。被调用的JavaScript代码可以返回一个值，这个值将被传递给那个事件处理函数。例如，给某个链接添加一个onclick事件处理函数，并让这个处理函数所触发的JavaScript代码返回布尔值true或false。这样，当这个链接被点击时，如果那段JavaScript代码返回值是true，onclick函数就认为“这个链接被点击了”；反之，如果返回值是false，onclick函数就认为“这个链接没有被点击”。
所以onclick函数这样写：
```javascript
onclick="showPic(this);return false;"
```
所以gallery.html最终代码：
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8"/>
  <title>Image Gallery</title>
</head>
<body>
  <h1>Snapshots</h1>
  <ul>
    <li>
    <a href="images/fireworks.jpg" title="a firework display" onclick="showPic(this);return false;">fireworks</a>
    </li>
    <li>
    <a href="images/coffee.png" title="a cup of coffee" onclick="showPic(this);return false;">coffee</a>
    </li>
    <li>
    <a href="images/rose.jpg" title="a red rose" onclick="showPic(this);return false;">rose</a>
    </li>
    <li>
    <a href="images/bigben.jpeg" title="the famouse clock" onclick="showPic(this);return false;">bigben</a>
    </li>
  </ul>
  <img id="placeholder" src="images/placeholder.jpg" alt="my image gallery"/>
  <script type="text/javascript" src="scripts/showPic.js"></script>
</body>
</html>
```
## 扩展
目的：想在图片下面添加描述，并且切换图片时切换不同的文本。为此我们需要用到几个新的DOM属性：
### childNodes属性 
在一棵节点树上，childNodes属性可以用来获取任何一个元素的所有子元素，它是一个包含这个元素全部子元素的**数组**:
```javascript
element.childNodes
```
例如获取某个文档body元素的所有子元素：
```javascript
document.getElementsByTagName("body")[0].childNodes        //每份文档只有一个body元素，所有数组下标为0
```
### nodeType属性
由childNodes属性返回的数组包含所有类型的节点，而不仅仅是元素节点。事实上，文档里几乎每一样东西都是一个节点，甚至连空格和换行符都会被解释为节点，而它们也全都包含在childNodes属性所返回的数组中。
nodeType属性总共有12种取值，但其中仅有三种具有实用价值：
- **元素节点**的nodeType属性值为1。
- **属性节点**的nodeType属性值为2。
- **文本节点**的nodeType属性值为3。

### 在标记里增加一段描述
在gallery.html文件的&lt;img&gt;后面加上：
```html
<p id="description">choose an image</p>
```
在显示图片时，把这个文本节点的值替换成目标图片链接的title值。
### 用JavaScript改变这段描述
```javascript
var text=whichpic.getAttribute("title");
var description=document.getElementById("description");
```
然后是实现文本的切换。
### nodeValue属性
如果想改变一个文本节点的值，那就使用DOM提供的nodeValue属性，它用来得到和设置一个节点的值。
```javascript
node.nodeValue
```
但是有个必须注意的细节：在用nodeValue属性获取description对象的值时，得到的并不是包含在这个段落里的文本。&lt;p&gt;元素本身的nodeValue属性是一个空值，包含在&lt;p&gt;元素里的文本是另一种节点，它是&lt;p&gt;元素的第一个子节点。
### firstChild和lastChild属性
firstChild属性代表childNodes数组里的第一个元素，lastChild属性代表childNodes数组里的最后一个元素。
```javascript
node.firstChild    //等价于node.childNodes[0]
node.lastChild     //等价于node.childNodes[node.childNodes.length-1]
```
### 利用nodeValue属性刷新这段描述
```javascript
var text=whichpic.getAttribute("title");
var description=document.getElementById("description");
description.firstChild.nodeValue=text;
```
如果想让这个图片库更美观，可以给它增加下面给这样的样式表：
```css
body {
	font-family:"Helvetica","Arial",serif;
	color:#333;
	background-color:#ccc;
	margin:1em 10%
	}
h1 {
	color:#333;
	background-color:transparent;
	}
a {
	color:#c60;
	background-color:transparent;
	font-weight:bold;
	text-decoration:none;
	}
ul {
	padding:0;
	}
li {
	float:left;
	padding:1em;
	list-style:none;
	}
img {
	display:block;
	clear:both;
	}
```
## 小结
学会写一个简单的JavaScript图片库
介绍了DOM提供的几个新属性：
- childNodes
- nodeType
- nodeValue
- firstChild
- lastChild

## 最终代码
gallery.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8"/>
  <title>Image Gallery</title>
  <link rel="stylesheet" href="styles/layout.css" media="screen"/>
</head>
<body>
  <h1>Snapshots</h1>
  <ul>
    <li>
    <a href="images/fireworks.jpg" title="a firework display" onclick="showPic(this);return false;">fireworks</a>
    </li>
    <li>
    <a href="images/coffee.png" title="a cup of coffee" onclick="showPic(this);return false;">coffee</a>
    </li>
    <li>
    <a href="images/rose.jpg" title="a red rose" onclick="showPic(this);return false;">rose</a>
    </li>
    <li>
    <a href="images/bigben.jpeg" title="the famouse clock" onclick="showPic(this);return false;">bigben</a>
    </li>
  </ul>
  <img id="placeholder" src="images/placeholder.jpg" alt="my image gallery"/>
  <p id="description">choose an image</p>
  <script type="text/javascript" src="scripts/showPic.js"></script>
</body>
</html>
```
showPic.js
```javascript
function showPic(whichpic){
	var source = whichpic.getAttribute("href");
	var placeholder = document.getElementById("placeholder");
	placeholder.setAttribute("src",source);
	
	var text=whichpic.getAttribute("title");
	var description=document.getElementById("description");
	description.firstChild.nodeValue=text;
	}
```
layout.css
```css
body {
	font-family:"Helvetica","Arial",serif;
	color:#333;
	background-color:#ccc;
	margin:1em 10%
	}
h1 {
	color:#333;
	background-color:transparent;
	}
a {
	color:#c60;
	background-color:transparent;
	font-weight:bold;
	text-decoration:none;
	}
ul {
	padding:0;
	}
li {
	float:left;
	padding:1em;
	list-style:none;
	}
img {
	display:block;
	clear:both;
	}
```
