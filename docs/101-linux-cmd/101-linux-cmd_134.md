# `cmp` 命令

`cmp` 命令是一个简单的实用工具，用于逐字节比较两个文件。

如果文件相同，`cmp` 不会产生输出并返回成功的退出状态。如果文件不同，它会报告第一个差异发生的字节和行号。

## 语法

```sh
      $ cmp [OPTION]... FILE1 [FILE2] 

```

## 示例：

在以下示例中，让我们假设我们有三个文件：

file1.txt:

```sh
      hello world 

```

file2.txt:

```sh
      hello world 

```

file3.txt:

```sh
      hello World 

```

### 1. 比较两个相同的文件

当文件相同，`cmp` 不会产生输出。这是确认两个文件相同的标准方式。

```sh
      $ cmp file1.txt file2.txt 

```

(无输出显示)

### 2. 比较两个不同的文件

当发现差异时，`cmp` 会报告第一个不同字节的地址。

```sh
      $ cmp file1.txt file3.txt
file1.txt file3.txt differ: byte 7, line 1 

```

### 3. 显示所有不同的字节 (--verbose 或 -l)

-l（小写 L）标志非常强大。它为文件中的每个差异打印字节编号（十进制）和不同字节的值（八进制）。

```sh
      $ cmp -l file1.txt file3.txt
 7 167 127 

```

#### 说明：

这个输出意味着在字节位置 7，file1.txt 有八进制值 167（字母 'w'），而 file3.txt 有八进制值 127（字母 'W'）。

### 4. 仅比较前 "n" 个字节 (--bytes 或 -n)

您可以将比较限制在特定数量的字节。在这里，我们只比较前 5 个字节。由于 "hello" 在两个文件中都是相同的，`cmp` 没有找到差异。

```sh
      $ cmp -n 5 file1.txt file3.txt 

```

(无输出显示)

### 5. 忽略初始 "n" 个字节 (--ignore-initial 或 -i)

您可以告诉 `cmp` 在开始比较之前跳过文件开头的特定数量的字节。在这里，我们跳过了前 6 个字节，因此比较从字母 'w' 开始。

```sh
      $ cmp -i 6 file1.txt file3.txt
file1.txt file3.txt differ: byte 1, line 1 

```

#### 说明：

输出现在显示差异在 "字节 1"，因为比较是在忽略最初的 6 个字节之后开始的。

### 常用选项

| 短标志 | 长标志 | 描述 |
| --- | --- | --- |
| -b | --print-bytes | 打印不同的字节。 |
| -i SKIP | --ignore-initial=SKIP | 跳过两个文件开头的 SKIP 个字节。 |
| -l | --verbose | 输出字节编号和所有不同字节的值。 |
| -n LIMIT | --bytes=LIMIT | 比较最多 LIMIT 字节。 |
| -s | --quiet, --silent | 抑制所有输出。仅返回退出状态。 |
