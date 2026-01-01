# `rsync`命令

`rsync`命令可能是最常用的命令之一。它用于通过 SSH 安全地从一台服务器复制文件到另一台服务器。

与执行类似任务的`scp`命令相比，`rsync`使传输速度更快，并且在传输中断的情况下，你可以恢复/继续传输过程。

在这个教程中，我将向你展示如何使用`rsync`命令，从一台服务器复制文件到另一台，并分享一些有用的技巧！

在开始之前，你需要拥有 2 台 Linux 服务器。我将使用 DigitalOcean 进行演示并部署 2 台 Ubuntu 服务器。

你可以使用我的推荐链接获得免费的$100 信用额度，你可以用它来部署虚拟机，并在几个 DigitalOcean 服务器上测试指南：

**[DigitalOcean $100 免费信用额度](https://m.do.co/c/2a9bba940f39)**

## 将本地服务器上的文件传输到远程

这是最常见的原因之一。本质上，这就是你从当前所在的服务器（源服务器）复制文件到远程/目标服务器的方式。

你需要做的是 SSH 到保存你文件的服务器，cd 到你想传输的目录：

```sh
      cd /var/www/html 

```

然后运行：

```sh
      rsync -avz user@your-remote-server.com:/home/user/dir/ 

```

上述命令将复制服务器上当前文件夹中的所有文件和目录到你的远程服务器。

命令概述：

+   `-a`：用于指定你想要递归并保留文件权限等。

+   `-v`：是详细模式，它增加了在传输过程中你获得的信息量。

+   `-z`：此选项，在将文件发送到目标机器时，`rsync`会压缩文件数据，这减少了传输的数据量——这在慢速连接中非常有用。

我建议查看以下网站，它非常详细地解释了命令和参数：

[`explainshell.com/explain?cmd=rsync+-avz`](https://explainshell.com/explain?cmd=rsync+-avz)

如果远程服务器上的 SSH 服务没有运行在标准的`22`端口上，你可以使用带有特殊 SSH 端口的`rsync`：

```sh
      rsync -avz -e 'ssh -p 1234' user@your-remote-server.com:/home/user/dir/ 

```

## 将远程服务器上的文件传输到本地

在某些情况下，你可能想从你的远程服务器传输文件到本地服务器，在这种情况下，你需要使用以下语法：

```sh
      rsync -avz your-user@your-remote-server.com:/home/user/dir/ /home/user/local-dir/ 

```

再次提醒，如果你有一个非标准的 SSH 端口，你可以使用以下命令：

```sh
      rsync -avz -e 'ssh -p 2510' your-user@your-remote-server.com:/home/user/dir/ /home/user/local-dir/ 

```

## 仅传输缺失的文件

如果你只想传输缺失的文件，可以使用`--ignore-existing`标志。

这对于确保网站或服务器迁移后没有缺失文件，进行最后的同步非常有用。

基本上，命令将是相同的，除了附加的`--ignore-existing`标志：

```sh
      rsync -avz --ignore-existing  user@your-remote-server.com:/home/user/dir/ 

```

## 结论

使用`rsync`是一种快速且安全地将一些文件从一个机器传输到另一个机器的好方法。

对于更多酷的 Linux 网络工具，我建议查看以下教程：

[你应该知道的 15 个顶级 Linux 网络工具！](https://devdojo.com/serverenthusiast/top-15-linux-networking-tools-that-you-should-know)

希望这能帮到您！

初始发布于此：[如何使用 rsync 将文件从一个 Linux 服务器传输到另一个 Linux 服务器](https://devdojo.com/bobbyiliev/how-to-transfer-files-from-one-linux-server-to-another-using-rsync)
