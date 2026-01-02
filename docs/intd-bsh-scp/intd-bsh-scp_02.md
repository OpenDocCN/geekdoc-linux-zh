# Bash 结构

让我们先创建一个具有 `.sh` 扩展名的新文件。例如，我们可以创建一个名为 `devdojo.sh` 的文件。

要创建那个文件，你可以使用 `touch` 命令：

```sh
      touch devdojo.sh 

```

或者你可以使用你的文本编辑器：

```sh
      nano devdojo.sh 

```

为了使用 bash shell 解释器执行/运行 bash 脚本文件，脚本文件的第一行必须指示到 bash 可执行文件的绝对路径：

```sh
      #!/bin/bash 

```

这也被称为 [Shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))。

唯一的作用就是指示操作系统使用 `/bin/bash` 可执行文件来运行脚本。

然而，bash 并不总是在 `/bin/bash` 目录中，尤其是在非 Linux 系统上，或者由于作为可选包安装，因此你可能需要使用：

```sh
      #!/usr/bin/env bash 

```

它会在 PATH 环境变量中列出的目录中搜索 bash 可执行文件。
