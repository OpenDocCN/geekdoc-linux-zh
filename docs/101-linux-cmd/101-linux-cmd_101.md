# `chown` 命令

`chown` 命令使得更改文件或目录的所有权成为可能。在 Linux 中，用户和组是基础元素，使用 `chown` 命令可以更改文件或目录的所有者。也可以递归地更改文件夹的所有权

### 示例：

1.  更改文件的所有者

```sh
      chown user file.txt 

```

1.  更改文件所属组

```sh
      chown :group file.txt 

```

1.  在一行中更改用户和组

```sh
      chown user:group file.txt 

```

1.  递归更改文件夹的所有权

```sh
      chown -R user:group folder 

```

### 语法：

```sh
      chown [-OPTION] [DIRECTORY_PATH] 

```
