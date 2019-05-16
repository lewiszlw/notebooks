---
title: Chrome开发者工具使用方法
date: 2016-12-23 10:56:28
tags: Chrome
categories: 奇技淫巧
---
Chrome开发工具（又称DevTools），是一套内嵌在chrome浏览器内部的web编写和调试工具。DevTools提供给web开发人员深入地访问浏览器内部和web应用的机会。DevTools可以有效地跟踪布局问题，设置JavaScript断点，以及进行javascript代码的优化。
<!--more-->
# 打开DevTools
方法一：右键任意点击-审查元素/检查（翻译不同）
方法二：F12
方法三：Ctrl+Shift+I
方法四：点击右上角菜单标志-更多工具-开发者工具

# DevTools窗口概览
## 箭头按钮
用于在页面选择一个元素来审查和查看它的相关信息，当我们在Elements这个按钮页面下点击某个Dom元素时，箭头按钮会变成选择状态。
![Chrome开发者工具使用方法1](http://ofolh8dcq.bkt.clouddn.com/Chrome%E5%BC%80%E5%8F%91%E8%80%85%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%951.PNG)
## 设备按钮
点击它可以切换到不同的终端进行开发模式，移动端和pc端的一个切换，可以选择不同的移动终端设备，同时可以选择不同的尺寸比例，chrome浏览器的模拟移动设备和真实的设备相差不大。
![Chrome开发者工具使用方法2](http://ofolh8dcq.bkt.clouddn.com/Chrome%E5%BC%80%E5%8F%91%E8%80%85%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%952.PNG)
## Elements
Elements 面板让你看到一棵 DOM 树,并允许你进行 DOM 元素检验和动态编辑。
![Chrome开发者工具使用方法5](http://ofolh8dcq.bkt.clouddn.com/Chrome%E5%BC%80%E5%8F%91%E8%80%85%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%955.PNG)
## Console
![Chrome开发者工具使用方法3](http://ofolh8dcq.bkt.clouddn.com/Chrome%E5%BC%80%E5%8F%91%E8%80%85%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%953.PNG)
JavaScript控制台提供了两种主要功能为开发人员测试web页面和应用程序。在这个地方你可以:
- 在开发过程中打印诊断信息。
- 利用shell工具进行Documents和Devtools的交互。

## Sources
js资源页面：这个页面内我们可以找到当前浏览器页面中的js等源文件，方便我们查看和调试。
![Chrome开发者工具使用方法4](http://ofolh8dcq.bkt.clouddn.com/Chrome%E5%BC%80%E5%8F%91%E8%80%85%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%954.PNG)

## Network
Network 面板提供了实时的资源请求和网络下载。识别和解决这些超过预期的请求时间是页面优化的重要一步。
![Chrome开发者工具使用方法6](http://ofolh8dcq.bkt.clouddn.com/Chrome%E5%BC%80%E5%8F%91%E8%80%85%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%956.PNG)

## Timeline
Timeline 面板给你一个完整的关于加载和使用web应用程序或页面的时间花费的信息。所有事件，从解析JavaScript加载资、计算样式、重新渲染都将绘制于Timeline面板上。
![Chrome开发者工具使用方法7](http://ofolh8dcq.bkt.clouddn.com/Chrome%E5%BC%80%E5%8F%91%E8%80%85%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%957.PNG)
## Profiles
Profiles 面板可以查看web应用或页面的执行时间和内存使用。这些有助于理解，资源被使用在哪里，帮助优化代码。提供的分析器有:
- CPU profiler 显示页面的 JavaScript 函数的执行时间.
- Heap profiler 显示页面的 JavaScript 对象和 DOM 节点内存分配。
- JavaScript profiler 显示页面脚本的执行时间。

## Application
Application 面板可以查看页面上加载的资源。也可以和 HTML5 Database、 Local Storage 、 Cookies 、 AppCache 等进行交互。
## Security
Security 面板可以告诉你这个网站的安全性，查看有效的证书等。
## Audits
Audits 面板可以帮你分析页面性能，有助于优化前端页面，分析后得到的报告。
# Elements面板
## Styles子面板
用来查看、修改页面上的元素，包括DOM标签，以及css样式的查看、修改，还有相关盒模型的图形信息。当我们鼠标选择某个元素时，右侧的css样式对应的会展示出此元素的样式信息。此时可以在右侧进行一个修改，修改即可在页面上生效。灰色的element.style样式同样可以进行添加和书写，唯一的区别是在这里添加的样式是添加到了该元素内部，实现方式即：该div元素的style属性。这个页面的功能很强大，在我们做了相关的页面后，修改样式是一块很重要的工作，细微的差距都需要调整，但是不可能说做到每修改一点即编译一遍代码，再刷新浏览器查看效果，这样很低效。我们可以一次性在浏览器中修改之后，再到代码中进行修改。

Styles面板里显示的是：在选中节点上应用的样式，通过横线分割每一条rule，rule的权重越靠上越高。
可以对rule进行下列操作：
1. 修改：
      选择器 -> 点击对应的选择器（除element.style，它不是选择器）；
      样式 -> 点击对应样式；
2. 添加：
      rule，选择器 ->（后面描述）；
      样式 -> 在rule的空白处点击；
3. 删除：
      rule -> 将所有样式删除即可；
      样式 -> 编辑为空，或者将样式的前的checkbox取消勾选；

在调试一些度量属性时，有些快捷键是非常好用的：
1. 当你在调height时
   up -> 以1为增量增加高度；
   down -> 以1为减量减少高度；
2. 当你在调元素的opacity时 
   alt+up -> 以0.1为增量增加透明度；
   alt+down -> 以0.1为减量减少透明度；
3. 有时你希望跨度更大些
   shift+up或pageup -> 以10为增量
   shift+down或pagedown -> 以10为减量
   shift+pageup -> 以100为增量
   shift+pagedown -> 以100为减量

## Computed子面板
盒模型的详细信息。
# Sources面板
js调试方法：
1. 找到目标js文件
2. 点击行号设置断点
3. 点击右侧按钮进行调试，右侧按钮分别是：暂停/继续、单步执行、单步跳入此执行块、单步跳出此执行块、禁用/启用所有断点
![Chrome开发者工具使用方法8](http://ofolh8dcq.bkt.clouddn.com/Chrome%E5%BC%80%E5%8F%91%E8%80%85%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%958.PNG)

js代码修改：
在当前的代码执行区域，在调试中如果发现需要修改的地方，也是可以立即修改的，修改后保存即可生效，这样就免去了再到代码中去书写，再刷新回看了。

# 附录
如果想要最新版本的DevTools，应该使用[Google Chrome Cannary](https://www.google.com/intl/en/chrome/browser/canary.html)

后序：因为DevTools工具很多方法还没有用过，所以有些地方写的很简略，等以后熟悉了再补上......


--------------
> 参考文档：[Chrome DevTools](https://developer.chrome.com/devtools/index)
　　　　　[Chrome 开发者工具中文手册](https://github.com/CN-Chrome-DevTools/CN-Chrome-DevTools)
　　　　　[超完整Chrome浏览器客户端调试大全](http://www.igeekbar.com/igeekbar/post/156.htm?from=singlemessage&isappinstalled=0)
