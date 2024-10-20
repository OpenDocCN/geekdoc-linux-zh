# 如何在 Linux 下测试网站加载速度——测试网站响应时间

> 原文：<https://blog.eldernode.com/test-website-loading-speed-linux/>

![How to test website loading speed in Linux](img/5e0c018ae1a185061e21e731f526cfe6.png)

一个 Linux 系统管理员需要知道一些 Linux 技巧。在这篇文章中，你将学习如何在 Linux 下测试网站加载速度。《极品飞车》不仅仅是一款游戏，它对于用户和服务器管理员来说都是非常重要和严肃的。

## 如何在 Linux 下测试网站加载速度

在下面，我们将向你展示如何从 Linux 的命令行测试网站的响应时间。您将看到如何以秒为单位检查时间，这需要:

**1- To** 执行名称解析。

**2-用于**到服务器的 TCP 连接。

**3- For** 文件传输开始。

**4-用于**要传输的第一个字节。

**5-用于完成**操作。

此外，对于启用 HTTPS 的站点，您将验证如何测试时间，以秒为单位，它需要:

*   为了重定向
*   到服务器的 SSL 连接/握手(待完成)

[购买 Linux 虚拟私有服务器](https://eldernode.com/linux-vps/)

当然，你知道 **cURL** ，是的，这是一个强大的命令行工具，可以在使用文件、FTP、FTPS、HTTP、HTTPS 和许多其他协议的服务器之间传输数据。但是我们将向你学习一个鲜为人知的功能。

完成一个操作后，你可以使用 **cURL** 、 -w 这个有用的选项，在 stdout 上打印信息。这意味着您可以使用一些与时间相关的变量来测试上面列出的网站的不同响应时间。

```
$ curl -s -w 'Testing Website Response Time for :%{url_effective}\n\nLookup Time:\t\t%{time_namelookup}\nConnect Time:\t\t%{time_connect}\nPre-transfer Time:\t%{time_pretransfer}\nStart-transfer Time:\t%{time_starttransfer}\n\nTotal Time:\t\t%{time_total}\n' -o /dev/null http://www.google.com
```

您可能会看到变量，我们会查看它们的格式:

**time _ name lookup**–从开始到名称解析完成所用的时间，以秒为单位。

**time _ connect**–从开始到与远程主机(或代理)的 TCP 连接完成所用的时间，以秒为单位。

**time _ pre transfer**–从开始到文件传输即将开始所用的时间，以秒为单位。

**time _ start transfer**–从开始到第一个字节即将被传输所花费的时间(以秒为单位)。

**time _ total**–完整操作持续的总时间，以秒为单位(毫秒级分辨率)。

如果格式太长，你也可以使用下面的语法。

```
$ curl -s -w "@format.txt" -o /dev/null https://www.google.com
```

在上面的命令中，标志:

*   告诉 curl 安静地工作。
*   -w–在 stdout 上打印信息。
*   -o–用于重定向输出(这里我们通过将输出重定向到 **/dev/null** 来丢弃输出)。

对 HTTPS 站点运行以下命令。

```
$ curl -s -w 'Testing Website Response Time for :%{url_effective}\n\nLookup Time:\t\t%{time_namelookup}\nConnect Time:\t\t%{time_connect}\nAppCon Time:\t\t%{time_appconnect}\nRedirect Time:\t\t%{time_redirect}\nPre-transfer Time:\t%{time_pretransfer}\nStart-transfer Time:\t%{time_starttransfer}\n\nTotal Time:\t\t%{time_total}\n' -o /dev/null https://www.google.com
```

同样，在上面的格式中，新的时间变量是:

**time _ app connect**–从开始到远程主机的 SSL 连接/握手完成所用的时间，以秒为单位。

**time _ redirect**–在最终事务开始之前，包括名称查找、连接、预传输和传输在内的所有重定向步骤所用的时间，以秒为单位；它计算多个重定向的完整执行时间。

**注** :

**1-** 由于响应时间值总是变化的，所以试着收集几个值并获得平均速度，因为你运行不同的测试。

从上面命令的结果可以看出，通过 HTTP 访问网站比通过 HTTPS 要快得多。

建议您访问 cURL 手册页以获得更多信息。

```
man curl
```

#### 结论

#### 如果您没有收到合适的结果，您需要考虑在您的服务器上或代码中进行一些调整。

跟随 [Linux 小技巧](https://eldernode.com/tag/linux-tricks/) 你可能会更见多识广。

亲爱的用户，我们希望你会喜欢这个如何在 Linux 中测试网站加载速度的教程，你可以在评论区提出关于这个培训的问题，或者解决 [Eldernode](https://eldernode.com/) 培训领域的其他问题，请参考 [提问页面](https://eldernode.com/ask) 部分并在其中提出你的问题。

Dear user, we hope you would enjoy this tutorial how to test website loading speed in Linux, you can ask questions about this training in the comments section, or to solve other problems in the field of [Eldernode](https://eldernode.com/) training, refer to the [Ask page](https://eldernode.com/ask) section and raise your problems in it.