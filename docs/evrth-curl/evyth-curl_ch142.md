# TFTP

简单文件传输协议（TFTP）是一种简单的明文协议，允许客户端从远程主机获取文件或将文件放置在远程主机上。

此协议的主要用例是通过本地网络获取引导映像。TFTP 与其他许多协议相比，还有一个特点，那就是它使用 UDP 而不是 TCP（大多数其他协议使用 TCP）。

没有 TFTP 的安全版本或变种。

## 下载

从选择的 TFTP 服务器下载文件：

```sh
curl -O tftp://localserver/file.boot
```

## 上传

将文件上传到选择的 TFTP 服务器：

```sh
curl -T file.boot tftp://localserver/
```

## TFTP 选项

TFTP 协议通过使用“块”将数据传输到通信的另一端。当设置 TFTP 传输时，双方要么同意使用默认的 512 字节块大小，要么协商使用不同的块大小。curl 支持 8 到 65464 字节的块大小。

您可以使用`--tftp-blksize`让 curl 使用与默认值不同的块大小。例如，请求 8192 字节的块大小如下：

```sh
curl --tftp-blksize 8192 tftp://localserver/file
```

已有证据表明，有些服务器实现根本不处理选项协商，因此 curl 也有完全关闭设置选项尝试的能力。如果您不幸需要与这样的服务器一起工作，可以使用如下标志：

```sh
curl --tftp-no-options tftp://localserver/file
```
