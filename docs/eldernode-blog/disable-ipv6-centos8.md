# 如何在 CentOS 8 中禁用 IPv6-在 CentOS 内核中禁用 IPv6

> 原文：<https://blog.eldernode.com/disable-ipv6-centos8/>

![How to Disable IPv6 in CentOS 8](img/b42cbee800eb530b3f069318e9e8c938.png)

在下面的 [CentOS 8](https://eldernode.com/tag/learning-centos-8/) 教程中，我们将学习**如何在 CentOS 中禁用 IPv6。**互联网协议版本 **6** ( **IPv6** )是 IPv6 计算机网络中涉及的网络接口的标识符。如果您不想使用 Ipv6 寻址，您可以选择暂时或永久停用它。

## 如何在 CentOS 8 禁用 IPv6

让我们浏览本教程的步骤，向您展示在 CentOS 8 [Linux](https://www.linux.org/) 机器中禁用 IPv6 的几种方法。

### 在 CentOS 8 禁用 IPv6

要检查您的 **CentOS 8** 机器上是否启用了 **IPv6** ，请键入:

```
ip a | grep inet6
```

如果启用了 **IPv6** ，您会看到一些 **inet6** 行。因此，如果该命令不打印任何内容，则 IPv6 在您的所有网络接口上都被禁用。

### 使用 sysctl 命令禁用 IPv6】

该方法用于临时禁用 **IPv6** 。要使更改生效，您不需要重启系统。

首先创建一个新的 sysctl 配置文件**/etc/sysctl . d/70-IPv6 . conf**。

```
vi /etc/sysctl.d/70-ipv6.conf 
```

然后，添加以下几行并保存文件。

```
net.ipv6.conf.all.disable_ipv6 = 1  net.ipv6.conf.default.disable_ipv6 = 1
```

并且，使用下面的命令禁用 **IPv6。**

```
sysctl --load /etc/sysctl.d/70-ipv6.conf 
```

之后，应该禁用 IPv6。

接下来，运行以下命令验证 IPv6 是否被禁用。

```
ip a | grep inet6 
```

如果该命令没有返回任何内容，则意味着 **IPv6** 已在您的所有网络接口上被禁用。

当您使用此方法时，一旦您重新启动系统，您的一些网络接口可能仍然使用 **IPv6** 。当 **CentOS 8** 使用网络管理器时，就会发生这种情况。

键入以下命令以完全停止使用 **IPv6。**

```
nmcli connection modify interface ipv6.method ignore 
```

最后，重启你的 **CentOS 8** 机器。

```
reboot
```

### 

[**购买 VPS 服务器**](https://eldernode.com/vps/)

### 使用内核启动选项禁用 IPv6】

禁用 IPv6 的最佳方法是内核启动选项要求在配置后重新启动系统。

可以用 [vi 文本编辑器](https://eldernode.com/use-vi-full-text-editor/) 打开默认的 GRUB 配置文件 **/etc/default/grub** 来使用这种方法。

```
vi /etc/default/grub
```

接下来，移动到文件的末尾，按下 **O** 创建一个新行，并键入以下内容。

```
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX ipv6.disable=1" 
```

您可以保存并退出配置文件。

键入以下命令定位 grub 文件并更新 **GRUB CFG** 文件。

```
ls -lh /etc/grub*.cfg 
```

下面你会看到 **2 GRUB CFG** 文件路径: **/boot/grub2/grub.cfg** 和**/boot/EFI/EFI/centos/GRUB . CFG**。

使用以下命令创建一个新的 GRUB 配置文件，并将其保存到 **/boot/grub2/grub.cfg.**

```
grub2-mkconfig -o /boot/grub2/grub.cfg 
```

再次键入以下命令创建一个新的 GRUB 配置文件，并将其保存到**/boot/EFI/EFI/centos/GRUB . CFG**。

```
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg 
```

最后，重启你的 **CentOS 8** 机器。

```
reboot
```

重启后，使用以下命令验证 **IPv6** 是否被禁用。

```
ip a | grep inet6
```

如果您没有看到该命令的任何输出，这意味着 **IPv6** 被禁用。

**好样的** ！达到这一点意味着你已经完成了教程。你已经了解了在你的 CentOS 8 Linux 机器上禁用 **IPv6** 的两种方法。正如您所记得的，第一种方法是使用 **sysctl** ，而第二种方法是使用**内核引导选项**。虽然使用 **sysctl** 禁用 **IPv6** 是暂时的，但是**内核引导选项**是永久的，并且是最好的方法。

亲爱的用户，我们希望您喜欢本教程，您可以在评论区提出关于本次培训的问题，或者解决 [Eldernode](https://bit.ly/2Y5rVUu) 培训领域的其他问题，请参考 [提问页面](https://eldernode.com/ask) 部分并在其中提出您的问题。