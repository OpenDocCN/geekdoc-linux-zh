# `badblocks`命令

`badblocks`命令用于在存储设备上搜索坏块。它可以扫描硬盘驱动器、SSD、USB 驱动器和其它存储媒体，以识别无法可靠存储数据的扇区。这对于维护数据完整性和系统可靠性至关重要。

## 语法

```sh
      badblocks [options] device [last-block] [first-block] 

```

## 关键特性

+   **非破坏性测试**：默认为只读测试

+   **破坏性测试**：进行彻底检查的写测试

+   **模式测试**：使用特定模式来检测错误

+   **进度报告**：显示扫描进度和结果

+   **输出选项**：针对不同用例的多种格式

## 基本用法

### 简单读测试

```sh
      # Basic read test (non-destructive)
sudo badblocks /dev/sdb

# Test specific partition
sudo badblocks /dev/sdb1

# Verbose output
sudo badblocks -v /dev/sdb 

```

### 显示进度

```sh
      # Show progress during scan
sudo badblocks -s /dev/sdb

# Show progress with verbose output
sudo badblocks -sv /dev/sdb 

```

## 测试模式

### 1. 只读测试（默认）

```sh
      # Non-destructive read test
sudo badblocks /dev/sdb

# Read test with verbose output
sudo badblocks -v /dev/sdb

# Read test showing progress
sudo badblocks -sv /dev/sdb 

```

### 2. 非破坏性读写测试

```sh
      # Non-destructive read-write test
sudo badblocks -n /dev/sdb

# Backup original data, test, then restore
sudo badblocks -nv /dev/sdb 

```

### 3. 破坏性写测试

```sh
      # WARNING: This will destroy all data!
sudo badblocks -w /dev/sdb

# Destructive test with verbose output
sudo badblocks -wv /dev/sdb

# Write test with progress
sudo badblocks -wsv /dev/sdb 

```

## 常用选项

### 基本选项

```sh
      # -v: Verbose output
sudo badblocks -v /dev/sdb

# -s: Show progress
sudo badblocks -s /dev/sdb

# -o: Output bad blocks to file
sudo badblocks -o badblocks.txt /dev/sdb

# -b: Specify block size (default 1024)
sudo badblocks -b 4096 /dev/sdb 

```

### 高级选项

```sh
      # -c: Number of blocks to test at once
sudo badblocks -c 65536 /dev/sdb

# -p: Number of passes (for write tests)
sudo badblocks -w -p 2 /dev/sdb

# -t: Test pattern for write tests
sudo badblocks -w -t 0xaa /dev/sdb

# -f: Force operation even if mounted
sudo badblocks -f /dev/sdb 

```

## 实际示例

### 1. 基本驱动器健康检查

```sh
      # Unmount the device first
sudo umount /dev/sdb1

# Run basic read test
sudo badblocks -sv /dev/sdb

# Save results to file
sudo badblocks -sv -o badblocks_sdb.txt /dev/sdb 

```

### 2. 详尽的驱动器测试

```sh
      # Full destructive test (destroys data!)# Make sure to backup first!
sudo badblocks -wsv /dev/sdb

# Multiple pass destructive test
sudo badblocks -wsv -p 3 /dev/sdb 

```

### 3. 测试特定范围

```sh
      # Test blocks 1000 to 2000
sudo badblocks -sv /dev/sdb 2000 1000

# Test first 1GB (assuming 4K blocks)
sudo badblocks -sv -b 4096 /dev/sdb 262144 0 

```

### 4. 与 fsck 的集成

```sh
      # Create bad blocks file
sudo badblocks -sv -o /tmp/badblocks /dev/sdb1

# Use with fsck to mark bad blocks
sudo fsck.ext4 -l /tmp/badblocks /dev/sdb1

# For new filesystem
sudo mke2fs -l /tmp/badblocks /dev/sdb1 

```

## 不同的块大小

### 选择块大小

```sh
      # 1KB blocks (default)
sudo badblocks -b 1024 /dev/sdb

# 4KB blocks (common for modern drives)
sudo badblocks -b 4096 /dev/sdb

# 512 byte blocks (traditional sector size)
sudo badblocks -b 512 /dev/sdb

# Match filesystem block size
sudo tune2fs -l /dev/sdb1 | grep "Block size"
sudo badblocks -b 4096 /dev/sdb1 

```

## 测试模式

### 写测试模式

```sh
      # Alternating pattern (0xaa = 10101010)
sudo badblocks -w -t 0xaa /dev/sdb

# All ones pattern
sudo badblocks -w -t 0xff /dev/sdb

# All zeros pattern
sudo badblocks -w -t 0x00 /dev/sdb

# Random pattern
sudo badblocks -w -t random /dev/sdb 

```

### 多种模式

```sh
      # Test with multiple patterns
sudo badblocks -w -t 0xaa -t 0x55 -t 0xff -t 0x00 /dev/sdb

# Four-pass test with different patterns
sudo badblocks -wsv -p 4 -t 0xaa -t 0x55 -t 0xff -t 0x00 /dev/sdb 

```

## 输出和报告

### 标准输出

```sh
      # Basic output (just bad block numbers)
sudo badblocks /dev/sdb

# Verbose output with details
sudo badblocks -v /dev/sdb

# Progress indicator
sudo badblocks -s /dev/sdb 

```

### 将结果保存到文件

```sh
      # Save bad blocks list
sudo badblocks -o badblocks.txt /dev/sdb

# Append to existing file
sudo badblocks -o badblocks.txt /dev/sdb >> all_badblocks.txt

# Save with verbose output to different files
sudo badblocks -v -o badblocks.txt /dev/sdb 2> scan_log.txt 

```

## 与不同存储类型一起工作

### 1. 传统硬盘

```sh
      # Standard test for HDDs
sudo badblocks -sv /dev/sda

# Thorough test with multiple passes
sudo badblocks -wsv -p 4 /dev/sda 

```

### 2. 固态硬盘

```sh
      # Read-only test (preferred for SSDs)
sudo badblocks -sv /dev/sdb

# Non-destructive test
sudo badblocks -nsv /dev/sdb

# Avoid excessive write tests on SSDs 

```

### 3. USB 驱动器

```sh
      # Test USB drive
sudo badblocks -sv /dev/sdc

# Fast test for quick verification
sudo badblocks -sv -c 65536 /dev/sdc 

```

### 4. SD 卡

```sh
      # Test SD card
sudo badblocks -sv /dev/mmcblk0

# Write test for fake capacity detection
sudo badblocks -wsv /dev/mmcblk0 

```

## 与文件系统的集成

### 1. ext2/ext3/ext4

```sh
      # Create filesystem with bad block check
sudo mke2fs -c /dev/sdb1

# Check existing filesystem
sudo fsck.ext4 -c /dev/sdb1

# Thorough check
sudo fsck.ext4 -cc /dev/sdb1 

```

### 2. 与 e2fsck 一起使用

```sh
      # Create bad blocks list
sudo badblocks -sv -o /tmp/badblocks /dev/sdb1

# Apply to filesystem
sudo e2fsck -l /tmp/badblocks /dev/sdb1 

```

## 监控和自动化

### 1. 定期检查

```sh
      # Create script for regular checking#!/bin/bash
DEVICE="/dev/sdb"
LOGFILE="/var/log/badblocks_$(date +%Y%m%d).log"

sudo badblocks -sv -o /tmp/badblocks "$DEVICE" > "$LOGFILE" 2>&1

if [ -s /tmp/badblocks ]; then
    echo "Bad blocks found on $DEVICE!" | mail -s "Bad Blocks Alert" admin@domain.com
fi 

```

### 2. SMART 集成

```sh
      # Check SMART status first
sudo smartctl -a /dev/sdb

# Run badblocks if SMART shows issues
sudo smartctl -t short /dev/sdb
sleep 300
sudo smartctl -a /dev/sdb
sudo badblocks -sv /dev/sdb 

```

## 性能考虑

### 1. 速度优化

```sh
      # Increase block count for faster scanning
sudo badblocks -c 65536 /dev/sdb

# Use larger block size
sudo badblocks -b 4096 -c 16384 /dev/sdb

# Combine options for maximum speed
sudo badblocks -sv -b 4096 -c 65536 /dev/sdb 

```

### 2. 系统影响

```sh
      # Run with lower priority
sudo nice -n 19 badblocks -sv /dev/sdb

# Limit I/O impact
sudo ionice -c 3 badblocks -sv /dev/sdb

# Combine nice and ionice
sudo nice -n 19 ionice -c 3 badblocks -sv /dev/sdb 

```

## 结果解读

### 1. 无坏块

```sh
      # Output: "Pass completed, 0 bad blocks found."
        # This indicates a healthy drive 

```

### 2. 发现坏块

```sh
      # Output shows block numbers of bad sectors
        # Example:
        # Pass completed, 5 bad blocks found. (0/0/5 errors)
        # 1024
        # 2048
        # 4096
        # 8192
        # 16384 

```

### 3. 理解块号

```sh
      # Convert block numbers to byte offsets
        # Block 1024 with 4KB block size = 1024 * 4096 = 4,194,304 bytes
        # This helps locate physical position on drive 

```

## 故障排除

### 1. 权限拒绝

```sh
      # Must run as root
sudo badblocks /dev/sdb

# Check device permissions
ls -l /dev/sdb 

```

### 2. 设备忙碌

```sh
      # Unmount all partitions first
sudo umount /dev/sdb1
sudo umount /dev/sdb2

# Check for active processes
lsof /dev/sdb*

# Use force option if necessary
sudo badblocks -f /dev/sdb 

```

### 3. 慢速性能

```sh
      # Increase block count
sudo badblocks -c 65536 /dev/sdb

# Use appropriate block size
sudo badblocks -b 4096 /dev/sdb

# Check system load
iostat 1 

```

## 安全考虑

### 1. 数据备份

```sh
      # Always backup before destructive tests
sudo dd if=/dev/sdb of=/backup/sdb_backup.img bs=1M

# Or use filesystem-level backup
sudo rsync -av /mount/point/ /backup/location/ 

```

### 2. 驱动器健康评估

```sh
      # Check SMART data first
sudo smartctl -a /dev/sdb

# Look for reallocated sectors
sudo smartctl -A /dev/sdb | grep Reallocated 

```

### 3. 何时更换驱动器

```sh
      # Replace if:
        # - Many bad blocks found (>50-100)
        # - Bad blocks increasing over time
        # - SMART indicates drive failure
        # - Critical system drive affected 

```

## 替代工具

### 1. SMART 工具

```sh
      # Use smartctl for health monitoring
sudo smartctl -t long /dev/sdb
sudo smartctl -a /dev/sdb 

```

### 2. 厂商工具

```sh
      # Many manufacturers provide specific tools
        # - Western Digital: WD Data Lifeguard
        # - Seagate: SeaTools
        # - Samsung: Samsung Magician 

```

## 重要注意事项

+   读测试是安全且非破坏性的

+   写测试会破坏设备上的所有数据

+   在测试前始终卸载设备

+   应将测试结果与其他健康指标一起解读

+   定期测试有助于防止数据丢失

+   如果发现许多坏块，应考虑更换驱动器

+   现代驱动器有备用扇区用于坏块管理

`badblocks`命令是维护存储设备健康和防止数据丢失的必备工具。

更多详细信息，请查看手册：`man badblocks`
