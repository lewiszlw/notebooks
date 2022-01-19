# SIP 协议简介
会话发起协议（Session Initiation Protocol，缩写SIP）是一个由IETF MMUSIC工作组开发的协议，作为标准被提议用于创建，修改和终止包括视频，语音，即时通信，在线游戏和虚拟现实等多种多媒体元素在内的交互式用户会话。2000年11月，SIP被正式批准成为3GPP信号协议之一，并成为IMS体系结构的一个永久单元。SIP与H.323一样，是用于VoIP最主要的信令协议之一。

# SIP 网络单元
- **用户代理（User Agent）**：SIP用户代理是一个SIP逻辑网络端点，用于创建、发送、接收SIP消息并管理一个SIP会话。SIP用户代理又可分为用户代理客户端UAC（User Agent Client）和用户代理服务端UAS（User Agent Server）。UAC创建并发送SIP请求，UAS接收处理SIP请求，发送SIP响应。
- **代理服务器（Proxy）**：SIP代理服务器（PROXY）在网络上位于SIP UAC和UAS之间，用于帮助UAC和UAS间的消息路由。PROXY也可以执行路由策略控制（比如，检查SIP消息的合法性，确认消息是否允许被路由）。PROXY在转发SIP消息时，可能根据需要修改SIP消息的某些部分。
- **注册服务器（Register）**：SIP注册服务器用于接收SIP注册请求，并保存发送注册请求的UA的位置信息。


# SIP 消息的两种会话模式
- Session Model：会话中，对于消息体内容大于1300字节时，一般采用Session Model
- Pager Model：在Sip消息中，对于消息体不大于1300字节时，一般采用Pager Model

# Session Model
![Session Model 会话流程](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/protocol/SIP-SessionModel%E4%BC%9A%E8%AF%9D%E6%B5%81%E7%A8%8B.png)

以拨打AXE模式隐私号为例
![Session Mode 数据包发送流程](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/protocol/SIP-SessionModel%E6%95%B0%E6%8D%AE%E5%8C%85%E5%8F%91%E9%80%81%E6%B5%81%E7%A8%8B%E5%9B%BE.png)
1. 主叫 发送 INVITE 请求到 代理服务器
2. 代理服务器 返回 100 Connecting/Trying 响应到 主叫
3. 代理服务器 返回 183 振铃响应到 主叫
4. 代理服务器 请求传输 RTP 媒体流给 主叫
5. 代理服务器 返回 200 OK 响应到 主叫
6. 主叫 返回 ACK 请求到 代理服务器，会话建立
7. （代理服务器 发送“请输入分机号，以#键结束”音频流给 主叫）
8. 主叫 输入分机号（DTMF）给 代理服务器
9. 主叫 请求传输 RTP 媒体流给 代理服务器
10. （主叫 与 代理服务器 之间传输媒体流）
11. 主叫 发送 BYT 请求到 代理服务器 终止会话
12. 代理服务器 发送 200 OK 响应给 主叫，会话终止

# Pager Model
在SIP消息中，对于消息体不大于1300字节时，一般采用Pager Model。SIP消息通信中采用MESSAGE方法，MESSAGE方法本身并不建立Dialog，在多数应用中，每条IM消息都是独立的。
![Pager Model 会话流程](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/protocol/SIP-PagerModel%E4%BC%9A%E8%AF%9D%E6%B5%81%E7%A8%8B.png)

# SIP 消息
分三部分组成：
1. 请求行 (request-line) or 状态行 (status-line)
2. 消息头 (header)
3. 正文 (body)

INVITE 请求示例
```
INVITE sip:017821589701@10.224.0.92:5060 SIP/2.0
Via: SIP/2.0/UDP 10.38.17.11:5080;rport;branch=z9hG4bKFUmH6FKDBU8HQ
Max-Forwards: 70
From: <sip:59333141@10.38.17.11>;tag=DUSpe36F2ZH1K
To: <sip:017821589701@10.224.0.92:5060>
Call-ID: a3f93810-eec3-123a-0ca9-0242ac110002
CSeq: 46468446 INVITE
Contact: <sip:59333141@10.38.17.11:5080>
User-Agent: FreeSwitch
Allow: INVITE
Supported: linknat
Allow-Events: talk, hold, conference, refer
Content-Type: application/sdp
Content-Disposition: session
Content-Length: 242
X-FS-Support: update_display,send_info
Remote-Party-ID: <sip:59333141@10.38.17.11>;party=calling;screen=yes;privacy=off

v=0
o=FreeSWITCH 1642028306 1642028307 IN IP4 10.38.17.11
s=FreeSWITCH
c=IN IP4 10.38.17.11
t=0 0
m=audio 16170 RTP/AVP 8 0 101
a=rtpmap:8 PCMA/8000
a=rtpmap:0 PCMU/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-15
a=ptime:20
```

100 Connecting 响应示例
```
SIP/2.0 100 Connecting
Via: SIP/2.0/UDP 10.38.17.11:5080;rport=5080;branch=z9hG4bKFUmH6FKDBU8HQ
From: <sip:59333141@10.38.17.11>;tag=DUSpe36F2ZH1K
To: <sip:017821589701@10.224.0.92:5060>
Call-ID: a3f93810-eec3-123a-0ca9-0242ac110002
CSeq: 46468446 INVITE
Content-Length: 0
```

183 Session Progress 响应示例
```
SIP/2.0 183 Session Progress
Via: SIP/2.0/UDP 10.38.17.11:5080;rport=5080;branch=z9hG4bKFUmH6FKDBU8HQ
From: <sip:59333141@10.38.17.11>;tag=DUSpe36F2ZH1K
To:  <sip:017821589701@10.224.0.92:5060>;tag=727C7DE6-61DF9C3C00036919-A6BFD700
Call-ID: a3f93810-eec3-123a-0ca9-0242ac110002
CSeq: 46468446 INVITE
Allow: INVITE,ACK,OPTIONS,BYE,CANCEL,REGISTER,INFO,PRACK,SUBSCRIBE,NOTIFY,UPDATE,MESSAGE,REFER
Contact: <sip:017821589701@10.224.0.92:5060;transport=udp>
Content-Type: application/sdp
Content-Length: 216

v=0
o=- 290046927 290046927 IN IP4 10.224.0.92
s=-
c=IN IP4 10.224.0.92
t=0 0
m=audio 10082 RTP/AVP 8 101
a=rtpmap:8 PCMA/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-15
a=sendrecv
a=direction:both
```

## 请求行
格式：Method Request-URI SIP-Version CRLF

### 请求方法
- REGISTER：注册联系信息
- INVITE：发起会话请求
- ACK：INVITE请求的响应的确认
- CANCEL：取消请求
- BYE：终结会话
- OPTIONS：查询服务器能力
- MESSAGE：RFC3428对 SIP 协议的扩展，增加了MESSAGE方法。采用Pager Model进行通信，传递不超过1300字节的数据。MESSAGE方法详细可参考 “SIP-RFC3428” https://tools.ietf.org/html/rfc3428

## 状态行
格式： SIP-Version Status-Code Reason-Phrase CRLF

### 状态码
- 1xx：临时响应，表示请求消息正在被处理
- 2xx：成功响应，表示请求已被成功接收完全理解并接收
- 3xx：重定向响应，表示需采取进一步完成请求
- 4xx：客户机错误，表示请求消息中包含语法错误信息或服务器无法完成客户机的请求
- 5xx：服务器错误，表示服务器无法合法完成请求
- 6xx：全局故障，表示任何服务器都无法完成该请求

常用状态码
- 100 试呼叫（Trying）
- 180 振铃（Ringing）
- 181 呼叫正在前转（Call is Being Forwarded）
- 200 成功响应（OK）
- 302 临时迁移（Moved Temporarily）
- 400 错误请求（Bad Request）
- 401 未授权（Unauthorized）
- 403 禁止（Forbidden）
- 404 用户不存在（Not Found）
- 408 请求超时（Request Timeout）
- 480 暂时无人接听（Temporarily Unavailable）
- 486 线路忙（Busy Here）
- 504 服务器超时（Server Time-out）
- 600 全忙（Busy Everywhere）

## 消息头
- Call-ID：由本地设备(Client)生成，全局唯一，每次呼叫这个值唯一不变
- From：表示请求的发起者
- To：表示请求的接收者
- Via：Via头域是被服务器插入request中，用来检查路由环的，并且可以使response根据via找到返回的路	
- Max-Forwards：用于表示这个包最多可以传送多少跳，每经过一跳都会减一当Max-Forwards==0系统会返回483。默认为70
- Contact：包含源的URI信息，用来给响应方直接和源建立连接用
- Content-Type：指明消息体的类型（SDP会话描述协议）
- Content-Length：指明消息体的字节大小