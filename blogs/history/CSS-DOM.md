---
title: CSS-DOM
date: 2016-11-10 20:27:53
tags: [JavaScript,CSS,DOM]
categories: [读书笔记,JavaScriptDOM编程艺术]
---
# 三位一体的网页
我们在浏览器看到的网页其实是由以下三层信息构成的一个共同体：
- 结构层
- 表示层
- 行为层

<!--more-->
## 结构层
网页的结构层（structural layer）由HTML或XHTML之类的标记负责创建的。标签（tag）对网页内容的语义含义做出了描述，例如&lt;p&gt;标签表达了这样一种语义：“这是一个文本段”。但这些标签并不包含任何关于内容如何显示的信息。
## 表示层
表示层（presentation layer）由CSS负责完成。CSS描述页面内容应该如何呈现。
## 行为层
行为层（behavior layer）负责内容应该如何响应事件这一问题。这是JavaScript语言和DOM主宰的领域。

网页的表示层和行为层总是存在的，即使未明确地给出任何具体的指令也是如此。此时，web浏览器将应用它的默认样式和默认事件处理函数。

## 分离
- 使用（X）HTML去搭建文档的结构；
- 使用CSS去设置文档的呈现效果；
- 使用DOM脚本去实现文档的行为。

不过在这三种技术之间存在着一些潜在的重叠区域。
# style属性
文档的每个元素节点还有一个属性style。style属性包含着元素的样式，查询这个属性将**返回一个对象而不是一个简单的字符串**。也就是说，不仅文档里每个元素是一个对象，每个元素都有一个style属性，它们也是一个对象。
## 获取样式
例如为了查出某个元素在浏览器里的显示颜色，我们需要使用style对象的color属性：
```javascript
element.style.color
```
但有些属性的获取方式略有不同，例如font-family，你不能简单的查询style对象的font-family，因为“font”和“family”之间的连字符与JavaScript语言中的减法操作符相同，JavaScript会把它解释为减号。当你需要引用一个中间带减号的CSS属性时，DOM要求你用驼峰命名法。CSS属性font-family变成fontFamily：
```javascript
element.style.fontFamily
```
其他：CSS属性background-color——DOM属性backgroundColor，CSS属性magin-top-width——DOM属性marginTopWidth，以此类推。而且绝大多数样式属性的返回值与它们的设置值都采用同样的计量单位。
通过style属性获取样式有很大的局限性，**style属性只能返回内嵌样式**。换句话说，只有把CSS style属性插入到标记里，才可以用DOM style属性去查询那些信息。DOM style属性无法查询引用的外部CSS文件和放在&lt;head&gt;部分的&lt;style&gt;标签里的样式。
## 设置样式
style对象的各个属性是可读写的，可以通过赋值操作来更新样式：
```javascript
element.style.property=value   //style对象的属性值是一个字符串，所以赋值时要加上引号
```
# 何时该用DOM脚本设置样式
## 根据元素在节点树里的位置来设置样式
通过CSS声明样式的具体做法主要有三种：
第一种是为标签元素统一地声明样式
```css
p{
	font-size: 1em;
}
```
第二种是为有特定class属性的所有元素统一声明样式
```css
.fineprint{
	font-size: 8em;
}
```
第三种是为有独一无二id属性的元素单独声明样式
```css
#intro{
	font-size: 1.2em;
}
```
也可以为有类似属性的多个元素声明样式
```css
input[type*="text"]{
	font-size: 1.2em;
}
```
在现代浏览器中，甚至可以根据元素的位置声明样式
```css
p:first-of-type{
	font-size: 2em;
}
```
现在，CSS还无法根据元素之间的相对位置关系找出某个特定的元素，但这对DOM来说却是轻而易举。例如我们利用DOM找出文档中紧跟在每个h1元素后面的那个元素，并把样式添加给它：
由于nextSibling属性是查找下一个节点，而这里我们需要查找下一个元素节点，所以编写getNextElement函数来实现：
```javascript
function getNextElement(node){
		if(node.nodeType==1){
				return node;
			}
		if(node.nextSibling){
				return getNextElement(node.nextSibling);
			}
		return null;
	}
```
所以代码为：
```javascript
function styleHeaderSiblings(){
		if(!document.getElementsByTagName) return false;
		var headers=document.getElementsByTagName("h1");
		var elem;
		for(var i=0;i<headers.length;i++){
				elem=getNextElement(headers[i].nextSibling);
				elem.style.fontWeight="bold";
				elem.style.fontSize="1.2em";
			}
	}
function getNextElement(node){
		if(node.nodeType==1){
				return node;
			}
		if(node.nextSibling){
				return getNextElement(node.nextSibling);
			}
		return null;
	}
addLoadEvent(styleHeaderSiblings);
```
## 根据某种条件反复设置某种样式
编写一个表格：
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Cities</title>
</head>
<body>
  <table>
    <caption>Itinerary</caption>
	<thead>
	  <tr>
	    <th>WHEN</th>
	    <th>WHERE</th>
	  </tr>
	</thead>
	<tbody>
	  <tr>
	    <td>June 9th</td>
	    <td>Portland,<abbr title="Oregon">OR</abbr></td>
	  </tr>
	  <tr>
	    <td>June 10th</td>
	    <td>Seattle,<abbr title="Washington">WA</abbr></td>
	  </tr>
	  <tr>
	    <td>June 11th</td>
	    <td>Sacramento,<abbr title="California">CA</abbr></td>
	  </tr>
	</tbody>
  </table>
</body>
</html>
```
给其加上样式：
```css
body{
	font-family: "Helvetica","Arial",sans-serif;
	background-color: #fff;
	color: #000;
}
table{
	margin: auto;
	border: 1px solid #699;
}
caption{
	margin: auto;
	padding: .2em;
	font-size: 1.2em;
	font-weight: bold;
}
th{
	font-weight: normal;
	font-style: italic;
	text-align: left;
	border: 1px dotted #699;
	background-color: #9cc;
	color: #000;
}
th,td{
	width: 10em;
	padding: .5em;
}
```
效果如下：
![CSS-DOM1](http://ofolh8dcq.bkt.clouddn.com/CSS-DOM1.PNG)
让表格里的行更可读的常用技巧是交替改变它们的背景色，从而形成斑马线效果，使相邻的两行泾渭分明。
若浏览器支持CSS3，那就只需要如下两行样式：
```css
tr:nth-child(odd) {background-color: #ffc;}
tr:nth-child(even) {background-color: #fff;}
```
如果nth-child()不可用，表格较小可以为每个奇数行（或每个偶数行）设置一个class属性即可。
如果nth-child()不可用，且表格很大的时候，可以编写一个函数来为表格添加斑马线，步骤：
1.把文档里的所有table元素找出来。
2.对每个table元素，创建odd变量并把它初始化为false。
3.遍历这个表格里的所有数据行。
4.如果变量odd的值是true，设置样式并把odd变量修改为false。
5.如果变量odd的值是false，不设置样式，但把odd变量修改为true。

```javascript
function stripeTables(){
		if(!document.getElementsByTagName) return false;
		var tables=document.getElementsByTagName("table");
		var odd,rows;
		for(var i=0;i<tables.length;i++){
				var odd=false;
				rows=tables[i].getElementsByTagName("td");
				for(var j=0;j<rows.length;j++){
						if(odd==true){
								rows[j].style.backgroundColor="#aaa";
								odd=false;
							}else{
									odd=true;
								}
					}
			}
	}
```
效果预览：
![CSS-DOM2](http://ofolh8dcq.bkt.clouddn.com/CSS-DOM2.PNG)
## 响应事件
CSS提供的:hover等伪class属性允许我们根据HTML元素的状态来改变样式。DOM也可以通过onmouseover等事件对HTML元素的状态变化做出响应。例如，想让某行鼠标指针悬停其上时其文本变为粗体：
CSS：
```css
tr:hover{
		font-weight: bold;
	}
```
DOM：
```javascript
function highlightRows(){
		if(!document.getElementsByTagName) return false;
		var rows=document.getElementsByTagName("tr");
		for(var i=0;i<rows.length;i++){
				rows[i].onmouseover=function(){
						rows[i].style.fontWeight="bold";  //错误，rows[i]改为this
					}
				rows[i].onmouseout=function(){
						rows[i].style.fontWeight="normal";  //错误，同理
					}
			}
	}
```
在这一类场合，需要决定采用纯粹的CSS来解决，还是利用DOM来设置样式，你需要考虑一下因素：
- 这个问题最简单的解决方案是什么；
- 哪种解决方案会得到更多浏览器的支持。

# className属性
与其使用DOM直接改变某个元素的样式，不如通过JavaScript代码去更新这个元素的class属性，这样还可以让表示层和行为层分离得更加彻底。可以通过setAttribute方法来做这件事。更简单的方法是更新className属性。className属性是一个可读/可写的属性，凡是元素节点都有这个属性。
你可以用className属性得到一个元素的class属性：
```javascript
element.className
```
也可以用className属性和赋值操作符设置一个元素的class属性：
```javascript
element.className=value;
```
注意：通过className属性设置某个元素的class属性时将替换（而不是追加）该元素原有的class属性设置。不过你也可以利用字符串拼接操作实现追加。
## 对函数进行抽象
把一个具体的东西改进为一个较为通用的东西的过程叫做抽象（abstraction）。
例如styleHeaderSiblings函数，它仅适用于h1元素，你可以做如下改动：
```javascript
function styleHeaderSiblings(tag){
		if(!document.getElementsByTagName) return false;
		var elems=document.getElementsByTagName(tag);  //增加可读性，将headers改为elems
		var elem;
		for(var i=0;i<elems.length;i++){
				elem=getNextElement(elems[i].nextSibling);
				elem.style.fontWeight="bold";
				elem.style.fontSize="1.2em";
			}
	}
addLoadEvent(function()={styleHeaderSiblings("h1");});
```
