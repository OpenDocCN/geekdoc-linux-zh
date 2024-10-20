# 如何在 CentOS 8 - Eldernode 博客上安装 oVirt

> 原文：<https://blog.eldernode.com/install-ovirt-on-centos-8/>

![How to Install oVirt on CentOS 8](img/52b68b2726ae05fca3ea22c8e4fe826d.png)

oVirt 是一个网络虚拟化管理应用程序。它也是由 Redhat 创建的免费 web 虚拟化管理 web 应用程序。oVirt 基于 libvirt，它允许您管理托管在受支持线路上的虚拟设备，如 KVM、Xen 和 VirtualBox。oVirt 应用程序可以管理多台主机。该程序通过 HTTP over XML-RPC 与主机服务器通信。该计划有存储能力，如 NFS，iSCSI 和本地存储。oVirt 项目是一个开放的虚拟项目，它根据主机和来宾的高级功能创建了一个具有特殊功能的服务器虚拟化管理系统。该软件包括高可用性、高效传输、存储管理、系统调度等等。在本文中，我们尝试学习如何在 CentOS 8 上安装 oVirt。你可以访问 [Eldernode](https://eldernode.com/) 提供的套装来购买 [CentOS VPS](https://eldernode.com/centos-vps/) 服务器。

## **教程在 CentOS 8 上安装 oVirt**

oVirt 项目表明，思科、IBM、英特尔、NetApp、RedHat 和 SUSE 等公司已经联手帮助彼此构建用于虚拟平台开发的开源套件。使用 oVirt 项目，该行业获得了免费资源，这些资源明确地管理虚拟扇区。

oVirt 项目旨在在一个集成的虚拟平台内提供和构建一个繁荣的社区。该项目为主机和来宾提供高级虚拟化管理功能。oVirt 包括高可用性、动态传输、存储管理、系统调度等等。

oVirt 的目标是为开放式虚拟化管理提供互连且可单独重用的部分，并为大量私有和公共使用改进核心部分。关注本文，了解如何在 [CentOS](https://blog.eldernode.com/tag/centos/) 8 上安装 oVirt。

### **在 CentOS 8 上安装 oVirt | CentOS 7**

在本节中，我们将教您如何在 CentOS 8 上安装 oVirt。请注意，在开始安装之前，您必须在系统上设置 FQDN 主机名。在下面的命令中，您应该输入您的域地址，而不是 example.com。

```
hostnamectl set-hostname centos.example.com
```

在下一部分中，您必须使用您想要的编辑器打开 **/etc/** hosts 文件。这里我们使用纳米编辑器。

```
nano /etc/hosts
```

现在，您需要通过添加以下命令将您的系统链接到主机名。然后**保存**文件并退出。

```
your-server-ip centos.example.com
```

### 如何安装所需的库到**安装 oVirt**

完成上一步后，您需要将 oVirt 和其他所需的存储库添加到您的系统中。首先，您需要通过运行以下命令来安装 oVirt 存储库:

```
dnf install https://resources.ovirt.org/pub/yum-repo/ovirt-release44.rpm
```

然后需要启用 Java 包工具，pki-deps 和 [PostgreSQL](https://blog.eldernode.com/tutorial-postgresql-installation-ubuntu-20/) 模块。为此，您必须执行以下命令:

```
dnf module enable javapackages-tools -y
```

```
dnf module enable pki-deps -y
```

```
dnf module enable postgresql:12 -y
```

### **如何安装奥维特引擎**

您必须首先通过执行以下命令来更新存储库:

```
dnf update -y
```

现在，您可以通过运行以下命令轻松安装 oVirt 引擎:

```
dnf install ovirt-engine -y
```

成功安装 oVirt 后，现在可以通过运行以下命令来配置 oVirt 引擎:

```
engine-setup
```

## 结论

oVirt 是一款开源分布式虚拟化解决方案，旨在管理基础设施。oVirt 使用可信的 KVM 管理程序，并基于其他几个项目，包括 libvirt、Gluster、PatternFly 和 Ansible。在本文中，我们试图了解如何在 CentOS 8 上安装 oVirt。