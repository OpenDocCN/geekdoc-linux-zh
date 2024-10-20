# 使用 Make 文件自动化 Linux 构建的 5 种方法

> 原文：<https://blog.eldernode.com/5-ways-to-automate-your-linux-builds-with-make-files/>

![5 Ways to Automate Your Linux Builds with Make Files](img/e3dddc88cd4a59b8d4013e35b1214ec6.png)

如果您的项目有许多文件，翻译和链接它们将需要时间。要解决这个问题，您可以将命令保存在项目旁边的一个名为 Make File without extension 的文本文件中，以特定格式翻译您的程序。本文将向您介绍用 Make 文件自动化 Linux 构建的 5 种方法。查看 [Eldernode](https://eldernode.com/) 网站上提供的软件包，购买自己的[购买 Linux VPS](https://eldernode.com/linux-vps/) 主机。

## **用 Make 文件自动化 Linux 构建**

Make 是一个 Unix 实用程序，用于在更新特定文件时运行或更新任务，并且需要一个 Make 文件来定义一组要运行的任务。Make 文件是编译软件项目所必需的一组命令，以特殊格式存储在文本文件中。如果在 make 文件目录中键入 make，将执行 make 文件中的命令。Make 文件还包含一个规则列表，它将告诉系统您想要执行什么命令。这里有 5 种用 Make 文件自动化你的 [Linux](https://blog.eldernode.com/using-tar-command-on-linux/) 构建的方法。

在这篇来自 [Linux 培训](https://blog.eldernode.com/tag/linux/)系列的文章的续篇中，您将学习如何以 5 种方式用 Make 文件自动化您的 Linux 构建。

## **1-安装 GNU Make**

GNU Make 是一个从 Make 文件中获取知识来构建程序的工具。为每个非源文件制作文件列表，以及如何从其他文件中计算它们。控制从源文件程序生成程序的可执行文件和其他非源文件。事实上，GNU Make 包含了从程序的源文件生成程序的可执行文件和其他非源文件。

在 [Linux](https://blog.eldernode.com/tag/kali-linux/) 操作系统中，make 程序可能已经安装。如果没有安装，请按照下面的说明进行安装。

如果你使用的是 **Ubuntu/Debian** ，输入以下命令安装 [GNU](https://blog.eldernode.com/install-gnu-octave-on-ubuntu-20-04/) Make:

```
apt install make
```

您可以通过安装发行版附带的开发工具来充分利用 make:

```
apt install build-essential
```

要在 **Fedora/RHEL** 上安装 GNU Make，运行以下命令:

```
yum install make
```

如果您想充分利用 make，请安装如下所示的开发工具:

```
yum groupinstall ‘Development Tools’
```

在 **Arch/Manjaro** 上安装 GNU Make，使用以下命令:

```
pacman -S make
```

输入以下命令安装开发工具:

```
pacman -S base-devel
```

## **2-为你的项目创建一个新目录**

首先为你的项目创建一个新目录，运行以下命令:

```
mkdir new-directory
```

现在，使用下面的命令检查是否创建了目录:

```
ls -l
```

## **3-添加一个名为 Makefile 的文件**

在第一步中，您应该导航到创建的目录以添加 Makefile 文件。为此，请运行以下命令:

```
cd
```

导航到该目录后，可以使用以下命令向其中添加 Makefile:

```
touch Makefile
```

您也可以使用文本编辑器将 Makefile 添加到目录中，如下所示:

```
nano Makefile
```

您可以使用以下命令检查是否添加了文件:

```
ls
```

## **4-写一些规则**

现在是时候为 Makefile 写一些规则了。如果要这样做，Makefile 规则的一般语法如下:

```
target [target...] : [dependent ....]  [ command ...]
```

你可以在括号中写下你想要的参数，有可选的。请注意，省略号表示一个或多个，每个命令前需要有制表符。

例如，如果您想要定义一个规则，使您的目标从其他文件中 hello，请编写如下规则:

```
hello: main.o factorial.o hello.o  $(CC) main.o factorial.o hello.o -o hello
```

如果命令返回失败状态，您应该使终止，如下所示:

```
clean:  -rm *.o *~ core paper
```

Make 会忽略命令行上以破折号开头的返回状态。

每个人都希望 Makefiles 中有特定的目标。你可以期待所有的目标被发现制造，安装和清洗:

**1- make all:** 编译所有东西，在安装应用程序之前进行本地测试。

**2- make install:** 在正确的位置安装应用程序。

**3-清理:**清理应用程序，清除可执行文件、任何临时文件、目标文件等。

## **5-润使**

make 实用程序在 makefile 被读取和解释时工作。要运行 make，请输入以下命令:

```
make
```

现在 GNU make 将寻找一个名为 makefile、Makefile 或 GNUmakefile 的文件。一旦找到其中一个 makefiles，就会构建第一个目标。

如果 make 找不到合适的 makefile，您将看到以下错误:

```
make: *** No targets specified and no makefile found. Stop.
```

您可以使用 **-f** 选项来指定 makefile。make 命令语法如下:

```
make -f <i>filename</i>
```

记住要替换 makefile 的名称，而不是文件名。

如果您想从 makefile 中定义的几个目标中构建一个特定的目标，请在运行 make 时使用下面的语法:

```
make target
```

就是这样！

## 结论

Make Files 定义了一组要执行的任务，您可以使用它从源代码编译程序。在本文中，我们向您介绍了使用 Make 文件自动化 Linux 构建的 5 种方法。如果你有任何问题或建议，可以在评论区联系我们。我希望这篇教程对你有用，并帮助你自动化你的 Linux 构建。