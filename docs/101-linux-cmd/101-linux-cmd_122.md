# `zcat` 命令

`zcat` 允许您查看一个压缩文件。

### 示例：

1.  要查看压缩文件的内容：

```sh
      ~$ zcat test.txt.gz
Hello World 

```

1.  它也可以与多个文件一起工作：

```sh
      ~$ zcat test2.txt.gz test.txt.gz
hello
Hello world 

```

### 语法：

`zcat` 命令的一般语法如下：

```sh
      zcat [  -n ] [  -V ] [  File ... ] 

```
