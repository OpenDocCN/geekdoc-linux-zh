# 在 Linux 上安装 RAID 的两种简单方法

> 原文：<https://blog.eldernode.com/2-easy-methods-to-install-raid-on-linux/>

![2 Easy Methods to Install RAID on Linux](img/bd46023fa135346392ca5489a715db01.png)

Raid 是一种可以用来提高数据存储设备的速度和可靠性的技术。在 Raid 系统中，至少并行使用两个存储设备。这些设备可以是硬盘或 SSD。本文将介绍在 Linux 上安装 RAID 的两种简单方法。如果你打算购买一台 Linux VPS 服务器，你可以在 [Eldernode](https://eldernode.com/) 网站上查看提供的软件包。

## **Linux 上如何安装 RAID**

[RAID](https://blog.eldernode.com/introducing-raid-advantages-and-disadvantages/) 代表独立磁盘冗余阵列，是一种数据存储虚拟化技术。事实上，它是一种将相同的数据存储在多个硬盘或固态驱动器( [SSD](https://blog.eldernode.com/partition-debian-10-with-ssd-storage/) )的不同位置的方法，以在驱动器出现故障的情况下保护数据。这项技术将多个物理磁盘驱动器组件组合在一个或多个逻辑单元中，以提高性能和/或实现数据冗余。它有不同的级别，根据实施的 RAID 级别，您可以利用它的优势。

### **1-在 Linux 上创建 RAID 系统**

第一步是选择要使用的存储设备和技术。当您想要在多个物理驱动器上创建驱动器时，您应该使用具有相同容量的存储设备，或者这些存储设备应该具有相同的类型和型号。

![RAID-data-storage-virtualization](img/ef862e4eec2c9719ade1e4e343a4288a.png)

您可以选择的三种主要技术选项是硬件 RAID、混合 RAID(硬件和软件元素的组合)和软件 RAID。[硬件 RAID](https://blog.eldernode.com/4-reasons-to-choose-hardraid-over-softraid/) 和混合 RAID 需要昂贵的 RAID 控制器，在发生故障的情况下，只有当您用相同的型号替换它时，阵列恢复才是可能的。

**安装 mdadm** 工具，使用以下命令创建和管理 RAID 系统:

**Debian/Ubuntu:**

```
apt install mdadm
```

**CentOS/Redhat:**

```
sudo yum install mdadm
```

**苏斯:**

```
sudo zypper install mdadm
```

**Arch Linux:**

```
sudo pacman -S mdadm
```

在安装过程中，将管理阵列所需的设置保留为默认值。

使用以下命令检查是否有硬盘:

```
cat /proc/partitions
```

你会得到硬盘的列表。标记为 sda、sdb 和 sdc。sda 是安装操作系统的地方，sdb 和 sdd 是构成 raid 系统的地方。您应该在构建系统之前准备好系统的硬盘。

要在驱动器中创建分区，请使用以下命令:

```
sudo fdisk /dev/sdb
```

键入 **p** 查看分区，键入 **n** 创建新分区。

您可以指定主 **(p)** 或扩展 **(e)** 分区类型。选择第一个分区类型 **1** 。将其他设置设为默认值，并按下**进入**。现在应该创建了新的分区。

然后按下 **w** 记录变化。驱动器同步将开始。

您需要对其他驱动器重复相同的步骤。完成操作后，输入以下命令:

```
sudo partprobe
```

最后，使用以下命令检查驱动器是否存在:

```
cat /proc/partitions
```

将显示系统注册的驱动器。让我们去创建数组。

您可以使用以下命令创建阵列:

```
sudo mdadm --create /dev/md0 -a yes -l 5 -n 3 /dev/sdb1 /dev/sdc1 /dev/sdd1
```

让我们进入下一步。

### **2-将 RAID 转换到文件系统**

![Raid](img/e2048f1f45508da96f41df1be9484325.png)

在此步骤中，运行以下命令启动转换过程:

```
mkfs.ext4 /dev/md0
```

现在您应该挂载分区了。为此，请运行以下命令:

```
mount /dev/md0 /mnt
```

您可以检查驱动器的容量，如下所示:

```
df -h
```

您将看到挂载的分区。

使用以下命令在那里创建一个文件，以确保它正常工作:

```
dd if=/dev/zero of=/mnt/file.mp4 bs=4096 count=100000
```

要检查创建的目录:

```
cd /mnt
```

并使用以下命令检查 RAID 系统的当前状态:

```
cat /proc/mdstat
```

请记住，某些故障会导致阵列处于非活动状态。尽管可能没有任何驱动器错误，但阵列将被标记为禁用。在这种情况下，按如下方式停止阵列:

```
sudo mdadm --stop /dev/md0
```

然后，您应该使用下面的命令重新组装它:

```
sudo mdadm --assemble --scan --force
```

就是这样！

## 结论

RAID 是通过几个硬盘保存或存储相同数据的基本方法。在本文中，我们教了您两种在 Linux 上安装 RAID 的简单方法。我希望这篇教程对你有用，并帮助你在 Linux 上安装 RAID。如果您有任何问题或建议，可以在评论区联系我们。