# `mv` 命令

`mv` 命令允许您在 UNIX 类文件系统中将一个或多个文件或目录从一个位置移动到另一个位置。它可以用于两个不同的功能：

+   重命名文件或文件夹。

+   将一组文件移动到不同的目录。

***注意：**重命名时不会在磁盘上消耗额外的空间，且 mv 命令不会提供确认提示*

### 语法：

```sh
      mv [options] source (file or directory)  destination 

```

### 示例：

1.  要重命名名为 old_name.txt 的文件：

```sh
      mv old_name.txt new_name.txt 

```

1.  将名为 *essay.txt* 的文件从当前目录移动到名为 *assignments* 的目录，并将其重命名为 *essay1.txt*：

```sh
      mv essay.txt assignments/essay1.txt 

```

1.  将名为 *essay.txt* 的文件从当前目录移动到名为 *assignments* 的目录中，且不重命名

```sh
      mv essay.txt assignments 

```

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-f` | `--force` | 强制移动，通过覆盖目标文件而不提示 |
| `-i` | `--interactive` | 在覆盖前进行交互式提示 |
| `-u` | `--update` | 仅在源文件比目标文件新或目标文件缺失时移动 |
| `-n` | `--no-clobber` | 不覆盖现有文件 |
| `-v` | `--verbose` | 打印源文件和目标文件 |
| `-b` | `--backup` | 创建现有目标文件的备份 |
