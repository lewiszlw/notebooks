---
title: 阻止Google自动跳转到香港服务器的方法
date: 2016-12-30 19:19:43
tags: Google
categories: 奇技淫巧
---
前几天笔记本改了hosts文件，但是却时而可以上Google时而不行。而且我的另一台台式电脑没这种问题，我的室友也没出现这种问题，这一直让我很困惑，不知道原因出在哪。
<!--more-->
今天又出现了这种问题，无法上Google。

仔细看了一眼，卧槽，怎么不是google.com？？？（黑人问号

怎么跳到www.google.com.hk去了？？？

WTF？？？

好吧，是我太傻逼了，之前出这种问题居然没发现（囧rz=З

上网查了查，原来是因为Google的服务器搬离大陆后，大陆地区用户用Google服务时会自动跳到香港的google.com.hk。

解决方法：
在地址后面加上/ncr，意思是强制不跳转。比如：https://www.google.com/ncr

修改chrome地址栏默认搜索引擎：
因为默认Google搜索引擎没法更改，所以新增加一个搜索引擎，名字和关键字随便，网址填写
```
http://www.google.com/search?q=%s&{google:RLZ}{google:originalQueryForSuggestion}{google:assistedQueryStats}{google:searchFieldtrialParameter}{google:bookmarkBarPinned}{google:searchClient}{google:sourceId}{google:instantExtendedEnabledParameter}{google:omniboxStartMarginParameter}{google:contextualSearchVersion}ie={inputEncoding}
```
示例：
![阻止Google自动跳转到香港服务器的方法1](http://ofolh8dcq.bkt.clouddn.com/%E9%98%BB%E6%AD%A2Google%E8%87%AA%E5%8A%A8%E8%B7%B3%E8%BD%AC%E5%88%B0%E9%A6%99%E6%B8%AF%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9A%84%E6%96%B9%E6%B3%951.PNG)


顺便附上：[修改hosts文件实现翻墙](http://zlw.poker/2016/12/19/%E4%BF%AE%E6%94%B9hosts%E6%96%87%E4%BB%B6%E5%AE%9E%E7%8E%B0%E7%BF%BB%E5%A2%99/)

-----------------------------------------------
参考资料：
- [如何不让google.com跳转到google.com.hk？](https://www.zhihu.com/question/20460191)
