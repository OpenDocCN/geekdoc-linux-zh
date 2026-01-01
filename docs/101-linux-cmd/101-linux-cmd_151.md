# `mkfs`命令

`mkfs`（创建文件系统）命令用于在存储设备上创建文件系统。它使用特定的文件系统类型（如 ext4、XFS、FAT32 等）格式化分区或整个磁盘。这对于为 Linux 系统准备存储设备是必不可少的。

## 语法

```sh
      mkfs [options] [-t type] device
mkfs.type [options] device 

```

## 关键特性

+   **多种文件系统支持**：ext2/3/4、XFS、FAT32、NTFS 等

+   **自定义参数**：块大小、inode 比率、标签和特性

+   **快速格式化与完整格式化**：快速格式化或彻底初始化

+   **高级选项**：加密、压缩和性能调整

## 基本用法

### 创建文件系统

```sh
      # Auto-detect and create default filesystem
sudo mkfs /dev/sdb1

# Specify filesystem type
sudo mkfs -t ext4 /dev/sdb1

# Direct filesystem creation
sudo mkfs.ext4 /dev/sdb1
sudo mkfs.xfs /dev/sdb2
sudo mkfs.fat /dev/sdb3 

```

### 常见文件系统类型

```sh
      # ext4 (recommended for Linux)
sudo mkfs.ext4 /dev/sdb1

# XFS (good for large files)
sudo mkfs.xfs /dev/sdb1

# FAT32 (cross-platform compatibility)
sudo mkfs.fat -F32 /dev/sdb1

# NTFS (Windows compatibility)
sudo mkfs.ntfs /dev/sdb1 

```

## 文件系统特定选项

### 1. ext4 文件系统

```sh
      # Basic ext4 creation
sudo mkfs.ext4 /dev/sdb1

# With label
sudo mkfs.ext4 -L MyData /dev/sdb1

# Custom block size
sudo mkfs.ext4 -b 4096 /dev/sdb1

# Custom inode ratio
sudo mkfs.ext4 -i 4096 /dev/sdb1

# With journal
sudo mkfs.ext4 -J size=128 /dev/sdb1 

```

### 高级 ext4 选项

```sh
      # Disable journaling (ext2-like)
sudo mkfs.ext4 -O ^has_journal /dev/sdb1

# Enable encryption support
sudo mkfs.ext4 -O encrypt /dev/sdb1

# Set reserved blocks percentage
sudo mkfs.ext4 -m 1 /dev/sdb1

# Custom UUID
sudo mkfs.ext4 -U 12345678-1234-1234-1234-123456789012 /dev/sdb1 

```

### 2. XFS 文件系统

```sh
      # Basic XFS creation
sudo mkfs.xfs /dev/sdb1

# Force creation (overwrite existing)
sudo mkfs.xfs -f /dev/sdb1

# With label
sudo mkfs.xfs -L MyXFS /dev/sdb1

# Custom block size
sudo mkfs.xfs -b size=4096 /dev/sdb1

# Custom sector size
sudo mkfs.xfs -s size=4096 /dev/sdb1 

```

### 高级 XFS 选项

```sh
      # Separate log device
sudo mkfs.xfs -l logdev=/dev/sdc1 /dev/sdb1

# Real-time device
sudo mkfs.xfs -r rtdev=/dev/sdd1 /dev/sdb1

# Custom inode size
sudo mkfs.xfs -i size=512 /dev/sdb1

# Allocation group size
sudo mkfs.xfs -d agcount=8 /dev/sdb1 

```

### 3. FAT32 文件系统

```sh
      # Basic FAT32 creation
sudo mkfs.fat -F32 /dev/sdb1

# With label
sudo mkfs.fat -F32 -n USBDRIVE /dev/sdb1

# Custom cluster size
sudo mkfs.fat -F32 -s 8 /dev/sdb1

# Custom volume ID
sudo mkfs.fat -F32 -i 12345678 /dev/sdb1 

```

### 4. NTFS 文件系统

```sh
      # Basic NTFS creation
sudo mkfs.ntfs /dev/sdb1

# Quick format
sudo mkfs.ntfs -Q /dev/sdb1

# With label
sudo mkfs.ntfs -L WindowsData /dev/sdb1

# Custom cluster size
sudo mkfs.ntfs -c 4096 /dev/sdb1 

```

## 实际示例

### 1. 准备 USB 驱动器

```sh
      # Check device name
lsblk

# Create partition table (if needed)
sudo fdisk /dev/sdc
# or
sudo cfdisk /dev/sdc

# Format with FAT32 for compatibility
sudo mkfs.fat -F32 -n MYUSB /dev/sdc1

# Mount and test
sudo mkdir /mnt/usb
sudo mount /dev/sdc1 /mnt/usb 

```

### 2. 设置数据驱动器

```sh
      # Create ext4 with optimal settings
sudo mkfs.ext4 -L DataDrive -b 4096 -m 1 /dev/sdb1

# Create mount point
sudo mkdir /mnt/data

# Add to fstab for automatic mounting
echo "LABEL=DataDrive /mnt/data ext4 defaults 0 2" | sudo tee -a /etc/fstab

# Mount
sudo mount /mnt/data 

```

### 3. 高性能存储

```sh
      # XFS for large files and high performance
sudo mkfs.xfs -f -L HighPerf -b size=4096 -d agcount=8 /dev/sdb1

# Or ext4 with performance optimizations
sudo mkfs.ext4 -L HighPerf -b 4096 -E stride=32,stripe-width=64 /dev/sdb1 

```

### 4. SSD 优化

```sh
      # ext4 for SSD with TRIM support
sudo mkfs.ext4 -L SSD -b 4096 -E discard /dev/sdb1

# XFS for SSD
sudo mkfs.xfs -f -L SSD -K /dev/sdb1

# Add discard option in fstab
# /dev/sdb1 /mnt/ssd ext4 defaults,discard 0 2 

```

## 高级配置

### 1. 大型文件系统

```sh
      # ext4 for very large filesystems
sudo mkfs.ext4 -T largefile4 /dev/sdb1

# XFS with optimizations for large files
sudo mkfs.xfs -f -i size=512 -d agcount=32 /dev/sdb1

# Increase inode ratio for many small files
sudo mkfs.ext4 -T small /dev/sdb1 

```

### 2. RAID 配置

```sh
      # ext4 for RAID arrays
sudo mkfs.ext4 -b 4096 -E stride=16,stripe-width=32 /dev/md0

# XFS for RAID
sudo mkfs.xfs -f -d su=64k,sw=2 /dev/md0

# Where:
# stride = (chunk-size / block-size)
# stripe-width = (number-of-data-disks * stride) 

```

### 3. 加密支持

```sh
      # ext4 with encryption
sudo mkfs.ext4 -O encrypt /dev/sdb1

# Setup LUKS encryption first
sudo cryptsetup luksFormat /dev/sdb1
sudo cryptsetup open /dev/sdb1 encrypted_disk
sudo mkfs.ext4 /dev/mapper/encrypted_disk 

```

## 备份和安全

### 1. 格式化前检查

```sh
      # Verify device
lsblk /dev/sdb
fdisk -l /dev/sdb

# Check for mounted filesystems
mount | grep /dev/sdb
lsof /dev/sdb*

# Backup important data
sudo dd if=/dev/sdb of=/backup/sdb_backup.img bs=1M 

```

### 2. 分区表备份

```sh
      # Backup partition table
sudo sfdisk -d /dev/sdb > sdb_partition_table.txt

# Restore if needed
sudo sfdisk /dev/sdb < sdb_partition_table.txt 

```

### 3. 使用前测试

```sh
      # Create filesystem
sudo mkfs.ext4 /dev/sdb1

# Mount and test
sudo mkdir /mnt/test
sudo mount /dev/sdb1 /mnt/test

# Basic functionality test
echo "test" | sudo tee /mnt/test/testfile
cat /mnt/test/testfile
sudo rm /mnt/test/testfile

# Unmount
sudo umount /mnt/test 

```

## 故障排除

### 1. 设备忙碌错误

```sh
      # Check what's using the device
lsof /dev/sdb1
fuser -v /dev/sdb1

# Unmount if mounted
sudo umount /dev/sdb1

# Kill processes if necessary
sudo fuser -k /dev/sdb1 

```

### 2. 空间不足

```sh
      # Check available space
fdisk -l /dev/sdb

# Verify partition size
cat /proc/partitions

# Check for existing filesystems
file -s /dev/sdb1 

```

### 3. 权限问题

```sh
      # Must run as root
sudo mkfs.ext4 /dev/sdb1

# Check device permissions
ls -l /dev/sdb1

# Add user to disk group if needed
sudo usermod -a -G disk username 

```

## 性能优化

### 1. 块大小选择

```sh
      # For small files (default: 4096)
sudo mkfs.ext4 -b 1024 /dev/sdb1

# For large files
sudo mkfs.ext4 -b 4096 /dev/sdb1

# For very large files (ext4 only)
sudo mkfs.ext4 -b 65536 /dev/sdb1 

```

### 2. Inode 配置

```sh
      # More inodes for many small files
sudo mkfs.ext4 -i 1024 /dev/sdb1

# Fewer inodes for large files
sudo mkfs.ext4 -i 16384 /dev/sdb1

# Fixed number of inodes
sudo mkfs.ext4 -N 1000000 /dev/sdb1 

```

### 3. 日志优化

```sh
      # Large journal for write-heavy workloads
sudo mkfs.ext4 -J size=128 /dev/sdb1

# External journal device
sudo mkfs.ext4 -J device=/dev/sdc1 /dev/sdb1

# Disable journal for read-only media
sudo mkfs.ext4 -O ^has_journal /dev/sdb1 

```

## 专用用例

### 1. 可启动媒体

```sh
      # FAT32 for UEFI boot
sudo mkfs.fat -F32 -n BOOT /dev/sdb1

# ext4 for Linux boot
sudo mkfs.ext4 -L BOOT /dev/sdb2

# Install bootloader after filesystem creation 

```

### 2. 网络存储

```sh
      # XFS for NFS exports
sudo mkfs.xfs -f -L NFSSHARE /dev/sdb1

# ext4 for Samba shares
sudo mkfs.ext4 -L SAMBA /dev/sdb1 

```

### 3. 容器存储

```sh
      # ext4 with specific features for containers
sudo mkfs.ext4 -O project -L CONTAINERS /dev/sdb1

# XFS with project quotas
sudo mkfs.xfs -f -i size=512 /dev/sdb1 

```

## 监控和验证

### 1. 文件系统信息

```sh
      # ext4 information
sudo tune2fs -l /dev/sdb1

# XFS information
sudo xfs_info /dev/sdb1

# General filesystem info
df -T /mnt/mountpoint 

```

### 2. 健康检查

```sh
      # Check filesystem after creation
sudo fsck -n /dev/sdb1

# Mount and verify
sudo mount /dev/sdb1 /mnt/test
sudo df -h /mnt/test
sudo ls -la /mnt/test 

```

## 最佳实践

### 1. 规划

```sh
      # Determine optimal filesystem type based on use case:
        # - ext4: General purpose, good for most Linux use cases
        # - XFS: Large files, high performance, NAS/database storage
        # - FAT32: Cross-platform compatibility, small devices
        # - NTFS: Windows compatibility 

```

### 2. 标签

```sh
      # Always use descriptive labels
sudo mkfs.ext4 -L "SystemData" /dev/sdb1
sudo mkfs.xfs -L "MediaStorage" /dev/sdb2
sudo mkfs.fat -F32 -n "BACKUP" /dev/sdb3 

```

### 3. 文档

```sh
      # Document filesystem configurationecho "Created: $(date)" > /root/filesystem_log.txt
echo "Device: /dev/sdb1" >> /root/filesystem_log.txt
echo "Type: ext4" >> /root/filesystem_log.txt
echo "Label: DataDrive" >> /root/filesystem_log.txt 

```

## 重要注意事项

+   **在格式化前始终备份数据**

+   **仔细验证设备名称以避免数据丢失**

+   **在格式化前卸载文件系统**

+   **为您的用例选择合适的文件系统**

+   **在选择选项时考虑性能要求**

+   **创建后测试文件系统**

+   **为将来参考记录配置**

`mkfs`命令对于准备存储设备是基本的，应该在使用时仔细考虑需求和安全性程序。

更多详细信息，请查看手册：`man mkfs`
