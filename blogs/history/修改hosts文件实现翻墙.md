---
title: 修改hosts文件实现翻墙
date: 2016-12-19 11:30:04
tags: hosts
categories: 奇技淫巧
---
之前尝试过很多翻墙方式，下过各种代理，但大多有限制，无法长期免费使用，经小伙伴介绍说用修改hosts文件实现翻墙，自己试了下确实不错，上Google速度很快，方便查询资料文档和解决学习中遇到的问题。
<!--more-->
# hosts文件介绍
hosts是一个没有扩展名的系统文件，可以用记事本等工具打开，其作用就是将一些常用的网址域名与其对应的IP地址建立一个关联“数据库”，当用户在浏览器中输入一个需要登录的网址时，系统会首先自动从hosts文件中寻找对应的IP地址，一旦找到，系统会立即打开对应网页，如果没有找到，则系统会再将网址提交DNS域名解析服务器进行IP地址的解析。

浏览器访问网站，要首先通过DNS服务器把要访问的网站域名解析成一个唯一的IP地址，之后，浏览器才能对此网站进行定位并且访问其数据。
操作系统规定，在进行DNS请求以前，先检查系自己的Hosts文件中是否有这个域名和IP的映射关系。如果有，则直接访问这个IP地址指定的网络位置，如果没有，再向已知的DNS服务器提出域名解析请求。也就是说Hosts的IP解析优先级比DNS要高。

# 修改hosts文件
hosts文件位置：
不同系统位置可能不太一样，以我的win7为例，位置：C:\Windows\System32\drivers\etc\

可以通过windows系统的查找功能搜索hosts文件位置

注意：hosts文件具有系统属性，所以可能你找到了文件位置但没有显示hosts文件，可以打开【我的电脑】，依次点击【工具】、【文件夹选项】、【查看】，在【高级设置】中取消【勾选隐藏受保护的系统文件】即可。

找到别人写好的hosts文件，直接将内容复制到自己电脑hosts文件里面即可（替换）。
# 推荐hosts文件
- [https://github.com/racaljk/hosts](https://github.com/racaljk/hosts)
- [https://github.com/StevenBlack/hosts](https://github.com/StevenBlack/hosts)
- [https://github.com/highsea/Hosts](https://github.com/highsea/Hosts)
