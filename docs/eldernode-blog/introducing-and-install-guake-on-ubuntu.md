# 在 Ubuntu 20.04 - Eldernode 博客上介绍和安装 Guake

> 原文：<https://blog.eldernode.com/introducing-and-install-guake-on-ubuntu/>

![Introducing and Install Guake on Ubuntu 20.04](img/d291e06f3dcaee6260c312e39b4d0a09.png)

大多数终端应用程序在你的 Ubuntu 上都是打开的。即使不是，打开终端重复使用对你来说也可能是困难的。为了解决这个问题，推荐使用名为 Guake 的下拉控制台。在这篇文章中，我们将教你在 Ubuntu 20.04，21.04，18.04 上介绍和安装 Guake。如果你想购买一台 [**Linux VPS**](https://eldernode.com/linux-vps/) 服务器，你可以访问 [Eldernode](https://eldernode.com/) 中的软件包。

## **教程在 Ubuntu Linux 上安装 Guake**

### **介绍瓜科【完成】**

**Guake 是每个桌面和 Gnome 的下拉终端。Guake 提供了对终端的快速访问，通过按一个简单的键就可以隐藏和显示终端。Guake 大部分是用 Python 写的，有一小部分是用 C 写的，Guake 是一个自顶向下的模拟器，被很多发行版打包，包括 Fedora，Debian， [Ubuntu](https://blog.eldernode.com/tag/ubuntu/) 或者 ArchLinux。**

**它是通过不间断地按键来使用的。您可以通过击键立即显示和隐藏您的终端，执行命令，然后返回到之前的工作，而不会中断您的工作流程。**

## ****如何在 Ubuntu 20.04 上安装 Guake | Ubuntu 21.04****

**首先，您应该通过按 Ctrl+Alt+T 或通过应用程序启动器搜索栏打开终端，并运行以下命令来添加 PPA:**

```
`add-apt-repository ppa:linuxuprising/guake`
```

**然后通过输入以下命令来更新您的系统存储库索引:**

```
`sudo apt-get update`
```

**现在，您可以通过执行以下命令来安装 Guake:**

```
`sudo apt-get install guake`
```

**请注意，只有授权用户才能在 Ubuntu 上添加、删除和配置软件。**

**安装后，您将在主菜单中有两个输入。一个是访问首选设置，另一个是启动应用程序。**

**请记住添加您的密码以开始安装。**

**如果系统要求您使用 Y/n 选项继续安装，您应该输入 Y 并按 enter 键。**

**输入以下命令检查 Guake 版本号:**

```
`guake --version`
```

**您可以借助以下命令检查如何通过终端使用 Guake:**

```
`guake --help`
```

**输入以下命令以查看更多详细信息:**

```
`man guake`
```

### ****如何在 Ubuntu 20.04 上使用 Guake****

**Guake 可以从用户界面和命令行启动。通过输入以下命令将启动 Guake 应用程序:**

```
`guake`
```

**此外，您可以通过在应用程序启动器上输入 Guake 来访问 Guake 终端。**

**您应该按 F12 键来显示和隐藏 Guake 终端。**

**此外，您可以使用以下命令来显示 Guake 终端:**

```
`guake--show`
```

**要隐藏 Guake 终端，请输入以下命令:**

```
`guake --hide`
```

**如果要在 Guake 中打开一个新的选项卡，请运行以下命令:**

```
`guake -n [/path/to/folder]`
```

**如果您想退出 Guake，您只需输入以下命令:**

```
`guake -q`
```

### ****如何清除或清除****

**您可以使用以下命令删除 Guake:**

```
`sudo apt-get remove guake`
```

**输入以下命令将删除该工具及其所有配置:**

```
`sudo apt-get purge guake`
```

## **结论**

**本文教你如何在 Ubuntu 上安装 Guake。使用 Guake，您的 Ubuntu 桌面上总是有一个终端可用。此外，您还学习了如何在 Ubuntu 上删除 Guake。我希望这篇教程对你有用。**