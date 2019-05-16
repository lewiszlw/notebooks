---
title: 实用的GitHub小技巧
date: 2016-12-17 09:19:52
tags: github
categories: 奇技淫巧
---
收集一些关于使用GitHub的实用的小技巧（持续更新...）
<!--more-->
# 利用GitHub上的GitHub Pages功能搭建静态网站
见[如何用github和hexo搭建个人博客](http://zlw.poker/2016/10/19/%E5%A6%82%E4%BD%95%E7%94%A8github%E5%92%8Chexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)

[GitHub Pages官方介绍及文档](https://pages.github.com/)
# 精准分享代码
方法：url后加上
- #L行号（分享某一行代码）
- #L开始行号-#L结束行号（分享多行代码）

示例：
- [https://github.com/lewiszlw/lewiszlw.github.io/blob/master/js/totop.js**#L26**](https://github.com/lewiszlw/lewiszlw.github.io/blob/master/js/totop.js#L26)
- [https://github.com/lewiszlw/lewiszlw.github.io/blob/master/js/totop.js**#L15-#L22**](https://github.com/lewiszlw/lewiszlw.github.io/blob/master/js/totop.js#L15-#L22)

# 通过HTML方式嵌入GitHub项目star等按钮
方法：
- **Star：**
```html
<iframe src="https://ghbtns.com/github-btn.html?user=twbs&repo=bootstrap&type=star&count=true&size=large" frameborder="0" scrolling="0" width="160px" height="30px"></iframe>

<iframe src="https://ghbtns.com/github-btn.html?user=twbs&repo=bootstrap&type=star&count=true" frameborder="0" scrolling="0" width="170px" height="20px"></iframe>
```
- **Fork：**
```html
<iframe src="https://ghbtns.com/github-btn.html?user=twbs&repo=bootstrap&type=fork&count=true&size=large" frameborder="0" scrolling="0" width="158px" height="30px"></iframe>

<iframe src="https://ghbtns.com/github-btn.html?user=twbs&repo=bootstrap&type=fork&count=true" frameborder="0" scrolling="0" width="170px" height="20px"></iframe>
```
- **Watch：**
```html
<iframe src="https://ghbtns.com/github-btn.html?user=twbs&repo=bootstrap&type=watch&count=true&size=large&v=2" frameborder="0" scrolling="0" width="160px" height="30px"></iframe>

<iframe src="https://ghbtns.com/github-btn.html?user=twbs&repo=bootstrap&type=watch&count=true&v=2" frameborder="0" scrolling="0" width="170px" height="20px"></iframe>
```
- **Follow：**
```html
<iframe src="https://ghbtns.com/github-btn.html?user=mdo&type=follow&count=true&size=large" frameborder="0" scrolling="0" width="220px" height="30px"></iframe>

<iframe src="https://ghbtns.com/github-btn.html?user=mdo&type=follow&count=true" frameborder="0" scrolling="0" width="170px" height="20px"></iframe>
```
- 将user和repo改为自己GitHub用户名和仓库名就OK

示例：
![实用的GitHub小技巧1](http://ofolh8dcq.bkt.clouddn.com/%E5%AE%9E%E7%94%A8%E7%9A%84GitHub%E5%B0%8F%E6%8A%80%E5%B7%A71.PNG)

官方介绍：
[http://ghbtns.com/](http://ghbtns.com/)
# 查看项目的访问数据
方法：
在项目下，点击Graphs，然后再点击Traffic即可看到。
![实用的GitHub小技巧2](http://ofolh8dcq.bkt.clouddn.com/%E4%BD%BF%E7%94%A8%E7%9A%84GitHub%E5%B0%8F%E6%8A%80%E5%B7%A72.PNG)

Graphs其他功能介绍：
[Introducing the New GitHub Graphs](https://github.com/blog/1093-introducing-the-new-github-graphs/)
# trending排行榜
trending显示各类语言项目每日的排行榜，非常适合开发者fork学习。

方法：
1. 点击头像，选择Explore；
![实用的GitHub小技巧3](http://ofolh8dcq.bkt.clouddn.com/%E5%AE%9E%E7%94%A8%E7%9A%84GitHub%E5%B0%8F%E6%8A%80%E5%B7%A73.PNG)
2. 然后点击Trending。
![实用的GitHub小技巧4](http://ofolh8dcq.bkt.clouddn.com/%E5%AE%9E%E7%94%A8%E7%9A%84GitHub%E5%B0%8F%E6%8A%80%E5%B7%A74.PNG)

提示：右侧可以切换语言。
# 显示快捷键
方法：
任意界面“shift”+“?”可以显示快捷键。
# issue快速引用
方法：
选中文字，按“R”键快速引用。
# issue添加表情
方法：
输入冒号“:”添加表情。
# 更改项目语言
通常GitHub会根据项目相关文件代码的数量来自动识别你的项目是什么语言类型的项目。

方法：
1. 在项目根目录添加.gitattributes文件；
2. 文件内容如下：
```
*.githublang linguist-language=truelang
```

示例：
```
*.js linguist-language=go
*.css linguist-language=go
*.html linguist-language=go
```

原理：
将.js，.css，.html文件识别成go语言。

------------------
（后续有新的小技巧再慢慢补充...）
