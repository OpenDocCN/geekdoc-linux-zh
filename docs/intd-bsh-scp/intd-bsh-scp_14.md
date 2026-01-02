# 创建自定义 bash 命令

作为开发者或系统管理员，你可能会在终端上花费大量时间。我总是试图寻找优化任何重复性任务的方法。

有一种方法可以写简短的 bash 脚本或者创建自定义命令，也称为别名。例如，你不必每次都输入一个很长的命令，你只需为它创建一个快捷方式。

## 示例

让我们从以下场景开始，作为一个系统管理员，你可能需要经常检查你的 Web 服务器的连接，所以我会用`netstat`命令作为例子。

当我访问一个与 80 或 443 端口连接有问题服务器时，我通常会检查是否有任何服务监听这些端口以及端口的连接数。

以下`netstat`命令会显示我们当前在 80 和 443 端口上有多少 TCP 连接：

```sh
      netstat -plant | grep '80\|443' | grep -v LISTEN | wc -l 

```

这是一个相当长的命令，每次输入它可能会在长期内浪费时间，尤其是当你想快速获取信息时。

为了避免这种情况，我们可以创建一个别名，这样我们就不必输入整个命令，而只需输入一个简短的命令即可。例如，假设我们想要能够输入`conn`（连接的简称）并获取相同的信息。在这种情况下，我们只需要运行以下命令：

```sh
      alias conn="netstat -plant | grep '80\|443' | grep -v LISTEN | wc -l" 

```

这样，我们创建了一个名为`conn`的别名，它本质上是我们长`netstat`命令的快捷方式。现在，如果你只运行`conn`：

```sh
      conn 

```

你会得到与长`netstat`命令相同的输出。你可以更加有创意，添加一些像这样的信息消息：

```sh
      alias conn="echo 'Total connections on port 80 and 443:' ; netstat -plant | grep '80\|443' | grep -v LISTEN | wc -l" 

```

现在，如果你运行`conn`，你会得到以下输出：

```sh
      Total connections on port 80 and 443:
12 

```

现在，如果你注销并重新登录，你的别名将会丢失。在下一步中，你将看到如何使其持久化。

## 使更改持久化

为了使更改持久化，我们需要在我们的 shell 配置文件中添加`alias`命令。

默认情况下，在 Ubuntu 上这将是`~/.bashrc`文件，对于其他操作系统，这可能是`~/.bash_profile`。用你喜欢的文本编辑器打开文件：

```sh
      nano ~/.bashrc 

```

滚到页面底部并添加以下内容：

```sh
      alias conn="echo 'Total connections on port 80 and 443:' ; netstat -plant | grep '80\|443' | grep -v LISTEN | wc -l" 

```

保存并退出。

这样，即使你注销并重新登录，你的更改也会被保留，你将能够运行你的自定义 bash 命令。

## 列出所有可用的别名

要列出您当前 shell 中所有可用的别名，您只需运行以下命令：

```sh
      alias 

```

如果你看到某些命令有奇怪的行为，这将很有用。

## 结论

这是一种创建自定义 bash 命令或 bash 别名的做法。

当然，你可以实际编写一个 bash 脚本，并将脚本添加到你的`/usr/bin`文件夹中，但如果你没有 root 或 sudo 权限，这不会起作用，而使用别名你可以在不需要 root 权限的情况下做到这一点。

> **注意：** 这篇文章最初发布在[DevDojo.com](https://devdojo.com/bobbyiliev/how-to-create-custom-bash-commands)
