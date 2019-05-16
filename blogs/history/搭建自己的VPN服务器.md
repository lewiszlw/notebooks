---
title: 搭建自己的VPN服务器
date: 2017-01-04 19:00:54
tags: [VPS,VPN,Shadowsocks,Linux,Ubuntu,BBR]
categories: Linux
---
接触VPN还是从室友那里听说的。想到自己也经常访问国外网站，租一个VPS也不贵，所以就决定折腾一下。
<!--more-->
# 概念
VPS（Virtual Private Server）：
虚拟专用服务器，是指通过虚拟化技术在独立服务器中运行的专用服务器。每个使用VPS技术的虚拟独立服务器拥有各自独立的公网IP地址、操作系统、硬盘空间、内存空间、CPU资源等，还可以进行安装程序、重启服务器等操作，与运行一台独立服务器完全相同。（[维基百科](https://zh.wikipedia.org/wiki/%E8%99%9A%E6%8B%9F%E4%B8%93%E7%94%A8%E6%9C%8D%E5%8A%A1%E5%99%A8)）

VPN（Vitrual Private Network）：
虚拟专用网络。在公用网络上建立专用网络，进行加密通讯。在企业网络中有广泛应用。VPN网关通过对数据包的加密和数据包目标地址的转换实现远程访问。VPN有多种分类方式，主要是按协议进行分类。VPN可通过服务器、硬件、软件等多种方式实现。（[维基百科](https://zh.wikipedia.org/wiki/%E8%99%9B%E6%93%AC%E7%A7%81%E4%BA%BA%E7%B6%B2%E8%B7%AF)）
# 搭建过程
我的操作系统是Ubuntu16.04 LTS，我选择租赁[DigitalOcean](https://www.digitalocean.com/)公司的虚拟服务器。价格每月5刀，如果是学生的话可以用学校邮箱申请GitHub student pack，里面有50刀DigitalOcean优惠码，够你免费用很久。
## 注册DigitalOcean账号
很简单，主要是中间第二步，没有国外信用卡就选择PayPal方式，PayPal类似支付宝，你可以申请一个PayPal账号然后添加自己的国内银行卡号。
## 创建VPS
登陆后，选择右上角Create Droplet

操作系统我选的是Ubuntu 14.04×64

线路我选择的是New Youk 1（移动联通电信适合什么线路可以Google一下）

套餐我选的是每月5刀的那个套餐，1 CPU、512内存、20GB硬盘、1000G的流量，配置对于搭建VPN绰绰有余。当然搭建博客可能不够。

没有特殊服务就不选，填上服务器名字，也可以默认

最后确认点击最下面Create

等待半分钟就会创建成功
## SSH登录服务器
SSH 是一种远程登录计算机系统的协议，它帮助我们实现在服务器端的VPN配置。

终端输入：
```
ssh root@your_domain_or_ip
```
ip可以到你的DigitalOcean账号上查看。

第一次登录会让你修改密码，原密码DigitalOcean公司会发给你的注册邮箱上。

## 服务器端安装Shadowsocks
SSH登录服务器后
终端执行：
```
apt-get install python-pip
pip install shadowsocks 
```

创建shadowsocks.json配置文件：
```
vi /etc/shadowsocks.json
```
在文件里面写入一下内容：
```
{
		    "server":"your_server_ip",
			"server_port":8388,
			"local_address": "127.0.0.1",
			"local_port":1080,
			"password":"your_password",
			"timeout":300,
			"method":"aes-256-cfb",
			"fast_open": false
}
```

启动shadowsocks
```
ssserver -c /etc/shadowsocks.json -d start
```
## 本机安装Shadowsocks
我选择的是安装命令行shadowsocks，执行
```
apt-get install python-pip
pip install shadowsocks
```
可能会提示要安装其他一些依赖，按提示安装即可。

启动shadowsocks：
```
sslocal -s 11.22.33.44 -p 8388 -k "123456" -l 1080 -t 600 -m aes-256-cfb""
```
以上IP、端口、密码改下即可。

图方便可以先创建配置文件：
```
vi /home/lewis/shadowsocks.json
```
写入：
```
{
				"server":"11.22.33.44",
				"server_port":8388,
				"local_port": 1080,
				"password":"123456",
				"timeout":600,
				"method":"aes-256-cfb"
}
```
然后执行：
```
sslocal -c /home/lewis/shadowsocks.json
```
就可以打开ss了。
## 打开系统代理通道
并不是打开ss就可以用了，还需要设置系统的代理设置。
例如我的Ubuntu16.04 LTS系统：
![搭建自己的VPN服务器1](http://ofolh8dcq.bkt.clouddn.com/%E6%90%AD%E5%BB%BA%E8%87%AA%E5%B7%B1%E7%9A%84VPN%E6%9C%8D%E5%8A%A1%E5%99%A81.png)

也可以浏览器设置里设置代理。

完毕后就可以访问墙外的世界啦~
# 安装BBR实现加速VPS
## 概念
BBR拥塞控制算法，由Google开发其目的就是要尽量跑满带宽，并且尽量不要有排队的情况。简单一点 就是高延迟高丢包的tcp环境下的优化算法。
## 安装方法
首先ssh登录服务器：
```
ssh root@your_server_ip
```

下载及安装linux新内核（vps操作系统为64位ubuntu）
新内核已加入了该算法，安装即可享用：
```
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.9/linux-image-4.9.0-040900-generic_4.9.0-040900.201612111631_amd64.deb
dpkg -i linux-image-4.9.0*.deb
dpkg -l|grep linux-image
apt-get purge
```

更新grub系统引导文件及重启：
```
update-grub
reboot
```

配置BBR文件，开启BBR：
```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl –p
```

检查是否开启BBR：
```
sysctl net.ipv4.tcp_available_congestion_control
lsmod | grep bbr
```




-------------------------
参考教程：
- [Tong's Guide：建立自己的VPN服务器](http://www.people.virginia.edu/~tq7bw/vpn/)
- [ubuntu 安装 bbr 更好的加速你的国外VPS](https://wiki.vpsmm.com/bbr/)
- [什么是tcp bbr？](https://www.zhihu.com/question/53729940)
