# `split`命令

Linux 中的`split`命令用于将文件分割成更小的文件。

### 示例

1.  使用文件名将文件分割成更小的文件。

```sh
      split filename.txt 

```

1.  从前缀 file 开始将名为 filename 的文件分割成 200 行的段。

```sh
      split -l 200 filename file 

```

这将创建名为 fileaa、fileab、fileac、filead 等，每文件 200 行的文件。

1.  使用前缀 file 将名为 filename 的文件分割成 40 字节的段。

```sh
      split -b 40 filename file 

```

这将创建名为 fileaa、fileab、fileac、filead 等，大小为 40 字节的文件。

1.  使用`--verbose`标志分割文件以查看正在创建的文件。

```sh
      split filename.txt --verbose 

```

### 语法：

```sh
      split [options] filename [prefix] 

```

### 其他标志及其功能

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-a` | `--suffix-length=N` | 生成长度为 N 的后缀（默认 2） |
|  | `--additional-suffix=SUFFIX` | 将额外的 SUFFIX 附加到文件名上 |
| `-b` | `--bytes=SIZE` | 每个输出文件放置 SIZE 字节 |
| `-C` | `--line-bytes=SIZE` | 每个输出文件放置最多 SIZE 字节的记录 |
| `-d` |  | 使用从 0 开始的数字后缀，而不是字母 |
|  | `--numeric-suffixes[=FROM]` | 与-d 相同，但允许设置起始值 |
| `-x` |  | 使用从 0 开始的十六进制后缀，而不是字母 |
|  | `--hex-suffixes[=FROM]` | 与-x 相同，但允许设置起始值 |
| `-e` | `--elide-empty-files` | 不要在'-n'时生成空输出文件 |
|  | `--filter=COMMAND` | 将命令写入 shell；文件名为$FILE |
| `-l` | `--lines=NUMBER` | 每个输出文件放置 NUMBER 行/记录 |
| `-n` | `--number=CHUNKS` | 生成 CHUNKS 个输出文件；见下文解释 |
| `-t` | `--separator=SEP` | 使用 SEP 代替换行符作为记录分隔符；'\0'（零）指定空字符 |
| `-u` | `--unbuffered` | 在'-n r/...'时立即将输入复制到输出 |
|  | `--verbose` | 在每个输出文件打开前打印诊断信息 |
|  | `--help` | 显示此帮助信息并退出 |
|  | `--version` | 输出版本信息并退出 |

SIZE 参数是一个整数和可选的单位（例如：10K 是 10*1024）。单位是 K,M,G,T,P,E,Z,Y（1024 的幂）或 KB,MB,...（1000 的幂）。

CHUNKS 可以是：

| **CHUNKS** | **描述** |
| :-- | :-- |
| `N` | 根据输入大小分割成 N 个文件 |
| `K/N` | 输出 N 中的第 K 个到 stdout |
| `l/N` | 不分割行/记录分割成 N 个文件 |
| `l/K/N` | 输出 N 中的第 K 个到 stdout 而不分割行/记录 |
| `r/N` | 类似于'l'，但使用轮询分配 |
| `r/K/N` | 同样，但只输出 N 中的第 K 个到 stdout |
