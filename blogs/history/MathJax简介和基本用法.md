---
title: MathJax简介和基本用法
date: 2016-11-29 19:57:57
tags: [MathJax,LaTex]
categories: blog
---
MathJax是一个JavaScript引擎，用来显示网络上的数学公式。它可以工作于所有流行的浏览器上。
<!--more-->
# 简介
MathJax是一个开源JavaScript库。它支持LaTeX、MathML、AsciiMath符号，可以运行于所有流行浏览器上。 它的设计目标是利用最新的web技术，构建一个支持math的web平台。支持主要的浏览器和操作系统,包括那些移动设备。 对大部分用户而言它不需要安装，即没有插件需要下载也没有软件需要安装，所以网页作者可以编写包含数学公式的页面并有信心可以自然而容易的浏览到它们。 只需要在页面中包含MathJax脚本和一些数学公式，其他的事情交给MathJax来处理吧。

MathJax使用网络字体（大部分浏览器都支持）去产生高质量的排版，使其在所有分辨率都可缩放和显示，这远比使用包含公式的图片要有效得多。 MathJax也可以用于屏幕阅读器，让视力受损者也可以使用。

使用MathJax显示数学公式是基于文本的，而非图片。它可以被搜索引擎使用，这意味着方程式和页面上的文字一样是可以被搜索的。 MathJax允许页面作者使用TeX、LaTeX符号和 MathML 或者 AsciiMath 去书写公式。 MathJax甚至可以将Tex格式转化为MathML格式，使其可以被原生支持MathML格式的浏览器更多的渲染。转化为MathML格式后你可以复制粘贴它们到其他程序中。

MathJax是模块化的，所以它仅仅在需要时才加载它的组件，同时也可以被扩展以实现更多功能。 MathJax同时也是高度可配置的，允许作者作出更适宜网站自身的自定义。 最重要的，MathJax的API可以让你在你的网页上动态的创建公式。
# 基本用法
## 获取MathJax
有三种方法
- 使用分布式网络中MathJax的副本
例如：
```html
<script type="text/javascript"
  src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
```
- 下载并安装一个MathJax的副本到服务器
  1. 获取MathJax的副本并放置在硬盘上。
  2. 配置MathJax以适合自己站点的需要。
  3. 链接MathJax到要包括公式的web页面。
  4. 将公式写入页面以便MathJax显示它们。
- 使用本地硬盘的副本

## 基本语法
### 插入公式
- 行内公式
```
$...$
```
- 行间公式
```
$$...$$   //注意要在上面空出一行
```

例如：行内公式`$y=\sum_{i=1}^{n}x$`为$y=\sum_{i=1}^{n}x$，行间公式为

$$ y=\sum_{i=1}^{n}x $$
### 上标和下标
- 上标：`^`
- 下标：`_`

例如：`$x^1$`为$x^1$；`$x_n`为$x_n$；`$x^{5^6}$`为$x^{5^6}$

### 括号
- 圆括号：`()`
- 方括号：`[]`
- 花括号：`\lbrace \rbrace`
- 尖括号：`\langle \rangle`
- 上取整：`\lceil \rceil`
- 下取整：`\lfloor \rfloor`

例如：`$\lbrace[(a+b)]\rbrace$`为$\lbrace[(a+b)]\rbrace$；`$\langle x,y \rangle$`为$\langle x,y \rangle$；`$\lceil x \rceil$`为$\lceil x \rceil$；`$\lfloor y \rfloor$`为$\lfloor y \rfloor$。

### 求和和累积
- 求和：`\sum`
- 累积：`\prod`

例如：`$\sum_{i=1}^{n}x$`为$\sum_{i=1}^{n}x$，`$\prod_0^{10}x^2$`为$\prod_0^{10}x^2$。

### 积分和极限
- 积分：`\int`
- 极限：`\lim`

例如：`$\int_a^b$`为$\int\_a^b$，`$\lim_{x\to 0}$`为$\lim\_{x\to 0}$。
### 分式和根式
- 分式：`\frac`
- 根式：`\sqrt`

例如：`$\sqrt[3]{\frac{x}{y}}$`为$\sqrt[3]{\frac{x}{y}}$。

### 矢量
- 矢量：`\vec`

例如：`$\vec{a}\cdot\vec{b}$`为$\vec{a} \cdot \vec{b}$。

### 省略号和公式编号
- 省略号：`\ldots`（与文本底线对齐）和`\cdots`（与文本中线对齐）
- 公式编号：`\tag{id}`

例如：`$f(x_1,x_2,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2$`为$f(x_1,x_2,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2$，公式编号：

$${ \sum_{i=0}^n i^2= \frac {(n^2+n)(2n+1)}{6}} \tag{1.1}$$

### 字体
例如：`\mathfrak`字体
`$\mathfrak{ABC}$`为$\mathfrak{ABC}$。

### 其他字母和特殊符号（常用）
$\alpha$：\alpha
$\beta$：\beta
$\Gamma$：\Gamma
$\gamma$：\gamma
$\delta$：\delta
$\Delta$：\Delta
$\epsilon$：\epsilon
$\theta$：\theta
$\pi$：\pi
$\Pi$：\Pi
$\lambda$：\lambda
$\mu$：\mu
$\xi$：\xi
$\sigma$：\sigma
$\phi$：\phi
$\Phi$：\Phi
$\omega$：\omega
$\Omega$：\Omega

关系运算符
$\pm$：\pm
$\times$：\times
$\div$：\div
$\mid$：\mid
$\ast$：\ast
$\bigodot$：\bigodot
$\bigotimes$：\bigotimes
$\bigoplus$：\bigoplus
$\leq$：\leq
$\geq$：\geq
$\neq$：\neq
$\approx$：\approx
$\equiv$：\equiv

集合运算符
$\emptyset$：\emptyset
$\in$：\in
$\notin$：\notin
$\subset$：\subset
$\supset$：\supset
$\subseteq$：\subseteq
$\supseteq$：\supseteq
$\bigcap$：\bigcap
$\bigcup$：\bigcup
$\bigvee$：\bigvee
$\bigwedge$：\bigwedge

对数运算符
$\log$：\log
$\lg$：\lg
$\ln$：\ln

三角运算符
$\angle$：\angle
$30^\circ$：30^\circ
$\sin$：\sin
$\cos$：\cos
$\tan$：\tan

微积分运算符
$\prime$：\prime
$\iint$：\iint
$\iiint$：\iiint
$\iiiint$：\iiiint
$\oint$：\oint
$\infty$：\infty

逻辑运算符
$\because$：\because
$\therefore$：\therefore
$\forall$：\forall
$\exists$：\exists
$\not=$：\not=
$\not>$：\not>
$\not\subset$：\not\subset

箭头符号
$\uparrow$：\uparrow
$\downarrow$：\downarrow
$\Uparrow$：\Uparrow
$\Downarrow$：\Downarrow
$\rightarrow$：\rightarrow
$\leftarrow$：\leftarrow
$\Rightarrow$：\Rightarrow
$\Leftarrow$：\Leftarrow
$\longrightarrow$：\longrightarrow
$\longleftarrow$：\longleftarrow
$\Longrightarrow$：\Longrightarrow
$\Longleftarrow$：\Longleftarrow


# 友情链接
- [官方网站www.mathjax.org](https://www.mathjax.org/)
- [MathJax中文文档](http://mathjax-chinese-doc.readthedocs.io/en/latest/#)
- [MathJax在线编辑器](http://latex.codecogs.com/eqneditor/editor.php)(生成公式代码或图片)



