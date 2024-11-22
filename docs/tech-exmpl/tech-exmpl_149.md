# 如何在使用 vi 或 vim（Linux）编辑器时通过行号跳转到特定行

> 原文：[`techbyexample.com/goto-line-number-vi-vim/`](https://techbyexample.com/goto-line-number-vi-vim/)

## **概述**

通常，在使用 **vi** 或 **vim** 打开 Linux 文件时，文件是没有行号显示的。要跳转到某一行，首先需要显示行号。可以使用以下命令在 **vi** 或 **vim** 编辑器中显示行号

```go
:set nu + Enter key
```

一旦在编辑器中显示了行号，就可以在编辑器中输入以下命令来跳转到特定的行号

```go
:n + Enter key
```

其中 **n** 是你想要跳转到的行号。让我们来看一个例子

## **示例**

以下是一个示例，展示如何首先显示行号，然后跳转到特定行：

+   创建一个包含一些内容的临时文件：

```go`*   Check the content of temp file using **cat** command:    ``` ~ $ cat temp.txt this is line 1 this is line 2 this is line 3 this is line 4 ```go    *   Now open the temp file with **vi** or **vim** and then type **“:set nu”** and then press **Enter** key. This will show line numbers in the opened editor like below:    ``` 1 this is line 1 2 this is line 2 3 this is line 3 4 this is line 4 ~ ~ ~ ~ :set nu ```go    *   Now go to a particular line number by using **“:n”** followed by **Enter** key where **n** is the line number you want to go to.  Here I have used **“:3”** to go to line number **3**.  After typing **“:3”** do not forget to press **Enter** key. You will see in your editor that the cursor is at the 3rd line:    ``` 1 this is line 1 2 this is line 2 3 this is line 3 4 this is line 4 ~ ~ ~ ~ :3  ```go    Users can use the above method to visit/update/delete the desired line. Once the operation is completed, the file can be closed with **“:q!”** (to quit the file without saving changes) or **“:wq”** (to quit the file with saved changes).````
