# 简介和设置

> 原文：[`learnbyexample.github.io/cli-computing/introduction-setup.html`](https://learnbyexample.github.io/cli-computing/introduction-setup.html)

回到 2007 年，我在一家半导体公司担任设计工程师时，在利用软件工具方面有一个艰难的开始。Linux 命令行、Vim 和 Perl 对我来说都是全新的。除了从同事和主管那里学习命令行工具外，我还记得在复印的书上阅读并做笔记（现在无法回忆起书名）。

最大的痛点是不知道一些实用的选项（例如，使用`grep --color`来突出显示匹配部分，使用`find -exec`在过滤文件上应用命令等）和工具（例如，使用`xargs`来绕过命令行参数过多的限制）。然后还有像`sed`和`awk`这样的具有令人生畏语法的工具。我无法解释为什么我没有充分利用 shell 脚本。我坚持使用 Perl 和 Vim，而不是学习这些实用的工具。我直到 2014 年离开工作后才知道论坛如[Stack Overflow](https://stackoverflow.com/)和[Unix Stack Exchange](https://unix.stackexchange.com/)。

当我有机会为大学生举办脚本课程研讨会时，我开始整理关于 Linux 命令行工具的知识。从 2016 年到 2018 年，我开始维护我的 Linux 命令行、Vim 和脚本语言的教程作为 GitHub 仓库。正如你可能猜到的，然后我开始润色这些材料，并[将它们作为电子书发布](https://learnbyexample.github.io/books/)。这是一个持续的过程，其中**Linux 命令行计算**是第 13 本电子书。

本书旨在教授 Linux 命令行工具和 Shell 脚本编程，适用于初学者到中级用户。提供了大量示例，以便更容易理解特定工具及其各种功能。提供了外部链接以供进一步阅读。重要的注意事项和警告格式化以突出显示正常文本。

写书对我来说总是有几个愉快的惊喜。这次我学习了像`mkdir -m`和`chmod =`这样的实用选项，对许多 shell 功能有了更好的理解等等。

本章将简要介绍 Linux。你还将看到有关设置命令行环境以跟随本书所展示内容的建议和说明。

## Linux 概述

引用来自[wikipedia](https://en.wikipedia.org/wiki/Linux)的部分内容：

> Linux 是一系列基于 Linux 内核的开源 Unix-like 操作系统家族，Linux 内核是一个操作系统内核，首次由林纳斯·托瓦兹于 1991 年 9 月 17 日发布。Linux 通常打包在 Linux 发行版中。
> 
> Linux 最初是为基于 Intel x86 架构的个人电脑开发的，但后来被移植到比任何其他操作系统更多的平台上。由于基于 Linux 的 Android 在智能手机上的主导地位，Linux 也拥有所有通用操作系统中最庞大的安装基础。
> 
> Linux 是自由和开源软件协作最突出的例子之一。任何人都可以根据其相应许可证的条款，以商业或非商业方式使用、修改和分发源代码，例如 GNU 通用公共许可证。

除了我在上一份工作中接触到的 Linux 之外，我从 2014 年开始使用 Linux，它非常适合我的需求。与 Windows 相比，Linux 轻量级、安全、稳定、快速，更重要的是，它不会强迫你升级硬件。阅读上面链接的维基百科文章，了解更多关于 Linux 的信息，包括它的用途等。

## Linux 发行版

再次引用[维基百科](https://en.wikipedia.org/wiki/Linux_distribution)：

> Linux 发行版（通常简称为 distro）是由基于 Linux 内核的软件集合组成的操作系统，通常还包括一个包管理系统。Linux 用户通常通过下载 Linux 发行版来获取操作系统，这些发行版适用于从嵌入式设备（例如，OpenWrt）和个人电脑（例如，Linux Mint）到强大的超级计算机（例如，Rocks Cluster Distribution）的广泛系统。

我使用的是 Ubuntu，它非常适合初学者。以下是一些帮助你选择发行版的资源：

+   [/r/linux4noobs wiki](https://old.reddit.com/r/linux4noobs/wiki/distro_selection) — 为新手提供的选型指南

+   [Linux 发行版列表](https://en.wikipedia.org/wiki/List_of_Linux_distributions) — 以分类列表形式提供关于知名 Linux 发行版的一般信息

+   [DistroWatch](https://distrowatch.com/) — 专注于讨论、评论和跟踪开源操作系统的网站。该网站特别关注 Linux 发行版和 BSD 的变种，尽管有时也会讨论其他开源操作系统

+   [轻量级 Linux 发行版](https://en.wikipedia.org/wiki/Light-weight_Linux_distribution) — 相比于功能更丰富的 Linux 发行版，使用更低的内存和/或更少的处理器速度要求

## 访问 Linux 环境

你通常可以从你想要安装的相应发行版网站上找到安装说明。或者，你可以在虚拟机上安装 Linux 或在线尝试。以下是一些帮助你开始的资源：

+   [安装 Ubuntu 桌面](https://ubuntu.com/tutorials/install-ubuntu-desktop)

+   [如何在虚拟机中使用 VirtualBox 运行 Ubuntu 桌面](https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox)

+   [DistroSea](https://distrosea.com/) — 在线探索和测试 Linux 发行版

如果您已经在 Windows 或 macOS 上，可以使用以下选项来获取 Linux 工具的访问权限：

+   [Git for Windows](https://git-scm.com/downloads) — 提供了用于在命令行中运行 Git 的 Bash 仿真

+   [Windows Subsystem for Linux](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux) — 在 Windows 上原生运行 Linux 二进制可执行文件的兼容层

+   [brew](https://brew.sh/) — macOS（或 Linux）的包管理器

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) ![警告](img/b5314b4a0acf0f436c4bf59486793342.png) 如果您对命令行使用完全陌生，我建议设置一个虚拟机。或者，也许是一个您可以自由实验的备用计算机。命令行中的错误可能比图形界面更具破坏性。例如，一个空格的打字错误可能导致数据丢失，使您的机器无法使用等。

## 设置

要跟随本书中展示的内容，您需要从我的[cli-computing 仓库](https://github.com/learnbyexample/cli-computing)中获取文件。一旦您有了 Linux 环境，请按照以下说明操作。如果以下使用的命令对您来说很陌生，请等待您到达 ls 部分（在那个地方您将获得返回这些说明的链接）。

要获取文件，您可以使用`git`命令克隆`cli-computing`仓库，或者下载`zip`版本。如果您还没有安装`git`命令，可能需要安装它，例如在类似 Debian 的系统上使用`sudo apt install git`。有关其他安装选项，请参阅[`git-scm.com/downloads`](https://git-scm.com/downloads)。

```sh
# option 1: use git
$ git clone --depth 1 https://github.com/learnbyexample/cli-computing.git

# option 2: download zip file
# you can also use 'curl -OL' instead of 'wget'
$ wget https://github.com/learnbyexample/cli-computing/archive/refs/heads/master.zip
$ unzip master.zip
$ mv cli-computing-master cli-computing 
```

一旦您有了文件，您将能够跟随本书中展示的命令。例如，您需要执行`ls.sh`脚本来处理 ls 部分。

```sh
$ cd cli-computing/example_files/scripts/
$ ls
cp.sh  file.sh  globs.sh  ls.sh  rm.sh    tar.sh
du.sh  find.sh  grep.sh   mv.sh  stat.sh  touch.sh

$ source ls.sh
$ ls -F
backups/    hello_world.py*  ip.txt     report.log  todos/
errors.log  hi*              projects/  scripts@ 
```

对于像 cat 命令这样的部分，您需要使用`text_files`目录中提供的示例输入文件。

```sh
$ cd cli-computing/example_files/text_files/
$ cat greeting.txt 
Hi there
Have a nice day 
```

## 命令行界面

命令行界面（CLI）允许您使用文本命令与计算机交互。例如，`cd`命令帮助您导航到特定目录。`ls`命令显示目录内容。在图形环境中，您会使用资源管理器（文件管理器）进行导航，目录内容默认显示。一些任务可以在 CLI 和 GUI 环境中完成，而另一些则仅适合在其中一个环境中完成。

使用 CLI 工具而不是 GUI 程序的一些优点如下：

+   自动化

+   执行更快

+   命令调用是可重复的

+   容易保存解决方案并与他人分享

+   相比于图形解决方案的不同 UI/UX，这是一个单一的环境

+   常见的文本界面使得工具可以轻松地相互通信

下面是一些缺点：

+   学习曲线陡峭

+   语法可能会变得非常复杂

+   需要熟悉大量的工具

+   错误往往具有更大的破坏性

您可以利用命令历史、快捷键和自动完成等特性来帮助处理大量的命令和语法问题。持续的练习将有助于熟悉命令行环境的特性。具有破坏潜力的命令通常会包括选项，允许手动确认和交互式使用，从而减少或完全避免因输入错误造成的影响。

## 章节

下面是剩余章节的列表：

+   命令行概述

+   管理文件和目录

+   Shell 功能

+   查看部分或整个文件内容

+   搜索文件和文件名

+   文件属性

+   管理进程

+   多功能文本处理工具

+   排序内容

+   比较文件

+   各种文本处理工具

+   Shell 脚本

+   Shell 自定义

## 资源列表

本书仅涵盖了 Linux 命令行使用的一小部分。像系统管理和网络这样的主题完全没有讨论。查看以下列表以了解这些主题并发现一些酷炫的工具：

+   [Linux 精选资源](https://learnbyexample.github.io/curated_resources/linux_cli_scripting.html) — 我为 Linux 命令行、Shell 脚本和其他相关主题收集的资源

+   [Awesome Linux](https://github.com/inputsh/awesome-linux) — 列出了一些使 Linux 更加酷炫的项目和资源

+   [Arch wiki：应用程序列表](https://wiki.archlinux.org/title/List_of_applications) — 按类别排序，有助于寻找包的参考
