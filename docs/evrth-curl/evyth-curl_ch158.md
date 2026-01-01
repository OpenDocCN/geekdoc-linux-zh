# JSON

curl 7.82.0 版本引入了`--json`选项，作为将 JSON 格式数据通过 POST 方式发送到 HTTP 服务器的新方法。此选项作为一个快捷方式，提供了一个单一选项来替代以下三个选项：

```sh
--data [arg]
--header "Content-Type: application/json"
--header "Accept: application/json"
```

此选项并不使 curl 真正理解或知道它发送的 JSON 数据，但它使发送它变得更容易。curl 不会触摸或解析它发送的数据，因此你需要自己确保它是有效的 JSON。

向服务器发送一个基本的 JSON 对象：

```sh
curl --json '{"tool": "curl"}' https://example.com/
```

从本地文件发送 JSON：

```sh
curl --json @json.txt https://example.com/
```

从 curl 的 stdin 发送 JSON：

```sh
echo '{"a":"b"}' | curl --json @- https://example.com/
```

你可以在同一个命令行中使用多个`--json`选项。这使得 curl 将选项的内容连接起来，一次性将所有数据发送到服务器。请注意，这种连接是基于纯文本的，并且它不会按照 JSON 格式合并 JSON 对象。

从文件发送 JSON 并将字符串连接到末尾：

```sh
curl --json @json.txt --json ', "end": "true"}' https://example.com/
```

## 构建要发送的 JSON

JSON 数据中使用的引号有时会使在 shell 和脚本中编写和使用它变得有些困难和繁琐。

使用一个专门的工具来完成这个目的可能会使事情对你来说更容易，特别是可能帮助你完成这个任务的工具是[jo](https://github.com/jpmens/jo)。

使用 jo 和`--json`选项向服务器发送一个基本的 JSON 对象。

```sh
jo -p name=jo n=17 parser=false | curl --json @- https://example.com/
```

## 接收 JSON

curl 本身并不知道或理解它发送或接收的内容，包括当服务器在响应中返回 JSON 时。

使用一个专门的工具来解析或格式化 JSON 响应可能会使事情对你来说更容易，特别是可能帮助你完成这个任务的工具是[jq](https://stedolan.github.io/jq/)。

向服务器发送一个基本的 JSON 对象，并格式化输出 JSON 响应：

```sh
curl --json '{"tool": "curl"}' https://example.com/ | jq
```

使用`jo`发送 JSON，使用`jq`打印响应：

```sh
jo -p name=jo n=17 | curl --json @- https://example.com/ | jq
```

jq 是一个功能强大且功能丰富的工具，用于提取、过滤和管理 JSON 内容，其功能远不止于格式化输出。
