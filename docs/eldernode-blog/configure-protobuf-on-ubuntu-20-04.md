# 教程在 Ubuntu 20.04 上配置 proto buf-elder node 博客

> 原文：<https://blog.eldernode.com/configure-protobuf-on-ubuntu-20-04/>

![Tutorial Configure Protobuf on Ubuntu 20.04](img/52f3780e44e6a537699f4efec7e73f2f.png)

Protobuf 是一种主要用于内部通信的标准，但最近他们也在努力使这种标准对用户友好，这在 Android 中目前是可能的，并且由于其在 HTTP/2 中的二进制可用性，已经为浏览器准备了示例。这就是为什么它有更好的速度和性能。在本文中，您将了解如何在 Ubuntu 20.04 上配置 Protobuf。如果你想购买一台 [**Linux VPS**](https://eldernode.com/linux-vps/) 服务器，你可以访问 [Eldernode](https://eldernode.com/) 中的软件包。

## **教程在 Ubuntu Linux 上安装和配置 proto buf**

### 简介 Protobuf:

Protobuf 是一个开源的、免费的、跨平台的库。并用于在构建应用程序时通过网络相互通信和存储数据。

Protobuf 是一个带有 proto 扩展名的文本文件，很容易理解。第一行标识了 Protobuf 版本，第三行定义了一个消息，它有一个标记为 1 的 int 字段、一个标记为 2 的 string 字段和一个标记为 3 的 bool 字段。

### Protobuf 特性

1.  定义数据类型
2.  数据自动压缩
3.  消息具有模式结构
4.  文档是在原型文件中编写的
5.  用任何编程语言读取数据
6.  随时开发模式结构
7.  比 JSON 快
8.  为您的语言自动生成代码

## **如何在 Ubuntu 20.04 上安装和配置 proto buf**

注意:在执行任何操作之前，请通过输入以下命令来更新您以前的软件包:

```
sudo apt update
```

然后，您可以通过运行以下命令在您的 [ubuntu](https://blog.eldernode.com/tag/ubuntu/) 系统上安装 Protobuf 编译器:

```
sudo apt install protobuf-compiler
```

### **如何在 Ubuntu 20.04 上卸载 proto buf**

您可以通过输入以下命令删除已安装的 Protobuf:

```
sudo apt-get remove protobuf-compiler
```

然后，您应该运行以下命令来删除相关软件包:

```
sudo apt-get autoremove protobuf-compiler
```

## 结论

本文教你如何在 Ubuntu 上使用命令行安装、配置和卸载 Protobuf。通过这个循序渐进的教程，你可以很容易地在你的 Ubuntu 20.04 系统上安装 Protobuf。