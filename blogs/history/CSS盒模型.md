---
title: CSS盒模型
date: 2016-12-19 08:18:46
tags: CSS
categories: Front-End
---
CSS盒模型（Box Model）规定了元素框处理元素内容、内边距、边框和外边距的方式。
<!--more-->
# CSS盒模型概述
![CSS盒模型1](http://ofolh8dcq.bkt.clouddn.com/CSS%E7%9B%92%E6%A8%A1%E5%9E%8B1.PNG)
元素框的最内部分是实际的内容，直接包围内容的是内边距。内边距呈现了元素的背景。内边距的边缘是边框。边框以外是外边距，外边距默认是透明的，因此不会遮挡其后的任何元素。

注意：背景应用于由内容和内边距、边框组成的区域。不包括外边距区域。

内边距、边框和外边距都是可选的，默认值是零。在 CSS 中，width 和 height 指的是**内容区域**的宽度和高度。增加内边距、边框和外边距不会影响内容区域的尺寸，但是会增加元素框的总尺寸。

例如：
如下代码设置盒模型：
```css
#box{
	width: 70px;
	margin: 10px;
	padding: 5px;
}
```
各边距如下：
![CSS盒模型2](http://ofolh8dcq.bkt.clouddn.com/CSS%E7%9B%92%E6%A8%A1%E5%9E%8B2.PNG)
# CSS内边距
元素的内边距在边框和内容区之间。控制该区域最简单的属性是 padding 属性。CSS padding 属性定义元素边框与元素内容之间的空白区域。

CSS padding 属性定义元素的内边距。padding 属性接受**长度值**或**百分比值**，但不允许使用负值。
- padding: 10px;内边距为10像素
- padding: 10%;内边距是父元素**width**的10%

CSS内边距属性
- padding：简写属性。作用是在一个声明中设置元素的所有内边距属性
- padding-bottom：设置元素的下内边距
- padding-left：设置元素的左内边距
- padding-right：设置元素的右内边距
- padding-top：设置元素的上内边距

# CSS边框
元素的边框 (border) 是围绕元素内容和内边距的一条或多条线。每个边框有 3 个方面：宽度、样式，以及颜色。CSS border 属性允许你规定元素边框的样式、宽度和颜色。

CSS 规范指出，边框绘制在“元素的背景之上”。这很重要，因为有些边框是“间断的”（例如，点线边框或虚线框），元素的背景应当出现在边框的可见部分之间。

CSS边框属性
- border：简写属性，用于把针对四个边的属性设置在一个声明（例如设置为10px solid red）
- border-style：用于设置元素所有边框的样式，或者单独地为各边设置边框样式
- border-width：简写属性，用于为元素的所有边框设置宽度，或者单独地为各边边框设置宽度
- border-color：简写属性，设置元素的所有边框中可见部分的颜色，或为 4 个边分别设置颜色
- border-bottom：简写属性，用于把下边框的所有属性设置到一个声明中
- border-bottom-color：设置元素的下边框的颜色
- border-bottom-style：设置元素的下边框的样式
- border-bottom-width：设置元素的下边框的宽度
- border-left：简写属性，用于把左边框的所有属性设置到一个声明中
- border-left-color：设置元素的左边框的颜色
- border-left-style：设置元素的左边框的样式
- border-left-width：设置元素的左边框的宽度
- border-right：简写属性，用于把右边框的所有属性设置到一个声明中
- border-right-color：设置元素的右边框的颜色
- border-right-style：设置元素的右边框的样式
- border-right-width：设置元素的右边框的宽度
- border-top：简写属性，用于把上边框的所有属性设置到一个声明中
- border-top-color：设置元素的上边框的颜色
- border-top-style：设置元素的上边框的样式
- border-top-width：设置元素的上边框的宽度

注意：简写属性时同样遵循top-right-bottom-left顺序。

常见border-style属性值有
- solid：实线边框
- dotted：点线边框
- dashed：虚线边框
- double：双线边框

# CSS外边距
围绕在元素边框的空白区域是外边距。设置外边距会在元素外创建额外的“空白”。设置外边距的最简单的方法就是使用 margin 属性，这个属性接受任何长度单位、百分数值甚至负值。
- margin: 10px;外边距为10像素
- margin: 10%;外边距为父元素**width**的10%
- margin: auto;

margin 的默认值是 0，所以如果没有为 margin 声明一个值，就不会出现外边距。但是，在实际中，浏览器对许多元素已经提供了预定的样式，外边距也不例外。例如，在支持 CSS 的浏览器中，外边距会在每个段落元素的上面和下面生成“空行”。因此，如果没有为 p 元素声明外边距，浏览器可能会自己应用一个外边距。当然，只要你特别作了声明，就会覆盖默认样式。

CSS外边距属性
- margin：简写属性。在一个声明中设置所有外边距属性
- margin-bottom：设置元素的下外边距
- margin-left：设置元素的左外边距
- margin-right：设置元素的右外边距
- margin-top：设置元素的上外边距

margin简写规则
- 如果缺少左外边距的值，则使用右外边距的值
- 如果缺少下外边距的值，则使用上外边距的值
- 如果缺少右外边距的值，则使用上外边距的值

注意，margin简写属性如果只设置左外边距，做法：margin: auto auto auto 10px;

利用margin属性实现盒模型居中：margin: 0 auto;
# CSS外边距合并
外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。
1. 当一个元素出现在另一个元素上面时，第一个元素的下外边距与第二个元素的上外边距会发生合并
![CSS盒模型3](http://ofolh8dcq.bkt.clouddn.com/CSS%E7%9B%92%E6%A8%A1%E5%9E%8B3.PNG)
2. 当一个元素包含在另一个元素中时（假设没有内边距或边框把外边距分隔开），它们的上和/或下外边距也会发生合并
![CSS盒模型4](http://ofolh8dcq.bkt.clouddn.com/CSS%E7%9B%92%E6%A8%A1%E5%9E%8B4.PNG)

注释：只有普通文档流中块框的垂直外边距才会发生外边距合并。**行内框**、**浮动框**或**绝对定位**之间的外边距不会合并。
# 示例
用CSS画三角形：
代码：
```html
<!DOCTYPE html>
<html lang="en">
<head>
        <meta charset="UTF-8">
        <title>test</title>
		<style>
			#triangle1{
					width:0;
					height:0;
					border-top:100px solid red;
					border-right:100px solid blue;
					border-bottom:100px solid yellow;
					border-left:100px solid green;
			}
			#triangle2{
					width:0;
					height:0;
					border-top:100px solid white;
					border-right:100px solid white;
					border-bottom:100px solid yellow;
					border-left:100px solid white;

			}
		</style>
</head>
<body>
		<div id="triangle1"></div>
		<div id="triangle2"></div>
</body>
</html>
```
效果：
![CSS盒模型5](http://ofolh8dcq.bkt.clouddn.com/CSS%E7%9B%92%E6%A8%A1%E5%9E%8B5.PNG)

--------------------------------------
> 参考文档：[www.w3school.com.cn](http://www.w3school.com.cn/css/css_boxmodel.asp)

