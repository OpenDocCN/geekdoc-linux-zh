# 如何在 bash/终端（Linux）中使用 vi 或 vim 删除文件内容  

> 原文：[`techbyexample.com/vim-delete-all-contents/`](https://techbyexample.com/vim-delete-all-contents/)  

## **概述**  

使用下面的快捷键，可以在 bash/终端中使用 vi 或 vim 删除文件的所有内容

```go
dG
```

## **示例**  

让我们看一个示例  

+   首先，让我们创建并写入一个临时文件  

```go
echo $'This is \ntest line' > temp.txt
```

+   让我们打印文件内容进行验证  

```go
~ $ cat temp.txt 
This is 
test line
```

+   现在运行 vi 或 vim 命令  

```go
~ $ vi temp.text
```

在 vi 编辑器中，你将看到如下内容  

```go
This is
test line
~                                                                                                                                                                                                                                             
~                                                                                                                                                                                                                                                                                                                                               
"temp.txt" 2L, 19C
```

现在按下‘d’，然后按‘G’（使用 shift 键将‘g’转换为大写）。请注意，这些键需要一个接一个地按，而不是一起按。按下键后，内容将被清除。在 vi 或 vim 编辑器中，你将看到如下内容  

```go
~                                                                                                                                                                                                                                             
~                                                                                                                                                                                                                                             
--No lines in buffer--
```

现在保存文件。使用快捷键  

```go
ESC key + :wg
```

现在再次检查文件的内容。它将为空  

```go
~ $ cat temp.txt
```
