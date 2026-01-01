# `tar`命令

`tar`命令代表磁带归档，用于创建归档和提取归档文件。此命令在 Linux 中提供归档功能。我们可以使用 tar 命令创建压缩或未压缩的归档文件，并维护和修改它们。

### 示例：

1.  在 abel 目录中创建 tar 文件：

```sh
      tar -cvf file-14-09-12.tar /home/abel/ 

```

1.  在当前目录中解压文件：

```sh
      tar -xvf file-14-09-12.tar 

```

### 语法：

```sh
      tar [options] [archive-file] [file or directory to be archived 

```

### 其他标志及其功能：

| **使用标志** | **描述** |
| :-- | :-- |
| `-c` | 创建归档 |
| `-x` | 提取归档 |
| `-f` | 使用给定文件名创建归档 |
| `-t` | 显示或列出归档文件中的文件 |
| `-u` | 归档并添加到现有的归档文件中 |
| `-v` | 显示详细信息 |
| `-A` | 连接归档文件 |
| `-z` | zip，告诉 tar 命令使用 gzip 创建 tar 文件 |
| `-j` | 使用 tbzip 过滤归档 tar 文件 |
| `w` | 验证归档文件 |
| `r` | 更新或添加已存在的.tar 文件中的文件或目录 |
| `-?` | 显示项目的简要总结 |
| `-d` | 查找归档与文件系统之间的差异 |
| `--usage` | 显示可用的 tar 选项 |
| `--version` | 显示已安装的 tar 版本 |
| `--show-defaults` | 显示默认启用的选项 |
| **选项标志** | **描述** |
| :-- | :-- |
| `--check-device` | 在增量归档期间检查设备号 |
| `-g` | 用于允许与 GNU 格式增量备份的兼容性 |
| `--hole-detection` | 用于检测稀疏文件中的空洞 |
| `-G` | 用于允许与旧 GNU 格式增量备份的兼容性 |
| `--ignore-failed-read` | 在文件读取错误时不要退出程序 |
| `--level` | 设置创建归档的转储级别 |
| `-n` | 假设归档是可寻址的 |
| `--no-check-device` | 创建归档时不要检查设备号 |
| `--no-seek` | 假设归档是不可寻址的 |
| `--occurrence=N` | 仅处理每个文件的第 N 次出现 |
| `--restrict` | 禁用可能有害的选项 |
| `--sparse-version=MAJOR,MINOR` | 设置要使用的稀疏格式版本 |
| `-S` | 高效处理稀疏文件 |
| **覆盖控制标志** | **描述** |
| :-- | :-- |
| `-k` | 不替换现有文件 |
| `--keep-newer-files` | 不替换比归档版本更新的现有文件 |
| `--keep-directory-symlink` | 不替换现有的符号链接 |
| `--no-overwrite-dir` | 保留现有目录的元数据 |
| `--one-top-level=DIR` | 将所有文件提取到 DIR 中 |
| `--overwrite` | 覆盖现有文件 |
| `--overwrite-dir` | 覆盖目录的元数据 |
| `--recursive-unlink` | 在提取之前递归删除目录中的所有文件 |
| `--remove-files` | 在将文件添加到目录后删除文件 |
| `--skip-old-files` | 提取时不要替换现有的文件 |
| `-u` | 在提取之前删除每个文件 |
| `-w` | 写入归档后验证归档 |
