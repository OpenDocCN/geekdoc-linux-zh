# 如何在 Ubuntu 20.04 上安装和卸载 home brew-elder node 博客

> 原文：<https://blog.eldernode.com/install-and-uninstall-homebrew-on-ubuntu/>

![How to Install and Uninstall Homebrew on Ubuntu 20.04](img/4633310409a56f9756f777a143213033.png)

Homebrew 是一个开源的软件包管理软件，它是为 Mac 操作系统和基于 Ruby 语言的 Linux 发行版开发的。这个包管理器最初是为了给使用 Mac 的 Linux 用户提供正确的体验而开发的。许多开发者和用户都在使用 Homebrew，因为它简单易用，命令简单明了。这个软件，像大多数类似的包管理器一样，使用线性用户界面，没有图形界面。在这篇文章中，我们将教你**如何在 Ubuntu 20.04** 上安装和卸载 Homebrew。如果你想买一个 [**Ubuntu VPS**](https://eldernode.com/ubuntu-vps/) 服务器，你可以在 [Eldernode](https://eldernode.com/) 看到可用的软件包。

## **什么是家酿？**

家酿是一个免费的开源软件包管理系统，它简化了苹果 macOS 操作系统和 Linux 的软件安装。这个工具是由 Max Howell 在 2009 年编写的。2013 年 3 月，Homebrew 成功完成了一项 Kickstarter 活动，为测试和构建公式的服务器筹集资金，并成功筹集了 14859 英镑。

然后家酿库从 Howell 的 GitHub 帐户迁移到它的项目帐户。2015 年 2 月，由于缺乏对二进制文件的访问，SourceForge 崩溃，Homebrew 将其托管转移到 Bintray。2016 年，家酿 1.0.0 版本发布，2021 年 2 月起，家酿由 34 人团队维护。

## **如何在 Ubuntu 20.04 上安装家酿|** **Ubuntu 21.04**

在这一节，我们将一步步教你如何在 Ubuntu 20.04 上安装 Homebrew。为此，只需按顺序执行以下步骤。

第一步，打开 Ubuntu 终端。然后运行以下命令来更新系统:

```
sudo apt update
```

```
sudo apt-get install build-essential
```

在下一步中，要在 Ubuntu 20.04 上运行 Linuxbrew，您需要使用以下命令在系统上安装 Git:

```
sudo apt install git -y
```

需要记住的一点是，Brew 官网提供了下载安装 Homebrew 的**脚本。您可以使用以下命令下载并运行该脚本:**

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

安装完脚本后，您现在需要使用以下命令将 Brew 命令添加到系统路径中:

```
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

您可以通过运行以下命令来确保 HomeBrew 正常工作:

```
brew doctor
```

请注意，这可能会提供一个安装和删除 GCC 的警告。因此，您可以通过运行以下命令来安装它:

```
brew install gcc
```

### **如何在 Ubuntu 20.04 上卸载家酿**

正如你在上一节看到的，我们教你如何在 Ubuntu 20.04 上安装 Homebrew。如果您想卸载它，您可以轻松地运行以下命令:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
```

## 结论

家酿是 Mac 最受欢迎的软件包管理器之一，但它也可以安装在 Linux 上下载和安装各种软件包。在 Ubuntu Linux 上，我们已经有了一个 APT 包管理器，可以安装各种各样的应用程序和其他包。在这里，我们试着教你如何在 Ubuntu 20.04 上安装 Homebrew。