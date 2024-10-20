# 如何找到 SMTP 地址？-教程 ElderNode 博客

> 原文：<https://blog.eldernode.com/how-to-find-the-smtp-address/>

![How to find the SMTP address](img/7d149624db5748365614fc5e6e8a54b8.png)

如何找到 SMTP 地址？ [SMTP](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) ( **简单邮件传输协议**)是用于发送和接收电子邮件的 TCP/ IP 协议。由于邮件存储限制，该协议通常与 [POP3](https://en.wikipedia.org/wiki/Post_Office_Protocol) ( **邮局协议 3** )或 [IMAP](https://en.wikipedia.org/wiki/Internet_Message_Access_Protocol#:~:text=In%20computing%2C%20the%20Internet%20Message,is%20defined%20by%20RFC%203501.) ( **互联网消息访问协议**)协议一起使用。

今天，要使用 SMTP 服务作为电子邮件发送者协议，您需要知道它的地址和发送端口。

有几种方法可以得到 SMTP。在这篇文章中，你会学到找到 SMTP 的最简单的方法。请继续关注本文的其余部分。

***[【廉价 VPS】用比特币托管，在 Eldernode](https://eldernode.com/vps-hosting/)*** 完善货币

### 了解如何查找 SMTP 地址

**1。**T3 首先打开你的 **Windows 命令提示符**或 CMD 。

注意:从开始菜单打开 CMD ，**输入** CMD 搜索后打开。

也可以打开**运行窗口** ( Winkey + R )打开 CMD、 **type** cmd 点击 OK 。

**2。** 然后输入以下命令进入 [nslookup](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc725991(v=ws.11)?redirectedfrom=MSDN) 环境:

```
nslookup
```

Nslookup 是一个 DNS 搜索工具，可以让你找到在任何域名的 DNS 中创建的记录。

其中一个 DNS 记录是 MX ，意思是邮件交换，表示邮件服务的地址。

**3。** 进入 **Nslookup 环境**后，输入以下命令设置在 MX 上收到的响应类型:

```
set type=mx
```

**4。** 进入上面的**命令**后，可以进入想要接收的域记录和 MX 地址。

**例如**，在这一节我们输入 Gmail.com 的*域。你会在下面看到收到的回答。*

*变成蓝色的部分是 MX 记录，或者 SMTP 地址。*

```
*`> gmail.com  Server: UnKnown  Address: 8.8.8.8    Non-authoritative answer:  gmail.com MX preference = 10, mail exchanger = alt1.gmail-smtp-in.l.google.com  gmail.com MX preference = 20, mail exchanger = alt2.gmail-smtp-in.l.google.com  gmail.com MX preference = 40, mail exchanger = alt4.gmail-smtp-in.l.google.com  gmail.com MX preference = 30, mail exchanger = alt3.gmail-smtp-in.l.google.com  gmail.com MX preference = 5, mail exchanger = gmail-smtp-in.l.google.com`*
```

*同样，您可以找到并使用您的站点或您想到的任何其他站点的 SMTP 地址。*

***尊敬的用户**，我们希望您能喜欢这个[教程](https://eldernode.com/category/tutorial/)，您可以在评论区提出关于本次培训的问题，或者解决[老年人节点培训](https://eldernode.com/blog/)领域的其他问题，请参考[提问页面](https://eldernode.com/ask)部分，并尽快提出您的问题。腾出时间给其他用户和专家来回答你的问题。*

*好运。*