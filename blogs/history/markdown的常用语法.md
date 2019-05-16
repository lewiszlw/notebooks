---
title: markdown的常用语法
date: 2016-10-27 09:12:04
tags: markdown
categories: blog
---
## 导语
[Markdown](http://www.markdown.cn/)是一个 Web 上使用的文本到HTML的转换工具，可以通过简单、易读易写的文本格式生成结构化的HTML文档。目前 github、Stackoverflow 等网站均支持这种格式。 
## 常用语法
<!--more-->
### 段落和换行
一个 Markdown 段落是由**一个或多个连续**的文本行组成，它的**前后要有一个以上的空行**（空行的定义是显示上看起来像是空的，便会被视为空行。比方说，若某一行只包含空格和制表符，则该行也会被视为空行）。普通段落不该用空格或制表符来缩进。
### 标题
Markdown支持两种标题的语法，类Setext和类atx形式。
类Setext形式使用底线的形式，利用=（最高阶标题）和-（第二阶标题），例如：
```
This is an H1
=============

This is an H2
-------------
```
任何数量的=和-都可以有效果。
类atx形式则是在行首插入1到6个# ，对应到标题1到6阶，例如：
```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```
### 区块引用
```
> 这里是引用内容
> 这里是引用内容
```
也允许你偷懒只在整个段落的第一行最前面加上 >：
```
> 这里是引用内容
这里是引用内容...
```
区块引用可以嵌套（例如：引用内的引用），只要根据层次加上不同数量的 > ：
```
 This is the first level of quoting.
> 
>> This is nested blockquote.
> 
> Back to the first level.
```
`注意返回上一级引用的写法`
引用的区块内也可以使用其他的 Markdown 语法，包括标题、列表、代码区块等。
### 列表
Markdown 支持有序列表和无序列表。
无序列表使用星号、加号或是减号作为列表标记：
```
* red
* green
* blue

+ red
+ green
+ blue

- red
- green
- blue
```
有序列表则使用数字接着一个英文句点：
```
1. bird
2. mchale
3. parish
```
`很重要的一点是，你在列表标记上使用的数字并不会影响输出的 HTML 结果`

如果列表项目间用空行分开，在输出 HTML 时 Markdown 就会将项目内容用&lt;p&gt;标签包起来，举例来说：
```
* Bird
* Magic

转换为：
<ul>
<li>Bird</li>
<li>Magic</li>
</ul>


* Bird

* Magic

转换为：
<ul>
<li><p>Bird</p></li>
<li><p>Magic</p></li>
</ul>
```
列表项目可以包含多个段落，每个项目下的段落都必须缩进 4 个空格或是 1 个制表符：
```
1.  This is a list item with two paragraphs. Lorem ipsum dolor
    sit amet, consectetuer adipiscing elit. Aliquam hendrerit
    mi posuere lectus.

    Vestibulum enim wisi, viverra nec, fringilla in, laoreet
    vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
    sit amet velit.

2.  Suspendisse id sem consectetuer libero luctus adipiscing.
```
如果要在列表项目内放进引用，那 > 就需要缩进：
```
*   A list item with a blockquote:

    > This is a blockquote
    > inside a list item.
```
### 代码区块
和程序相关的写作或是标签语言原始码通常会有已经排版好的代码区块，通常这些区块我们并不希望它以一般段落文件的方式去排版，而是照原来的样子显示，Markdown 会用&lt;pre&gt;和&lt;code&gt;标签来把代码区块包起来。
要在 Markdown 中建立代码区块很简单，只要简单地缩进 4 个空格或是 1 个制表符就可以：
```
这是一个普通段落：

    这是一个代码区块。
```
`注意要有空行`
### **分隔线**
你可以在一行中用**三个以上**的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线：
```
* * *

***

*****

- - -

---------------------------------------
```
### 链接
Markdown 支持两种形式的链接语法： 行内式和参考式两种形式。
行内式：
```
This is [an example](http://example.com/) inline link.

See my [About](/about/) page for details.    //相对路径
```
参考式：
```
This is [an example][id] reference-style link.

//接着，在文件的任意处，你可以把这个标记的链接内容定义出来
[id]: http://example.com/  "Optional Title Here"
```
### 强调
Markdown 使用星号（*）和底线（_）作为标记强调字词的符号:
```
*text*    //斜体
**text**    //加粗
***text***    //加粗和斜体
```
### 代码
一小段代码：
```
`text`
``text``
```
大段代码：
```
```text```

### 图片
Markdown 使用一种和链接很相似的语法来标记图片，同样也允许两种样式： 行内式和参考式。
行内式：
`![alt text](path)`
参考式：
`![alt text][id]`
`[id]:path`
**建议使用[七牛云](http://www.qiniu.com/)作为图床。上传网站图片到七牛云，生成外链，然后复制到文档中。**
### 表格
```
操作对象|创建         |删除       |修改
--------|-------------|-----------|-----------
模式    |CREATE SCHEMA|DROP SCHEMA|
表      |CREATE TABLE |DROP TABLE |ALTER TABLE
视图    |CREATE VIEW  |DROP VIEW  |
索引    |CREATE INDEX |DROP INDEX |ALTER INDEX
```
效果为：

操作对象|创建         |删除       |修改
--------|-------------|-----------|-----------
模式    |CREATE SCHEMA|DROP SCHEMA|
表      |CREATE TABLE |DROP TABLE |ALTER TABLE
视图    |CREATE VIEW  |DROP VIEW  |
索引    |CREATE INDEX |DROP INDEX |ALTER INDEX
```
|操作对象|创建           |删除        |修改        |
|--------|:-------------:|:-----------|-----------:|
|模式    |CREATE SCHEMA  |DROP SCHEMA |            |
|表      |CREATE TABLE   |DROP TABLE  |ALTER TABLE |
|视图    |CREATE VIEW    |DROP VIEW   |            |
|索引    |CREATE INDEX   |DROP INDEX  |ALTER INDEX |
```
效果为：

|操作对象|创建           |删除        |修改        |
|--------|:-------------:|:-----------|-----------:|
|模式    |CREATE SCHEMA  |DROP SCHEMA |            |
|表      |CREATE TABLE   |DROP TABLE  |ALTER TABLE |
|视图    |CREATE VIEW    |DROP VIEW   |            |
|索引    |CREATE INDEX   |DROP INDEX  |ALTER INDEX |
## Markdown免费编辑器
Windows平台
- [GitHub Atom](https://atom.io/)
- [MarkdownPad](http://markdownpad.com/)
- [MarkPad](http://code52.org/DownmarkerWPF/)

Linux平台
- [ReText](https://sourceforge.net/p/retext/home/ReText/)

Mac平台
- [Mou](http://25.io/mou/)

在线编辑器
- [Markable.in](https://markable.in/)
- [Dillinger.io](http://dillinger.io/)
- [Cmd Markdown](https://www.zybuluo.com/mdeditor)

浏览器插件
- [MaDe](https://chrome.google.com/webstore/detail/made/oknndfeeopgpibecfjljjfanledpbkog)（Chrome）


## 遇到的问题
Q：列表下面的段落会缩进
在列表后加一段空行

Q：如何插入html标签
需要用到转义字符。例如插入&lt;head&gt;标签，写成`&lt;head&gt;`。
