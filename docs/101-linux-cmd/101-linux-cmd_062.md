# `diff/sdiff` 命令

此命令用于通过逐行比较文件来显示文件中的差异。

### 语法：

```sh
      diff [options] File1 File2 

```

### 示例

1.  假设有两个文件，分别命名为 a.txt 和 b.txt，它们包含以下 5 个印度邦的信息-：

```sh
      $ cat a.txt
Gujarat
Uttar Pradesh
Kolkata
Bihar
Jammu and Kashmir

$ cat b.txt
Tamil Nadu
Gujarat
Andhra Pradesh
Bihar
Uttar pradesh 

```

输入 diff 命令后，我们将得到以下输出。

```sh
      $ diff a.txt b.txt
0a1
> Tamil Nadu
2,3c3
< Uttar Pradesh
 Andhra Pradesh
5c5
 Uttar pradesh 

```

### 标志及其功能

| **短标志** | **描述** |
| --- | --- |
| `-c` | 要以上下文模式查看差异，请使用 -c 选项。 |
| `-u` | 要以统一模式查看差异，请使用 -u 选项。它与上下文模式类似 |
| `-i` | 默认情况下，此命令区分大小写。要使此命令对 diff 不区分大小写，请使用 -i 选项。 |
| `-version` | 此选项用于显示当前在您的系统上运行的 diff 版本。 |
