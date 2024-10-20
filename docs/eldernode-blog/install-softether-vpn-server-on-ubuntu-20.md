# 如何在 Ubuntu 20.04 上安装 SoftEther VPN 服务器

> 原文：<https://blog.eldernode.com/install-softether-vpn-server-on-ubuntu-20/>

![How to Install SoftEther VPN Server on Ubuntu 20.04](img/248bfd468d0703e3f239fd6490978d00.png)

如果你想访问防火墙网络下完全保护的内容，SoftEther 是一个很好的选择。此外，如果您使用的是不安全的互联网连接，最好使用 SoftEther VPN 协议来保护您正在尝试做的事情。如果您使用此协议，您的地理位置将从另一个国家注册，您将能够访问基于特定地理位置的有限内容。本文将教你如何在 Ubuntu 20.04 上安装 SoftEther VPN 服务器。你可以查看 [Eldernode](https://eldernode.com/) 网站上提供的软件包，购买自己的 [**Ubuntu VPS**](https://eldernode.com/ubuntu-vps/) 服务器。

## **什么是 SoftEther VPN 服务器？**

SoftEther VPN 协议是一个开发和分发 SoftEther VPN 的免费、开源、跨平台和多协议的 VPN 程序。这是从微软购买 OpenVPN 和其他基于 SSTP 的 VPN 协议的替代方案。该协议提供高速和低延迟的客户端访问。由 SoftEther VPN 创建的 VPN 隧道是高度安全的，并且它们具有非常低的数据漏洞风险。此外，SoftEther VPN 协议提供 AES 和 RSA 加密，具有出色的性能，非常高的速度，以及 NAT 和 DNS 功能。

在这篇来自 [Ubuntu 培训](https://blog.eldernode.com/tag/ubuntu/)系列的文章的续篇中，您将学习如何在 Ubuntu 20.04 上安装 SoftEther VPN 服务器。

### **在 Ubuntu 上安装 SoftEther VPN 服务器的先决条件**

以下是在 Ubuntu 20.04 上安装 SoftEther VPN 服务器的先决条件:

–运行 Ubuntu 20.04 server for VPN 服务器的服务器

–运行 Ubuntu 20.04 server for VPN client 的服务器

–在两台服务器上配置超级用户密码

## **在 Ubuntu 20.04 上安装 SoftEther VPN 服务器**

我们先去解释一下 Ubuntu 20.04 上 SoftEther VPN 服务器的安装方法。为此，只需遵循以下步骤:

首先，使用以下命令更新系统包缓存:

```
apt-get update
```

现在，您应该通过运行以下命令来安装其他必需的依赖项:

```
apt-get install build-essential gnupg2 gcc make
```

这一步就是从其[官网](https://www.softether-download.com/en.aspx?product=softether)下载 SoftEther VPN 最新版本的时候了。此外，您可以使用下面的命令来完成此操作:

```
wget http://www.softether-download.com/files/softether/v4.38-9760-rtm-2021.08.17-tree/Linux/SoftEther_VPN_Server/64bit_-_Intel_x64_or_AMD64/softether-vpnserver-v4.38-9760-rtm-2021.08.17-linux-x64-64bit.tar.gz
```

下载完成后，通过输入以下命令提取下载的文件:

```
tar -xvzf softether-vpnserver-v4.38-9760-rtm-2021.08.17-linux-x64-64bit.tar.gz
```

转到您将下载文件解压缩到的目录。然后通过运行以下命令安装 SoftEther VPN 服务器:

```
cd vpnserveer
```

```
make
```

接下来，您必须将提取的目录移动到 **/ust/local 目录**，如下所示:

```
cd ..
```

```
mv vpnserver /usr/local
```

并为 vpnserver 目录设置适当的权限:

```
cd /usr/local/vpnserver/
```

```
chmod 600 *
```

```
chmod 700 vpnserver
```

```
chmod 700 vpncmd
```

### **为软交换 VPN** 创建 Systemd 服务文件

您可以通过创建系统服务文件来管理 SoftEther [VPN](https://blog.eldernode.com/installing-vpn-on-windows-server-2012/) 服务。为此，请运行以下命令:

```
nano /etc/init.d/vpnserver
```

现在添加下面的行:

```
#!/bin/sh  # chkconfig: 2345 99 01  # description: SoftEther VPN Server  DAEMON=/usr/local/vpnserver/vpnserver  LOCK=/var/lock/subsys/vpnserver  test -x $DAEMON || exit 0  case "$1" in  start)  $DAEMON start  touch $LOCK  ;;  stop)  $DAEMON stop  rm $LOCK  ;;  restart)  $DAEMON stop  sleep 3  $DAEMON start  ;;  *)  echo "Usage: $0 {start|stop|restart}"  exit 1  esac  exit 0
```

保存文件并关闭它。

接下来，输入以下命令创建所需的目录，并为系统服务文件设置适当的权限:

```
mkdir /var/lock/subsys  chmod 755 /etc/init.d/vpnserver
```

要启动 SoftEther VPN，请运行以下命令:

```
/etc/init.d/vpnserver start
```

最后，您可以在系统重新启动时启动 SoftEther VPN 服务。为此，请使用下面的命令:

```
update-rc.d vpnserver defaults
```

## **如何在 Ubuntu 20.04 上配置 SoftEther VPN 服务器**

在本节中，您将了解如何在 Ubuntu 20.04 上配置 SoftEther VPN 服务器。

首先，您应该使用以下命令将目录更改为 **/usr/local/vpnserver** :

```
cd /usr/local/vpnserver  ./vpncmd
```

如果提示您选择 VPN 组件，请键入 **1** 并按两次 **Enter** :

```
By using vpncmd program, the following can be achieved.    1\. Management of VPN Server or VPN Bridge  2\. Management of VPN Client  3\. Use of VPN Tools (certificate creation and Network Traffic Speed Test Tool)    Select 1, 2 or 3: 1
```

现在，您应该使用下面的命令设置密码:

```
ServerPasswordSet
```

如果要创建集线器并设置密码，请输入以下命令:

```
HubCreate myhub
```

要进入集线器，请运行以下命令:

```
Hub myhub
```

您可以使用以下命令允许集线器作为虚拟局域网工作:

```
SecureNatEnable
```

现在是创建 VPN 用户的时候了。为此，请运行以下命令:

```
UserCreate vpnuser
```

要为 VPN 用户设置密码，请使用以下命令:

```
UserPasswordSet vpnuser
```

通过启用 IPsec，您可以让多协议工作。为此，请运行以下命令:

```
IPsecEnable
```

接下来，回答如下所示的所有问题:

```
IPsecEnable command - Enable or Disable IPsec VPN Server Function  Enable L2TP over IPsec Server Function (yes / no): yes    Enable Raw L2TP Server Function (yes / no): yes    Enable EtherIP / L2TPv3 over IPsec Server Function (yes / no): yes    Pre Shared Key for IPsec (Recommended: 9 letters at maximum): vpnserver    Default Virtual HUB in a case of omitting the HUB on the Username: myhub    The command completed successfully.
```

要退出 VPN 配置，只需运行以下命令:

```
exit
```

在最后一步中，您应该使用以下命令配置 UFW 防火墙，以允许端口 443、5555、992 和 1194 通过:

```
ufw allow 443/tcp  ufw allow 5555/tcp  ufw allow 992/tcp  ufw allow 1194/udp
```

并重新加载 UFW 防火墙:

```
ufw reload
```

### **在 Ubuntu 20.04 上安装软以太 VPN 客户端**

一旦成功安装了 SoftEther VPN 服务器，您需要在客户端机器上安装 SoftEther VPN 客户端。首先，使用以下命令安装所需的依赖项:

```
apt-get install build-essential gnupg2 gcc make
```

要下载最新版本的 SoftEther VPN 客户端，请运行以下命令:

```
wget http://www.softether-download.com/files/softether/v4.38-9760-rtm-2021.08.17-tree/Linux/SoftEther_VPN_Client/64bit_-_Intel_x64_or_AMD64/softether-vpnclient-v4.38-9760-rtm-2021.08.17-linux-x64-64bit.tar.gz
```

下载完成后，使用以下命令提取下载的文件:

```
tar -xvzf softether-vpnclient-v4.38-9760-rtm-2021.08.17-linux-x64-64bit.tar.gz
```

现在，您应该通过运行以下命令创建一个目录来存储 VPN 客户端脚本:

```
mkdir /root/vpnscript
```

然后导航到 VPN 脚本下载页面并下载所有脚本:

```
cd /root/vpnscript 
```

```
wget https://raw.githubusercontent.com/mfaizanse/intellexlab-files/main/softether-vpn-client/remove-client.sh
```

```
wget https://raw.githubusercontent.com/mfaizanse/intellexlab-files/main/softether-vpn-client/setup-client.sh 
```

```
wget https://raw.githubusercontent.com/mfaizanse/intellexlab-files/main/softether-vpn-client/vpn-connect.sh
```

```
wget https://raw.githubusercontent.com/mfaizanse/intellexlab-files/main/softether-vpn-client/vpn-disconnect.sh
```

```
wget https://raw.githubusercontent.com/mfaizanse/intellexlab-files/main/softether-vpn-client/vpn_config
```

下载完成后，您应该为下载的脚本设置适当的权限:

```
chmod 755 *3
```

现在，您必须编辑 **vpn_config** 文件，并通过运行以下命令定义您的 vpn 客户端目录、VPN 服务器 IP、VPN 用户名和本地网关 IP:

```
nano vpn_config
```

并添加以下几行:

```
CLIENT_DIR="/root/vpnclient"  NIC_NAME="Marilyn"  ACCOUNT_NAME="vpnuser"  VPN_HOST_IPv4="vpn-server-ip"  LOCAL_GATEWAY="gateway-ip-of-client-machine"
```

保存并关闭文件。

现在使用以下命令设置 VPN 客户端:

```
./setup-client.sh
```

您应该提供您的 VPN 服务器 IP、端口、集线器名称、VPN 用户名、虚拟适配器名称和密码，如下所示:

```
Connected to VPN Client "localhost".    VPN Client>AccountCreate vpnuser  AccountCreate command - Create New VPN Connection Setting  Destination VPN Server Host Name and Port Number: IpNumber:Port    Destination Virtual Hub Name: myhub    Connecting User Name: vpnuser    Used Virtual Network Adapter Name: Marilyn    The command completed successfully.    vpncmd command - SoftEther VPN Command Line Management Utility  SoftEther VPN Command Line Management Utility (vpncmd command)  Version 4.38 Build 9760 (English)  Compiled 2021/08/17 22:32:49 by buildsan at crosswin  Copyright (c) SoftEther VPN Project. All Rights Reserved.    Connected to VPN Client "localhost".    VPN Client>AccountPassword vpnuser  AccountPasswordSet command - Set User Authentication Type of VPN Connection Setting to Password Authentication  Please enter the password. To cancel press the Ctrl+D key.    Password: **********  Confirm input: **********      Specify standard or radius: radius    The command completed successfully.
```

要连接到 VPN 服务器，请运行以下命令:

```
./vpn-connect.sh
```

最后，您将看到一个名为 vpn_Marilyn 的新 VPN 接口已经创建。要进行检查，请运行以下命令:

```
ifconfig
```

## 结论

SoftEther VPN 服务器是一款 VPN 软件，是 OpenVPN 和 Microsoft VPN 服务器的替代产品。在这篇文章中，我们教你如何在 Ubuntu 20.04 上安装 SoftEther VPN 服务器。我希望这篇教程能帮助你在 Ubuntu 20.04 上安装 SoftEther VPN 服务器，并对你有用。如果你有任何疑问或问题，可以在评论区联系我们。