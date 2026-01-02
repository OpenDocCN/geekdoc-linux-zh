# 编写你的第一个 Bash 脚本

让我们尝试将到目前为止所学的内容结合起来，创建我们的第一个 Bash 脚本！

## 规划脚本

例如，我们将编写一个脚本来收集有关我们服务器的一些有用信息，例如：

+   当前磁盘使用率

+   当前 CPU 使用率

+   当前 RAM 使用率

+   检查确切的内核版本

随意调整脚本，添加或删除功能，使其符合你的需求。

## 编写脚本

你需要做的第一件事是创建一个具有`.sh`扩展名的新文件。我将创建一个名为`status.sh`的文件，因为我们将要创建的脚本将给我们提供服务器的状态。

一旦创建了文件，就用你喜欢的文本编辑器打开它。

如我们在第一章中学到的，在我们的 Bash 脚本的第一行，我们需要指定所谓的[Shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))：

```sh
      #!/bin/bash 

```

Shebang 所做的只是指示操作系统使用/bin/bash 可执行文件来运行脚本。

## 添加注释

接下来，如第六章所讨论的，让我们先添加一些注释，这样人们就可以很容易地弄清楚脚本是用作什么的。要做到这一点，你可以在 shebang 之后直接添加以下内容：

```sh
      #!/bin/bash 
        # Script that returns the current server status 

```

## 添加你的第一个变量

然后让我们继续应用第四章中学到的知识，并添加一些我们可能在脚本中想要使用的变量。

在 bash 中给变量赋值，你只需要使用`=`符号。例如，让我们将我们服务器的计算机名存储在一个变量中，这样我们以后就可以使用它：

```sh
      server_name=$(hostname) 

```

通过使用`$()`，我们告诉 bash 实际解释命令，然后将值赋给我们的变量。

现在如果我们输出变量，我们会看到当前的计算机名：

```sh
      echo $server_name 

```

## 添加你的第一个函数

如你阅读第十二章后所知，为了在 bash 中创建一个函数，你需要使用以下结构：

```sh
      function function_name() {
    your_commands
} 

```

让我们创建一个函数，该函数返回我们服务器上的当前内存使用情况：

```sh
      function memory_check() {
    echo ""
	echo "The current memory usage on ${server_name} is: "
	free -h
	echo ""
} 

```

函数的简要概述：

+   `function memory_check() {` - 这是我们定义函数的方式

+   `echo ""` - 这里我们只是打印一个新行

+   `echo "The current memory usage on ${server_name} is: "` - 这里我们打印一条小消息和`$server_name`变量

+   `}` - 最后这是关闭函数的方式

然后一旦定义了函数，为了调用它，只需使用函数名即可：

```sh
      # Define the functionfunction memory_check() {
    echo ""
	echo "The current memory usage on ${server_name} is: "
	free -h
	echo ""
}

# Call the function
memory_check 

```

## 添加更多函数挑战

在查看解决方案之前，我挑战你使用上面的函数自己编写几个函数。

函数应该执行以下操作：

+   当前磁盘使用率

+   当前 CPU 使用率

+   当前 RAM 使用率

+   检查确切的内核版本

如果你不确定需要使用哪些命令来获取这些信息，请随时使用谷歌搜索。

一旦你准备好了，请随意向下滚动并查看我们是如何做到的，并比较结果！

注意，有多个正确的方法可以做到这一点！

## 示例脚本

这里是最终结果的样子：

```sh
      #!/bin/bash ### BASH script that checks:#   - Memory usage#   - CPU load#   - Number of TCP connections#   - Kernel version##

server_name=$(hostname)

function memory_check() {
    echo ""
	echo "Memory usage on ${server_name} is: "
	free -h
	echo ""
}

function cpu_check() {
    echo ""
	echo "CPU load on ${server_name} is: "
    echo ""
	uptime
    echo ""
}

function tcp_check() {
    echo ""
	echo "TCP connections on ${server_name}: "
    echo ""
	cat  /proc/net/tcp | wc -l
    echo ""
}

function kernel_check() {
    echo ""
	echo "Kernel version on ${server_name} is: "
	echo ""
	uname -r
    echo ""
}

function all_checks() {
	memory_check
	cpu_check
	tcp_check
	kernel_check
}

all_checks 

```

## 结论

Bash 脚本非常棒！无论你是 DevOps/SysOps 工程师、开发者，还是仅仅是一个 Linux 爱好者，你都可以使用 Bash 脚本来组合不同的 Linux 命令，自动化无聊且重复的日常任务，这样你就可以专注于更有生产力和更有趣的事情了！

> **注意：** 这篇文章最初发布在 [DevDojo.com](https://devdojo.com/bobbyiliev/introduction-to-bash-scripting)
