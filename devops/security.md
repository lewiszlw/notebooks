# CSRF（跨站请求伪造）
跨站请求伪造Cross-site request forgery（简称CSRF, 读作 [sea-surf]）是一种典型的利用cookie-session漏洞的攻击。

举例：
1. 假设你经常使用bank.example.com进行网上转账
2. 你图方便没有登出bank.example.com，随后又访问了一个恶意网站
3. 当你进行了相关操作（如提交按钮），恶意网站的页面可能使用Javascript自动完成提交。尽管恶意网站没有办法盗取你的session cookie（从而假冒你的身份），但恶意网站向bank.example.com发起请求时，你的cookie会被自动发送过去。

# xxx