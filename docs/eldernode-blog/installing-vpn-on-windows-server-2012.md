# 如何在 Windows Server 2012 - Eldernode 上安装 VPN

> 原文：<https://blog.eldernode.com/installing-vpn-on-windows-server-2012/>

![How to install VPN on Windows Server 2012](img/2b9fadcd3d2f0807cb55277e4a399927.png)

我们决定通过 Windows Server 培训系列来教你如何在 Windows Server 2012 上安装 [VPN](https://en.wikipedia.org/wiki/Virtual_private_network) 。在我们开始在 Windows Server 上配置 VPN 之前，我们先来看看 VPN 及其性能。如果你愿意，你可以选择你的完美的 [Windows 虚拟专用服务器](https://eldernode.com/windows-vps/)包并在上面安装 Windows Server 2012，然后继续按照说明操作。

VPN 或虚拟专用网络是一组连接到公共网络(如互联网)的计算机。在商业场所，VPN 用于与远程数据中心通信。人们可以使用 VPN 来访问实际上不在 Lan 上的网络资源(例如，您在另一个国家，在 VPN 的帮助下连接到您的组织，您可以控制所有的服务器)。此外，VPN 可以加密通信。

## 如何在 Windows Server 2012 上安装 VPN

当您连接到 VPN 时，您实际上正在使用 VPN 客户端，并且您正在使用密码连接到互联网上的 VPN 服务器。因此，您与连接到该服务器的其他人在一个网络上，并且您可以根据您组织的策略来使用资源。

最重要的是你的通信是用 VPN 加密的。

在 Windows Server 2012 上安装 VPN 很容易，请执行以下步骤。这个配置是服务器端的。这意味着在另一个地方配置了一个 VPN 服务器，您作为客户端连接到它。

### 1。如何安装与远程访问相关的角色

打开 [**服务器管理器**](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager) ，点击**管理**。然后选择**添加角色和功能**。

![Add Roles and Features in server manager](img/c65a1f8c9343e95df426f3f655b0961a.png)

点击**下一个**进入**角色**页签:

![How to add roles and features in server manager](img/1e0cec63f73ae625cc6b95399cc8d423.png)

***

![select installation type in server manager](img/4ea9f58c20c49305e9ed995788e87add.png)

***

![how to select server to add roles and features in server manager](img/7d0bb22765eec1e62821d120c8f020c1.png)

然后选择**远程访问**，点击**下一步**:

![select remote access in roles section](img/3dce3642bc2461d72f75164f686168f5.png)

您不需要从**功能**选项卡中选择任何内容。点击**下一个**:

![add roles and features wizard](img/ec07e7b4a139cbce78c76cdbcd24e6a3.png)

再次点击**下一个**:

![remote access settings in server manager](img/432e37a9452d3f27025b5cdf14d73326.png)

选择**直接访问和 VPN** (RAS):

![how to select Direct access and vpn in server manager](img/e116f89c92866a572f891cec4874ca41.png)

将出现一个对话框，介绍系统中不存在的功能。点击**添加功能**:

![Add roles and features wizard](img/57c8b31ffcb1eb9289804d11059be96b.png)

安装**远程访问** **角色。**需要几分钟:

![Install Remote Access Role in server manager](img/dcd9fff22071c3d2849ee41993ad5b98.png)

***

![install directaccess and vpn (RAS)](img/fd392b675f4462e00305c6e183da15eb.png)

### 2。安装和配置 VPN 教程

回到服务器管理控制台，点击**远程访问**。选择您的服务器并右键单击它。然后点击**远程访问管理**:

![Remote Access Management](img/86cc9418501179d204fa566d5f68a72a.png)

选择**运行入门向导**，如下图所示:

![Run the Getting Started the Wizard](img/51b5130dd90eb5053237e6bd6e14ea1f.png)

选择**仅部署 VPN**，这里程序将开始安装:

![How to configure remote access to deploy vpn](img/c88b6dd097ccf19796272b74a6b59e8e.png)

现在选择您的服务器并右键单击它。

选择**配置并启用路由和远程访问**:

![Configure and Enable Routing and Remote access](img/3a781bb4f6cc0fb6a3f59a400741cbaf.png)

将出现新的安装指南:

![Routing and Remote access server setup wizard](img/2eb924d9297014ff1fd784e5745d9588.png)

选择**自定义配置**，点击**下一步**:

![How to Configure and Enable Routing and Remote access](img/bb14508ac26f2665dbd7d7cfe36a61f8.png)

只需选择 **VPN 访问**选项:

![How to enable vpn service in server manager](img/aca0fb838900907e19be68c11836790c.png)

**最后**，完成流程，打开程序:

![How to complete the routing and remote access server](img/c7aefbb838353ccca363b37066afb903.png)

***

![How to start the vpn service ](img/d30d822ec3d95c56273ecbd84be3c4e5.png)

***注意:*** 路由器和防火墙必须正确配置才能支持 VPN 性能。

### 3。如何启用远程控制的用户访问

您可以允许用户通过 **Active Directory** 设置进行远程访问。

![Remote Access from the Active Directory settings](img/eb0648601541ed0c24c919f27802725a.png)

## 结论

由于用户的高需求，如何在 Windows Server 2012 中安装 VPN 是本文中最重要和最实用的案例之一。本教程还解释了如何安装与远程访问相关的角色以及如何为远程控制启用用户访问的完整步骤。