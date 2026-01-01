# `nano`命令

`nano`命令让您可以创建/编辑文本文件。

### 安装：

Nano 文本编辑器预安装在 macOS 和大多数 Linux 发行版上。它是`vi`和`vim`的替代品。要检查您的系统是否已安装，请输入：

```sh
      nano --version 

```

如果您没有安装`nano`，您可以通过包管理器来完成：

Ubuntu 或 Debian：

```sh
      sudo apt install nano 

```

### 示例：

1.  打开一个现有文件，输入`nano`后跟文件路径：

```sh
      nano /path/to/filename 

```

1.  创建一个新文件，输入`nano`后跟文件名：

```sh
      nano filename 

```

1.  使用光标在特定行和字符处打开文件，请使用以下语法：

```sh
      nano +line_number,character_number filename 

```

### 一些快捷键及其功能概述：

| **快捷键** | **描述** |
| :-- | :-- |
| `Ctrl + S` | 保存当前文件 |
| `Ctrl + O` | 提供写入文件（“另存为”）的选项 |
| `Ctrl + X` | 关闭缓冲区，退出 nano |
| `Ctrl + K` | 将当前行剪切到剪切板 |
| `Ctrl + U` | 粘贴剪切板内容 |
| `Alt + 6` | 将当前行复制到剪切板 |
| `Alt + U` | 撤销上一个操作 |
| `Alt + E` | 重做上一个撤销的操作 |
