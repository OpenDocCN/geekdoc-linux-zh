# 对于 C++ 程序员

libcurl 提供了一个 C API。C 和 C++ 类似但并不相同。在使用 libcurl 进行 C++ 开发时，有一些事情需要记住。

## 字符串是 C 字符串，而不是 C++ 字符串对象

当你将字符串传递给接受 `char *` 的 libcurl API 时，这意味着你不能将这些函数传递 C++ 字符串或对象。

例如，如果你使用 C++ 构建一个字符串，然后希望该字符串用作 URL：

```sh
std::string url = "https://example.com/foo.asp?name=" + i;
curl_easy_setopt(curl, CURLOPT_URL, url.c_str());
```

## 回调注意事项

由于 libcurl 是一个 C 库，它对 C++ 成员函数或对象一无所知。你可以通过使用静态成员函数并传递指向类的指针来相对容易地克服这个限制。

这里有一个使用 C++ 方法作为回调的写回调示例：

```sh
// f is the pointer to your object.
static size_t YourClass::func(void *buffer, size_t sz, size_t n, void *f)
{
  // Call non-static member function.
  static_cast<YourClass*>(f)->nonStaticFunction();
}

// This is how you pass pointer to the static function:
curl_easy_setopt(hcurl, CURLOPT_WRITEFUNCTION, YourClass::func);
curl_easy_setopt(hcurl, CURLOPT_WRITEDATA, this);
```
