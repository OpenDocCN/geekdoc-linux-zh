# 如何找出哪个进程监听特定的端口- Eldernode

> 原文：<https://blog.eldernode.com/find-process-listening-port/>

![How to Find Out Which Process Listening on a Particular Port](img/faee4df81dd996cb6d787b1972f2f6df.png)

Linux 系统管理员需要知道一些 Linux 命令。加入我们，了解如何找出哪个进程监听特定端口。但首先，让我们了解一下港口的基本情况。端口是代表通信端点的逻辑实体，并且与操作系统中的给定进程或服务相关联。为了让您的学习更好地进行，选择您自己的具有即时激活功能的 [VPS 服务器](https://eldernode.com/vps/)。

## 如何找出哪个进程监听某个特定的端口

最近，我们解释了如何在 Linux 中找到所有开放端口的 [列表。](https://eldernode.com/find-list-open-ports-linux/)加入我们这篇文章，看看在 [Linux](https://www.linux.org/) 中找到监听特定端口的进程/服务的 3 种不同方法。

**netstat 命令**用于显示网络连接、路由表、接口统计等信息。它可以在所有类似 Unix 的操作系统上使用，包括 Linux 和 Windows 操作系统。

如果你的**没有默认安装**，使用下面的[命令](https://blog.eldernode.com/command-tips-linux/)安装。

```
$ sudo yum install net-tools	#RHEL/CentOS   $ sudo apt install net-tools	#Debian/Ubuntu  $ sudo dnf install net-tools	#Fedora 22+
```

安装完成后，您可以使用它和 **grep 命令**来查找监听 Linux 中特定端口的进程或服务，如下所示(指定端口)。

```
$ netstat -ltnp | grep -w ':80'
```

在上面的命令中，标志。

**l**–告诉 netstat 只显示监听套接字。

**t**–告诉它显示 tcp 连接。

**n**–指示它显示数字地址。

**p**–显示进程 ID 和进程名称。

**grep-w**–显示精确字符串的匹配(:80)。

### `2:使用 lsof 命令`

`**lsof 命令**(列出打开的文件)用于列出 Linux 系统上所有打开的文件。要在您的系统上安装它，请键入下面的[命令](https://blog.eldernode.com/linux-server-monitoring-commands/)。`

```
`$ sudo yum install lsof	        #RHEL/CentOS   $ sudo apt install lsof		#Debian/Ubuntu  $ sudo dnf install lsof		#Fedora 22+`
```

`要查找侦听特定端口的进程/服务，请键入(指定端口)。`

```
`$ lsof -i :80` 
```

### ``3:使用定影器命令``

``**fuser 命令**显示使用 [Linux](https://blog.eldernode.com/tutorial-linux-rpm-command/) 中指定文件或文件系统的进程的 PID。让我们看看第三种也是最后一种方法是什么来找出哪个进程在监听特定的端口。``

``您可以按如下方式安装它:``

```
``$ sudo yum install psmisc	#RHEL/CentOS   $ sudo apt install psmisc	#Debian/Ubuntu  $ sudo dnf install psmisc	#Fedora 22+`` 
```

``您可以通过运行下面的命令(指定端口)来查找监听特定端口的进程/服务。``

```
``$ fuser 80/tcp`` 
```

``然后使用 PID 号和 **ps 命令**找到进程名，如下所示。``

```
``$ ps -p 2053 -o comm=  $ ps -p 2381 -o comm=``
```

## ``结论``

``在本文中，您了解了如何找出哪个进程在监听特定的端口。因此，在本指南的 3 个建议中选择一个合适的方法。追踪 [Linux 命令](https://eldernode.com/tag/linux-commands-tutorial/)找到更多有用的文章。``