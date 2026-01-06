# 附录 H. 重要文件

> 原文：[`tldp.org/LDP/abs/html/files.html`](https://tldp.org/LDP/abs/html/files.html)

**启动文件**

这些文件包含提供给作为用户 shell 运行的 Bash 以及在系统初始化后调用的所有 Bash 脚本的别名和 环境变量。

`/etc/profile`

系统级默认设置，主要设置环境（所有 Bourne 类型的 shell，而不仅仅是 Bash [[1]](#FTN.AEN23892))

`/etc/bashrc`

系统级函数和 Bash 的 别名

`` `$HOME`/.bash_profile ``

用户特定的 Bash 环境默认设置，位于每个用户的家目录中（`/etc/profile` 的本地对应文件）

`` `$HOME`/.bashrc ``

用户特定的 Bash 初始化文件，位于每个用户的家目录中（`/etc/bashrc` 的本地对应文件）。只有交互式 shell 和用户脚本会读取此文件。参见 附录 M 以获取 `.bashrc` 文件的示例。

**登出文件**

`` `$HOME`/.bash_logout ``

用户特定的指令文件，位于每个用户的家目录中。从登录（Bash）shell 退出时，该文件中的命令将执行。

**数据文件**

`/etc/passwd`

系统上所有用户账户的列表，包括他们的身份、家目录、所属的组以及默认的 shell。请注意，用户密码并不存储在此文件中，[[2]](#FTN.AEN23937) 而是以加密形式存储在 `/etc/shadow` 中。

**系统配置文件**

`/etc/sysconfig/hwconf`

列出和描述附加的硬件设备。这些信息以文本形式呈现，可以被提取和解析。

```sh
bash$ **grep -A 5 AUDIO /etc/sysconfig/hwconf**	      
class: AUDIO
 bus: PCI
 detached: 0
 driver: snd-intel8x0
 desc: "Intel Corporation 82801CA/CAM AC'97 Audio Controller"
 vendorId: 8086

```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 此文件存在于 Red Hat 和 Fedora Core 安装中，但可能在其他发行版中缺失。 |

### 备注

| [[1]](files.html#AEN23892) | 这不适用于 **csh**、**tcsh** 以及与经典 Bourne shell （**sh**）无关或不是其子类的其他 shell。 |
| --- | --- |
| [[2]](files.html#AEN23937) | 在 UNIX 的旧版本中，密码曾经存储在 `/etc/passwd` 中，这也是文件名称的由来。 |
