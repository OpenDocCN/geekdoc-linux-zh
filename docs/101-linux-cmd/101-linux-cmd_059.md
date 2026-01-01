# `sort`命令

`sort`命令用于排序文件，将记录按特定顺序排列。默认情况下，sort 命令假定文件内容为 ASCII 格式进行排序。使用 sort 命令的选项也可以用于数值排序。

### 示例：

假设你创建了一个名为 file.txt 的数据文件：

```sh
      Command : 
$ cat > file.txt
abhishek
chitransh
satish
rajan
naveen
divyam
harsh 

```

对文件进行排序：现在使用 sort 命令

语法：

```sh
      sort filename.txt 

```

```sh
      Command:
$ sort file.txt

Output :
abhishek
chitransh
divyam
harsh
naveen 
rajan
satish 

```

注意：此命令实际上不会更改输入文件，即 file.txt 文件。

### 对混合大小写内容的文件进行排序功能

即大写和小写：当我们有一个包含大小写字母的混合文件时，首先会对大写字母进行排序，然后是小写字母。

示例：

创建一个 mix.txt 文件

```sh
      Command :
$ cat > mix.txt
abc
apple
BALL
Abc
bat 

```

现在使用 sort 命令

```sh
      Command :
$ sort mix.txt
Output :
Abc                                                                                                                                                    
BALL                                                                                                                                                   
abc                                                                                                                                                    
apple                                                                                                                                                  
bat 

```
