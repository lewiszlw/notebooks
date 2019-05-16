---
title: 在Eclipse中使用Maven构建Java Web项目
date: 2017-06-18 08:54:02
tags: [JavaWeb,Maven,Eclipse]
categories: blog
---
Maven是一个项目管理和综合工具。Maven提供了开发人员构建一个完整的生命周期框架。开发团队可以自动完成项目的基础工具建设，Maven使用标准的目录结构和默认构建生命周期。
在多个开发团队环境时，Maven可以设置按标准在非常短的时间里完成配置工作。由于大部分项目的设置都很简单，并且可重复使用，Maven让开发人员的工作更轻松，同时创建报表，检查，构建和测试自动化设置。
<!--more-->
# 安装Maven
最新的Eclipse Neon 3（Eclipse IDE for Java EE Developers）集成了Maven插件。

也可以在线安装：Help--&gt;Install New Solfware--&gt;add，输入地址[http://m2eclipse.sonatype.org/sites.m2e](http://m2eclipse.sonatype.org/sites.m2e)，勾上选项，等待下载安装，重启即可。

# 使用Maven构建Java Web项目
在Eclipse中依次点击：File--&gt;New--&gt;--&gt;Maven Project，点击next，然后选中maven-archetype-webapp，依次填写Group Id和Artifact Id，点击Finish完成。

# 目录结构
![在Eclipse中使用Maven构建Java Web项目1](http://ofolh8dcq.bkt.clouddn.com/%E5%9C%A8Eclipse%E4%B8%AD%E4%BD%BF%E7%94%A8Maven%E6%9E%84%E5%BB%BAJava%20Web%E9%A1%B9%E7%9B%AE1.png)

- pom.xml：Maven配置文件
- src/main/：项目主体目录
  - src/main/java/：项目源代码目录
  - src/main/resource/：所需资源目录
  - src/main/filters/：资源过滤文件目录
  - src/main/webapp/：web应用目录
- src/test/：项目测试目录
  - src/test/java/：测试代码目录
  - src/test/resource/：测试所需资源目录
  - src/test/filters/：测试资源过滤文件目录
- src/site/：与site相关的资源目录
- target/：输出目录，存放项目构建后的文件和目录

# 配置文件解析
pom.xml
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.lewiszlw</groupId>
  <artifactId>MavenWeb</artifactId>
  <packaging>war</packaging>
  <version>0.0.1-SNAPSHOT</version>
  <name>MavenWeb Maven Webapp</name>
  <url>http://maven.apache.org</url>

  <!-- 属性配置 -->  
  <properties>  
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
  </properties> 
							    
  <!-- 依赖配置 -->
  <dependencies>
     <!-- 添加JUnit -->
     <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>3.8.1</version>
        <scope>test</scope>
        </dependency>

     <!-- 添加Servlet -->
	 <dependency>
	    <groupId>javax.servlet</groupId>
	    <artifactId>servlet-api</artifactId>
	    <version>2.5</version>
	    <scope>provided</scope>
	 </dependency>
  </dependencies>
  <build>
     <finalName>MavenWeb</finalName>
  </build>
</project>
```
