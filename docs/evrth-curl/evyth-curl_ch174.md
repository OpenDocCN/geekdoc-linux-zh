# 自定义头部

在 HTTP 请求中，在初始请求行之后，通常跟随一系列请求头部。这是一组以空白行结束的`name: value`对，该空白行将头部与随后的请求体（有时为空）分开。

`curl`默认在其请求中传递一些默认头部，出于其自身的账户，例如`Host:`, `Accept:`, `User-Agent:`以及一些可能取决于用户要求`curl`执行的操作的其他头部。

用户可以替换`curl`自己设置的所有头部。你只需告诉`curl`的`-H`或`--header`使用新的头部，如果头部字段与这些头部之一匹配，它就会替换内部的头部，或者将指定的头部添加到请求中要发送的头部列表中。

要更改`Host:`头部，请这样做：

```sh
curl -H "Host: test.example" http://example.com/
```

要添加`Elevator: floor-9`头部，请这样做：

```sh
curl -H "Elevator: floor-9" http://example.com/
```

如果你只想删除一个内部生成的头部，只需将其提供给`curl`而不带值，即在冒号右边什么也不写。

要关闭`User-Agent:`头部，请这样做：

```sh
curl -H "User-Agent:" http://example.com/
```

最后，如果你确实想要添加一个冒号右边没有内容的头部（这很少见），那个神奇的标记是让头部字段名以一个*分号*结尾。就像这样：

```sh
curl -H "Empty;" http://example.com
```
