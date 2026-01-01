# `cp` 命令

`cp` 是一个用于复制文件和目录的命令行实用程序。`cp` 代表复制。此命令用于复制文件或文件组或目录。它创建一个磁盘上文件的精确镜像，具有不同的文件名。cp 命令在其参数中至少需要两个文件名。

### 示例：

1.  将源文件的内容复制到目标文件。

```sh
      cp sourceFile destFile 

```

如果目标文件不存在，则创建该文件并将内容复制到其中。如果它已存在，则覆盖该文件。

1.  要将文件复制到另一个目录，指定目标目录的绝对路径或相对路径。

```sh
      cp sourceFile /folderName/destFile 

```

1.  要复制一个目录，包括其所有文件和子目录

```sh
      cp -R folderName1 folderName2 

```

上述命令创建目标目录，并递归地从源目录复制所有文件和子目录到目标目录。

如果目标目录已存在，则将源目录及其内容复制到目标目录内部。

1.  要复制文件和子目录，但不复制源目录

```sh
      cp -RT folderName1 folderName2 

```

### 语法：

cp 命令的一般语法如下：

```sh
      cp [OPTION] SOURCE DESTINATION
cp [OPTION] SOURCE DIRECTORY
cp [OPTION] SOURCE-1 SOURCE-2 SOURCE-3 SOURCE-n DIRECTORY 

```

第一种和第二种语法用于将源文件复制到目标文件或目录。第三种语法用于将多个源（文件）复制到目录。

#### 一些有用的选项

1.  `-i` (交互式) `i` 代表交互式复制。使用此选项时，系统在覆盖目标文件之前会先警告用户。cp 会提示用户进行响应，如果按 y 则覆盖文件，而按其他任何选项则不复制。

```sh
      $ cp -i file1.txt fileName2.txt
cp: overwrite 'file2.txt'? y 

```

1.  `-b`(备份) `-b`(备份)：使用此选项，cp 命令在相同文件夹中以不同的名称和格式创建目标文件的备份。

```sh
      $ ls
a.txt b.txt

$ cp -b a.txt b.txt

$ ls
a.txt b.txt b.txt~ 

```

1.  `-f`(强制) 如果系统无法打开目标文件进行写操作，因为用户没有对此文件的写权限，则使用 -f 选项与 cp 命令一起，首先删除目标文件，然后从源文件到目标文件复制内容。

```sh
      $ ls -l b.txt
-r-xr-xr-x+ 1 User User 3 Nov 24 08:45 b.txt 

```

用户、组和其他人没有写权限。

没有使用 `-f` 选项，命令未执行

```sh
      $ cp a.txt b.txt
cp: cannot create regular file 'b.txt': Permission denied 

```

使用 -f 选项，命令执行成功

```sh
      $ cp -f a.txt b.txt 

```

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-i` | --interactive | 在覆盖前提示 |
| `-f` | --force | 如果无法打开现有目标文件进行写操作，因为用户没有对此文件的写权限，则使用 -f 选项与 cp 命令一起，首先删除目标文件，然后从源文件到目标文件复制内容。 |
| `-b` | - | 在同一文件夹中以不同的名称和格式创建目标文件的备份。 |
| `-r or -R` | `--recursive` | **cp** 命令通过递归复制整个目录结构来显示其递归行为。 |
| `-n` | `--no-clobber` | 不覆盖现有文件（覆盖之前的 -i 选项） |
| `-p` | - | 尽可能保留指定的属性（默认：模式、所有权、时间戳），如果可能，还可以保留其他属性：上下文、链接、xattr、全部 |
