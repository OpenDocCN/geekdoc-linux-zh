# `gunzip` 命令

`gunzip` 命令是 `gzip` 命令的反义词。换句话说，它解压缩由 `gzip` 命令压缩的文件。

"`gunzip` 接受一系列文件作为参数。它将每个以 `.gz`、`-gz`、`.z`、`-z` 或 `_z`（不区分大小写）结尾且以正确的魔数开头的文件替换为未压缩文件，并移除原始扩展名。"

### 示例：

1.  解压缩文件

```sh
      gunzip filename.gz 

```

1.  "递归地解压缩目录内所有与 `gunzip` 支持的压缩文件格式匹配的文件：

```sh
      gunzip -r directory_name/ 

```

1.  "解压缩当前工作目录中所有后缀匹配 `.tgz` 的文件：

```sh
      gunzip -S .tgz * 

```

1.  列出压缩和未压缩的大小、压缩比以及输入压缩文件的未压缩名称：

```sh
      gunzip -l file_1 file_2 

```

### 语法：

```sh
      gunzip [ -acfhklLnNrtvV ] [-S suffix] [ name ...  ] 

```

### 关于使用 gzip、gunzip 和 tar 命令的视频教程：

[本视频](https://www.youtube.com/watch?v=OBtG8zfVwuI) 展示了如何在 Unix shell 中进行压缩和解压缩。它使用 `gunzip` 作为解压缩命令。

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| -c | --stdout | 写入标准输出，保持原始文件不变 |
| -h | --help | 提供帮助信息 |
| -k | --keep | 保持（不删除）输入文件 |
| -l | --list | 列出压缩文件内容 |
| -q | --quiet | 抑制所有警告 |
| -r | --recursive | 递归地对目录进行操作 |
| -S | --suffix=SUF | 在压缩文件上使用后缀 SUF |
|  | --synchronous | 同步输出（如果系统崩溃更安全，但速度较慢） |
| -t | --test | 测试压缩文件完整性 |
| -v | --verbose | 详细模式 |
| -V | --version | 显示版本号 |

### **注意：gunzip filename.gz 等同于 gzip -d filename.gz。

### 警告：默认情况下，不使用 -c 选项运行 gunzip 会删除原始压缩文件。
