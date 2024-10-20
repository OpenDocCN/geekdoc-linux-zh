# 在 Kali Linux - Eldernode 博客上介绍和安装 apktool

> 原文：<https://blog.eldernode.com/introducing-and-install-apktool-on-kali/>

![Introducing And Install apktool On Kali Linux](img/f567951ef79cdd5217a376e4d30e830f.png)

Apktool 是一个强大的工具，允许你逆向工程第三方，封闭的，二进制的和嵌入式的应用程序。使用 Android 包，您可以将资源文件解码成几乎原始的形式。然后修改源代码，将解码后的资源重新编译回 APK，最后重新编译应用程序。它也在 Apache 2.0 许可之下。本文是**介绍并在 Kali Linux 上安装 apk tool**。要购买您自己的 [Linux VPS](https://eldernode.com/linux-vps/) ，请访问 [Eldernode](https://eldernode.com/) 上的可用软件包来购买您需要的东西。

## **逐步介绍 Kali Linux 上的 apk tool**

### **apk tool 是什么？**

它是一个逆向工程 Android apk 文件的工具。Apktool 有一个类似于项目的结构，这使得它很容易使用。要使用 Apktool，需要 Java 8 (Java 7 也可以)，以及 Android SDK、AAPT 和 smali 的基础知识。要调试 Smali 代码，自动化一些重复性的任务如构建 apk 等是一个理想的选择。

可以安装在 [Linux](https://blog.eldernode.com/tag/linux/) 、 [Windows](https://eldernode.com/windows-vps/) 和 macOS 上。Apktool 是一个项目的集合，包含子项目和一些帮助您从源代码构建它的依赖项。apk 是一个包含资源和汇编的 java 代码的 zip 文件。一旦解压缩 APK，就会发现 classes.dex、META-INF、AndroidManifest.xml 和 resources.arsc 以及更多文件。但是如果你愿意，你可以把这些编译好的文件扔掉。

### **【Apktool 特性及用法(Kali 上的 apk tool 介绍及安装)**

Apktool 允许您解码 APK 资源，将解码的资源重新构建为二进制 APK，并组织和处理 apk。以及重复任务的自动化。虽然 Apktool 非常容易使用，但是这个工具的独特优势是双向的。它允许你反编译并修改一个应用程序。然后，您将使用 Apktool 重新编译它。这样，它将重新编译并生成一个新的 APK 文件。

因为重新设计 Android APK 文件是 Apktool 的主要用途，但是主要用途有多种选择。让我们看看 Apktool 中这些重要的开关是什么。

1- Apktool d[ecode] :该开关用于调用解码 APK 选项

2- Apktool b[uild] :此开关用于调用构建 APK 选项

3-apk tool if | install-framework:您可以使用该选项来安装和存储框架

## **如何在 Kali Linux 上安装 apk tool**

如果您正在使用 [Kali Linux](https://blog.eldernode.com/tag/linux/) 并且您希望安装 Apktool，让我说 Apktool 是预先安装在渗透测试应用程序下的，如图所示。

使用以下命令在 Kali 上安装 Apktool 以及它所依赖的任何其他软件包。

```
sudo apt-get install apktool
```

它将安装 Apktool。要检查安装的 Apktool 的版本，请在 Kali Linux 应用程序中单击 Apktool。此外，您可以打开一个终端，点击下面的命令来查看版本。

```
Apktool -version
```

### **如何从 Kali Linux** 卸载 apk tool

你可以随时从 [Kali Linux](https://blog.eldernode.com/install-and-configure-kali-linux-on-vps/) 中卸载 Apktool 。为此，只需运行下面的命令并删除 Apktool 包本身。

```
sudo apt-get remove apktool
```

此外，您可以卸载 Apktool 及其依赖项。因此，要**删除 Apktool 包**和任何其他不再需要的依赖包，请键入:

```
sudo apt-get remove --auto-remove apktool
```

可以通过运行下面的命令删除本地/配置文件。您只需要小心使用它，因为 Purgedconfig/data 不会通过重新安装软件包来恢复。

```
sudo apt-get purge apktool
```

运筹学

```
sudo apt-get purge --auto-remove apktool
```

## 结论

在本文中，我们向您介绍了 Apktool，您学习了如何在 Kali Linux 上安装 Apktool。一旦安装了这个工具，您就可以开始解码 APK 资源，将解码后的资源重新构建成二进制 APK，最后，您可以根据框架资源来组织和处理 apk。如果您知道该工具的任何替代方案，请在 [Eldernode Community](https://community.eldernode.com/) 上与您的朋友讨论。