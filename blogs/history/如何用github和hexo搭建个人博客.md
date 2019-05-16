---
title: 如何用github和hexo搭建个人博客
date: 2016-10-19 00:13:12
tags: [hexo,github]
categories: blog
---
## STEP1：
注册github账号，创建repository，名称需要和你的账号对应，名称为yourname.github.io。
<!--more-->
## STEP2：
进入repository设置（setting）页面，下拉到底部，点击Launch automatic page generator按钮，  
按要求输入相关名称，然后点击Continue to layouts，选择一个主题然后发布。至此，你的站点  
已经搭建起来了，你可以进入你的主页http://yourname.github.io 进行查看了。
## STEP3：
安装[git](https://git-scm.com/downloads/),安装[Node.js](https://nodejs.org/en/)。
## STEP4:
安装hexo：鼠标右键任意地方，选择Git Bash，然后输入命令  
`$ npm install hexo -g`  
`$ hexo init blog`
`$ cd blog`
`$ npm install`
`$ hexo s`
然后访问localhost:4000，你就可以看到生成好的博客。
## STEP5:
部署本地文件到github。首先打开hexo配置文件_config.yml，更改如下  
`deploy:`  
   `type: git`  
   `repository: http://github.com/yourname/yourname.github.io.git`  
   `branch: master`  
然后输入命令：
`$ hexo g`  
`$ hexo d`  
此时，我们的博客已经搭建起来。
## STEP6：
主题：你可以在网上找自己喜欢的主题，然后根据主题制作者提供的文档来进行更改。
附上知乎大牛用爬虫搜索的[hexo主题](https://www.zhihu.com/question/24422335)，以及[hexo官网主题](https://hexo.io/themes/)  
备注：命令和配置属性的冒号后面都必须加上一个空格。
