# 教程如何在 Centos 7 上逐步设置 WiFi-elder node

> 原文：<https://blog.eldernode.com/how-do-i-setup-wifi-on-centos-7/>

![How Do I Setup WiFi On Centos 7](img/060a80416162770cb8b88679492118fa.png)

教程我如何一步一步在 Centos 7 上设置 WiFi？CentOS 或**O**operating**S**system**C**community**E**enterprise 是 2004 年 3 月发布的 Linux 发行版。联系我们选择您的完美 [CentOS VPS](https://eldernode.com/centos-vps/) 套餐。这个开源项目由一个大型社区开发和支持，基于 Red Hat Enterprise[Linux](https://blog.eldernode.com/tag/linux/)(RHEL)资源包。这是一种商业分发，只能与支付支持合同一起使用。RHEL 提供商 Red Hat 需要公开源代码，以满足软件组件的各种许可。CentOS 项目使开发人员能够在计划使用免费的等效软件时轻松使用 RHEL 源代码。

## 如何在 Centos 7 上设置 WiFi

在你的服务器上设置 WiFi 是一个非常必要的决定。你可能还想在你的 Linux 操作系统上配置 Wi-Fi。在本文中，您将学习如何在 CentOS 7 上设置 WiFi，以便开始与您的 [Linux VPS](https://eldernode.com/linux-vps/) 一起工作。无线网络被称为 **Wi-Fi** 或 802.11 网络，因为它们覆盖了 IEEE 802.11 技术。Wi-Fi 的主要优势是它与几乎任何操作系统、游戏设备和高级打印机兼容。

### 什么是 WiFi

WiFi 或无线 T2 是一种使用无线电波连接网络的技术。Wi-Fi 连接使用无线路由器旁边的无线适配器连接到网络，允许用户访问互联网服务。Wi-Fi 通过 2.4GHz 到 5GHz 之间的广播为您的设备提供无线连接，具体取决于网络上的数据量。

### 无线网络如何工作

像手机一样，Wi-Fi 网络使用无线电波通过网络传输信息。计算机必须有一个无线适配器，将发送的数据转换为无线电信号。这个信号将通过天线传输到一个称为路由器的编码器。解密后，数据通过有线以太网连接发送到互联网。

### 如何在 Linux 上设置 WiFi

大多数人喜欢用图形工具来管理他们的计算机，但是也有一些人喜欢用命令行来管理。如果你是这些人中的一员，你应该知道管理 WiFi 有点难。在这种情况下，wpa_supplicant 可以用作命令行工具。

### 网络扫描

如果你已经很了解你的网络信息，你可以跳过这一步。如果没有，您可以用几个简单的命令来识别您想要连接的网络。wpa_supplicant 有一个名为 wpa_cli 的工具，它提供了一个命令行界面来管理 **WiFi 连接**。您可以使用这个选项来配置任何东西，但是设置配置文件似乎更容易。现在以 root 权限运行 wpa_cli，然后扫描您的网络:

```
wpa_cli    > scan
```

扫描过程可能需要几分钟。获得所需信息后，键入 quit 退出环境。创建一个区块并加密您的密码。有一个更简单的工具，您可以使用它来开始配置文件。此选项采用您的网络名称，并创建一个包含网络配置块的文件。该文件已加密:

```
wpa_passphrase networkname password >    /etc/wpa_supplicant/wpa_supplicant.conf
```

然后，开始您的配置:

现在你的配置文件位于**/etc/wpa _ supplicant/wpa _ supplicant . conf .**

此时，在您喜欢的编辑器中打开文件，并清除密码行。然后将下面一行添加到配置文件的顶部:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=wheel
```

此选项允许**轮**组中的用户管理 wpa_supplicant。将该选项的其余部分添加到网络块中。如果您连接到一个隐藏的网络，您可以添加以下行来告诉 wpa_supplicant 扫描它:

```
scan_ssid=1
```

现在，您需要进行与密钥管理和协议相关的设置。这些设置与 WPA2 相关:

```
proto=RSN
```

```
key_mgmt=WPA-PSK
```

为了增加安全性，您应该只使用 CCMP:

```
group=CCMP
```

```
pairwise=CCMP
```

**最后**，需要设置网络优先级。将首先连接更高的值。

```
priority=10
```

**保存**您的配置并重启 wpa_supplicant 以使更改生效。这当然不是配置无线网络的最佳方式，但在某些情况下会很有用。

毕竟，让我们回顾一下以下步骤，这些步骤以简单而有用的方式帮助您在 CentOS 7 上设置 **WiFi:**

**1-** 进入您的系统设置= >网络设置

**2-** 点击选项卡“无线”

**3-** 点击右侧的“添加”按钮，然后从下拉列表中选择“无线”。

**4-** 点击 SSID 字段旁边的“扫描”

**5-** 从扫描显示的网络图中双击您想要使用的连接名称。这将自动填充 SSID 字段。

**6-** 点击无线安全选项卡

**7-** 从下拉列表中选择密码类型。在我的情况下，这是水渍险/WPA2 个人。

**8-** 输入密码

**9-** 点击“确定”。

**10-** 现在点击“应用”

**11-** 打开 Konquerer 网络浏览器

尝试去一个普通的网站

如果它**不工作**，在你的机器右下角寻找一个 WLAN 接口工具。通过点击打开它，然后在右边，点击你刚刚建立的连接。

如果您有标准的 Wifi 网络连接，您现在应该已经连接上了。

### 提示

**1-** 您可以使用 iwconfig/iw 命令、 [cat 命令](https://blog.eldernode.com/useful-linux-commands/)或 GUI 来查看 WLAN 接口的速度。

**2-** 如果您希望激活无线接口，请运行以下命令列出无线接口。您应该是根用户，因为没有根级别的访问权限，普通用户不能接触配置文件。

```
ifconfig -a
```

**3-** 要了解您的 WiFi 信号水平，请使用 iwspy 命令。您应该在输出中收到以 dBm 为单位的信号质量数。

```
iwconfig wlan0 | grep -i --color signal
```

如果你渴望为 Linux 管理无线和有线网络，使用 Wicd，( **W** 无线 **I** 接口 **C** 连接 **D** aemon)一个开源工具。

## 结论

在本文中，我们试图帮助您在 CentOS 7 上设置 WiFi。如果您有兴趣阅读更多教程，请找到我们关于如何在 CentOS 7 上删除网络管理器的文章[。](https://blog.eldernode.com/remove-network-manager-centos-7/)