# 如何在 python 中读取文件？

> 原文：[`techbyexample.com/how-to-read-a-file-in-python/`](https://techbyexample.com/how-to-read-a-file-in-python/)

## **使用 open 方法：**

我们使用`open`方法打开文件，并使用`read`方法读取文件内容。

**注意：** 使用`with`语句时，文件的关闭操作会自动处理，因此不需要显式地关闭文件。

```go
def readAFile(path):
       with open(path, 'r') as f:         #f is file descriptor
       return f.read()

readAFile(path)
```

+   [文件](https://techbyexample.com/tag/file/)*   [python](https://techbyexample.com/tag/python/)*   [读取](https://techbyexample.com/tag/read/)
