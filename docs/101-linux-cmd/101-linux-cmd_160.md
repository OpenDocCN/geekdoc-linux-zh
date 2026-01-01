# `cfdisk` 命令

`cfdisk` 命令是 Linux 的基于 curses 的磁盘分区工具。它提供了一个用户友好的、基于文本的界面，用于创建、删除和管理磁盘分区。与 `fdisk` 不同，`cfdisk` 提供了一个更直观的菜单驱动方法。

## 语法

```sh
      cfdisk [options] [device] 

```

## 关键特性

+   **交互式界面**: 菜单驱动分区管理

+   **实时显示**: 显示当前分区表

+   **安全操作**: 需要显式写入命令以保存更改

+   **多种文件系统**: 支持各种分区类型

+   **调整大小支持**: 可以调整现有分区的大小

## 基本用法

### 启动 cfdisk

```sh
      # Open cfdisk on primary disk
sudo cfdisk /dev/sda

# Open cfdisk on secondary disk
sudo cfdisk /dev/sdb

# Auto-detect and use first available disk
sudo cfdisk 

```

### 导航

+   **上/下箭头**: 在分区之间导航

+   **左/右箭头**: 在菜单选项之间导航

+   **Enter**: 选择菜单选项

+   **Tab**: 在界面元素之间移动

## 菜单选项

### 主要操作

+   **新**: 创建新分区

+   **删除**: 删除所选分区

+   **调整大小**: 调整所选分区的大小

+   **类型**: 更改分区类型

+   **写入**: 将更改写入磁盘

+   **退出**: 退出而不保存（如果没有写入更改）

### 其他选项

+   **可引导**: 切换可引导标志

+   **验证**: 检查分区表一致性

+   **打印**: 显示分区信息

## 常见任务

### 1. 创建新分区

```sh
      # Start cfdisk
sudo cfdisk /dev/sdb

# Steps in cfdisk:
# 1\. Select "New" from menu
# 2\. Choose partition size
# 3\. Select partition type (primary/extended)
# 4\. Choose partition type (Linux, swap, etc.)
# 5\. Select "Write" to save changes
# 6\. Type "yes" to confirm
# 7\. Select "Quit" to exit 

```

### 2. 删除分区

```sh
      # In cfdisk interface:
        # 1\. Navigate to partition to delete
        # 2\. Select "Delete" from menu
        # 3\. Confirm deletion
        # 4\. Select "Write" to save changes
        # 5\. Type "yes" to confirm 

```

### 3. 更改分区类型

```sh
      # In cfdisk interface:
        # 1\. Navigate to target partition
        # 2\. Select "Type" from menu
        # 3\. Choose new partition type from list
        # 4\. Select "Write" to save changes 

```

## 常见分区类型

### Linux 分区类型

+   **83**: Linux 文件系统

+   **82**: Linux 交换分区

+   **8e**: Linux LVM

+   **fd**: Linux RAID 自动检测

### 其他常见类型

+   **07**: NTFS/HPFS

+   **0c**: FAT32 LBA

+   **ef**: EFI 系统分区

+   **01**: FAT12

## 命令行选项

```sh
      # Show help
cfdisk --help

# Show version
cfdisk --version

# Use alternative device
cfdisk /dev/sdc

# Start with specific partition table type
cfdisk -t gpt /dev/sdb
cfdisk -t dos /dev/sdb 

```

## 实际示例

### 1. 分区新 USB 驱动器

```sh
      # Insert USB drive (assume it's /dev/sdc)# Check device name
lsblk

# Start cfdisk
sudo cfdisk /dev/sdc

# Create new partition table if needed
# Create partitions as needed
# Write changes and quit 

```

### 2. 添加交换分区

```sh
      # Start cfdisk on target disk
sudo cfdisk /dev/sda

# Create new partition
# Set type to "82" (Linux swap)
# Write changes

# Format as swap
sudo mkswap /dev/sda3

# Enable swap
sudo swapon /dev/sda3 

```

### 3. 为 LVM 准备磁盘

```sh
      # Start cfdisk
sudo cfdisk /dev/sdb

# Create partition
# Set type to "8e" (Linux LVM)
# Write changes

# Create physical volume
sudo pvcreate /dev/sdb1 

```

## 安全特性

### 1. 变更跟踪

```sh
      # cfdisk tracks all changes
        # Shows asterisk (*) next to modified partitions
        # Changes only applied when "Write" is selected 

```

### 2. 需要确认

```sh
      # Writing changes requires typing "yes"
        # Provides final warning before applying changes
        # Can quit without saving if no "Write" performed 

```

### 3. 验证

```sh
      # Built-in partition table verification
        # Warns about potential issues
        # Suggests corrections for problems 

```

## 与不同分区表一起工作

### 1. GPT (GUID 分区表)

```sh
      # Create GPT partition table
sudo cfdisk -t gpt /dev/sdb

# Features:
# - Supports >2TB disks
# - Up to 128 partitions
# - Backup partition table
# - 64-bit LBA addressing 

```

### 2. MBR/DOS 分区表

```sh
      # Create MBR partition table
sudo cfdisk -t dos /dev/sdb

# Limitations:
# - 4 primary partitions maximum
# - 2TB disk size limit
# - Extended partitions for >4 partitions 

```

## 与其他工具的集成

### 1. 分区后

```sh
      # Verify partition creation
lsblk /dev/sdb
fdisk -l /dev/sdb

# Format partitions
sudo mkfs.ext4 /dev/sdb1
sudo mkfs.xfs /dev/sdb2

# Mount partitions
sudo mkdir /mnt/partition1
sudo mount /dev/sdb1 /mnt/partition1 

```

### 2. 变更前备份

```sh
      # Backup partition table before changes
sudo sfdisk -d /dev/sdb > sdb_partition_backup.txt

# Restore if needed
sudo sfdisk /dev/sdb < sdb_partition_backup.txt 

```

## 故障排除

### 1. 权限问题

```sh
      # Must run as root or with sudo
sudo cfdisk /dev/sdb

# Check device permissions
ls -l /dev/sdb 

```

### 2. 设备忙碌

```sh
      # Unmount all partitions on device first
sudo umount /dev/sdb1
sudo umount /dev/sdb2

# Check for active processes
lsof /dev/sdb* 

```

### 3. 分区表损坏

```sh
      # Verify partition table
sudo cfdisk /dev/sdb

# If corrupted, recreate partition table
# (This will destroy all data!)
sudo cfdisk -t gpt /dev/sdb  # For GPT
sudo cfdisk -t dos /dev/sdb  # For MBR 

```

## 最佳实践

### 1. 总是备份

```sh
      # Backup important data before partitioning# Create partition table backup
sudo sfdisk -d /dev/sdb > partition_backup.txt 

```

### 2. 验证设备

```sh
      # Double-check device name before starting
lsblk
fdisk -l

# Ensure you're working on correct disk 

```

### 3. 计划分区布局

```sh
      # Plan your partition scheme:
        # - Root partition (/)
        # - Swap partition
        # - Home partition (/home)
        # - Boot partition (/boot) if needed 

```

### 4. 考虑对齐

```sh
      # Modern cfdisk handles alignment automatically
        # Uses 1MB alignment by default
        # Optimal for SSDs and advanced format drives 

```

## 与其他工具的比较

### 与 fdisk 对比

+   **cfdisk**: 菜单驱动，用户友好

+   **fdisk**: 命令驱动，更可脚本化

### 与 parted 对比

+   **cfdisk**: 简单的界面，基本操作

+   **parted**: 更高级的功能，可命令行脚本化

### 与 gparted 对比

+   **cfdisk**: 基于文本，轻量级

+   **gparted**: 图形界面，需要 X11

## 重要注意事项

+   在修改分区之前始终卸载分区

+   除非你明确选择“写入”，否则更改不会写入

+   在进行分区更改之前备份重要数据

+   一些操作可能需要系统重启才能生效

+   在处理系统磁盘时请格外小心

+   考虑使用 LVM 进行更灵活的分区管理

`cfdisk` 命令在易用性和功能之间提供了优秀的平衡，适用于磁盘分区任务。

更多详细信息，请查看手册：`man cfdisk`
