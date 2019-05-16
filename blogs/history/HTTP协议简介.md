---
title: HTTP协议简介
date: 2017-07-08 19:15:31
tags: [HTTP,计算机网络]
categories: 基础知识
---
HTTP（Hyper Text Transfer Protocol，超文本传输协议）是用于万维网（World Wide Web）服务器与本地浏览器之间数据传输的协议。它是一个基于TCP/IP协议的应用层协议。
<!--more-->
# HTTP简介
![HTTP协议简介](http://ofolh8dcq.bkt.clouddn.com/HTTP%E5%8D%8F%E8%AE%AE%E7%AE%80%E4%BB%8B1.png)

主要特点：
1. 简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。
2. 灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。
3. HTTP 0.9和1.0使用非持续连接：限制每次连接只处理一个请求，服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
4. HTTP 1.1使用持续连接：不必为每个web对象创建一个新的连接，一个连接可以传送多个对象。
5. 无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

**无状态协议：**
协议的状态是指下一次传输可以“记住”这次传输信息的能力。
http是不会为了下一次连接而维护这次连接所传输的信息,为了保证服务器内存。
比如客户获得一张网页之后关闭浏览器，然后再一次启动浏览器，再登陆该网站，但是服务器并不知道客户关闭了一次浏览器。
由于Web服务器要面对很多浏览器的并发访问，为了提高Web服务器对并发访问的处理能力，在设计HTTP协议时规定Web服务器发送HTTP应答报文和文档时，不保存发出请求的Web浏览器进程的任何状态信息。这有可能出现一个浏览器在短短几秒之内两次访问同一对象时，服务器进程不会因为已经给它发过应答报文而不接受第二期服务请求。由于Web服务器不保存发送请求的Web浏览器进程的任何信息，因此HTTP协议属于无状态协议（Stateless Protocol）。

**HTTP协议是无状态的和Connection: keep-alive的区别：**
无状态是指协议对于事务处理没有记忆能力，服务器不知道客户端是什么状态。从另一方面讲，打开一个服务器上的网页和你之前打开这个服务器上的网页之间没有任何联系。
HTTP是一个无状态的面向连接的协议，无状态不代表HTTP不能保持TCP连接，更不能代表HTTP使用的是UDP协议（无连接）。
从HTTP/1.1起，默认都开启了Keep-Alive，保持连接特性，简单地说，当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接。
Keep-Alive不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如Apache）中设定这个时间。

# HTTP之URL
URL（Uniform Resource Locator）, 统一资源定位符,是互联网上用来标识某一处资源的地址。
URL由三部分组成：资源类型、存放资源的主机域名、资源文件名。
URL的一般语法格式为：protocol :// hostname[:port] / path / [;parameters][?query]#fragment
1. protocol：协议。如http、https、ftp、file等。
2. hostname：主机名。如www.baidu.com，有时，在主机名前也可以包含连接到服务器所需的用户名和密码（格式：username:password@hostname）。
3. port：端口。可选，省略时使用方案的默认端口，如http默认端口为80。
4. path：路径。由零或多个“/”符号隔开的字符串，一般用来表示主机上的一个目录或文件地址。
5. parameters：参数。用于指定特殊参数的可选项。
6. query：查询。可选，用于给动态网页（如使用CGI、ISAPI、PHP/JSP/ASP/ASP。NET等技术制作的网页）传递参数，可有多个参数，用“&”符号隔开，每个参数的名和值用“=”符号隔开。
7. fragment：信息片段/锚点。字符串，用于指定网络资源中的片断。例如一个网页中有多个名词解释，可使用fragment直接定位到某一名词解释。

# HTTP消息结构
HTTP是基于客户端/服务端（C/S）的架构模型，通过一个可靠的链接来交换信息，是一个无状态的请求/响应协议。
一个HTTP"客户端"是一个应用程序（Web浏览器或其他任何客户端），通过连接到服务器达到向服务器发送一个或多个HTTP的请求的目的。
一个HTTP"服务器"同样也是一个应用程序（通常是一个Web服务，如Apache Web服务器或IIS服务器等），通过接收客户端的请求并向客户端发送HTTP响应数据。

## 客户端请求消息
客户端发送一个HTTP请求到服务器的请求消息包括以下格式：请求行（request line）、请求头部（header）、空行和请求数据四个部分组成：
![HTTP协议简介2](http://ofolh8dcq.bkt.clouddn.com/HTTP%E5%8D%8F%E8%AE%AE%E7%AE%80%E4%BB%8B2.png)

## 服务器响应消息
HTTP响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文。
![HTTP协议简介3](http://ofolh8dcq.bkt.clouddn.com/HTTP%E5%8D%8F%E8%AE%AE%E7%AE%80%E4%BB%8B3.jpg)

# HTTP请求方法
HTTP1.0定义了三种请求方法： GET, POST 和 HEAD方法。
HTTP1.1新增了五种请求方法：OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法。

序号|方法 |    描述
----|----|-------------------------------------------------
1   |GET |请求指定的页面信息，并返回实体主体
2   |HEAD|类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头
3   |POST|向指定资源提交数据进行处理请求（例如提交表单或者上传文件），数据被包含在请求体中，POST请求可能会导致新的资源的建立和/或已有资源的修改
4   |PUT |从客户端向服务器传送的数据取代指定的文档的内容
5   |DELETE|请求服务器删除指定的页面
6   |CONNECT|HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器
7   |OPTIONS|允许客户端查看服务器的性能
8   |TRACE |回显服务器收到的请求，主要用于测试或诊断

GET和POST请求的区别：
1. 在客户端，GET 方式在通过 URL 提交数据，数据在URL中可以看到；POST 方式，数据放置在HTML HEADER内提交。
2. GET方式提交的数据最多只能有1024字节，而POST则没有此限制。
3. 安全性问题。正如在 1 中提到，使用 GET 的时候，参数会显示在地址栏上，而 POST 不会。所以，如果这些数据是中文数据而且是非敏感数据，那么使用  GET；如果用户输入的数据不是中文字符而且包含敏感数据，那么还是使用 POST 为好。
4. GET用于信息获取，而且应该是安全的和幂等的，POST表示可能修改变服务器上的资源的请求。

安全的和幂等的：所谓安全的意味着该操作用于获取信息而非修改信息。幂等的意味着对同一  URL  的多个请求应该返回同样的结果。完整的定义并不像看起来那样严格。换句话说， GET  请求一般不应产生副作用。从根本上讲，其目标是当用户打开一个链接时，她可以确信从自身的角度来看没有改变资源。比如，新闻站点的头版不断更新。虽然第二次请求会返回不同的一批新闻，该操作仍然被认为是安全的和幂等的，因为它总是返回当前的新闻。反之亦然。 POST 请求就不那么轻松了。POST 表示可能改变服务器上的资源的请求。仍然以新闻站点为例，读者对文章的注解应该通过 POST 请求实现，因为在注解提交之后站点已经不同了（比方说文章下面出现一条注解）。

# HTTP响应头信息

应答头|说明
-----|-----------------------
Allow|服务器支持哪些请求方法（如GET、POST等）
Content-Encoding|文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。Java的GZIPOutputStream可以很方便地进行gzip压缩，但只有Unix上的Netscape和Windows上的IE 4、IE 5才支持它。因此，Servlet应该通过查看Accept-Encoding头（即request.getHeader("Accept-Encoding")）检查浏览器是否支持gzip，为支持gzip的浏览器返回经gzip压缩的HTML页面，为其他浏览器返回普通页面
Content-Length|表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据。如果你想要利用持久连接的优势，可以把输出文档写入 ByteArrayOutputStream，完成后查看其大小，然后把该值放入Content-Length头，最后通过byteArrayStream.writeTo(response.getOutputStream()发送内容
Content-Type|表示后面的文档属于什么MIME类型。Servlet默认为text/plain，但通常需要显式地指定为text/html。由于经常要设置Content-Type，因此HttpServletResponse提供了一个专用的方法setContentType
Date|当前的GMT时间。你可以用setDateHeader来设置这个头以避免转换时间格式的麻烦
Last-Modified|文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。Last-Modified也可用setDateHeader方法来设置
Location|表示客户应当到哪里去提取文档。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302
Refresh|表示浏览器应该在多少时间之后刷新文档，以秒计。除了刷新当前文档之外，你还可以通过setHeader("Refresh", "5; URL=http://host/path")让浏览器读取指定的页面。 注意这种功能通常是通过设置HTML页面HEAD区的＜META HTTP-EQUIV="Refresh" CONTENT="5;URL=http://host/path"＞实现，这是因为，自动刷新或重定向对于那些不能使用CGI或Servlet的HTML编写者十分重要。但是，对于Servlet来说，直接设置Refresh头更加方便。 注意Refresh的意义是"N秒之后刷新本页面或访问指定页面"，而不是"每隔N秒刷新本页面或访问指定页面"。因此，连续刷新要求每次都发送一个Refresh头，而发送204状态代码则可以阻止浏览器继续刷新，不管是使用Refresh头还是＜META HTTP-EQUIV="Refresh" ...＞。 注意Refresh头不属于HTTP 1.1正式规范的一部分，而是一个扩展，但Netscape和IE都支持它
Server|服务器名字。Servlet一般不设置这个值，而是由Web服务器自己设置
Set-Cookie|设置和页面关联的Cookie。Servlet不应使用response.setHeader("Set-Cookie", ...)，而是应使用HttpServletResponse提供的专用方法addCookie

# HTTP状态码
常见的HTTP状态码：

状态码|英文名称|中文描述
------|-------|------------
200   |OK     |请求成功。一般用于GET与POST请求
301   |Moved Permanently|永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替
403   |Forbidden|服务器理解请求客户端的请求，但是拒绝执行此请求
404   |Not Found|服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面
500   |Internal Server Error|服务器内部错误，无法完成请求
503   |Service Unavailable|由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中

# HTTP Content-Type
Content-Type，内容类型，一般是指网页中存在的Content-Type，用于定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件，这就是经常看到一些Asp网页点击的结果却是下载到的一个文件或一张图片的原因。
常见的HTTP Content-Type：

文件扩展名|Content-Type（Mime-Type）
----------|--------------------------
.doc|application/msword
.gif|image/gif
.jpe|image/jpeg
.jpeg|image/jpeg
.png|image/png
.htm|text/html
.html|text/html
.jsp|text/html
.pdf|application/pdf
.avi|vedio/avi
.mp3|audio/mp3


