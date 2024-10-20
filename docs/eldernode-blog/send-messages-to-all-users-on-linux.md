# 教程向 Linux 服务器| Eldernode 上的所有用户发送消息

> 原文：<https://blog.eldernode.com/send-messages-to-all-users-on-linux/>

![Tutorial send messages to all users on Linux Server](img/fc4a2b79ae5ef83ea651b20bad2aca4a.png)

如何向 Linux 服务器上的所有用户发送消息？

在 [linux 教程](https://blog.eldernode.com/tag/linux/)系列的这一部分中，我们将教您如何向 linux 中的所有用户发送消息，因此您可以使用一个简单的命令向通过 SSH 连接到 linux 服务器的所有用户发送您想要的消息。linux 管理人员最喜欢的命令之一是 WALL 命令，它允许您在一瞬间向所有用户发送消息。

例如，您考虑让所有用户知道服务器将在周六下午 6 点进入维护模式一小时，并申请从 7 点开始使用服务器。

在这种情况下，Wall 命令会对您有所帮助，您可以使用它。

加入我们，学习如何使用 Wall 命令向 Linux 上的所有用户发送消息。

### **向[Linux](https://blog.eldernode.com/tag/linux/)T3 上的所有用户发送消息**

*   要以集成方式向用户发送消息，请使用 Wall 命令，如下所示:

```
wall "System will go down for 1 hours maintenance at 6:00 PM" 
```

您可以简单地在单词 Wall 后面和引号之间写下您的消息，并按 Enter 键，在 inter 之后，此消息将被快速发送给用户。

*   现在，如果您打算在用户收到消息后，系统不显示命令行，直到它点击 Inter 以确保用户已经阅读了消息，您必须使用 **n** 参数。

```
wall -n "System will go down for 1 hours maintenance at 6:00 PM" 
```

通过这种方式，您可以与其他在线用户共享关于您的服务器的通知，以获得服务器状态的通知。我们希望您已经充分利用了通过 Wall 命令向 Linux 上的所有用户发送消息的技术。

如有疑问或问题，可向[提问系统](https://eldernode.com/ask/)咨询，提供指导。

祝你好运。