---
title: CSS控制背景
date: 2016-12-19 20:16:33
tags: CSS
categories: Front-End
---
CSS 允许应用纯色作为背景，也允许使用背景图像创建相当复杂的效果。
<!--more-->
# 背景色
可以使用 background-color 属性为元素设置背景色。这个属性接受任何合法的颜色值。

可以为所有元素设置背景色，这包括 body 一直到 em 和 a 等行内元素。

元素背景的范围
background-color 属性为元素设置一种纯色。这种颜色会填充元素的内容、内边距和边框区域，扩展到元素边框的外边界（但不包括外边距）。如果边框有透明部分（如虚线边框），会透过这些透明部分显示出背景色。

background-color 不能继承，其默认值是 transparent。transparent 有“透明”之意。也就是说，如果一个元素没有指定背景色，那么背景就是透明的，这样其祖先元素的背景才能可见。

background-color属性值
- color_name	规定颜色值为颜色名称的背景颜色（比如 red）。
- hex_number	规定颜色值为十六进制值的背景颜色（比如 #ff0000）。
- rgb_number	规定颜色值为 rgb 代码的背景颜色（比如 rgb(255,0,0)）。
- transparent	默认。背景颜色为透明。
- inherit	规定应该从父元素继承 background-color 属性的设置。

JavaScript 语法：object.style.backgroundColor="#00FF00"
# 背景图像
要把图像放入背景，需要使用 background-image 属性。background-image 属性的默认值是 none，表示背景上没有放置任何图像。

元素的背景占据了元素的全部尺寸，包括内边距和边框，但不包括外边距。

默认地，背景图像位于元素的左上角，并在水平和垂直方向上重复。

提示：请设置一种可用的背景颜色，这样的话，假如背景图像不可用，页面也可获得良好的视觉效果。

详细说明
background-image 属性会在元素的背景中设置一个图像。
根据 background-repeat 属性的值，图像可以无限平铺、沿着某个轴（x 轴或 y 轴）平铺，或者根本不平铺。
初始背景图像（原图像）根据 background-position 属性的值放置。

background-image属性值
- url('URL')	指向图像的路径。
- none	默认值。不显示背景图像。
- inherit	规定应该从父元素继承 background-image 属性的设置。

JavaScript 语法：object.style.backgroundImage="url(stars.gif)"

注意：不仅可以为body元素设置背景，还可以为一个段落设置背景，甚至可以为行内元素设置背景图像。
![CSS控制背景1](http://ofolh8dcq.bkt.clouddn.com/CSS%E6%8E%A7%E5%88%B6%E8%83%8C%E6%99%AF1.PNG)
# 背景重复
background-repeat 属性定义了图像的平铺模式。
从原图像开始重复，原图像由 background-image 定义，并根据 background-position 的值放置。

background-repeat属性值
- repeat	默认。背景图像将在垂直方向和水平方向重复。
- repeat-x	背景图像将在水平方向重复。
- repeat-y	背景图像将在垂直方向重复。
- no-repeat	背景图像将仅显示一次。
- inherit	规定应该从父元素继承 background-repeat 属性的设置。

注：不设置background-repeat属性，效果和设置background-repeat属性值为repeat一样。

JavaScript 语法：object.style.backgroundRepeat="repeat-y"
# 背景定位
可以利用 background-position 属性改变图像在背景中的位置。

background-position属性值
- 关键字
  - top left
  - top center
  - top right
  - center left
  - center center
  - center right
  - bottom left
  - bottom center
  - bottom right
  注：如果您仅规定了一个关键词，那么第二个值将是"center"。默认值：0% 0%。
- 百分比
  - x% y%
  第一个值是水平位置，第二个值是垂直位置。左上角是 0% 0%。右下角是 100% 100%。如果您仅规定了一个值，另一个值将是 50%。
- 长度数值
  - xpos ypos
  第一个值是水平位置，第二个值是垂直位置。左上角是 0 0。单位是像素 (0px 0px) 或任何其他的 CSS 单位。如果您仅规定了一个值，另一个值将是50%。您可以混合使用 % 和 position 值。
 
JavaScript 语法：	object.style.backgroundPosition="center"
# 背景关联
background-attachment 属性设置背景图像是否固定或者随着页面的其余部分滚动。

background-attachment属性值
- scroll	默认值。背景图像会随着页面其余部分的滚动而移动。
- fixed	当页面的其余部分滚动时，背景图像不会移动。
- inherit	规定应该从父元素继承 background-attachment 属性的设置。

JavaScript 语法：	object.style.backgroundAttachment="fixed"

# 小结
CSS背景属性
- background	简写属性，作用是将背景属性设置在一个声明中。
- background-color	设置元素的背景颜色。
- background-image	把图像设置为背景。
- background-position	设置背景图像的起始位置。
- background-attachment	背景图像是否固定或者随着页面的其余部分滚动。
- background-repeat	设置背景图像是否及如何重复。

background简写顺序：background-color、background-image、background-position、background-attachment、background-repeat。

注：设置background-position、background-attachment、background-repeat属性前必须先设置background-image属性。
# 常见问题
Q：通常网站的背景小图都会整合到一张大图片中，如何正确展示所需的小背景呢？
通常是通过background-position属性移动大图片使小图片正好展示在方框内。


-------------------
> 参考文档：[www.w3school.com.cn](http://www.w3school.com.cn/css/css_background.asp)
