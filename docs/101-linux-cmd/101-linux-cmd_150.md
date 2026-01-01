# `fsck` 命令

`fsck`（文件系统检查）命令用于检查和修复 Linux 文件系统。它可以检测和修复各种文件系统不一致性、损坏问题和结构问题。它是维护文件系统完整性和从系统崩溃中恢复的必备工具。

## 语法

```sh
      fsck [options] [filesystem...] 

```

## 关键特性

+   **多种文件系统支持**：支持 ext2, ext3, ext4, XFS 等

+   **自动检测**：可以自动检测文件系统类型

+   **交互式修复**：在修复前提示确认

+   **批处理模式**：可以在没有用户交互的情况下自动运行

+   **只读模式**：检查而不做更改

## 基本用法

### 简单文件系统检查

```sh
      # Check specific partition
sudo fsck /dev/sdb1

# Check by mount point
sudo fsck /home

# Check with verbose output
sudo fsck -v /dev/sdb1 

```

### 检查所有文件系统

```sh
      # Check all file systems in /etc/fstab
sudo fsck -A

# Check all except root
sudo fsck -AR

# Check all with progress
sudo fsck -AV 

```

## 常见选项

### 基本选项

```sh
      # -y: Answer "yes" to all questions
sudo fsck -y /dev/sdb1

# -n: Answer "no" to all questions (read-only)
sudo fsck -n /dev/sdb1

# -f: Force check even if file system seems clean
sudo fsck -f /dev/sdb1

# -v: Verbose output
sudo fsck -v /dev/sdb1 

```

### 高级选项

```sh
      # -p: Automatically repair (preen mode)
sudo fsck -p /dev/sdb1

# -r: Interactive repair mode
sudo fsck -r /dev/sdb1

# -t: Specify file system type
sudo fsck -t ext4 /dev/sdb1

# -C: Show progress bar
sudo fsck -C /dev/sdb1 

```

## 文件系统特定命令

### 1. ext2/ext3/ext4 (e2fsck)

```sh
      # Check ext4 file system
sudo fsck.ext4 /dev/sdb1

# Force check
sudo e2fsck -f /dev/sdb1

# Fix bad blocks
sudo e2fsck -c /dev/sdb1

# Thorough check with bad block scan
sudo e2fsck -cc /dev/sdb1 

```

### 2. XFS (xfs_repair)

```sh
      # Check XFS file system (read-only)
sudo xfs_repair -n /dev/sdb1

# Repair XFS file system
sudo xfs_repair /dev/sdb1

# Verbose repair
sudo xfs_repair -v /dev/sdb1 

```

### 3. FAT32 (fsck.fat)

```sh
      # Check FAT32 file system
sudo fsck.fat /dev/sdb1

# Repair FAT32
sudo fsck.fat -r /dev/sdb1

# Verbose check
sudo fsck.fat -v /dev/sdb1 

```

## 实际示例

### 1. 在挂载前检查

```sh
      # Always check before mounting suspicious drives
sudo fsck -n /dev/sdb1

# If clean, mount normally
sudo mount /dev/sdb1 /mnt/usb

# If errors found, repair first
sudo fsck -y /dev/sdb1
sudo mount /dev/sdb1 /mnt/usb 

```

### 2. 系统崩溃后的恢复

```sh
      # Boot from live CD/USB# Check root file system
sudo fsck -f /dev/sda1

# Check other partitions
sudo fsck -f /dev/sda2
sudo fsck -f /dev/sda3

# Reboot if repairs were made
sudo reboot 

```

### 3. 定期维护

```sh
      # Force check on all file systems
sudo fsck -Af

# Check with automatic repair
sudo fsck -Ap

# Check with progress indicators
sudo fsck -AVC 

```

## 处理不同场景

### 1. 只读检查

```sh
      # Check without making changes
sudo fsck -n /dev/sdb1

# Verbose read-only check
sudo fsck -nv /dev/sdb1

# Generate report only
sudo fsck -n /dev/sdb1 > fsck_report.txt 2>&1 

```

### 2. 自动修复

```sh
      # Repair automatically (dangerous!)
sudo fsck -y /dev/sdb1

# Safer automatic repair
sudo fsck -p /dev/sdb1

# Batch mode for multiple file systems
sudo fsck -Ap 

```

### 3. 交互式修复

```sh
      # Interactive mode (default)
sudo fsck /dev/sdb1

# Ask before each fix
sudo fsck -r /dev/sdb1

# Show detailed information
sudo fsck -rv /dev/sdb1 

```

## 启动时文件系统检查

### 1. 自动检查

```sh
      # Configure automatic checks in /etc/fstab
        # Sixth field controls fsck behavior:
        # 0 = no check
        # 1 = check first (root filesystem)
        # 2 = check after root filesystem
        # Example /etc/fstab entry:
        # /dev/sda1 / ext4 defaults 1 1
        # /dev/sda2 /home ext4 defaults 1 2 

```

### 2. 在下一次启动时强制检查

```sh
      # Create forcefsck file (traditional method)
sudo touch /forcefsck

# Or use tune2fs for ext filesystems
sudo tune2fs -C 1 -c 1 /dev/sda1

# Set maximum mount count
sudo tune2fs -c 30 /dev/sda1 

```

### 3. 检查间隔

```sh
      # Set check interval (ext filesystems)
sudo tune2fs -i 30d /dev/sda1  # Check every 30 days
sudo tune2fs -i 0 /dev/sda1    # Disable time-based checks

# Set mount count interval
sudo tune2fs -c 25 /dev/sda1   # Check every 25 mounts
sudo tune2fs -c 0 /dev/sda1    # Disable mount-based checks 

```

## 常见问题故障排除

### 1. 无法卸载的文件系统

```sh
      # Try read-only check first
sudo fsck -n /dev/sdb1

# If errors found, try repair
sudo fsck -y /dev/sdb1

# For severe corruption
sudo fsck -f /dev/sdb1 

```

### 2. 超级块损坏

```sh
      # List backup superblocks
sudo mke2fs -n /dev/sdb1

# Use backup superblock
sudo e2fsck -b 32768 /dev/sdb1

# Try different backup
sudo e2fsck -b 98304 /dev/sdb1 

```

### 3. Lost+Found 目录

```sh
      # Recovered files appear in lost+found
ls -la /mnt/partition/lost+found/

# Files are numbered by inode
# Use file command to identify type
file /mnt/partition/lost+found/*

# Restore files based on content 

```

## 高级修复选项

### 1. 损坏块处理

```sh
      # Scan for bad blocks during check
sudo e2fsck -c /dev/sdb1

# Thorough bad block scan
sudo e2fsck -cc /dev/sdb1

# Use existing bad block list
sudo badblocks -sv /dev/sdb1 > badblocks.list
sudo e2fsck -l badblocks.list /dev/sdb1 

```

### 2. 日志恢复

```sh
      # ext3/ext4 journal recovery
sudo e2fsck -y /dev/sdb1

# Force journal recovery
sudo tune2fs -O ^has_journal /dev/sdb1
sudo e2fsck -f /dev/sdb1
sudo tune2fs -j /dev/sdb1 

```

### 3. Inode 问题

```sh
      # Check inode usage
sudo e2fsck -D /dev/sdb1

# Rebuild directory index
sudo e2fsck -D -f /dev/sdb1

# Fix inode count problems
sudo e2fsck -f /dev/sdb1 

```

## 监控和日志记录

### 1. 检查结果

```sh
      # View fsck results
dmesg | grep -i fsck

# Check system logs
journalctl | grep fsck
tail -f /var/log/messages | grep fsck 

```

### 2. 文件系统状态

```sh
      # Check filesystem status
sudo tune2fs -l /dev/sda1 | grep -i "filesystem state"

# Check last fsck time
sudo tune2fs -l /dev/sda1 | grep -i "last checked"

# Check mount count
sudo tune2fs -l /dev/sda1 | grep -i "mount count" 

```

### 3. 自动监控

```sh
      # Script to check filesystem health#!/bin/bashfor fs in $(awk '$3 ~ /^ext[234]$/ && $2 != "/" {print $1}' /etc/fstab); do
    echo "Checking $fs..."
    sudo fsck -n "$fs" || echo "Errors found on $fs"
done 

```

## 预防和最佳实践

### 1. 定期维护

```sh
      # Schedule regular checks
        # Add to crontab for non-critical systems
        # 0 3 * * 0 /sbin/fsck -Ap > /var/log/fsck.log 2>&1 

```

### 2. 正确关机

```sh
      # Always shutdown properly
sudo shutdown -h now

# Use sync before emergency shutdown
sync
sudo shutdown -h now 

```

### 3. UPS 保护

```sh
      # Install UPS monitoring
sudo apt install apcupsd  # For APC UPS
sudo apt install nut      # Network UPS Tools

# Configure automatic shutdown on power loss 

```

## 恢复场景

### 1. 启动失败

```sh
      # Boot from live USB/CD# Mount root filesystem read-only
sudo mount -o ro /dev/sda1 /mnt

# Copy important data
sudo cp -r /mnt/home/user/important /backup/

# Unmount and check
sudo umount /mnt
sudo fsck -f /dev/sda1 

```

### 2. 数据恢复

```sh
      # Use ddrescue for severely damaged drives
sudo ddrescue /dev/sdb /backup/disk.img /backup/disk.log

# Then fsck the image
sudo fsck -f /backup/disk.img

# Mount and recover data
sudo mount -o loop /backup/disk.img /mnt/recovery 

```

### 3. 多个错误

```sh
      # Progressive repair approach
sudo fsck -n /dev/sdb1           # Check first
sudo fsck -p /dev/sdb1           # Auto-repair safe issues
sudo fsck -y /dev/sdb1           # Force repair remaining issues
sudo fsck -f /dev/sdb1           # Final thorough check 

```

## 性能考虑

### 1. 速度优化

```sh
      # Use progress indicator
sudo fsck -C /dev/sdb1

# Parallel checking (careful with dependencies)
sudo fsck -A -P

# Skip time-consuming checks when appropriate
sudo fsck -p /dev/sdb1 

```

### 2. 系统影响

```sh
      # Run during low-activity periods# Schedule during maintenance windows# Use ionice for lower priority
sudo ionice -c 3 fsck /dev/sdb1 

```

## 重要安全提示

+   **始终在检查前卸载**文件系统

+   **在修复前备份重要数据**

+   **在操作期间切勿中断** fsck

+   **首先使用只读模式**来评估损坏情况

+   **理解自动修复模式的风险**

+   **从可启动介质启动**以检查根文件系统

+   **在开始修复前准备好恢复计划**

## 退出代码

```sh
      # fsck exit codes:
        # 0: No errors
        # 1: Filesystem errors corrected
        # 2: System should be rebooted
        # 4: Filesystem errors left uncorrected
        # 8: Operational error
        # 16: Usage or syntax error
        # 32: Checking canceled by user request
        # 128: Shared-library error 

```

`fsck` 命令对于维护文件系统完整性和从损坏问题中恢复至关重要。

更多详细信息，请查看手册：`man fsck`
