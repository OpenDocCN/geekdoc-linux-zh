# 如何在 Linux 上检查文件大小和目录大小

> 原文：[`techbyexample.com/find-the-file-size-and-directory-size-on-linux/`](https://techbyexample.com/find-the-file-size-and-directory-size-on-linux/)

## 1\. 使用‘du -sh’命令

**语法**

```go
du -sh file/directory name
```

**示例**

+   查找文件的大小。

```go
[root@no1010211066225 git-dir]# du -sh calculate_files.py 
4.0K	calculate_files.py
```

+   查找目录的大小。

```go
[root@mac git-dir]# du -sh someDocs
2.9M	someDocs
```

+   查找字典中所有文件的大小。

```go
[root@mac git-dir]# du -sh scripts/*
4.0K	scripts/bastionhost_eips.json
8.0K	scripts/create_bastion.rb
```

## 2\. 使用‘stat’命令

**语法**

```go
stat filename 
```

**示例**

```go
[root@nmac git-dir]# stat workflow.py
  File: ‘workflow.py’
  Size: 43000     	Blocks: 88         IO Block: 4096   regular file
Device: fd01h/64769d	Inode: 1058286     Links: 1
Access: (0755/-rwxr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
Context: unconfined_u:object_r:admin_home_t:s0
Access: 2020-09-23 08:33:54.299246431 +0000
Modify: 2020-09-03 05:42:45.113104679 +0000
Change: 2020-09-03 05:42:45.113104679 +0000
 Birth: -
```

**注意：** stat 命令不会显示目录的内容大小。其大小保持固定。这并不是目录内容的大小。

## 3\. 使用‘ls -l’命令

**语法**

```go
ls -l filename 
```

**示例**

```go
[root@mac git-dir]# ls -l workflow.py 
-rwxr-xr-x. 1 root root 43000 Sep  3 05:42 workflow.py
```

**注意：** 在上面命令的输出中，43000 是文件的字节大小。

```go
[root@mac git-dir] ls -lh workflow.py 
-rwxr-xr-x. 1 root root 42K Sep  3 05:42 workflow.py
```

**注意：** 在上面命令的输出中，42k 是文件的千字节大小。这是更友好的输出。

+   [文件](https://techbyexample.com/tag/file/)*   [大小](https://techbyexample.com/tag/size/)
