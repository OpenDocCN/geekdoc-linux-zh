# 常见 Apache 错误故障排除- Eldernode

> 原文：<https://blog.eldernode.com/apache-errors-troubleshooting/>

![Common Apache Errors Troubleshooting](img/e2280e9461d590c6403addd286cfeb17.png)

在接下来的 Apache 教程中，在本文中，您将学习常见 Apache 错误的故障排除。本系列教程解释了如何对使用 Apache webserver 时可能遇到的一些最常见的错误进行故障排除和修复。

本系列中的每一篇教程都包括对常见的 Apache 配置、网络、文件系统或权限错误的描述。本系列文章首先概述了可以用来对 [Apache](https://www.apache.org/) 进行故障排除的命令和日志文件。后续教程详细分析了特定的错误。为了让这个指南更好地发挥作用，尝试购买和使用 [VPS](https://eldernode.com/linux-vps/) 或 [Linux 虚拟主机](https://eldernode.com/linux-hosting/)。

## 常见 Apache 错误故障排除

有三个主要命令和一组常见的日志位置，可以用来开始排除 Apache 错误。通常，在对 Apache 进行故障排除时，您将按此处指示的顺序使用这些命令，然后检查日志文件中的特定诊断数据。

在大多数 Linux 发行版中，您通常用来对 Apache 进行故障排除的命令有:

**system CTL**–用于通过 systemd 服务管理器控制 Linux 服务并与之交互。
**——用于查询和查看 systemd 生成的日志。
**Apache CTL**–排除故障时，该命令用于检查 Apache 的配置。**

**这些命令、如何使用它们以及 Apache 的日志位置(您可以在其中找到关于错误的其他信息)将在下面的章节中详细描述。**

****请注意** :在 Debian 和 Ubuntu 系统上，apache 的服务和进程名是 apache2，而在 CentOS、Fedora 等 RedHat 衍生系统上，Apache 的服务和进程名是 httpd。除了服务和正在运行的进程名之间的差异之外，启动、停止和检查 Apache 的状态，以及使用 journalctl 的日志在任何使用 systemd 管理 Apache 服务的 Linux 系统上应该都是一样的。确保为您的 Linux 发行版使用正确的名称。**

### ****systemctl** 针对阿帕奇的命令**

**要使用 **systemd** 服务管理器排除常见的 Apache 错误，第一步是检查系统上 Apache 进程的状态。下面的 **systemctl** 命令将查询 **systemd** 以获得 Apache 进程的状态。**

**在 Ubuntu 和 Debian 系统上运行:**

```
`sudo systemctl status apache2.service -l --no-pager`
```

****-l** 标志将确保输出不被截断或省略。**–无寻呼机** 标志将确保输出将直接发送到您的终端，而不需要您进行任何交互来查看它。您应该会收到如下输出:**

**输出**

```
`● apache2.service - The Apache HTTP Server     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)    Drop-In: /lib/systemd/system/apache2.service.d             └─apache2-systemd.conf     Active: active (running) since Mon 2020-07-13 14:43:35 UTC; 1 day 4h ago    Process: 929 ExecStart=/usr/sbin/apachectl start (code=exited, status=0/SUCCESS)   Main PID: 1346 (apache2)      Tasks: 55 (limit: 4702)     CGroup: /system.slice/apache2.service             ├─1346 /usr/sbin/apache2 -k start  . . .`
```

**要在 CentOS 和 Fedora 系统上检查 Apache 进程，请运行:**

```
`sudo systemctl status httpd.service -l --no-pager`
```

**输出**

```
`● httpd.service - The Apache HTTP Server     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)     Active: active (running) since Tue 2020-07-14 19:46:52 UTC; 3s ago       Docs: man:httpd.service(8)   Main PID: 21217 (httpd)     Status: "Started, listening on: port 80"      Tasks: 213 (limit: 2881)     Memory: 16.6M     CGroup: /system.slice/httpd.service             ├─21217 /usr/sbin/httpd -DFOREGROUND  . . .  Jul 14 19:46:52 localhost.localdomain httpd[21217]: Server configured, listening on: port 80`
```

**在任一情况下，记下输出中的 **活动** 行。如果您的 Apache 服务器没有显示前面例子中突出显示的 **活动(运行)** ，但是您期望它应该显示，那么可能有一个错误。通常，如果出现问题，您的输出中会有如下一行(注意突出显示的失败部分):**

**示例错误输出**

**如果您的 Apache 进程或配置有问题，您可以使用 **journalctl** 命令进一步排除故障。**

```
`Active: failed (Result: exit-code) since Tue 2020-07-14 20:01:29 UTC; 1s ago`
```

**如果您的 Apache 进程或配置有问题，您可以使用 **journalctl** 命令进一步排除故障。**

****用于阿帕奇的命令****

### ****要检查 Apache 的 **systemd** 日志，可以使用 **journalctl** 命令。Apache 的 **systemd** 日志通常会指出 Apache 进程的启动或管理是否存在问题。****

****这些日志独立于 Apache 的请求和错误日志。 **journalctl** 显示来自 **systemd** 的日志，这些日志描述了 Apache 服务本身，从启动到关闭，以及在此过程中可能遇到的任何进程错误。****

****在 Ubuntu 和 Debian 系统上，使用以下命令检查日志:****

****–since today标志将限制命令的输出，仅记录当天 00:00:00 开始的条目。使用此选项将有助于限制在检查错误时需要检查的日志条目的数量。您应该会收到如下输出:****

```
**`sudo journalctl -u apache2.service --since today --no-pager`**
```

****`输出`****

****`如果您使用基于 CentOS 或 Fedora 的系统，请使用以下版本的命令:`****

```
**``Jul 14 20:12:14 ubuntu2004 systemd[1]: Starting The Apache HTTP Server...  Jul 14 20:12:14 ubuntu2004 systemd[1]: Started The Apache HTTP Server.``**
```

****`输出`****

```
**``sudo journalctl -u httpd.service --since today --no-pager``**
```

****`如果有错误，您将在输出中看到类似下面的一行，Linux 发行版之间的主要区别是突出显示的 yourhostname 部分:`****

```
**``Jul 14 20:13:09 centos8 systemd[1]: Starting The Apache HTTP Server...  . . .  Jul 14 20:13:10 centos8 httpd[21591]: Server configured, listening on: port 80``**
```

****`示例错误输出`****

****`如果您的 Apache 服务器在 **日志** 中有错误，那么下一步就是使用 **apachectl** 命令行工具调查 Apache 的配置。`****

```
**``Jul 14 20:13:37 yourhostname systemd[1]: Failed to start The Apache HTTP Server.``**
```

****`故障排除用**Apache CTL**`****

### ****`大多数 Linux 发行版都包括 Apache 的 **apachectl** 实用程序。 **apachectl** 是一个帮助检测和诊断 Apache 配置问题的无价工具。`****

****`要使用 **apachectl** 解决问题，请使用**Apache CTL config test**命令测试您的 Apache 配置。该工具将解析您的 Apache 文件，并在尝试启动服务器之前检测任何错误或缺失的设置。`****

****`在基于 Ubuntu、Debian、CentOS 和 Fedora 的发行版上运行如下命令:`****

****`一个有效的 Apache 配置将产生如下所示的输出:`****

```
**``sudo apachectl configtest``**
```

****`输出`****

****`根据您的 Linux 发行版，可能会有其他行混入输出中，但是重要的一行是说 **语法 OK** 的那一行。`****

```
**``Syntax OK``**
```

****`如果您的 Apache 配置中有错误，比如某个指令引用了一个未启用的模块，甚至是一个错别字， **apachectl** 都会检测到它，并尝试通知您这个问题。`****

****`例如，试图对未启用的 Apache 模块使用指令将导致如下所示的 apachectl configtest 消息。`****

****`示例错误输出`****

****`在本例中， **ssl** 模块未启用，因此在测试配置时， **SSLEngine** 指令会产生错误。最后一行还表示**Apache 错误日志可能有更多的信息** ，这是下一个查找更详细的调试信息的地方。`****

```
**``AH00526: Syntax error on line 232 of /etc/apache2/apache2.conf:  Invalid command 'SSLEngine', perhaps misspelled or defined by a module not included in the server configuration  Action 'configtest' failed.  The Apache error log may have more information.``**
```

****`Apache 日志文件`****

### ****`Apache 日志文件对于故障排除是非常有用的资源。通常，您在浏览器或其他 HTTP 客户端收到的任何错误都会在 Apache 的日志中有相应的条目。有时，Apache 还会将与配置、内置模块和其他调试信息相关的错误输出到其日志文件中。`****

****`要在对 Fedora、CentOS 或 RedHat 服务器上的 Apache 进行故障排除时检查日志文件中的错误，请检查**/var/log/httpd/error _ log**文件。`****

****`如果您正在对 Debian 或 Ubuntu 衍生的系统进行故障排除，请使用类似于 **tail** 或 **less** 的工具检查**/var/log/Apache 2/error . log**中的错误。例如，要使用**查看错误日志的最后两行，请运行以下命令:**`****

****`**用你想检查的行数代替命令中的数字 **2** 。在 CentOS 或 Fedora 系统上，要检查的日志文件是**/var/log/httpd/error _ log**。**`****

```
`**`sudo tail -n 2 /var/log/apache2/error.log`**`
```

**`**错误示例类似于下面几行。不管您使用哪个 Linux 发行版来运行 Apache 服务器:**`**

**`**错误日志示例**`**

**`**该输出中的两行是不同的错误消息。它们都引用了导致错误的模块(第一行是 **代理** ，第二行是 **代理 _http** )，并且包含了一个特定于该模块的错误代码。第一个， **AH00957** ，表示 Apache 服务器试图使用 **代理** 模块连接到后端服务器(在本例中为端口 9090 上的 127.0.0.1 ),但未能成功。**`**

```
`**`[Wed Jul 15 01:34:12.093005 2020] [proxy:error] [pid 13949:tid 140150453516032] (13)Permission denied: AH00957: HTTP: attempt to connect to 127.0.0.1:9090 (127.0.0.1) failed  [Wed Jul 15 01:34:12.093078 2020] [proxy_http:error] [pid 13949:tid 140150453516032] [client 127.0.0.1:42480] AH01114: HTTP: failed to make connection to backend: 127.0.0.1`**`
```

**`**第二个错误源于第一个错误:AH01114 是一个 **proxy_http** 模块错误，它也表明 Apache 无法连接到已配置的后端服务器来发出 http 请求。**`**

**`**这些示例行仅用于说明目的。如果您正在诊断 Apache 服务器的错误，那么日志中的错误行可能会有不同的内容。不管您的 Linux 发行版如何，日志中任何错误行的格式都将包括相关的 Apache 模块和错误代码，以及错误的文本描述。**`**

**`**一旦您知道是什么导致了 Apache 服务器的问题，您就可以继续研究和解决这个问题。错误代码和文本描述特别有用，因为它们为您提供了明确而具体的术语，您可以使用这些术语来缩小问题的可能原因的范围。**`**

**`****结论****`**

## **`**Apache 错误的故障排除范围很广，从诊断服务本身的错误，到定位模块的错误配置选项，或者详细检查定制的访问控制规则。这篇关于诊断 Apache 问题的介绍解释了如何使用许多实用程序来帮助缩小错误的可能原因。通常，您会以相同的顺序使用这些实用程序，尽管您总是可以跳过一些，或者如果您对问题可能是什么有一个大致的概念，就直接从检查日志开始。如果你有兴趣学习更多关于 Apache 的知识，可以找到我们的相关文章，比如如何在 Debian 10 上使用 Let's Encrypt 来保护 Apache 的安全，或者如何在 Ubuntu 20.04 上使用 Let's Encrypt 来保护 Apache 的安全，比如 T2 的文章。**`**

**`**但是，作为故障排除的一般顺序，有条不紊地按照描述的顺序使用这些工具会有所帮助。使用 systemctl 开始故障排除，以检查 Apache 服务器的状态。如果需要更多信息，可以使用 **journalctl** 命令检查 Apache 的 **systemd** 日志。如果在检查 **日志** 后问题仍然不明显，下一步是使用**Apache CTL config test**测试 Apache 的配置。最后，为了进行深入的故障排除，检查 Apache 的日志文件通常会指出一个特定的错误，并提供有用的诊断消息和错误代码。**`**

**`**但是，作为故障排除的一般顺序，有条不紊地按照描述的顺序使用这些工具会有所帮助。使用 systemctl 开始故障排除，以检查 Apache 服务器的状态。如果需要更多信息，可以使用 **journalctl** 命令检查 Apache 的 **systemd** 日志。如果在检查 **日志** 后问题仍然不明显，下一步是使用**Apache CTL config test**测试 Apache 的配置。最后，为了进行深入的故障排除，检查 Apache 的日志文件通常会指出一个特定的错误，并提供有用的诊断消息和错误代码。**`**