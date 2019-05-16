---
title: 虚拟机安装Linux系统及常用软件
date: 2016-12-29 08:45:27
tags: [Linux,Ubuntu,VMware]
categories: Linux
---
之前有安装过一次Win7+Ubuntu双系统，可是不知道是引导有问题还是笔记本问题，用了一段时间结果两个系统都进不去，无奈只能重装，装的单系统Win7，可是自从那次使用Linux系统后一直难以忘怀，不论是系统的流畅度，还是里面包含的geek精神，都留给了我很深的印象。所以想趁考完试离下一门考试还有段时间的空隙，就拿虚拟机来折腾折腾Linux系统......
<!--more-->
## 虚拟机安装Linux系统
虚拟机我用的是VMware 12，Linux系统我用的是比较流行的桌面发行版Ubuntu，版本是Ubuntu16.04 LTS。
VMware 12网上有很多下载源，也可以[私信我](http://zlw.poker/about/)哦。Ubuntu到中国官网下载[cn.ubuntu.com](http://cn.ubuntu.com/download/)。安装过程不再详述，网上有很多非常详细的过程。（例如，[VMware12安装虚拟机教程、Ubuntu16.04安装教程](http://jingyan.baidu.com/article/c275f6ba07e269e33d756714.html)）

遇到的问题：
安装完Ubuntu后重启，结果一直黑屏。（实际上屏幕有一两行英文，忘记写的什么了）
原因：
现在电脑大都是独立显卡，虚拟机设置里面有一项根据我们的主机情况默认勾选了加速3D图像这个选项，去掉勾选即可解决。

安装完后最好在Ubuntu里安装Vmware Tools，这样不仅可以实现主机和虚拟机之间的文件共享，同时支持自由拖拽的功能，鼠标也可以在主机和虚拟机之间自由移动（不用再按Ctrl+Alt了），还可以令虚拟机屏幕全屏化。安装教程不在赘述，点[这里](http://jingyan.baidu.com/article/3065b3b6e8dedabecff8a435.html)。
## Ubuntu软件安装
### 系统更新
安装完系统后，需要一些更新补丁，Ctrl+AltT调出终端，执行：
```
sudo apt-get update
sudo apt-get upgrade
```

### 卸载LibreOffice
LibreOffice是Ubuntu自带的开源office软件，但是并不好用，选择用WPS代替
```
sudo apt-get remove libreoffice-common
```

### 删除Amazon链接
```
sudo apt-get remove unity-webapps-common
```

### 修改hosts
教程见[修改hosts文件实现翻墙](http://zlw.poker/2016/12/19/%E4%BF%AE%E6%94%B9hosts%E6%96%87%E4%BB%B6%E5%AE%9E%E7%8E%B0%E7%BF%BB%E5%A2%99/)
Ubuntuhosts位置在`/etc/hosts`，修改需要root权限。
```
sudo su   //获取root权限
sudo gedit /etc/hosts   //用gedit打开hosts文件，粘贴进去保存即可
exit     //退出root权限
```

### 安装搜狗输入法
首先下载[搜狗拼音deb安装包](http://pinyin.sogou.com/linux/)，然后执行
```
cd 路径   //进入安装包所在文件夹，也可以到文件夹空白处点击右键-终端
sudo dpkg -i 安装包全名   //包括后缀
```
可能安装报错，可以先执行下面语句安装依赖后再执行
```
sudo apt-get install -f   //修复依赖
```
然后修改系统默认输入法：系统设置-语言支持-将ibus改成fctix，添加或删除语言-找到Chinese（simple）-应用到整个系统，然后重启即可。

提示：
切换输入法是Ctrl+Space，跟Windows略有不同。
### 安装Chrome
因为进chrome官网下载没成功（头疼，无论Windows还是Linux上官网安装都不行），所以找安装包自己安装，然后更新到最新版Chrome。

推荐下载地址：[pkgs.org](https://pkgs.org/debian-ubuntu/google-amd64/google-chrome-stable_55.0.2883.87-1_amd64.deb.html)，也可以[私信我](http://zlw.poker/about/)哦<(￣ˇ￣)/

下载完后执行
```
cd  所在目录
sudo dpkg -i 全名
```
最后在dash中搜索chrome，点击运行，右键点击将其固定在启动器上（相当于Windows任务栏）。

### 安装Vim
执行
```
sudo apt-get install vim
```
### 安装eclipse
可以直接执行
```
sudo apt-get install eclipse
```
一键搞定，不过版本稍旧。

也可以到官网下载最新的eclipse linux安装包，自己安装，配置环境变量。
**安装JDK**
JAVA JDK有两个版本，一个是开源版本openJDK，一个是oracle官方版本JDK
安装openJDK：
```
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```
安装oracle Java JDK：
```
sudo apt-get install python-software-properties
sudo add-get-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```
查看Java 版本：
```
java -version
```
如果你两个jdk都安装了，可以执行下面语句切换jdk：
```
sudo update-alternatives --config java
```
**配置环境变量**
```
sudo gedit /etc/profile
```
如果使用openjdk，则添加
```
export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64/jre/bin"
```
如果使用oracle jkd
```
export JAVA_HOME="/usr/lib/jvm/java-8-oracle/jre/bin"
```
**安装eclipse**
下载[eclipse安装器](http://www.eclipse.org/downloads/)

解压并允许其中的eclipse-inst
```
tar xzvf eclipse-inst-linux64.tar.gz
cd eclipse-installer
./eclipse-inst
```

选择安装类型，选择安装路径

创建快捷菜单项（这个不知道为什么没成功╮（╯＿╰）╭，试了好几种方法）
创建快捷方式文件：
```
vim .local/share/applications/eclipse.desktop
```
写入如下内容：
```
[Desktop Entry]
Name=Eclipse
Type=Application
Exec=/home/eclipse/java-neon/eclipse/eclipse
Terminal=false
Icon=/home/eclipse/java-neon/eclipse/icon.xpm
Comment=Integrated Development Environment
NoDisplay=Development;IDE;
Categoreis=Development;IDE;
Name[en]=Eclipse
```
Exec和Icon分别eclipse文件夹下eclipse文件和icon.xpm文件的路径。


### 安装wineQQ
Linux下安装QQ一直是个很头疼的问题，因为腾讯一直不开发QQ Linux版，网页版QQ又做得烂，完全没法用，强烈吐槽腾讯。
安装方法大概有几种：安装安卓模拟器，安装虚拟机（Windows系统），安装wineQQ等等。

我试过好几种方法，觉得安装wineQQ国际版最简单，用着也还不错，基本的聊天传输文件都没问题：
首先下载[wineQQ国际版](http://www.ubuntukylin.com/application/show.php?lang=cn&id=279)，
进入安装包所在目录依次执行：
```
sudo dpkg -i fonts-wqy-microhei_0.2.0-beta-2_all.deb
sudo dpkg -i ttf-wqy-microhei_0.2.0-beta-2_all.deb
sudo dpkg -i wine-qqintl_0.1.3-2_i386.deb
```
注意：由于有依赖关系，所以必须按照上面的顺序执行命令。
如果最后一个包安装失败了，可以先执行下面这条命令再试：
```
sudo apt-get install -f
```
安装完毕后在dash搜索qq即可。

缺点：安装完后，可能会有软件依赖冲突，如果有依赖冲突则无法安装其他软件。查看命令：
```
sudo dpkg --configure -a
```
有冲突则只能卸载wineQQ寻求其他安装方法了。
卸载wine-qqintl：
```
sudo dpkg -r wine-qqintl
```
我的没有冲突，耶~

### 安装WPS
安装WPS很头疼，一个国内公司，服务器也在国内，下载速度竟然极其的慢。不到100M大小，我下了三个小时还没下载完，最后弃之，选择网上找安装包下载。如果嫌弃下载慢的同学可以[私信我](http://zlw.poker/about/)。
进入安装包所在目录执行
```
sudo dpkg -i 全名
```
然后dash搜索WPS即可。
### 安装shutter
shutter是一款比较好用的截图软件。
```
sudo apt-get install shutter
```
当然也可以用Ubuntu自带的截图功能。
### 安装Git
执行
```
sudo apt-get install git
```
### 搭建Hexo框架
搭建Hexo博客框架需要先安装Git和node.js。

**安装node.js**

安装前先安装g++
```
sudo apt-get install g++
```

然后到[node.js官网](https://nodejs.org/en/download/)下载Source Code（源码！！！）。依次执行
```
./configure
make
sudo make install
```
注意：make过程较漫长，建议先泡杯茶，嘻嘻。
期间执行第三条命令时出了问题，可以执行
```
sudo apt install nodejs-legacy   //注意apt
```
查看是否安装好node.js
```
node -v  //查看node.js版本
```

**安装npm**
```
curl http://npmjs.org/install.sh | sudo sh
```
安装这个时也出现问题，改执行
```
sudo apt install npm
```

**安装Hexo**
```
sudo npm install hexo -g
```

**创建博客**
建一个目录存放博客，然后进入目录，依次执行
```
hexo init
npm install
```
然后具体步骤见[如何用github和hexo搭建个人博客](http://zlw.poker/2016/10/19/%E5%A6%82%E4%BD%95%E7%94%A8github%E5%92%8Chexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)
注：上传至github pages还需要安装：
```
npm install hexo-deployer-git --save
```
### 安装最新版Code::Blocks
```
sudo add-apt-repository ppa:damien-moore/codeblocks-stable
sudo apt update
sudo apt install codeblocks build-essential
```

卸载
```
sudo apt remove codeblocks
sudo apt autoremove
```
### 安装为知笔记
```
sudo apt-get-repository ppa:wiznote-team
sudo apt-get update
sudo apt-get install wiznote
```
### 安装迅雷
安装包在[这里](http://forum.ubuntu.org.cn/viewtopic.php?t=461341)

[XwareDesktop官方使用说明](https://github.com/Xinkai/XwareDesktop/wiki/%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)


目前就只安装了这些，以后有其他安装在慢慢更新。。。

------------------------
参考文档：
- [vmware workstation 12安装ubuntu kylin 16.04虚拟机](http://www.linuxdiyf.com/linux/20066.html)
- [独立显卡笔记本 VMware 安装 ubuntu开机启动后卡住不动黑屏 终极解决办法](http://blog.csdn.net/chutianya/article/details/45954271)
- [如何给Ubuntu 安装Vmware Tools](http://jingyan.baidu.com/article/3065b3b6e8dedabecff8a435.html)
- [VMware12安装虚拟机教程、Ubuntu16.04安装教程](http://jingyan.baidu.com/article/c275f6ba07e269e33d756714.html)
- [ubuntu16.04主题美化和软件推荐](http://zcheng.ren/2016/08/21/ubuntuSoftware/)
- [Ubuntu 16.04安装搜狗输入法](http://www.th7.cn/system/lin/201604/163024.shtml)
- [搜狗输入法 for linux 安装指南](http://pinyin.sogou.com/linux/help.php)
- [Ubuntu 16.04 安装QQ解决方案](http://www.2cto.com/os/201607/529804.html)
- [ubuntu下使用hexo搭建博客](http://blog.csdn.net/kesarchen/article/details/50579550)
- [手把手教你在Ubuntu平台搭建 Github + Hexo 免费博客](http://www.tuicool.com/articles/biaeYfV)
- [Ubuntu 16.04安装Eclipse](http://www.linuxidc.com/Linux/2016-07/133482.htm)
- [Ubuntu16.04 安装Java JDK](http://topspeedsnail.com/ubuntu16-install-java-jdk/)
- [Ubuntu16.04 安装Eclipse](http://blog.topspeedsnail.com/archives/4813/)
- [Ubuntu16.04 安装Code::Blocks](http://blog.topspeedsnail.com/archives/4808/)
- [Linux原生迅雷下载 for ubuntu（新增ubuntu16.04 64位安装包）](http://forum.ubuntu.org.cn/viewtopic.php?t=461341)


