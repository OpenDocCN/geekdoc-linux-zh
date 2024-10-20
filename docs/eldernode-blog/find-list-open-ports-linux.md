# 找出 Linux - Eldernode 中所有开放端口的列表

> 原文：<https://blog.eldernode.com/find-list-open-ports-linux/>

![Find out list of all Open Ports in Linux](img/cdbc3a7854e2dedec72d203fa24b7c72.png)

一个 Linux 系统管理员需要知道一些 Linux 技巧。在本文中，您将学习如何找出 Linux 中所有开放端口的列表。

在计算机网络中，更确切地说，在软件术语中，端口是一个逻辑实体，它充当通信的端点，以标识在 [Linux vps](https://eldernode.com/linux-vps/) 操作系统上的给定应用程序或进程。它是一个 16 位数字( **0** 到 **65535** )，用于区分终端系统上的不同应用。

两个最流行的[互联网传输协议](http://www.nyu.edu/classes/jcf/g22.2262-001_sp10/slides/session9/TheInternetTransportProtocols-TCP&UDP.pdf)、**传输控制协议** ( **TCP** )和**用户数据报协议** ( **UDP** )以及其他不太为人所知的协议使用端口号进行通信会话。

此外，IP 地址、端口和协议(如 **TCP/UDP** )的组合被称为套接字，每个服务必须有一个唯一的套接字。

## 如何找出 Linux 中所有开放端口的列表

加入我们来讨论计算机网络中的端口，然后转到如何**列出 Linux 中所有开放的端口**。

让我们看一下不同类别的端口:

1.  0-1023–众所周知的端口，也称为系统端口。
2.  1024-49151–注册端口，也称为用户端口。
3.  49152-65535–动态端口，也称为专用端口。

您可以使用以下命令查看 Linux 中的 **/etc/services** 文件中不同应用程序和端口/协议组合的列表。

```
cat /etc/services   OR  cat /etc/services | less
```

网络服务和端口

```
# /etc/services:  # $Id: services,v 1.48 2009/11/11 14:32:31 ovasik Exp $  #  # Network services, Internet style  # IANA services version: last updated 2009-11-10  #  # Note that it is presently the policy of IANA to assign a single well-known  # port number for both TCP and UDP; hence, most entries here have two entries  # even if the protocol doesn't support UDP operations.  # Updated from RFC 1700, ``Assigned Numbers'' (October 1994).  Not all ports  # are included, only the more common ones.  #  # The latest IANA port assignments can be gotten from  #       http://www.iana.org/assignments/port-numbers  # The Well Known Ports are those from 0 through 1023\.  # The Registered Ports are those from 1024 through 49151  # The Dynamic and/or Private Ports are those from 49152 through 65535  #  # Each line describes one service, and is of the form:  #  # service-name  port/protocol  [aliases ...]   [# comment]    tcpmux          1/tcp                           # TCP port service multiplexer  tcpmux          1/udp                           # TCP port service multiplexer  rje             5/tcp                           # Remote Job Entry  rje             5/udp                           # Remote Job Entry  echo            7/tcp  echo            7/udp  discard         9/tcp           sink null  discard         9/udp           sink null  systat          11/tcp          users  systat          11/udp          users  daytime         13/tcp  daytime         13/udp  qotd            17/tcp          quote  qotd            17/udp          quote  msp             18/tcp                          # message send protocol  msp             18/udp                          # message send protocol  chargen         19/tcp          ttytst source  chargen         19/udp          ttytst source  ftp-data        20/tcp  ftp-data        20/udp  # 21 is registered to ftp, but also used by fsp  ftp             21/tcp  ftp             21/udp          fsp fspd  ssh             22/tcp                          # The Secure Shell (SSH) Protocol  ssh             22/udp                          # The Secure Shell (SSH) Protocol  telnet          23/tcp  telnet          23/udp
```

要列出 Linux 中所有打开的端口或当前正在运行的端口，包括 **TCP** 和 **UDP** ，您将使用 netstat，这是一个用于监控网络连接和统计的强大工具。

```
**netstat -lntu**    Proto Recv-Q Send-Q Local Address               Foreign Address             State        tcp        0      0 0.0.0.0:22                  0.0.0.0:*                   LISTEN        tcp        0      0 0.0.0.0:3306                0.0.0.0:*                   LISTEN        tcp        0      0 0.0.0.0:25                  0.0.0.0:*                   LISTEN        tcp        0      0 :::22                       :::*                        LISTEN        tcp        0      0 :::80                       :::*                        LISTEN        tcp        0      0 :::25                       :::*                        LISTEN        udp        0      0 0.0.0.0:68                  0.0.0.0:*
```

在哪里，

1.  -l–仅打印监听套接字
2.  -n–显示端口号
3.  -t–启用 tcp 端口列表
4.  -u–启用 udp 端口列表

请考虑您也可以使用 **ss** 命令，这是一个众所周知的用于在 Linux 系统中检查套接字的实用程序。运行下面的命令列出所有打开的 [TCP 和 UCP 端口](https://blog.eldernode.com/watch-tcp-udp-ports/):

使用 ss 命令列出所有网络端口

```
ss -lntu    Netid State      Recv-Q Send-Q               Local Address:Port       Peer Address:Port   udp   UNCONN     0      0                    *:68                     *:*       tcp   LISTEN     0      128                  :::22                    :::*       tcp   LISTEN     0      128                  *:22                     *:*       tcp   LISTEN     0      50                   *:3306                   *:*       tcp   LISTEN     0      128                  :::80                    ::*       tcp   LISTEN     0      100                  :::25                    :::*       tcp   LISTEN     0      100                  *:25
```

结论

在本文中，通过阅读本文，您已经完成了本教程，并学会了如何在 [Linux](https://blog.eldernode.com/linux-server-monitoring-commands/) 中找到所有开放端口的列表。最后一点，对于系统和网络管理员来说，理解计算机网络中端口的概念是非常重要的。如果你需要更多关于这个主题的信息，可以找到我们的相关文章，关于[如何找出哪个进程监听特定的端口](https://blog.eldernode.com/find-process-listening-port/)和[Linux 中 5 个有趣的命令行提示和技巧](https://blog.eldernode.com/command-tips-linux/)。

## Conclusion

In this article, by reaching here you have finished this tutorial and learned how to Find out list of all Open Ports in [Linux](https://blog.eldernode.com/linux-server-monitoring-commands/). For the last point, understanding the concept of ports in computer networking is very vital for system and network administrators. If you need more information about this subject find our related articles on [How to Find Out Which Process Listening On A Particular Port](https://blog.eldernode.com/find-process-listening-port/) AND [5 Interesting Command Line Tips and Tricks in Linux](https://blog.eldernode.com/command-tips-linux/).