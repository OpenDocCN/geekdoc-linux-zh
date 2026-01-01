# `cpio` 命令

`cpio` (输入，输出) 命令是一个多功能的归档工具，可以创建和提取归档，复制文件，并处理特殊文件类型。它特别适用于系统备份、创建 initramfs 图像和与磁带归档一起工作。

## 语法

```sh
      cpio [options] < name-list
cpio [options] -i [patterns] < archive
cpio [options] -o > archive
cpio [options] -p destination-directory 

```

## 操作模式

### 1. 输出模式 (-o)

```sh
      # Create archive from file list
find . -name "*.txt" | cpio -o > archive.cpio

# Create compressed archive
find . -type f | cpio -o | gzip > archive.cpio.gz

# Verbose output
find . -name "*.conf" | cpio -ov > config_backup.cpio 

```

### 2. 输入模式 (-i)

```sh
      # Extract archive
cpio -i < archive.cpio

# Extract to specific directory
cd /restore && cpio -i < /backup/archive.cpio

# Extract specific files
cpio -i "*.txt" < archive.cpio 

```

### 3. 透明模式 (-p)

```sh
      # Copy files to another directory
find /source -type f | cpio -p /destination

# Preserve attributes while copying
find /etc -name "*.conf" | cpio -pdm /backup/configs 

```

## 常用选项

### 基本选项

```sh
      # -o: Copy-out (create archive)
find . | cpio -o > archive.cpio

# -i: Copy-in (extract archive)
cpio -i < archive.cpio

# -p: Pass-through (copy files)
find . | cpio -p /destination

# -v: Verbose output
find . | cpio -ov > archive.cpio

# -t: List archive contents
cpio -tv < archive.cpio 

```

### 高级选项

```sh
      # -d: Create directories as needed
cpio -id < archive.cpio

# -m: Preserve modification times
cpio -im < archive.cpio

# -u: Unconditional extraction (overwrite)
cpio -iu < archive.cpio

# -H: Specify archive format
cpio -oH newc > archive.cpio

# -B: Use 5120-byte blocks
cpio -oB > archive.cpio 

```

## 归档格式

### 可用格式

```sh
      # Binary format (default, obsolete)
cpio -o > archive.cpio

# New ASCII format (recommended)
cpio -oH newc > archive.cpio

# Old ASCII format
cpio -oH odc > archive.cpio

# CRC format
cpio -oH crc > archive.cpio

# TAR format
cpio -oH tar > archive.tar

# USTAR format
cpio -oH ustar > archive.tar 

```

## 实际示例

### 1. 系统备份

```sh
      # Backup entire system (excluding certain directories)
find / -path /proc -prune -o -path /sys -prune -o -path /dev -prune -o -print | \
sudo cpio -oH newc | gzip > system_backup.cpio.gz

# Backup specific directories
find /etc /home /var -type f | cpio -oH newc > important_files.cpio

# Backup with verbose output
find /home/user -type f | cpio -ovH newc > user_backup.cpio 

```

### 2. 创建 initramfs

```sh
      # Create initramfs image (common in Linux boot process)cd /tmp/initramfs
find . | cpio -oH newc | gzip > /boot/initramfs.img

# Extract existing initramfs
cd /tmp/extract
zcat /boot/initramfs.img | cpio -id 

```

### 3. 选择性文件操作

```sh
      # Archive only configuration files
find /etc -name "*.conf" -o -name "*.cfg" | cpio -oH newc > configs.cpio

# Archive files modified in last 7 days
find /home -type f -mtime -7 | cpio -oH newc > recent_files.cpio

# Archive files larger than 1MB
find /var/log -type f -size +1M | cpio -oH newc > large_logs.cpio 

```

### 4. 与压缩归档一起工作

```sh
      # Create compressed archive
find . -type f | cpio -oH newc | gzip > archive.cpio.gz
find . -type f | cpio -oH newc | bzip2 > archive.cpio.bz2
find . -type f | cpio -oH newc | xz > archive.cpio.xz

# Extract compressed archives
zcat archive.cpio.gz | cpio -id
bzcat archive.cpio.bz2 | cpio -id
xzcat archive.cpio.xz | cpio -id 

```

## 特殊文件类型

### 1. 设备文件和特殊文件

```sh
      # Archive including device files
find /dev -type c -o -type b | sudo cpio -oH newc > device_files.cpio

# Archive symbolic links
find . -type l | cpio -oH newc > symlinks.cpio

# Archive with all file types preserved
find . | sudo cpio -oH newc > complete_backup.cpio 

```

### 2. 保留属性

```sh
      # Preserve ownership and permissions
find . | sudo cpio -oH newc > backup.cpio
sudo cpio -idm < backup.cpio

# Preserve modification times
cpio -im < archive.cpio

# Preserve all attributes in pass-through mode
find /source | sudo cpio -pdm /destination 

```

## 文件过滤和选择

### 1. 模式匹配

```sh
      # Extract specific file patterns
cpio -i "*.txt" "*.conf" < archive.cpio

# Extract files in specific directory
cpio -i "etc/*" < archive.cpio

# Extract with shell globbing
cpio -i "*backup*" < archive.cpio 

```

### 2. 排除模式

```sh
      # Create archive excluding certain files
find . -type f ! -name "*.tmp" ! -name "*.log" | cpio -oH newc > clean_archive.cpio

# Use grep to filter
find . -type f | grep -v -E '\.(tmp|log|cache)$' | cpio -oH newc > filtered.cpio 

```

## 网络操作

### 1. 远程备份

```sh
      # Send archive over network
find /home | cpio -oH newc | ssh user@remote "cat > backup.cpio"

# Compressed network backup
find /data | cpio -oH newc | gzip | ssh user@remote "cat > data_backup.cpio.gz"

# Receive archive from network
ssh user@remote "cat backup.cpio" | cpio -id 

```

### 2. 磁带操作

```sh
      # Write to tape device
find /home | cpio -oH newc > /dev/st0

# Read from tape
cpio -itv < /dev/st0  # List contents
cpio -id < /dev/st0   # Extract

# Multi-volume archives
find /large_dataset | cpio -oH newc --split-at=700M > /dev/st0 

```

## 归档管理

### 1. 列出归档内容

```sh
      # List all files in archive
cpio -tv < archive.cpio

# List with detailed information
cpio -itv < archive.cpio

# Count files in archive
cpio -it < archive.cpio | wc -l

# Search for specific files
cpio -it < archive.cpio | grep "filename" 

```

### 2. 验证归档

```sh
      # Test archive integrity
cpio -it < archive.cpio > /dev/null

# Compare with filesystem
find . | cpio -oH newc | cpio -it | sort > archive_list.txt
find . | sort > filesystem_list.txt
diff archive_list.txt filesystem_list.txt 

```

### 3. 更新归档

```sh
      # Append to existing archive (not directly supported)# Workaround: extract, add files, recreate
mkdir /tmp/archive_work
cd /tmp/archive_work
cpio -id < /path/to/archive.cpio
# Add new files
find . | cpio -oH newc > /path/to/new_archive.cpio 

```

## 性能优化

### 1. 块大小调整

```sh
      # Use larger block size for better performance
cpio -oB > archive.cpio          # 5120 bytes
cpio -o --block-size=32768 > archive.cpio  # 32KB blocks

# For tape drives
cpio -o --block-size=65536 > /dev/st0  # 64KB blocks 

```

### 2. 压缩策略

```sh
      # Different compression methods
find . | cpio -oH newc | gzip -1 > fast_compress.cpio.gz    # Fast
find . | cpio -oH newc | gzip -9 > best_compress.cpio.gz    # Best ratio
find . | cpio -oH newc | lz4 > lz4_compress.cpio.lz4        # Very fast
find . | cpio -oH newc | xz -9 > xz_compress.cpio.xz        # Best ratio 

```

### 3. 并行处理

```sh
      # Use parallel compression
find . | cpio -oH newc | pigz > parallel_compressed.cpio.gz

# Parallel decompression
pigz -dc archive.cpio.gz | cpio -id 

```

## 与其他工具的集成

### 1. 与 find 结合

```sh
      # Complex find expressions
find /var/log -name "*.log" -size +10M -mtime +30 | cpio -oH newc > old_large_logs.cpio

# Execute commands during find
find . -name "*.txt" -exec grep -l "important" {} \; | cpio -oH newc > important_texts.cpio 

```

### 2. 与 rsync 结合

```sh
      # Create incremental backups
rsync -av --link-dest=/backup/previous /source/ /backup/current/
find /backup/current -type f | cpio -oH newc > incremental.cpio 

```

### 3. 与 tar 兼容

```sh
      # Convert tar to cpio
tar -cf - files | cpio -oH tar > archive.tar

# Convert cpio to tar
cpio -it < archive.cpio | tar -cf archive.tar -T - 

```

## 故障排除

### 1. 常见错误

```sh
      # "Premature end of file" error# Check if archive is complete or corrupted
file archive.cpio

# Permission denied errors
# Use sudo for system files
sudo cpio -id < archive.cpio

# "Cannot create directory" errors
# Use -d option
cpio -id < archive.cpio 

```

### 2. 调试

```sh
      # Verbose mode for debugging
cpio -idv < archive.cpio

# Check archive format
file archive.cpio
hexdump -C archive.cpio | head

# Test archive before extraction
cpio -it < archive.cpio > /dev/null
echo $?  # Should return 0 for success 

```

### 3. 恢复

```sh
      # Partial archive recovery
dd if=damaged_archive.cpio of=partial.cpio bs=512 count=1000
cpio -id < partial.cpio

# Skip damaged portions
cpio -id --only-verify-crc < archive.cpio 

```

## 脚本和自动化

### 1. 备份脚本

```sh
      #!/bin/bash# Automated backup script
BACKUP_DIR="/backup/$(date +%Y%m%d)"
SOURCE="/home /etc /var/log"

mkdir -p "$BACKUP_DIR"
for dir in $SOURCE; do
    find "$dir" -type f | cpio -oH newc | gzip > "$BACKUP_DIR/$(basename $dir).cpio.gz"
done 

```

### 2. 恢复脚本

```sh
      #!/bin/bash# Automated restoration script
ARCHIVE="$1"
DEST="${2:-/restore}"

if [ ! -f "$ARCHIVE" ]; then
    echo "Archive not found: $ARCHIVE"
    exit 1
fi

mkdir -p "$DEST"
cd "$DEST"

if [[ "$ARCHIVE" =~ \.gz$ ]]; then
    zcat "$ARCHIVE" | cpio -idm
else
    cpio -idm < "$ARCHIVE"
fi 

```

## 最佳实践

### 1. 归档命名

```sh
      # Use descriptive names with dates
backup_$(hostname)_$(date +%Y%m%d_%H%M%S).cpio.gz

# Include source information
home_backup_$(date +%Y%m%d).cpio
etc_configs_$(date +%Y%m%d).cpio 

```

### 2. 验证

```sh
      # Always verify critical archives
find /important | cpio -oH newc > critical.cpio
cpio -it < critical.cpio | wc -l
find /important | wc -l 

```

### 3. 文档

```sh
      # Document archive contents
cpio -itv < archive.cpio > archive_contents.txt
echo "Created: $(date)" >> archive_info.txt
echo "Source: /path/to/source" >> archive_info.txt 

```

## 重要注意事项

+   **选择合适的格式**：使用 `newc` 格式适用于现代系统

+   **保留权限**：使用 `-m` 并以适当的用户运行

+   **测试归档**：始终验证归档完整性

+   **使用压缩**：与 gzip/bzip2/xz 结合以实现空间效率

+   **处理特殊文件**：小心处理设备文件和符号链接

+   **网络安全**：使用安全通道进行远程操作

+   **备份策略**：定期备份并进行验证

`cpio` 命令在创建灵活的归档和系统备份方面非常强大，尤其是在与 find 和压缩工具结合使用时。

更多详细信息，请查看手册：`man cpio`
