---
title: 用JavaScript实现动画效果
date: 2016-11-13 20:11:59
tags: JavaScript
categories: [读书笔记,JavaScriptDOM编程艺术]
---
# 动画基础知识
如果我们想随着时间的变化而不断改变某个元素的样式，则只能使用JavaScript。JavaScript能够按照预定的时间间隔重复调用一个函数，而这意味着我们可以随着时间的推移而不断改变某个元素的样式。动画是样式随时间变化的完美例子之一。简单地说，动画就是让元素的位置随时间而不断地发生变化。
<!--more-->
## 位置
网页元素在浏览器窗口里的位置是一种表示性的信息。因此，位置信息通常是由CSS负责设置的。
```css
element{
	position: absolute;
	top: 50px;
	left: 100px;
}
```
这将把element元素摆放到距离浏览器窗口的左边界100像素，距离浏览器窗口的上边界50像素的位置上。
position属性的合法值有：
- static：默认值。意思是有关元素按照它们在标记里出现的先后顺序出现在浏览器窗口里；
- fixed：相对于视窗来定位，意味着即便页面滚动，它还是会停留在相同位置；
- relative：与static类似，不过它还可以（通过应用float属性）从文档的正常显示顺序里脱离出来；
- absolute：与fixed类似，不过它是相对于父级position元素定位。

设置一个元素的top、bottom、left和right属性将把该元素摆放到距文档顶、底、左和右边界特定距离的位置。**为防止它们发生冲突，最好只使用top或只使用bottom属性，left和right属性同理。**
假设文档有一个这样的元素：
```html
<p id="message">Whee!</p>
```
用JavaScript函数来设置这个元素的位置：
```javascript
function positionMessage(){ 
		if(!document.getElementById) return false;
		if(!document.getElementById("message")) return false;
		var elem=document.getElementById("message");
		elem.style.position="absolute";
		elem.style.left="50px";
		elem.style.top="100px";
	}
```
改变这个元素的位置函数：
```javascript
function moveMessage(){
		if(!document.getElementById) return false;
		if(!document.getElementById("message")) return false;
		var elem=document.getElementById("message");
		elem.style.left="200px";
	}
```
## 时间
JavaScript函数setTimeout能够让某个函数在经过一段预定的时间之后才开始执行。这个函数有两个参数：
- 第一个参数通常是一个字符串，其内容是将要执行的那个函数的名字；
- 第二个参数是一个数值，它以**毫秒**为单位设定了需要经过多长时间后才开始执行第一个参数所给的函数。

```javascript
setTimeout("function",interval)
```
如果想取消某个正在排队等待执行的函数，就必须事先把setTimeout函数的返回值赋值给一个变量（variable=setTimeout("function",interval)），然后用clearTimeout的函数来取消“等待执行”队列里的某个函数：
```javascript
clearTimeout(variable)
```
修改positionMessage函数，让它在5秒之后再去调用moveMessage函数：
```javascript
function positionMessage(){
		if(!document.getElementById) return false;
		if(!document.getElementById("message")) return false;
		var elem=document.getElementById("message");
		elem.style.position="absolute";
		elem.style.left="50px";
		elem.style.top="100px";
		movement=setTimeout("moveMessage",5000);
	}
```
movement变量对应着在positionMessage函数里定义的setTimeout调用。它是一个全局变量，我在声明它时未使用关键字var。这意味着那个“跳跃”行为可以在positionMessage函数以外的地方被取消。
## 时间递增量
真正的动画效果应该是一个渐变的过程。更新moveMessage函数：
1.获得元素的当前位置；
2.如果元素已经到达它的目的地，则退出这个函数；
3.如果元素尚未到达它的目的地，则把它向目的地移近一点儿；
4.经过一段时间间隔之后从步骤1开始重复上述步骤。

```javascript
function moveMessage(){
		if(!document.getElementById) return false;
		if(!document.getElementById("message")) return false;
		var elem=document.getElementById("message");
		xpos=parseInt(elem.style.left);
		ypos=parseInt(elem.style.top);
		if(xpos==200&&ypos==100){
				return true;
			}
		if(xpos<200){
				xpos++;
			}
		if(xpos>200){
				xpos--;
			}
		if(ypos<100){
				ypos++;
			}
		if(ypos>100){
				ypos--;
			}
		elem.style.left=xpos+"px";
		elem.style.top=ypos+"px";
		movement=setTimeout("moveMessage()",10);
	}
```
## 抽象
### 创建moveElement函数
参数：
- 打算移动的元素ID：elementID;
- 该元素的目的地的“左”位置：final_x；
- 该元素的目的地的“上”位置：final_y；
- 两次移动之间的停顿时间：interval。

```javascript
function moveElement(elementID,final_x,final_y,interval){
		if(!document.getElementById) return false;
		if(!document.getElementById(elementID)) return false;
		var elem=document.getElementById(elementID);
		var xpos=parseInt(elem.style.left);
		var ypos=parseInt(elem.style.top);
		if(xpos==final_x && ypos==final_y){
				return true;
			}
		if(xpos<final_x){
				xpos++;
			}
		if(xpos>final_x){
				xpos--;
			}
		if(ypos<final_y){
				ypos++;
			}
		if(ypos>final_y){
				ypos--;
			}
		elem.style.left=xpos+"px";
		elem.style.top=ypos+"px";
		movement=setTimeout("moveElement('"+elementID+"',"+final_x+","+final_y+","+interval+")",interval);  //注意每次还要传递参数	
	}
```
# 实用的动画
W3C在它们的Web Content Accessibility Guidelines 7.2节里给出这样的建议：“除非浏览器允许用户‘冻结’移动着的内容，否则就应该避免让内容在页面中移动。”
## 提出问题
一个包含一系列链接的网页，当用户把鼠标指针悬停在其中的某个链接上时，告诉用户这个链接将把他们带往何方，可以展示一张预览图片。下面是基本文档代码清单：
```html
<!DOCTYPE html>
<html lang="en">
<head>
        <meta charset="UTF-8">
        <title>Web Design</title>
</head>
<body>
		<h1>Web Design</h1>
		<p>These are the things you should konw.</p>
		<ol id="linklist">
		        <li><a href="structure.html">Structure</a></li>
		        <li><a href="presentation.html">Presentation</a></li>
		        <li><a href="behavior.html">Behavior</a></li>
		</ol>
</body>
</html>
```
## 解决问题
如果为每个链接分别准备一张预览图片，在切换显示这些图片时总会有一些延迟。除此之外，简单地切换显示这些图片也不是我们期望的效果。下面是要做的事：
- 为所有的预览图片生成一张“集体照”形式的图片；
- 隐藏这张“集体照”图片的绝大部分；
- 当用户把鼠标指针悬停在某个链接的上方时，只显示这张“集体照”图片的相应部分。

## CSS
CSS的overflow属性用来处理一个元素的尺寸超出其容器尺寸的情况。overflow属性的可取值有4种：visible、hidden、scroll和auto：
- visible：不裁剪溢出的内容。浏览器将把溢出的内容呈现在其容器元素的显示区域以外，全部内容可见。
- hidden：隐藏溢出的内容。内容只显示在其容器的显示区域里，这意味着只有一部分内容可见。
- scroll：类似于hidden，浏览器将对溢出的内容进行隐藏，但显示一个滚动条以便让用户能够滚动看到内容的其他部分。
- auto：类似于scroll，但浏览器只在确实发生溢出时才显示滚动条。如果内容没有溢出，就不显示滚动条。

首先，需要把制作的400\*100像素的图片topics.gif放到一个容器元素。选择将其放入一个div元素，并把这个div元素的id属性值设置为slideshow。然后创建样式表文件layout.css，把它放入到styles文件夹。并将id为slideshow的div元素的尺寸设置为100\*100像素，position值设置为relative。因为让子图片使用绝对位置，通过使用relative，子元素的（0，0）坐标将固定在slideshow div的左上角。
```css
#slideshow{
	width:100px;
	height:100px;
	position:relative;
	overflow:hidden;        //设置为hidden
}
```
## JavaScript
利用moveElement函数来移动topics.gif图片，根据用户正把鼠标指针悬停在哪个链接上，我们将这个图片向左或向右移动。
```javascript
function prepareSlideshow(){
		if(!document.getElementById) return false;
		if(!document.getElementsByTagName) return false;
		if(!document.getElementById("linklist")) return false;
		if(!document.getElementById("preview")) return false;
		var preview=document.getElementById("preview");
		preview.style.position="absolute";
		preview.style.left="0px";
		preview.style.top="0px";
		var list=document.getElementById("linklist");
		var links=document.getElementsByTagName("a");
		links[0].onmouseover=function(){
						moveElement("preview",-100,0,10);
				}
		links[1].onmouseover=function(){
						moveElement("preview",-200,0,10);
				}
		links[2].onmouseover=function(){
						moveElement("preview",-300,0,10);
				}
	}
```
这里会出现一个问题：当我们把鼠标指针在链接之间快速地来回移动，动画效果将变得混乱。
## 变量作用域问题
动画效果不正确的问题是由一个全局变量引起的。在把moveMessage函数抽象化为moveElement函数的过程中，我们未对变量movement做任何修改，这留下了一个隐患：每当用户把鼠标指针悬停在某个链接上，不管上一次调用是否已经把图片移动到位，moveElement函数都会被再次调用并试图把这个图片移动到另一个地方去。于是，当用户在链接之间快速移动鼠标时，movement变量就会像一条拔河绳那样来回变化，而moveElement函数就会试图把图片同时移动到两个不同的地方去。如果用户移动鼠标的速度够快，积累在setTimeout队列里的事件就会导致动画产生滞后。为了消除动画滞后现象，可以用clearTimeout函数清除积累在setTimeout队列里的事件。
全局变量不可以，但用局部变量，clearTimeout函数调用语句将无法工作，因为局部变量movement在clearTimeout函数的上下文里不存在。我们需要一种介乎它们二者之间的东西，需要一个**只与被移动的那个元素有关的变量**。事实上，就是我们一直在用的“属性”。JavaScript允许我们为元素创建属性：
```javascript
element.property=value
```
与创建变量的区别是这个变量专属于某个特定的元素。
重写moveElement函数：
```javascript
function moveElement(elementID,final_x,final_y,interval){
		if(!document.getElementById) return false;
		if(!document.getElementById(elementID)) return false;
		var elem=document.getElementById(elementID);

		if(elem.movement){
				clearTimeout(elem.movement);
			}

		var xpos=parseInt(elem.style.left);
		var ypos=parseInt(elem.style.top);
		if(xpos==final_x && ypos==final_y){
				return true;
			}
		if(xpos<final_x){
				xpos++;
			}
		if(xpos>final_x){
				xpos--;
			}
		if(ypos<final_y){
				ypos++;
			}
		if(ypos>final_y){
				ypos--;
			}
		elem.style.left=xpos+"px";
		elem.style.top=ypos+"px";
		elem.movement=setTimeout("moveElement('"+elementID+"',"+final_x+","+final_y+","+interval+")",interval); 
	}     //为每个元素创建movement属性

```
## 改进动画效果
moveElement函数每次移动都是一个像素的距离，移动速度有些慢。改进为：如果那个元素与它的目的地距离较远，就让它每次前进一大步；如果那个元素与它的目的地距离较近，就让它每次前进一小步。也可以说让它每次移动十分之一。
```javascript
function moveElement(elementID,final_x,final_y,interval){
		if(!document.getElementById) return false;
		if(!document.getElementById(elementID)) return false;
		var elem=document.getElementById(elementID);
		if(elem.movement){
				clearTimeout(elem.movement);
			}
		if(!elem.style.left){
				elem.style.left="0px";
			}
		if(!elem.style.top){
				elem.style.top="0px";
			}
		var xpos=parseInt(elem.style.left);
		var ypos=parseInt(elem.style.top);
		var dist=0;
		if(xpos==final_x && ypos==final_y){
				return true;
			}
		if(xpos<final_x){
				dist=Math.ceil((final_x-xpos)/10);
				xpos=xpos+dist;
			}
		if(xpos>final_x){
				dist=Math.ceil((xpos-final_x)/10);
				xpos=xpos-dist;
			}
		if(ypos<final_y){
				dist=Math.ceil((final_y-ypos)/10);
				ypos=ypos+dist;
			}
		if(ypos>final_y){
				dist=Math.ceil((ypos-final_y)/10);
				ypos=ypos-dist;
			}
		elem.style.left=xpos+"px";
		elem.style.top=ypos+"px";
		movement=setTimeout("moveElement('"+elementID+"',"+final_x+","+final_y+","+interval+")",interval);
	}
```
Math.ceil(number)向上取整，防止dist小于1，因为无法做到将一个元素移动不到一个像素的距离。
## 添加安全检查
检查elem元素的left和/或top属性是否已设置：
```javascript
if(!elem.style.left){
		elem.style.left="0px";
	}
if(!elem.style.top){
		elem.style.top="0px";
	}
```
## 生成HTML标记
list.html文档里包含一些只是为了能够用JavaScript代码实现动画效果而存在的标记：
```html
<div id="slideshow">
  <img src="images/topics.gif" alt="building blocks of web design" id="preview" />
</div>
```
如果没有启用JavaScript支持，以上内容就是多余的了。既然如此，不如用JavaScript代码来生成它们。
