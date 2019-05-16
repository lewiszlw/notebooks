---
title: JavaScript语法
date: 2016-10-26 08:03:13
tags: JavaScript
categories: [读书笔记,JavaScriptDOM编程艺术]
---
## 准备工作
编写JavaScript脚本不需要任何特殊的软件，一个普通的文本编辑器和一个Web浏览器就足够了。
用JavaScript编写的代码必须通过HTML/XHTML文档才能执行。有两种方式可以做到这点：
第一种方式是**将JavaScript代码放到文档&lt;head&gt;标签中的&lt;script&gt;标签之间**：
<!--more-->
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Example</title>
  <script>
    JavaScript goes here...
  </script>
</head>
<body>
  Mark-up goes here...
</body>
</html>
```
第二种更好的方式是**把JavaScript代码存为一个扩展名为.js的独立文件**。
典型的做法是在文档的&lt;head&gt;部分放一个&lt;script&gt;标签，并把它的src属性指向该文件：
``` html
<DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8"/>
  <title>Example</title>
  <script src="file.js"></script>
</head>
<body>
  Mark-up goes here...
</body>
```
但最好的做法是把&lt;script&gt;标签放到HTML文档的最后，&lt;/body&gt;标签之前：
``` html
<DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8"/>
  <title>Example</title>
</head>
<body>
  Mark-up goes here...
  <script src="file.js"></script>
</body>
```
这样能使浏览器更快地加载页面。
`注意：前面例子中的<script>标签没有包含传统的type="text/javascript"属性。因为脚本默认是JavaScript，所以没必要指定这个属性。`
程序设计语言分为解释型和编译型两大类。Java或C++等语言需要一个**编译器**（compiler）。编译器是一种程序，能够把用Java等高级语言编写出来的源代码翻译为直接在计算机上执行的文件。
解释型程序设计语言不需要编译器——它们仅需要解释器。对于JavaScript语言，在互联网环境下，web浏览器负责完成有关的解释和执行工作。用编译型语言编写的代码有错误，这些错误在代码编译阶段节能被发现。而解释型语言代码中的错误只能等到解释器执行到有关代码时才能被发现。
## 语法
### 语句
用JavaScript编写的脚本，与其他语言编写的脚本一样，都由一系列指令构成，这些指令叫做语句（statement）。
程序员只需简单地把各条语句放在不同的行上就可以分隔它们，如下所示：
``` javascript
first statement
second statement
```
如果你想把多条语句放在同一行上，就必须像下面这样用分号分隔开它们：
``` javascript
first statement;second statement;
```
**建议在每条语句的末尾都加上一个分号，这是一种良好的编程习惯**：
``` javascript 
first statement;
second statement;
```
这样做让代码更容易阅读。让每条语句独占一行的做法能更容易追踪JavaScript脚本的执行顺序。
### 注释
有多种方式可以在JavaScript脚本中插入注释。
例如，如果用两个斜线作为一行的开始，这一行就会被当成一条注释。如果使用这种注释方式，就必须在每个注释行的开头加上两个斜线：
``` JavaScript
//自我提醒：有注释是好事
//多条注释
```
如果打算注释很多行，你可以用下面这种注释方法：
``` JavaScript
/*自我提醒
*有注释是好事
*/
```
这种注释方式在需要插入大段注释时很有用，它可以提高整个脚本的可读性。
### 变量
JavaScript允许程序员直接对变量赋值而无需事先声明（declare）。但提前声明变量是一种良好的编程习惯。下面的语句对变量mood和age做出了声明：
``` javascript
var mood;
var age;
```
不必单独声明每个变量，你也可以用一条语句一次声明多个变量：
``` javascript
var mood,age;
```
你甚至可以一石二鸟，把声明变量和对该变量赋值一次完成：
``` javascript
var mood="happy";
var age="23";
```
甚至可以像下面这样：
``` javascript
var mood="happy",age="23";
```
**在JavaScript语言里，变量和其他语法元素的名字都是区分大小写的。**名字是mood的变量与名字是Mood、MOOD或mOOd的变量没有任何关系，它们不是同一个变量。
JavaScript变量名允许包含`字母、数字、美元符号和下划线（但第一个字符不允许是数字）`。
为了让比较长的变量名更容易阅读，可以在变量名中适当的位置插入下划线：
``` javascript
var my_mood="happy";
```
另一种方式是使用驼峰格式（camel case）:
``` javascript
var myMood="happy";
```
通常驼峰格式是函数名、方法名和对象属性名命名的首选格式。
### 数据类型
有些语言要求在声明变量的同时还必须声明变量的数据类型，这种做法称为**类型声明**（typing）。必须明确类型声明的语言称为**强类型**（strongly typed）语言。JavaScript不需要进行类型声明，因此它是一种**弱类型**（weakly typed）语言。这意味着程序员可以在任何阶段改变变量的数据类型。
以下语句在强类型语言中是非法的，但在JavaScript里却完全没有问题：
```javascript
var age = "thirty three";
age = 33;
```
#### 字符串
字符串有**零个或多个**字符构成。字符包括（但不限于）字母、数字、标点符号和空格。**字符串必须包在引号里，单引号或双引号都可以**。下面这两条语句含义完全相同：
``` javascript
var mood = "happy";
var mood = 'happy';
```
你可以随意选用引号，但最好根据字符串所包含的字符来选择。**如果字符串中包含双引号，就把整个字符串放在单引号里；如果字符串中包含单引号，就把整个字符串放在双引号里**：
``` javascript
var mood = "don't ask";
```
如果想在上面这条语句中使用单引号，就必须保证字母”n“和”t“之间的单引号能被当成这个字符串的一部分。这种情况下这个单引号需要被看做一个普通字符，而不是这个字符串的结束标志。这种情况我们需要对这个字符进行**转义**（escaping）。在JavaScript里用反斜线对字符进行转义：
``` javascript
var mood = 'don\'t ask';
```
类似地
``` javascript
var mood = "about 5'10\" tall";
```
**作为一个好的编程习惯，不管选择用双引号还是单引号，请在整个脚本中保持一致。**
#### 数值
JavaScript允许使用带小数点的数值，并且允许任意位小数，这样的数称为**浮点数**（floating-point number）：
``` javascript
var age = 33.25;
```
也可以使用负数：
``` javascript
var temperature = -20;
```
也支持负浮点数：
``` javascript
var temperature = -20.333333;
```
#### 布尔值
另一种重要的数据类型是**布尔**（boolean）类型。
布尔数据只有两个可选值——ture或false。布尔值不是字符串，`千万不要把布尔值用引号括起来`。布尔值false与字符串值“false”是两码事！
``` javascript
var married=true;   //把变量married设置为布尔值true
var married="true";   //把变量married设置为字符串“true”
```
### 数组
在JavaScript中，数组可以用**关键字Array**声明。声明数组的同时还可以指定数组初始元素个数，也就是这个数组的长度（length）：
``` javascript
var beatles = Array(4);
```
有时我们无法预知某个数组有多少个元素。我们可以在声明数组是不给出元素个数：
``` javascript
var beatles = Array();
```
向数组中添加元素的操作称为填充（populating）。在填充数组时，需要给出新元素的值和新元素在数组中的位置（即下标index）。数组里一个元素一个下标，下标必须用方括号括起来：
``` javascript
array[index] = element;
```
上面填充数组有些麻烦。有一种相对简单的方式，在声明数组的同时对它进行填充：
``` javascript
var beatles = Array("John","Paul","George","Ringo");
```
上面这条语句会为每个元素自动分配一个下标：第一个下标是0，第二个是1，以此类推。
我们甚至用不着明确地表明我们是在创建数组。事实上，只需用一对方括号把各个元素的初始值括起来就可以了：
``` javascript
var beatles=["John","Paul","George","Ringo"];
var years=[1940,1941,1942,1943];
var lennon=["John",1940,false];
```
数组元素还可以是变量：
``` javascript
var name="John";
beatles[0]=name;
```
数组元素的值还可以是另一个数组的元素：
``` javascript
var names=["John","Paul","George","Ringo"];
beatles[1]=names[3];
```
数组还可以包含其他的数组：
``` javascript
var lennon=["John",1940,false];
var beatles=[];
beatles[0]=lennon;
```
### 对象
与数组类似，对象也是使用一个名字表示一组值，对象的每个值都是对象的一个属性。创建对象使用**Object关键字**，使用点号来获取属性。
``` javascript
var lennon = Object();
lennon.name = "John";
lennon.year = 1940;
lennon.living = false;
```
创建对象还有一种更简洁的语法，即花括号语法：
``` javascript
{propertyName:value,propertyName:value}
```
例如：
``` javascript
var lennon = {name:"John",year:1940,living:false};
```
用对象来代替传统数组的做法意味着可以通过元素的名字而不是下标数字来引用它们。这大大提高了脚本的可读性。
``` javascript
var beatles=Array();
beatles[0]=lennon;
```
我们可以使用beatles[0].name得到值“John”。
进一步改进：
``` javascript
var beatles={};
beatles.vocalist=lennon;
```
现在，beatles.vocalist.name的值是“John”，beatles.vocalist.year的值是1940，beatles.vocalist.living的值是false。
## 操作
加号（+）是一个比较特殊的操作符，它既可以用于数值，也可以用于字符串。
``` javascript
var message = "I am feeling" + "happy";  //两个字符串

var mood="happy";
var message="I am feeling"+mood;      //字符串和变量

var year = 2005;
var message = "The year is "+year;    //字符串和数值
```
因为JavaScript是一种弱类型语言，所以字符串和数值拼接是允许的。此时，`数值将被自动转换为字符串`。
``` javascript
alert("10"+24);    //返回字符串“1024”
alert(10+24);      //返回数值34
```
另一个有用的快捷操作符是+=，它可以一次完成“加法和赋值”（或“拼接和赋值”）操作。
## 条件语句
最常见的条件语句是if语句：
``` javascript
if(condition){
  statements;
}
```
事实上，if语句中的花括号本身并不是必不可少的。如果if语句中的花括号部分只包含着一条语句的话，那就可以不使用花括号，而且这条if语句的全部内容可以写在同一行上：
```javascript
if(1>2) alert("The world has gone mad!");
```
if语句可以有一个else子句。包含在else子句中的语句会在给定条件为假时执行：
```javascript
if(1>2){
  alert("The world has gone mad!");
}else{
 alert("All is well with the world");
 }
```
### 比较操作符
有：>、<、>=、<=、==、!=
`=用于完成赋值操作，==用于比较操作`
相等操作符==并不表示严格相等，例如，比较false与一个空字符串：
```javascript
var a=false;
var b="";
if(a==b){
  alert("a equals b");   //条件语句的求值结果是true
}
```
要进行严格比较，就要使用另一种等号（===）。这个全等操作符会进行严格的比较，**不仅比较值，而且会比较变量的类型**。
**当然对于不等操作符!=也是如此。如果相比较严格不相等，就要使用!==**。
### 逻辑操作符
有：&&、||、!
逻辑操作符的**操作对象是布尔值**。返回一个布尔值。
`“逻辑非”（!）只能作用于单个逻辑操作数，其结果是把那个逻辑操作数所返回的布尔值取反。`
## 循环语句
### while循环
```javascript
while(condition){  //true则继续执行
  statements;
}
```
while循环和if语句唯一的区别是：只要给定条件的求值结果是true，包含在花括号里的代码就将反复地执行下去。
**do...while循环**
```javascript
do{
  statements;
}while(condition);  //true则继续循环
```
特点是花括号里的语句至少会被执行一次。
### for循环
```javascript
for(initial condition;test condition;alter condition){
  statements;
}
```
for循环最常见的用途之一是对某个数组里的全体元素进行遍历处理。这往往需要用到数组的array.length属性。
## 函数
如果需要多次使用同一段代码，可以把它们封装成一个函数（function）。函数就是一组允许在你的代码里随时调用的语句。事实上，每个函数实际上是一个短小的脚本。
你可以把不同的数据传递给它们，而它们将使用这些数据去完成预定的操作。我们把传递给函数的数据称为参数（argument）。
在定义函数时，你可以为它声明任意多个参数，只要用逗号把它们分隔开来就行。在函数内部，你可以像使用普通变量那样使用它的任何一个参数。
示例：
```javascript
function multiply(num1,num2){
  var total=num1*num2;
  alert(total);
}

//调用
multiply(10,2);
```
函数不仅能够（以参数的形式）接收数据，还能够返回数据。利用return语句可以让它返回一个数值、一个字符串、一个数组或一个布尔值。
示例：
```javascript
function multiply(num1,num2){
 var total=num1*num2;
 return total;
}
```
我们还可以把函数当做一种数据类型来使用，这意味着可以把一个函数的调用结果赋值给一个变量。
**变量的作用域**
全局变量（global variable）可以在脚本中的任何位置被引用。全局变量的作用域是整个脚本。
局部变量（local variable）只存在于声明它的那个函数的内部。局部变量的作用域仅限于某个特定的函数。
如果在某个函数中使用了var，那个变量就将被视为一个局部变量，它只存在于这个函数的上下文中；反之，如果没有使用var，那个变量就将被视为一个全局变量。**如果脚本里已经存在一个与之同名的全局变量，这个函数就会改变那个全局变量的值**。
## 对象
对象是自包含的数据集合，包含在对象里的数据可以通过两种形式访问——属性（property）和方法（method）：
- 属性是隶属于某个特定对象的变量
- 方法是只有某个特定对象才能调用的函数  

对象就是由一些属性和方法组合在一起而构成的一个数据实体。
调用方法：
```javascript
Object.property
Object.method()
```
给对象创建实例需要使用new关键字，如下：
```javascript
var john = new Person;    //创建出Person对象的一个新实例john
```
### 内建对象
数组就是内建对象中的一种。当我们使用new关键字去初始化一个数组时，其实是在创建一个Array对象的新实例：
```javascript
var beatles = new Array();
```
其他内建对象例子有Math对象和Date对象。
### 宿主对象
除了内建对象，还可以在JavaScript脚本里使用一些已经预先定义好的其他对象。这些对象不是由JavaScript语言本身而是由它的运行环境提供的。具体到web应用，这个环境就是浏览器。**由浏览器提供的预定义对象被称为宿主对象（host object）**。
宿主对象包括Form、Image、Element和document对象等。
