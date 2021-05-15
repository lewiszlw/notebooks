# 响应状态码302、303、307区别
302 Found、303 See Other、307 Temporary Redirect 都是临时重定向。

302 状态码表示目标资源临时移动到了另一个 URI 上。由于重定向是临时发生的，所以客户端在之后的请求中还应该使用原本的 URI。服务器会在响应 Header 的 Location 字段中放上这个不同的 URI。浏览器可以使用 Location 中的 URI 进行自动重定向。

303 状态码表示服务器要将浏览器重定向到另一个资源，这个资源的 URI 会被写在响应 Header 的 Location 字段。从语义上讲，重定向到的资源并不是你所请求的资源，而是对你所请求资源的一些描述。303 常用于将 POST 请求重定向到 GET 请求，比如你上传了一份个人信息，服务器发回一个 303 响应，将你导向一个“上传成功”页面。不管原请求是什么方法，重定向请求的方法都是 GET（或 HEAD，不常用）。

307 的定义实际上和 302 是一致的，唯一的区别在于，307 状态码不允许浏览器将原本为 POST 的请求重定向到 GET 请求上。

**区别**：302 允许各种各样的重定向，一般情况下都会实现为到 GET 的重定向，但是不能确保 POST 会重定向为 POST；而 303 只允许任意请求到 GET 的重定向；307 和 302 一样，除了不允许 POST 到 GET 的重定向。

# 响应状态码301、302区别 
301 Moved Permanently，永久重定向，表示目标资源已被分配一个新的永久URI和任何未来对此资源的引用应该使用其中一个附带的URI。默认可缓存，意为者后续请求会直接请求新的URI，而不会再请求当前URI然后重定向过去。

302 Found，临时重定向，表示目标资源临时驻留在不同的URI下。由于重定向有时可能会被更改，因此客户端应该继续使用有效请求URI来处理将来的请求。

# HTTP2.0特点
- Server Push（当一个客户端请求资源X，而服务器知道它很可能也需要资源Z的情况下，服务器可以在客户端发送请求前，主动将资源Z推送给客户端）
- 多路复用的单一长连接：单一长连接（减少TCP握手带来的时延）、多路复用（HTTP2虽然只有一条TCP连接，但是在逻辑上分成了很多stream）
- 新的二进制格式和头部压缩

# HTTPS连接过程
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/protocol/HTTPS%E8%BF%9E%E6%8E%A5%E8%BF%87%E7%A8%8B.png)

1.建立连接获取证书

SSL 客户端通过 TCP 和服务器建立连接之后（443 端口），并且在一般的 tcp 连接协商（握手）过程中请求证书。即客户端发出一个消息给服务器，这个消息里面包含了自己可实现的算法列表和其它一些需要的消息，SSL 的服务器端会回应一个数据包，这里面确定了这次通信所需要的算法，然后服务器向客户端返回证书。（证书里面包含了服务器信息：域名。申请证书的公司，公共秘钥）。

2.证书验证

Client 在收到服务器返回的证书后，判断签发这个证书的公共签发机构，并使用这个机构的公共秘钥确认签名是否有效，客户端还会确保证书中列出的域名就是它正在连接的域名。

3.数据加密和传输

如果确认证书有效，那么生成对称秘钥并使用服务器的公共秘钥进行加密。然后发送给服务器，服务器使用它的私钥对它进行解密，这样两台计算机可以开始进行对称加密进行通信。


# HTTP与HTTPS区别
1. 端口，HTTP端口是80，HTTPS端口是443
2. 是否加密，HTTP报文是明文的，HTTPS报文是加密的

# HTTP报文构成
**请求报文**
![HttpRequest](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/protocol/HttpRequest.jpg)

示例
```
GET / HTTP/1.1
Host: www.baidu.com
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.143 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Referer: https://www.baidu.com/s?wd=HTTP%20%E5%8D%8F%E8%AE%AE%E6%9C%89%E5%87%A0%E7%A7%8D%E5%92%8C.....
Accept-Encoding: gzip, deflate, sdch, br
Accept-Language: zh-CN,zh;q=0.8
Cookie: BIDUPSID=670A04B660AAF2716D3120BEAF946A11; BAIDUID=2454D4....
RA-Ver: 3.0.8
RA-Sid: CA623F7A-20150914-060054-2b9722-5fde41
```

| 请求头 |	含义 |
|-------|-------|
| User-Agent |	产生请求的浏览器类型，User-Agent请求报头域允许客户端将它的操作系统、浏览器和其它属性告诉服务器 |
|Accept	| 客户端可识别的响应内容类型列表。eg：Accept：image/gif，表明客户端希望接受GIF图象格式的资源；Accept：text/html，表明客户端希望接受html文本。 |
|Accept-Language |	客户端可接受的自然语言 |
|Accept-chartset|	客户端可接受应答的字符集。eg：Accept-Charset:iso-8859-1,gb2312.如果在请求消息中没有设置这个域，缺省是任何字符集都可以接受。|
|Accept-Encoding |	客户端可接受的编码压缩格式 |
|HOST |	请求的主机名称，允许多个域名同处一个IP之地，即虚拟主机 |
|Connection	| 连接方式（close或keep-alive）|
|Cookie |	存储于客户端扩展字段，向同一域名的服务端发送该域的cookie |
|Authorization |	Authorization请求报头域主要用于证明客户端有权查看某个资源。当浏览器访问一个页面时，如果收到服务器的响应代码为401（未授权），可以发送一个包含Authorization请求报头域的请求，要求服务器对其进行验证。|

**响应报文**
![HttpResponse](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/protocol/HttpResponse.png)

示例
```
HTTP/1.1 200 OK
Server: bfe/1.0.8.18
Date: Wed, 04 Apr 2018 02:39:19 GMT
Content-Type: text/html
Content-Length: 2381
Last-Modified: Mon, 23 Jan 2017 13:27:56 GMT
Connection: Keep-Alive
ETag: "588604dc-94d"
Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
Pragma: no-cache
Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/
Accept-Ranges: bytes

<!DOCTYPE html>
<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>百度一下，你就知道</title></head> <body link=#0000cc> <div id=wrapper> <div id=head> <div class=head_wrapper> <div class=s_form> <div class=s_form_wrapper> <div id=lg> <img hidefocus=true src=//www.baidu.com/img/bd_logo1.png width=270 height=129> </div> <form id=form name=f action=//www.baidu.com/s class=fm> <input type=hidden name=bdorz_come value=1> <input type=hidden name=ie value=utf-8> <input type=hidden name=f value=8> <input type=hidden name=rsv_bp value=1> <input type=hidden name=rsv_idx value=1> <input type=hidden name=tn value=baidu><span class="bg s_ipt_wr"><input id=kw name=wd class=s_ipt value maxlength=255 autocomplete=off autofocus></span><span class="bg s_btn_wr"><input type=submit id=su value=百度一下 class="bg s_btn"></span> </form> </div> </div> <div id=u1> <a href=http://news.baidu.com name=tj_trnews class=mnav>新闻</a> <a href=http://www.hao123.com name=tj_trhao123 class=mnav>hao123</a> <a href=http://map.baidu.com name=tj_trmap class=mnav>地图</a> <a href=http://v.baidu.com name=tj_trvideo class=mnav>视频</a> <a href=http://tieba.baidu.com name=tj_trtieba class=mnav>贴吧</a> <noscript> <a href=http://www.baidu.com/bdorz/login.gif?login&amp;tpl=mn&amp;u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1 name=tj_login class=lb>登录</a> </noscript> <script>document.write('<a href="http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u='+ encodeURIComponent(window.location.href+ (window.location.search === "" ? "?" : "&")+ "bdorz_come=1")+ '" name="tj_login" class="lb">登录</a>');</script> <a href=//www.baidu.com/more/ name=tj_briicon class=bri style="display: block;">更多产品</a> </div> </div> </div> <div id=ftCon> <div id=ftConw> <p id=lh> <a href=http://home.baidu.com>关于百度</a> <a href=http://ir.baidu.com>About Baidu</a> </p> <p id=cp>&copy;2017&nbsp;Baidu&nbsp;<a href=http://www.baidu.com/duty/>使用百度前必读</a>&nbsp; <a href=http://jianyi.baidu.com/ class=cp-feedback>意见反馈</a>&nbsp;京ICP证030173号&nbsp; <img src=//www.baidu.com/img/gs.gif> </p> </div> </div> </div> </body> </html>
```

# HTTP请求方法
1.GET
请求访问特定资源，通常用作查询数据。

2.POST
向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST 请求可能会导致新的资源的建立和/或已有资源的修改。通常用作创建数据。

3.PUT
向指定资源位置上传其最新内容，通常用作更新数据（覆盖）。

4.DELETE
请求服务器删除Request-URL所标识的资源，通常用作删除数据。

5.OPTIONS
返回服务器针对特定资源所支持的HTTP请求方法，也可以利用向web服务器发送‘*’的请求来测试服务器的功能性。

6.HEAD
类似于 GET 请求，只不过返回的响应中没有具体的内容，用于获取报头。

7.PATCH
对已知资源进行局部更新。

# 浏览器输入URL后会发生什么
1.识别URL：协议、域名、端口、文件路径

2.查找本地hosts文件：用来保存域名以及域名对应的IP地址

3.询问本地域名服务器
3.1.询问根域名服务器（返回一级域名服务器IP）
3.2.询问一级域名服务器（如comDNS服务器、orgDNS服务器等，结果返回二级域名服务器IP）
3.3.询问二级域名服务器
...
得到IP地址

4.建立TCP连接
三次握手
HTTPS需要拿到server提供的公钥，然后生成一个随机对称秘钥，用公钥加密对称秘钥发送到server，后续请求用对称秘钥加密。

5.发送HTTP请求

6.服务器处理请求

7.返回响应结果

8.关闭TCP连接

9.浏览器解析HTML
