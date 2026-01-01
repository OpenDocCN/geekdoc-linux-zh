# `rmdir` 命令

**rmdir** 命令用于从 Linux 文件系统中删除空目录。只有当指定的目录为空时，rmdir 命令才会删除命令行中指定的每个目录。

### 使用方法和示例：

1.  删除目录及其父目录

```sh
      rmdir -p a/b/c			// is similar to 'rmdir a/b/c a/b a' 

```

1.  删除多个目录

```sh
      rmdir a b c				// removes empty directories a,b and c 

```

### 语法：

```sh
      rmdir [OPTION]... DIRECTORY... 

```

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-` | `--ignore-fail-on-non-empty` | 忽略仅因为目录非空而导致的每个失败 |
| `-p` | `--parents` | 删除目录及其父目录 |
| `-d` | `--delimiter=DELIM` | 使用 DELIM 代替 TAB 作为字段分隔符 |
| `-v` | `--verbose` | 对每个处理的目录输出诊断信息 |
