# 如何在 Debian 10 [Buster] - Eldernode 博客上安装 Xrdp

> 原文：<https://blog.eldernode.com/install-xrdp-on-debian-10-buster/>

![How to Install Xrdp on Debian 10 [Buster]](img/b3645a16ff7496c12077af058f12d3c0.png)

Xrdp 是一个开源工具，允许用户从 Windows 操作系统中访问他们的 Linux 桌面。除了支持 [Windows RDP](https://eldernode.com/buy-rdp/) 客户端，该工具还支持其他类似的客户端，如 FreeRDP、rdesktop、NeutrinoRDP。该程序的新版本还支持 TLS 以增加安全性。在本文中，我们试着教你**如何在 Debian 10【Buster】**上安装 Xrdp。你可以访问 [Eldernode](https://eldernode.com/) 中可用的包来购买一个 [Linux VPS](https://eldernode.com/linux-vps/) 服务器。

## **教程在 Debian 10 上安装 Xrdp【巴斯特】**

如果你使用过 Windows 系统，你一定知道所有版本的 Windows 都有一个特殊的功能，叫做远程桌面或 RDP，你可以通过它远程访问你的桌面屏幕。但是在类似 Unix 的系统上，因为大多数配置是基于文本的，所以支持 SSH 的用户通常可以连接到他们的远程系统并完成他们的工作。

您一定想到过，您想要在网络环境中从您的 Windows 系统远程访问您的 Linux 桌面屏幕。但是你知道，与 Windows 不同，RDP 默认不支持 RDP。幸运的是，在 Linux 上使用一个名为 Xrdp 的工具也可以做到这一点。

在本文的后续部分中，加入我们来教您如何在 Debian 10 上安装 Xrdp。

### **如何在 Debian 10 上安装桌面环境**

在做任何事情之前，你需要确保你作为一个根用户或者拥有 T2 权限的用户登录。

由于 Linux 服务器在默认情况下没有桌面环境，只能通过命令行管理，所以第一步是为 Xrdp 安装一个桌面环境。在本教程中，我们将安装适用于远程服务器的 Xfce 桌面环境。

### **如何更新 Debian 10**

您可以使用以下命令来安装 Xfce 桌面:

```
sudo apt update
```

```
sudo apt install xfce4 xfce4-goodies xorg dbus-x11 x11-xserver-utils
```

## **在 Debian 10 上安装 Xrdp**

现在你已经用 Sudo 作为 root 登录，并且成功地在 Debian 10 上安装了桌面环境，现在是时候在 Debian 10 上安装 Xrdp 了。因为 Debian 仓库包含 Xrdp 包，所以您必须使用下面的命令来安装它:

```
sudo apt install xrdp
```

您会注意到，Xrdp 服务在执行上述命令后会自动启动。使用以下命令确认服务正在运行:

```
sudo systemctl status xrdp
```

上述命令的输出将类似于以下内容:

```
Output    ● xrdp.service - xrdp daemon  Loaded: loaded (/lib/systemd/system/xrdp.service; enabled; vendor preset: enabled)  Active: active (running) since Wed 2020-05-25 11:09:10 UTC; 4s ago  ...
```

在下一步中，您必须使用以下命令将 xrdp 用户添加到该组的 **ssl-cert** 组，然后**重启**Xrdp 服务以应用更改:

```
sudo adduser xrdp ssl-cert
```

```
sudo systemctl restart xrdp
```

重要的一点是，如果使用 ufw 管理防火墙，需要打开 Xrdp 端口。应该注意，Xrdp 在所有接口上监听**端口 3389** 。如果我们想用一个例子向您解释这一点，我们必须说，要在特定的 IP 地址或 IP 范围内访问 Xrdp 服务器，您必须遵循以下命令行:

```
sudo ufw allow from 192.168.43.0/24 to any port 3389
```

## 结论

Linux 的远程管理通常使用 SSH 服务和命令行，但您可能对 Windows 等 Linux 图形环境的远程控制感兴趣。在这种情况下，Xrdp 软件就是您的解决方案。Xrdp 是一个免费、开源的微软 rdp 应用程序，它允许 Windows 以外的操作系统进行远程连接。在本文中，我们试图教你如何在 Debian 10 上安装 Xrdp。如果你愿意，也可以参考文章[如何在 centos 7](https://blog.eldernode.com/install-xrdp-on-centos-7/) 上安装 xrdp。