# 如何在 Ubuntu 20.04 - Eldernode 博客上安装 Binwalk

> 原文：<https://blog.eldernode.com/install-binwalk-on-ubuntu-20-04/>

![How To Install Binwalk On Ubuntu 20.04](img/4c5a6ec8f7fcd593fbbac3a02ebb570d.png)

Binwalk 是一个快速工具，用于在给定的二进制图像中搜索嵌入式文件和可执行代码。它很容易用于分析，逆向工程和提取固件图像。Binwalk 与为 Unix 文件实用程序创建的魔术签名兼容。此外，它还包括一个定制的魔术签名文件，其中包含改进的文件签名。Binwalk 在 MIT 的许可下，支持大多数平台，如 Linux、OSX、Cygwin、FreeBSD 和 Windows。您将能够非常容易地在所有这些服务器上安装 Binwalk。在本文中，你将学习如何在 Ubuntu 20.04 上安装 Binwalk。如果你还没有准备好自己的 [Ubuntu VPS](https://eldernode.com/ubuntu-vps/) ，看看 [Eldernode](https://eldernode.com/) 上可用的软件包，根据你的需求找到你想要的。

## **教程在 Ubuntu 20.04 上安装 Binwalk |**Ubuntu 18.04

在本教程中，我们试图学习如何在 Ubuntu 20.04 上安装 Binwalk。为了让本教程更好地发挥作用，请考虑下面的**先决条件**:

拥有 Sudo 权限的非 root 用户。

要进行设置，请遵循我们在 Ubuntu 20.04 上的[初始服务器设置。](https://blog.eldernode.com/initial-server-setup-on-ubuntu-20/)

### **在 Ubuntu 20.04 上安装 Binwalk**

Binwalk 是作为一个开源程序开发的，它介于软件和硬件之间。要分析二进制文件，您可以使用这个简单的 Linux 工具。Binwalk 会自动提取它在固件映像中找到的所有文件。安装 Binwalk 并不难。预装在 [Kali Linux](https://blog.eldernode.com/tag/kali-linux/) 操作系统上。那么，我们来看看在 Ubuntu 上安装 Binwalk 的过程如何。

#### **安装前 Binwalk**

虽然 Binwalk 支持 Python 2.7–3 . x，但它在 [Python 3](https://blog.eldernode.com/install-python-3-ubuntu-20/) 中确实运行得更快。然而，大多数系统都将 Python 2.7 设置为默认的 Python 解释器。因此，您可以在这里找到两者的安装步骤:

```
# Python2.7
```

```
sudo python setup.py install
```

```
# Python3.x
```

```
sudo python3 setup.py install
```

### **Ubuntu 20.04 上的 Binwalk 快速安装**

首先，您需要运行下面的命令来下载 Binwalk。

```
wget https://github.com/ReFirmLabs/binwalk/archive/master.zip
```

```
unzip master.zip
```

然后，要安装 Binwalk，请键入以下命令:

```
sudo apt update
```

```
sudo apt install binwalk
```

如果您有以前安装的 Binwalk 版本，建议您在升级之前卸载它。所以，运行:

```
cd /binwalk-master
```

```
sudo python setup.py uninstall
```

```
sudo python3 setup.py install
```

**除了 Python 解释器之外，Binwalk 没有安装依赖性。但是有一些可选的运行时依赖项。“ **python-lzma** ”模块用于提高签名扫描的可靠性。**

*****注*** : *python-lzma* 模块默认包含在 Python 3 中，但 Python 2.7 需要单独安装。**

```
`sudo apt-get install python-lzma`
```

**然而，Debian/Ubuntu 用户可以使用下面的命令自动安装所有的依赖项**

```
`sudo ./deps.sh`
```

### ****如何安装 Binwalk IDA 插件****

**如果您的系统上安装了 IDA，您可以安装 Binwalk IDA 插件。**

```
`python setup.py idainstall --idadir=/home/user/ida`
```

#### ****卸载 IDA 插件****

**此外，您可以通过运行以下命令来卸载 Binwalk IDA 插件:**

```
`python setup.py idauninstall --idadir=/home/user/ida`
```

### ****如何在 Ubuntu 20.04 上卸载 Binwalk****

**如果已经将 Binwalk 安装到标准系统位置，如 setup.py install，可以通过运行以下命令删除它。但是它不会删除任何手动安装的依赖项。**

```
`# Python2.7`
```

```
`sudo python setup.py uninstall`
```

```
`#Python3`
```

```
`sudo python3 setup.py uninstall`
```

**否则，使用下面的命令**从 [Linux](https://blog.eldernode.com/tag/linux/) 操作系统中移除包**。**

```
`sudo apt-get remove binwalk`
```

**此外，您可以运行以下命令来**删除 Binwalk 包**及其依赖项:**

```
`sudo apt-get remove --auto-remove binwalk`
```

**通过这种方式，您可以删除系统中不再需要的 Binwalk 及其所有依赖包。**

#### ****如何删除带有所有配置文件的 Binwalk****

**下面的命令会删除 Binwalk 以及所有配置文件和数据，因此必须小心使用，因为它是不可恢复的。**

```
`sudo apt-get purge binwalk`
```

**也可以使用以下命令:**

```
`sudo apt-get purge --auto-remove binwalk`
```

## **结论**

**在本文中，您了解了如何在 Ubuntu 20.04 上安装 Binwalk。你为法医分析师安装了一个重要的工具。如果你认识玩 CTFs 的人，他们可能会用这个工具来分析他们找到的文件。完成你的 Ubuntu 知识，并在 [Eldernode 社区](https://community.eldernode.com/)上与你的朋友讨论。**