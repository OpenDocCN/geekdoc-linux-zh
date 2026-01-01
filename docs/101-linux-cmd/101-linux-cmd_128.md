# `parted` 命令

`parted` 命令用于管理 Linux 上的硬盘分区。它可以用来添加、删除、缩小和扩展硬盘分区以及它们上所在的文件系统。运行 `parted` 命令需要系统管理员权限。

**注意**：`parted` 会立即将更改写入您的磁盘，在修改磁盘分区时要小心。

### 示例：

1.  显示所有块设备的分区布局：

```sh
      sudo parted -l 

```

1.  显示特定 `disk` 的分区表

```sh
      sudo parted disk print 

```

`disk` 的例子有 /dev/sda, /dev/sdb

1.  为特定磁盘创建 `label-type` 的新磁盘标签

```sh
      sudo parted mklabel disk label-type 

```

`label-type` 可以取值 "aix", "amiga", "bsd", "dvh", "gpt", "loop", "mac", "msdos", "pc98", 或 "sun"

1.  在特定 `disk` 的 `part-time` 类型的磁盘上创建一个新的分区，文件系统为 `fs-type`，大小为 `size` Mb。

```sh
      sudo parted disk mkpart part-time fs-type 1 size 

```

`part-time` 可以取值 "primary", "logical", "extended"。

`fs-type` 是可选的。它可以取值 "btrfs", "ext2", "ext3", "ext4", "fat16", "fat32", "hfs", "hfs+", "linux-swap", "ntfs", "reiserfs", "udf", 或 "xfs"

`size` 必须小于指定磁盘的总大小。要创建大小为 50Mb 的分区，<size>将取值为 50</size>

1.  `parted` 也可以以交互式格式运行。可以通过在交互会话中输入适当的命令来执行管理磁盘分区的操作。交互会话中的 `help` 命令会显示可以执行的所有可能的磁盘管理操作的列表。

```sh
      $ sudo parted
  GNU Parted 3.3
  Using /dev/sda
  Welcome to GNU Parted! Type 'help' to view a list of commands.
  (parted) print  # prints the partition table of the default selected disk - /dev/sda 
  Model: ATA VBOX HARDDISK (scsi)
  Disk /dev/sda: 53.7GB
  Sector size (logical/physical): 512B/512B
  Partition Table: msdos
  Disk Flags: 

  Number  Start   End     Size    Type     File system  Flags
   1      1049kB  53.7GB  53.7GB  primary  ext4         boot

  (parted) select /dev/sdb  # change the current disk on which operations have to be performed 
  Using /dev/sdb
  (parted) quit  # exit the interactive session 

```

### 语法形式：

```sh
      parted [options] [device [command [options...]...]] 

```

### 选项：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| -h | --help | 显示帮助信息，列出所有可能的 `commands [options]` |
| -l | --list | 列出所有块设备上的分区布局 |
| -m | --machine | 显示机器可解析的输出 |
| -v | --version | 显示版本 |

| -a | --align | 为新创建的分区设置对齐类型。它可以取以下值：`none`：使用磁盘类型允许的最小对齐

`cylinder`：将分区对齐到磁柱

`minimal`：使用磁盘拓扑信息给出的最小对齐

`optimal`：使用磁盘拓扑信息给出的最佳对齐 |
