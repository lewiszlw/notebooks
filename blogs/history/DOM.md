---
title: DOM
date: 2016-10-26 22:35:09
tags: [JavaScript,DOM]
categories: [读书笔记,JavaScriptDOM编程艺术]
---
## 文档：DOM中的“D”
如果没有document（文档），DOM也就无从谈起。当创建了一个网页并把它加载到web浏览器中时，DOM就在幕后悄然而生。它把你编写的网页文档转换为一个文档对象。
## 对象：DOM中的“O”
<!--more-->
JavaScript语言里的对象可以分为三种类型。
- 用户定义对象（user-defined object）:由程序员自行创建的对象。
- 内建对象（native object）：内建在JavaScript语言里的对象。
- 宿主对象（host object）：由浏览器提供的对象。  

## 模型：DOM中的“M”
DOM中的“M”代表着“Model”（模型）。
就像一个模型火车代表一列真正的火车、一张城市街道图代表一个实际存在的城市那样，DOM代表着加载到浏览器窗口的当前页面。
![DOM文档结构图](http://ofolh8dcq.bkt.clouddn.com/DOM-1.jpg)
## 节点
节点（node）这个词是个网络术语，它表示网络中的一个连接点。一个网络就是由一些节点构成的集合。
在DOM里有许多不同类型的节点，如元素节点、文本节点和属性节点。
### 元素节点
标签的名字就是元素的名字。文本段落元素的名字是“p”，无序清单元素的名字是“ul”,列表项元素的名字是“li”。
### 文本节点
在XHTML文档里，文本节点总是被包含在元素节点的内部。但并非所有的元素节点都包含有文本节点。在“购物清单”文档里，&lt;p&gt;元素包含着文本“Don't forget to buy this stuff.”。它是一个文本节点。&lt;ul&gt;元素没有直接包含任何文本节点，它包含着其他的元素节点（一些&lt;li&gt;元素），后者包含着文本节点。
### 属性节点
属性节点用来对元素做出更具体的描述。例如，几乎所有的元素都有一个title属性，而我们可以利用这个属性对包含在元素里的东西做出准确的描述。
### CSS
DOM并不是与网页结构打交道的唯一技术。我们还可以通过CSS（层叠样式表）告诉浏览器应该如何显示一份文档的内容。
继承（inheritance）是CSS技术中的一项强大功能。类似于DOM，CSS也把文档的内容视为一棵节点树。**节点树上的各个元素将继承其父元素的样式属性。**
例如，如果我们为body元素定义了一些颜色或字体，包含在body元素里的所有元素都将自动获得那些样式：
```html
body{
  color:white;
  background-color:black;
}
```
这些颜色将不仅作用于那些直接包含在&lt;body&gt;标签里的内容，还将作用于嵌套在body元素内部的所有元素。
为了把某一个或某几个元素与其他元素区别开来，需要使用class属性或id属性。
**class属性**
你可以在所有的元素上任意应用class属性：
```html
<p class="special">This paragraph has has the special class</p>
<h2 class="special">So does this headline</h2>
```
在样式表里，可以为class属性值相同的所有元素定义同一种样式:
```html
.special{
font-style: italic;
}
```
还可以像下面这样利用class属性为一种特定类型的元素定义一种特定的样式：
```html
h2.special{
text-transform:uppercase;
}
```
**id属性**
id属性的用途是给网页里的某个元素加上一个独一无二的标识符：
```
<ul id="purchases">
```
在样式表里，可以像下面这样为有特定id属性值的元素定义一种独享的样式：
```html
#purchases{
  border:1px solid white;
}
```
尽管id本身只能使用一次，样式表还是可以利用id属性为包含在改特定元素里的其他元素定义样式：
```html
#purchases li{
  font-weight:bold;
}
```
### 获取元素
有三种DOM方法可获取元素节点，分别是通过元素ID、通过标签名字和通过类名字来获取。
**1.getElementById**
它是document对象特有的函数，返回一个与那个有着给定id属性值的元素节点对应的对象。`注意大小写`。
```javascript
document.getElementById(id)
```
getElementById方法只有一个参数：你想获得的那个元素的id属性的值。`这个id值必须放在单引号或双引号里`。
例如：
```javascript
document.getElementById("purchases")
```
**2.getElementsByTagName**
getElementsByTagName方法返回一个对象数组，每个对象分别对应着文档里有着给定标签的一个元素。类似于getElementById，这个方法也是只有一个参数的函数，它的参数是标签名字：
```javascript
element.getElementsByTagName(tag)
```
它与getElementById方法有许多相似之处，但它返回的是一个数组。
例如：
```javascript
document.getElementsByTagName("li")

alert(document.getElementsByTagName("li").length)   //返回数组的长度
```
这个调用将返回一个对象数组，每个对象分别对应着document对象中的一个列表项元素。
有个简单办法可以减少不必要的打字量并改善代码的可读性：**只要把document.getElementsByTagName("li")赋值给一个变量即可。**
getElementsByTagName允许把一个通配符作为它的参数，而这意味着文档里的**每个元素**都将在这个函数所返回的的数组里占有一席之地。
```javascript
alert(document.getElementsByTagName("*").length);   //返回文档里元素节点的总数
```
**3.getElementsByClassName**
getElementsByClassName也只接受一个参数，就是类名：
```javascript
getElementsByClassName(class)
```
这个方法的返回值也与getElementsByTagName类似，都是一个具有相同类名的元素的数组。
例如：
```javascript
document.getElementsByClassName("sale")   //返回一个数组，其中包含类名为“sale”的所有元素
```
使用这个方法还可以查找那些带有多个类名的元素，只需在字符串参数中用空格分隔类名即可：
```javascript
document.getElementsByClassName("important sale")   //同时带有“important”和“sale”类名的元素
```
**4.小结**
简要总结：
- 一份文档就是一棵节点树
- 节点分为不同的类型：元素节点、属性节点和文本节点等。
- getElementById将返回一个对象，该对象对应着文档里的一个特定的元素节点。
- getElementsByTagName和getElementsByClassName将返回一个对象数组，它们分别对应着文档里的一组特定的元素节点。
- 每个节点都是一个对象。  

## 获取和设置属性
getAttribute方法获取属性，setAttribute方法更改属性节点的值。
### getAttribute
```javascript
object.getAttribute(attribute)  //只有一个参数——你打算查询的属性的名字
```
它只能通过元素节点对象调用。例如，可以与getElementsByTagName方法合用，获取每个&lt;p&gt;元素的title属性：
```javascript
var paras = document.getElementsByTagName("p");
for (var i=0;i<paras.length;i++){
	alert(paras[i]).getAttribute("title");
	}
```
### setAttribute
```javascript
object.setAttribute(attribute,value)
```
例如：
```javascript
var shopping = document.getElementById("purchases");
shopping.setAttribute("title","a list of goods");       //设置title属性为a list of goods
```
这里有一个非常值得关注的细节：通过setAttribute对文档做出修改后，在通过浏览器的view source选项查看文档的源代码时看到的仍将是改变前的属性值，也就是说，setAttribute做出的修改不会反映在文档本身的源代码里。这种“表里不一”的现象源自DOM的工作模式：先加载文档的静态内容，在动态刷新，动态刷新不影响文档的静态内容。这正是DOM的真正威力：对页面内容进行刷新却不需要在浏览器里刷新页面。
## 小结
- getElementById
- getElementsByTagName
- getElementsByClassName
- getAttribute
- setAttribute

这五个方法是将要编写的许多DOM脚本的基石。

