# `mount` 命令

`mount` 命令用于挂载一个文件系统并将其“附加”到一个现有的目录结构树中，使其可访问。

### 示例：

1.  显示版本信息：

```sh
      mount -V 

```

1.  在设备上找到的文件系统类型为 type，挂载到目录 dir：

```sh
      mount -t type device dir 

```

### 语法形式：

```sh
      mount [-lhV] 

```

```sh
      mount -a [-fFnrsvw] [-t vfstype] [-O optlist] 

```

```sh
      mount [-fnrsvw] [-t fstype] [-o options] device dir 

```

### 其他选项及其功能：

| **短选项** | **长选项** | **描述** |
| :-- | :-- | :-- |
| `-h` | `--help` | 显示帮助信息并退出 |
| `-n` | `--no-mtab` | 不写入 /etc/mtab 挂载 |
| `-a` | `--all` | 挂载 fstab 中提到的所有（给定类型的）文件系统 |
| `-r` | `--read-only` | 以只读方式挂载文件系统 |
| `-w` | `--rw` | 以读写方式挂载文件系统。 |
| `-M` | `--move` | 将子树移动到其他位置。 |
| `-B` | `--bind` | 在其他位置重新挂载子树（这样其内容在两个地方都可用）。 |
