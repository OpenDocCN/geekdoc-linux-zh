# `uname` 命令

`uname` 命令允许您打印系统信息，默认输出内核名称。

## 语法：

```sh
      $ uname [OPTION] 

```

## 示例

1.  打印所有系统信息。

```sh
      $ uname -a 

```

1.  打印内核版本。

```sh
      $ uname -v 

```

## 选项

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-a` | `--all` | 打印所有信息，如果未知则省略处理器和硬件平台。 |
| `-s` | `--kernel-name` | 打印内核名称。 |
| `-n` | `--nodename` | 打印网络节点主机名。 |
| `-r` | `--kernel-release` | 打印内核发布版本。 |
| `-v` | `--kernel-version` | 打印内核版本。 |
| `-m` | `--machine` | 打印机器硬件名称。 |
| `-p` | `--processor` | 打印处理器类型（非便携式）。 |
| `-i` | `--hardware-platform` | 打印硬件平台（非便携式）。 |
| `-o` | `--operating-system` | 打印操作系统。 |
