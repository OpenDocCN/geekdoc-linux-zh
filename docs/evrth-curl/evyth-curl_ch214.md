# 写入数据

写入回调通过`CURLOPT_WRITEFUNCTION`设置：

```sh
curl_easy_setopt(handle, CURLOPT_WRITEFUNCTION, write_callback);
```

`write_callback`函数必须匹配此原型：

```sh
size_t write_callback(char *ptr, size_t size, size_t nmemb,
                      void *userdata);
```

当 libcurl 接收到需要保存的数据时，会调用此回调函数。*ptr*指向传递的数据，该数据的大小是*size*乘以*nmemb*。

如果此回调未设置，libcurl 将默认使用`fwrite`。

写入回调在所有调用中尽可能传递尽可能多的数据，但它不能做出任何假设。它可能是一个字节，也可能是数千字节。传递给写入回调的最大正文数据量在 curl.h 头文件中定义：`CURL_MAX_WRITE_SIZE`（通常默认为 16KB）。如果为此传输启用了`CURLOPT_HEADER`，这将使头部数据传递给写入回调，你可以得到多达`CURL_MAX_HTTP_HEADER`字节的头部数据传递给它。这通常意味着 100KB。

如果传输的文件为空，此函数可能会以零字节数据调用。

传递给此函数的数据不会被空终止。例如，你不能使用 printf 的`%s`操作符来显示内容，也不能使用 strcpy 来复制它。

此回调应返回实际处理的字节数。如果该数字与传递给回调函数的数字不同，则向库发出错误条件信号。这会导致传输被中止，并且使用的 libcurl 函数返回`CURLE_WRITE_ERROR`。

在*userdata*参数中传递给回调的用户指针通过`CURLOPT_WRITEDATA`设置：

```sh
curl_easy_setopt(handle, CURLOPT_WRITEDATA, custom_pointer);
```

## 存储到内存

有一个普遍的需求是将检索到的响应存储在内存中，上述回调支持这一点。在这样做的时候，请务必小心，因为响应可能非常大。

你应以类似以下方式实现回调：

```sh
struct response {
  char *memory;
  size_t size;
};

static size_t
mem_cb(void *contents, size_t size, size_t nmemb, void *userp)
{
  size_t realsize = size * nmemb;
  struct response *mem = (struct response *)userp;

  char *ptr = realloc(mem->memory, mem->size + realsize + 1);
  if(!ptr) {
    /* out of memory! */
    printf("not enough memory (realloc returned NULL)\n");
    return 0;
  }

  mem->memory = ptr;
  memcpy(&(mem->memory[mem->size]), contents, realsize);
  mem->size += realsize;
  mem->memory[mem->size] = 0;

  return realsize;
}

int main()
{
  struct response chunk = {.memory = malloc(0),
                           .size = 0};

  /* send all data to this function  */
  curl_easy_setopt(curl_handle, CURLOPT_WRITEFUNCTION, mem_cb);

  /* we pass our 'chunk' to the callback function */
  curl_easy_setopt(curl_handle, CURLOPT_WRITEDATA, (void *)&chunk);

  free(chunk.memory);
}
```
