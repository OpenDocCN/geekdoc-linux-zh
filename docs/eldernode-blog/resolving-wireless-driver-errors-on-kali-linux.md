# 解决 Kali Linux 上的无线驱动程序错误

> 原文：<https://blog.eldernode.com/resolving-wireless-driver-errors-on-kali-linux/>

![Resolving Wireless Driver Errors on Kali Linux](img/98283d9461ab0880967f3e3e557a78f7.png)

Kali Linux 是专门从事安全性和渗透测试的 Linux 发行版。这个操作系统可以认为是下一个版本的回溯，不再更新。Kali Linux 基于 Debian，提供各种专业或精简版本，甚至有手机版和 flash 版。在这篇文章中，我们将教你如何解决 Kali Linux 上的无线驱动程序错误。如果你想买一台 [Linux VPS](https://eldernode.com/linux-vps/) ，你可以访问 Eldernode 网站上提供的计划。

## **教程解决 Kali 服务器上的无线驱动错误**

### **Kali Linux 及其无线驱动简介**

[Kali Linux](https://blog.eldernode.com/tag/kali-linux/) 包含近 300 个黑客攻击和渗透工具，Kali Linux 开发者在这个系统中只放有用和高效的工具。

为了确保 Kali Linux 的安全性和性能，该操作系统以开源版本提供，以便用户可以确定其准确性和安全性。但是不建议使用 Kali Linux 作为主要操作系统，Kali Linux 应该只用于渗透测试。

Kali Linux 在一个地方收集了所有免费和实用的渗透测试和安全工具，安装后即可使用。同时，Kali Linux 已经默认为您完成了连接测试工具的工作。

另一方面，鉴于网络和无线驱动程序特定于您的操作系统，您应该在更改操作系统后更新您的网络驱动程序。要自动完成这些更新，您可以下载驱动程序更新软件，或者按照本文的其余部分自行解决问题。

## **如何解决 Kali Linux 上的无线驱动错误**

在这一节中，我们将教你如何使用两种方法解决 [Kali Linux](https://blog.eldernode.com/install-and-configure-kali-linux-on-vps/) 上的无线驱动程序错误。要做到这一点，只需仔细遵循以下步骤。在第一种方法中，运行 Kali 终端后，必须运行以下命令:

```
nano /etc/apt/sources.list
```

打开配置文件后，您需要将以下命令放入其中。然后，要应用更改，按 **Ctrl+X** 保存文件并退出:

```
deb http://http.kali.org/kali kali-rolling main non-free contrib
```

现在，您可以使用以下命令来更新系统和软件包:

```
sudo apt update
```

```
apt dist-upgrade
```

此时，您需要运行以下命令来安装所有驱动程序:

```
sudo apt install firmware-linux
```

使用以下命令重新启动系统:

```
sudo reboot
```

在**第二种方法**中，你可以在第一步**下载[驱动文件](https://www.dropbox.com/s/g1ih02ka62x6m72/compat-wireless-2010-06-26-p.tar.bz2?dl=0)** 。

需要注意的重要一点是，下载 Wi-Fi 驱动文件后，一定要运行。然后在空白区域点击右键，在终端点击**打开。**

运行终端后，运行以下命令:

```
tar -jxvf compat-wireless-2010-06-26-p.tar.bz2
```

执行上述步骤后，您现在可以使用以下命令:

```
cd compat-*
```

最后，运行以下命令来启动 Wi-Fi:

```
make unload
```

```
make load
```

## 结论

在 Linux 上解决无线驱动程序问题是一种令人沮丧的经历。在本文中，我们试图教你解决 Kali Linux 上的无线驱动程序错误。如果你有任何问题，可以在评论区联系我们。