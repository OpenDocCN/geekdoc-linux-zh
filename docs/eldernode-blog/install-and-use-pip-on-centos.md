# 在 Centos 7 - Eldernode 博客上安装和使用 PIP 的最佳解决方案

> 原文：<https://blog.eldernode.com/install-and-use-pip-on-centos/>

![Tutorial Install and Use PIP on Centos 7 [Best]](img/b4760069e2de0712c4abab1b7fabcb81.png)

世界上最流行的编程语言通常都使用自己的包管理器。世界上最常见的语言之一是 Python，它使用 PIP。在本文中，我们将向您介绍 PIP，并教您如何在 Centos 7 或 Centos 8 VPS 上安装和使用 PIP。另外，如果你想购买一个 [**Linux VPS**](https://eldernode.com/linux-vps/) 主机，你可以访问 [Eldernode](https://eldernode.com/) 中的软件包。

## **如何在 Centos 7 上安装使用 PIP**

### **PIP 是什么？**

PIP 是 Python 的标准包管理系统，允许您安装和管理外部软件包。外部包不是用 Python 实现的包，也不是标准 Python 库的一部分。PIP 是 Linux 和 Unix 操作系统中最有用的命令行程序之一。PIP 将标准输出传输到另一个目的地，并用于将命令、程序或进程的输出发送到另一个目的地。Python 包列表中列出的所有包都可以使用 PIP 安装。

### 先决条件

## **如何在 Centos 7 上安装 PIP**

### **用百胜**安装 PIP

首先，您应该打开终端并输入以下命令来安装和启用 EPEL 存储库，因为 Centos 7 存储库中没有 PIP:

```
sudo yum install epel-release
```

然后通过执行以下命令刷新 repo 列表:

```
sudo yum repolist
```

在此步骤中，使用以下命令更新您的软件包:

```
sudo yum update
```

现在，您可以通过运行以下命令来安装 PIP:

```
sudo yum install python-pip
```

您可以使用以下命令来验证 PIP 的安装版本:

```
pip --version
```

您将收到类似以下内容的输出:

```
pip 8.1.2 from /usr/lib/python2.7/site-packages (python 2.7)
```

### **用 Curl 和 Python 安装 PIP**

此外，您可以使用 curl 和 Python 安装 PIP。为此，请在命令行中输入以下命令:

```
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"  python get-pip.py
```

## **如何在 Centos 7 上使用 PIP**

您可以使用以下命令查看有用命令的列表:

```
pip --help
```

您需要安装开发工具，因为您将需要它们来构建 Python 模块。为此，请执行以下命令:

```
sudo yum install python-devel
```

```
sudo yum groupinstall 'development tools'
```

您可以从 PyPI、版本控制、本地项目和分发文件安装软件包。

Twisted 是用 [Python](https://blog.eldernode.com/install-python-3-9-on-centos/) 编写的异步网络框架。您可以通过运行以下命令来安装 twisted 包:

```
pip install twisted
```

如果您想卸载它，请输入以下命令:

```
pip uninstall twisted
```

您可以使用以下命令从 PyPI 中搜索包:

```
pip search "twisted"
```

如果您想列出已安装的软件包，只需运行以下命令:

```
pip list
```

此外，您可以通过输入以下命令列出过期的软件包:

```
pip list --outdated
```

## 结论

在本文中，我们教您如何在 Centos 7 上安装和使用 PIP。有问题可以在评论里问。我希望这篇教程对你有用。