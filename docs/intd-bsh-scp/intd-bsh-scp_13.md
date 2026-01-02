# 调试、测试和快捷键

为了调试你的 bash 脚本，你可以在执行脚本时使用`-x`选项：

```sh
      bash -x ./your_script.sh 

```

或者你可以在想要调试的特定行之前添加`set -x`，`set -x`会启用一个 shell 模式，其中所有执行的命令都会打印到终端。

另一种测试你的脚本的方法是使用这里这个出色的工具：

[`www.shellcheck.net/`](https://www.shellcheck.net/)

只需将你的代码复制粘贴到文本框中，工具就会给你一些建议，告诉你如何改进你的脚本。

你也可以直接在你的终端中运行这个工具：

[`github.com/koalaman/shellcheck`](https://github.com/koalaman/shellcheck)

如果你喜欢这个工具，请确保在 GitHub 上给它加星并贡献！

作为系统管理员/DevOps，我大部分时间都在终端中度过。以下是我最喜欢的快捷键，它们帮助我在编写 Bash 脚本或仅仅在终端中工作时更快地完成任务。

如果你有一个非常长的命令，下面两个特别有用。

+   从光标处删除到行尾的所有内容：

```sh
      Ctrl + k 

```

+   从光标处删除到行首的所有内容：

```sh
      Ctrl + u 

```

+   从光标处向后删除一个单词：

```sh
      Ctrl + w 

```

+   向后搜索历史记录。这可能是我最常用的一个。它真的非常方便，大大加快了我的工作流程：

```sh
      Ctrl + r 

```

+   清除屏幕，我使用这个代替输入`clear`命令：

```sh
      Ctrl + l 

```

+   停止输出到屏幕：

```sh
      Ctrl + s 

```

+   在之前被`Ctrl + s`停止输出的情况下启用输出到屏幕：

```sh
      Ctrl + q 

```

+   终止当前命令

```sh
      Ctrl + c 

```

+   将当前命令放到后台：

```sh
      Ctrl + z 

```

我每天都会经常使用这些，这节省了我很多时间。

如果你认为我遗漏了任何内容，请随时加入[DigitalOcean 社区论坛](https://www.digitalocean.com/community/questions/what-are-your-favorite-bash-shortcuts)的讨论！
