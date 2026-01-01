# `nslookup` 命令

`nslookup` 命令是一个网络管理命令行工具，用于查询域名系统（DNS），以获取域名或 IP 地址映射或任何其他特定 DNS 记录。

## 语法

```sh
      nslookup [options] [host] 

```

## 选项

一些流行的选项标志包括：

```sh
      -domain=[domain-name]	Change the default DNS name.
-debug	Show debugging information.
-port=[port-number]	Specify the port for queries. The default port number is 53.
-timeout=[seconds]	Specify the time allowed for the server to respond.
-type=a	View information about the DNS A address records.
-type=any	View all available records.
-type=hinfo	View hardware-related information about the host.
-type=mx	View Mail Exchange server information.
-type=ns	View Name Server records.
-type=ptr	View Pointer records. Used in reverse DNS lookups.
-type=soa	View Start of Authority records. 

```

## 几个示例：

1.  查询 DNS 服务器

```sh
      nslookup www.google.com 

```

1.  指定要查询的端口

```sh
      nslookup -port=53 www.google.com 

```

1.  获取 MX 记录

```sh
      nslookup -type=mx google.com 

```

在这里，我向您展示了如何在 Linux 中使用 nslookup 命令。尽管有其他 DNS 查询工具，例如 dig，但 nslookup 可能是一个更好的选择，因为它几乎存在于每个系统中，是一个强大的工具。

更多详情：[Nslookup 维基百科](https://en.wikipedia.org/wiki/Nslookup)
