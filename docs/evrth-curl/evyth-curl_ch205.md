# 设置字符串选项

目前`curl_easy_setopt()`函数有超过 80 个选项接受字符串作为其第三个参数。

当在句柄中设置字符串时，libcurl 会立即复制该数据，这样应用程序就不必在传输过程中保留数据 - **有一个显著的例外**：`CURLOPT_POSTFIELDS`。

在句柄中设置 URL：

```sh
curl_easy_setopt(handle, CURLOPT_URL, "https://example.com");
```

## `CURLOPT_POSTFIELDS`

不同于 libcurl 总是复制数据的规则，`CURLOPT_POSTFIELDS`只存储数据的指针，这意味着使用此选项的应用程序**必须**在整个传输过程中保留内存。

如果这成问题，一个替代方案是使用`CURLOPT_COPYPOSTFIELDS`来复制数据。如果数据是二进制且不以第一个 null 字节结束，确保在使用此选项之前设置`CURLOPT_POSTFIELDSIZE`。

### 为什么？

`CURLOPT_POSTFIELDS`是一个例外的原因是历史遗留问题。最初（在 curl 7.17.0 之前），libcurl 不复制任何字符串参数，当引入当前行为时，此选项无法在不破坏行为的情况下转换，因此它必须像以前一样继续工作。现在这显得有些突出，因为其他选项都没有这样做...

## C++

如果你从 C++程序中使用 libcurl，重要的是要记住你不能传递 libcurl 期望的字符串对象。它必须是一个以 null 结尾的 C 字符串。通常你可以使用`c_str()`方法来实现这一点。

例如，将 URL 保存在字符串对象中，并在句柄中设置该对象：

```sh
std::string url("https://example.com/");
curl_easy_setopt(curl, CURLOPT_URL, url.c_str());
```
