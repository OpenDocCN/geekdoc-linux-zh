# 如何使用 SSH 命令在远程服务器上运行命令？

> 原文：[`techbyexample.com/run-remote-ssh-command/`](https://techbyexample.com/run-remote-ssh-command/)

## **概述**

使用带有 `-t` 参数的 `ssh` 命令，我们可以在远程服务器上运行命令。

**语法：**

```go
ssh user@server – t “Command to be executed at server”
```

**示例：**

```go
#Running chef-solo on remote server

CHEF_TARGET = "ssh -v ec2-user@ec2-52-24-147-93.us-west-2.compute.amazonaws.com"
CHEF_ROLE="my-app"
ssh ec2-user@ec2-55-124-347-93.us-west-2.compute.amazonaws.com -t "sudo chef-solo --node-name $CHEF_TARGET -o role[$CHEF_ROLE]"
```

```go
#Performing list command on remote server
ssh  ec2-user@10.24.256.11 - t "ls -la"
```

```go
#Creating a directory at remote server
ssh  -i pemfile.pem ec2-user@10.24.256.11 - t "sudo mkdir -p /root/home/test"
```

**注意：**用于服务器身份验证时，你可以使用以下两种方式。

+   基于公私钥的身份验证

+   基于密码的身份验证
