# 如何在 Kali Linux 上安装 GNURadio——在 Kali 上使用 GNURadio

> 原文：<https://blog.eldernode.com/install-gnuradio-on-kali-linux/>

![How to Install GNURadio on Kali Linux](img/176bbdc7fd31129c718a8bca2f6bd978.png)

GNURadio 是一个开发软件无线电项目的项目。该框架为软件无线电用户和爱好者提供了大多数信号处理模块和所有类型的电信调制。本文将解释**如何在 Kali Linux** 上安装 GNURadio。如果你想购买一台 **[Linux VPS](https://eldernode.com/linux-vps/)** 服务器，请查看 [Eldernode](https://eldernode.com/) 网站上提供的软件包。

## **教程在 Kali Linux 上安装 GNUradio**

### **GNURadio 是什么？**

GNURadio 是一个开源的免费软件开发工具包，为实现软件定义无线电提供信号处理模块。它可用于编写程序、从数字流接收数据、将数据传输到数字流，然后使用硬件传输该数字流。该框架包括滤波器、信道编码、同步元件、均衡器、解调器、编码器、解码器以及无线电系统中常用的许多其他模块。

GNURadio 支持 Python 和 C++开发，并提供图形化设计方法。它用于学术、业余爱好者和商业环境，支持无线通信研究和现实世界的无线电系统。

在这篇来自 [Kali Linux 培训](https://blog.eldernode.com/tag/kali-linux/)系列的文章的续篇中，我们打算教你如何在 Kali Linux 上安装 GNURadio。

## **如何在 Kali Linux 上安装 GNURadio**

在上一节向您介绍了 GNURadio 之后，现在我们想教您如何在 Kali Linux 上安装 GNURadio。安装 GNURadio 并不难，按照下面的步骤操作即可。第一步，**借助以下命令更新您的服务**包:

```
sudo apt update
```

您可以使用以下命令安装 GNURadio :

```
sudo apt install gnuradio
```

等待 GNURadio 安装完成。整个过程完成后，您将看到一个终端通知。

### **如何在 Kali Linux 上使用 GNURadio**

如果您想**创建 GNURadio 信号流程图**，您可以使用 GNURadio 配套图形工具:

```
gnuradio-companion -h
```

要显示已安装 GNURadio 的详细信息，请输入以下命令:

```
gnuradio-config-info -h
```

您可以使用下面的 GUI 为 GNURadio 创建过滤器:

```
gr_filter_design -h
```

## 常见问题解答

[sp _ easy agreement]

## 结论

GNURadio 可与外部 RF 硬件配合使用，创建软件定义的无线电和信号处理系统。GNURadio 包包含 gnuradio-companion，一个用于创建信号流图和生成流图源代码的图形工具，以及各种工具和应用程序。在本文中，我们教您如何在 Kali Linux 上安装 GNURadio。我希望这篇教程对你有用。