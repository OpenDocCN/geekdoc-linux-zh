# DICT

DICT 是一个用于字典查找的协议。

## 使用方法

为了娱乐，尝试

```sh
curl dict://dict.org/m:curl
curl dict://dict.org/d:heisenbug:jargon
curl dict://dict.org/d:daniel:gcide
```

‘m’ 的别名是 ‘match’ 和 ‘find’，而 ‘d’ 的别名是 ‘define’ 和 ‘lookup’。例如，

```sh
curl dict://dict.org/find:curl
```

破坏 RFC URL 描述的命令（但不是 DICT 协议）是

```sh
curl dict://dict.org/show:db
curl dict://dict.org/show:strat
```
