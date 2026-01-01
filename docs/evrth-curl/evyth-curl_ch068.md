# trurl

*trurl* 是一个独立的命令行工具，其唯一目的是解析、操作和输出 URL 及其部分。它是 curl 的配套工具，满足您的命令行和脚本需求。

trurl 使用 libcurl 的 URL 解析器。这确保了 curl 和 trurl 对 URL 的看法始终一致，并且这两个工具以相同和一致的方式解析它们。

## 用法

通常，您会将一个或多个 URL 传递给 trurl，并指定您希望输出的组件。可能还会在修改 URL（s）的同时进行。

trurl 了解 URL，每个 URL 由最多十个独立且独立的 *组件* 组成。这些组件可以使用 trurl 提取、删除和更新。

## trurl 示例命令行

**替换 URL 的主机名：**

```sh
$ trurl --url https://curl.se --set host=example.com
https://example.com/
```

**通过设置组件创建 URL：**

```sh
$ trurl --set host=example.com --set scheme=ftp
ftp://example.com/
```

**重定向 URL：**

```sh
$ trurl --url https://curl.se/we/are.html --redirect here.html
https://curl.se/we/here.html
```

**更改端口号：**

```sh
$ trurl --url https://curl.se/we/../are.html --set port=8080
https://curl.se:8080/are.html
```

**从 URL 中提取路径：**

```sh
$ trurl --url https://curl.se/we/are.html --get '{path}'
/we/are.html
```

**从 URL 中提取端口号：**

```sh
$ trurl --url https://curl.se/we/are.html --get '{port}'
443
```

**将路径段附加到 URL 上：**

```sh
$ trurl --url https://curl.se/hello --append path=you
https://curl.se/hello/you
```

**将查询段附加到 URL 上：**

```sh
$ trurl --url "https://curl.se?name=hello" --append query=search=string
https://curl.se/?name=hello&search=string
```

**从 stdin 读取 URL：**

```sh
$ cat urllist.txt | trurl --url-file -
...
```

**输出 JSON：**

```sh
$ trurl "https://fake.host/hello#frag" --set user=::moo:: --json
[
  {
    "url": "https://%3a%3amoo%3a%3a@fake.host/hello#frag",
    "parts": {
      "scheme": "https",
      "user": "::moo::",
      "host": "fake.host",
      "path": "/hello",
      "fragment": "frag"
    }
  }
]
```

**从查询中删除跟踪元组：**

```sh
$ trurl "https://curl.se?search=hey&utm_source=tracker" \
  --trim query="utm_*"
https://curl.se/?search=hey
```

**显示特定的查询键值：**

```sh
$ trurl "https://example.com?a=home&here=now&thisthen" -g '{query:a}'
home
```

**对查询组件中的键/值对进行排序：**

```sh
$ trurl "https://example.com?b=a&c=b&a=c" --sort-query
https://example.com?a=c&b=a&c=b
```

**处理使用分号分隔的查询：**

```sh
$ trurl "https://curl.se?search=fool;page=5" --trim query="search" \
  --query-separator ";"
https://curl.se?page=5
```

**在 URL 路径中接受空格：**

```sh
$ trurl "https://curl.se/this has space/index.html" --accept-space
https://curl.se/this%20has%20space/index.html
```

## 更多

您想了解的所有关于 trurl 的信息都可以在 [`curl.se/trurl`](https://curl.se/trurl) 找到。它可能已经适用于您选择的 Linux 发行版。
