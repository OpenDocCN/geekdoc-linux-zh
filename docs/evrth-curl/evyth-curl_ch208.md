# 获取选项信息

libcurl 提供了一个 API，实际上是一组函数，允许应用程序获取有关所有当前支持的*简单选项*的信息。它不返回选项的*值*，而是提供有关名称、ID 和类型的信息。

## 遍历所有选项

现代 libcurl 支持超过 300 个不同的选项。通过使用`curl_easy_option_by_next()`，应用程序可以遍历所有已知选项，并返回一个指向`struct curl_easyoption`的指针。

此函数仅返回此特定 libcurl 构建所知的选项信息。其他选项可能存在于较新的 libcurl 构建中，或者在不同的构建时启用/禁用选项的构建中。

示例，遍历所有可用选项：

```sh
const struct curl_easyoption *opt;
opt = curl_easy_option_by_next(NULL);
while(opt) {
  printf("Name: %s\n", opt->name);
  opt = curl_easy_option_by_next(opt);
}
```

## 通过名称查找特定选项

给定一个特定的简单选项名称，你可以要求 libcurl 返回一个指向`struct curl_easyoption`的指针。名称应提供，不带`CURLOPT_`前缀。

例如，一个应用程序可以这样询问 libcurl 关于`CURLOPT_VERBOSE`选项：

```sh
const struct curl_easyoption *opt = curl_easy_option_by_name("VERBOSE");
if(opt) {
  printf("This option wants CURLoption %x\n", (int)opt->id);
}
```

## 通过 ID 查找特定选项

给定一个特定的简单选项 ID，你可以要求 libcurl 返回一个指向`struct curl_easyoption`的指针。该“ID”是公共`curl/curl.h`头文件中提供的`CURLOPT_`前缀符号。

一个应用程序可以这样询问 libcurl`CURLOPT_VERBOSE`选项的名称：

```sh
const struct curl_easyoption *opt =
  curl_easy_option_by_id(CURLOPT_VERBOSE);
if(opt) {
  printf("This option has the name: %s\n", opt->name);
}
```

## `curl_easyoption`结构体

```sh
struct curl_easyoption {
  const char *name;
  CURLoption id;
  curl_easytype type;
  unsigned int flags;
};
```

在‘flags’中只有一个具有定义意义的位：如果`CURLOT_FLAG_ALIAS`被设置，则表示该选项是一个“别名”。这是一个为了向后兼容而提供的名称，如今更常用另一个名称的选项。如果你查找别名的 ID，你会得到该选项的新规范名称。
