---
title: CSS浮动布局
date: 2016-12-18 21:27:00
tags: CSS
categories: Front-End
---
# CSS浮动
浮动的框可以向左或向右移动，直到它的外边缘碰到**包含框**或另一个**浮动框**的边框位置。由于浮动框不在文档的普通流中，所以文档的普通流中的块框表现得就像浮动框不存在一样。
<!--more-->
当把框1向右浮动时，它脱离文档并且向右移动，直到它的右边缘碰到包含框的右边缘：
![CSS浮动布局1](http://ofolh8dcq.bkt.clouddn.com/CSS%E6%B5%AE%E5%8A%A8%E5%B8%83%E5%B1%801.PNG)

当框1向左浮动时，它脱离文档流并且向左移动，直到它的边缘碰到包含框的左边缘。因为它不再处于文档流中，所以它不占据空间，实际上覆盖住了框2，使框2从视图中消失。如果把所有三个框都像左移，那么框1向左浮动直到碰到包含框，另外两个框向左浮动直到碰到前一个浮动框。
![CSS浮动布局2](http://ofolh8dcq.bkt.clouddn.com/CSS%E6%B5%AE%E5%8A%A8%E5%B8%83%E5%B1%802.PNG)
如果包含框太窄，无法容纳水平排列的三个浮动元素，那么其他浮动块向下移动，直到有足够的空间。如果浮动元素的高度不同，那么当它们向下移动时可能被其他浮动元素“卡住”。
![CSS浮动布局3](http://ofolh8dcq.bkt.clouddn.com/CSS%E6%B5%AE%E5%8A%A8%E5%B8%83%E5%B1%803.PNG)

# 行框和清理
浮动框旁边的行框被缩短，从而给浮动框留出空间，行框围绕浮动框。因此，创建浮动框可以使文本围绕图像。
![CSS浮动布局4](http://ofolh8dcq.bkt.clouddn.com/CSS%E6%B5%AE%E5%8A%A8%E5%B8%83%E5%B1%804.PNG)

要想阻止行框围绕浮动框，需要对该框应用 clear 属性。clear 属性的值可以是 left、right、both 或 none，它表示框的哪些边不应该挨着浮动框。
为了实现这种效果，在被清理的元素的**上外边距**上添加足够的空间，使元素的顶边缘垂直下降到浮动框下面：
![CSS浮动布局5](http://ofolh8dcq.bkt.clouddn.com/CSS%E6%B5%AE%E5%8A%A8%E5%B8%83%E5%B1%805.PNG)
这是一个有用的工具，它让周围的元素为浮动元素留出空间。

示例：
![CSS浮动布局6](http://ofolh8dcq.bkt.clouddn.com/CSS%E6%B5%AE%E5%8A%A8%E5%B8%83%E5%B1%806.PNG)
# CSS float属性
float 属性定义元素在哪个方向浮动。以往这个属性总应用于图像，使文本围绕在图像周围，不过在 CSS 中，任何元素都可以浮动。浮动元素会生成一个块级框，而不论它本身是何种元素。

如果浮动非替换元素，则要指定一个明确的宽度；否则，它们会尽可能地窄。

注释：假如在一行之上只有极少的空间可供浮动元素，那么这个元素会跳至下一行，这个过程会持续到某一行拥有足够的空间为止。

float属性值
- left：元素向左浮动
- right：元素向右浮动
- none：默认值。元素不浮动，并会显示在其在文本中出现的位置
- inherit：规定应该从父元素继承float属性的值

JavaScript语法：object.style.cssFloat="left"

# CSS clear属性
clear 属性规定元素的哪一侧不允许其他浮动元素。

说明：clear 属性定义了元素的哪边上不允许出现浮动元素。在 CSS1 和 CSS2 中，这是通过自动为清除元素（即设置了 clear 属性的元素）增加上外边距实现的。在 CSS2.1 中，会在元素上外边距之上增加清除空间，而外边距本身并不改变。不论哪一种改变，最终结果都一样，如果声明为左边或右边清除，会使元素的上外边框边界刚好在该边上浮动元素的下外边距边界之下。

clear属性值
- left：在左侧不允许浮动元素
- right：在右侧不允许浮动元素
- both：在左右两侧均不允许浮动元素
- none：默认值。允许浮动元素出现在两侧
- inherit：规定应该从父元素继承clear属性的值

JavaScript语法：object.style.clea="left"
# 实例
田字格：
```html
<!DOCTYPE html>
<html lang="en">
<head>
        <meta charset="UTF-8">
        <title>test</title>
		<style>
			#div1{
					width:200px;
					height:200px;
					background: red;
					float:left;
			}
			#div2{
					width:200px;
					height:200px;
					background: blue;
					float:left;
			}
			#div3{
					width:200px;
					height:200px;
					background: yellow;
					float:left;
					clear:left;
			}
			#div4{
					width:200px;
					height:200px;
					background: green;
					float:left;
					float:left;
			}
		</style>
</head>
<body>
		<div id="container">
		        <div id="header"></div>
		        <div id="main">
						<div id="lside"></div>
						<div id="rside"></div>
				</div>
		        <div id="footer"></div>
		</div>
</body>
</html>
```
效果：
![CSS浮动布局7](http://ofolh8dcq.bkt.clouddn.com/CSS%E6%B5%AE%E5%8A%A8%E5%B8%83%E5%B1%807.PNG)










---------------------
> 参考文档：[www.w3school.com.cn](http://www.w3school.com.cn/css/css_positioning_floating.asp)
