# 教程在 Kali Linux[安全] - Eldernode 博客上安装并运行 Bluelog

> 原文：<https://blog.eldernode.com/install-run-bluelog-on-kali-linux/>

![Tutorial Install And Run Bluelog On Kali Linux [Security]](img/8fd60a4e28ba4a3d351b47d58b737cc3.png)

如果您需要快速找到一个区域中有多少可发现设备，您的解决方案是什么？当然，你需要一个工具来调查。Bluelog 正是你需要的工具，因为它是一个蓝牙站点调查工具。我们说“快”！这个工具与其他工具的不同之处在于它的报告速度。甚至不需要使用任何用户来帮助发现，因为它能够在没有任何人长时间参与的情况下在系统上运行来收集数据。在本文中，您将学习教程**在 Kali Linux** 上安装和运行 Bluelog。准备好你自己的 [Linux VPS](https://eldernode.com/linux-vps/) 并将发现的设备记录到 Bluelog 文件中。如果您正在使用虚拟机，请确保设备已打开，并在 USB 蓝牙设备中提供一个插件，将其连接到您的虚拟机。

## **如何在 Kali Linux 上安装运行 Bluelog**

Bluelog 在 GNU**G**general**P**public**L**license**v**version 2 下。为了识别所有隐藏的恶意蓝牙设备，你需要一个扫描仪。Bluelog 作为一个蓝牙扫描仪，可以帮助你观察你周围的东西。很明显，如果你在一个静止的位置长时间使用它，你会收集到那个区域的令人满意的轮廓。开源的 Bluelog 为你提供了基本的扫描，你可以使用它独特的“Blulog Live”功能，它提供了一个可以不断更新的网页，你可以使用你选择的 HTTP 守护进程。Bluelog 可以很好地与 GNU/Linux 操作系统支持的所有 USB 蓝牙设备一起工作，并且可以在所有的 [Linux](https://blog.eldernode.com/tag/linux/) 发行版上运行。Bluelog 软件有三类命令行选项可用，如**基本选项**、**记录选项**和**高级选项**。

## **如何在 Kali Linux 上安装 Bluelog**

这个简单的蓝牙扫描仪可以用下面的命令行安装，它也可以安装其他的 Bluelog 包。

```
sudo apt-get install bluelog
```

使用下面的命令安装依赖项和它所依赖的所有其他包。

```
sudo apt-get install
```

## **如何在 Kali Linux 上运行 Bluelog**

这是非常简单的扫描周围的所有蓝牙设备，并记录到一个文件中。正如我们提到的，使用物理机器或考虑在蓝牙设备打开的情况下使用虚拟机。当您运行 Kali Linux 时，您可能会遇到用于打开/关闭设备的热键不能正常工作的情况。如果是，添加额外的内核模块来解决问题。

### **如何扫描所有蓝牙设备并记录到一个文件**

**第一步:**

首先，让我们检查一下蓝牙设备是否正常工作。

```
hciconfig
```

通过这种方式，您可以看到您的系统或机器中的 MAC 和蓝牙设备。接口是 hci0。

**第二步:**

使用下面的命令开始扫描。

```
bluelog -i hci0 -o /root/Desktop/btdevices.log –v
```

等待大约 10 分钟，然后检查 btdevices.log 后的文件，以查看您附近的所有设备。

### 如何记录附加信息

现在，您应该记录其他信息，如制造商、广播名称和设备类别。然后检查文件 btdevices2.log。

```
bluelog -i hci0 -mnc -o /root/Desktop/btdevices2.log –v
```

一旦您执行了上述命令并检查了文件，您应该考虑到扫描过程是非常耗时的。耐心去捕捉更好的结果。

就是这样！使用该软件并确定环境中可能的蓝牙目标的数量。请注意，您将找不到已关闭可见性的设备。请随意购买您喜欢的 VPS，然后我们会在您身边学习如何在上面安装和配置 Kali Linux。

## 结论

在本文中，您学习了教程在 Kali Linux 上安装和运行 Bluelog。您可以自定义 Bluelog 的日志组件，并享受其有用的选项，如解决重试的能力，激活特定时间段的扫描窗口。如果你有兴趣阅读更多内容，可以找到我们关于如何在 Kali Linux 上配置 Burp 套件的文章。