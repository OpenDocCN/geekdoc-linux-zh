# 获取简单的 HTTP 页面

此示例仅从给定的 URL 获取 HTML 并将其发送到 stdout。可能是你可以编写的最简单的 libcurl 程序。

通过替换 URL，这也可以获取其他支持的协议的内容。

将输出发送到 stdout 是默认行为，通常不是你真正想要的。大多数应用程序会安装一个 写入回调 来接收到达的数据。

```sh
#include <stdio.h>
#include <curl/curl.h>

int main(void)
{
  CURL *curl;
  CURLcode res;

  curl = curl_easy_init();
  if(curl) {
    curl_easy_setopt(curl, CURLOPT_URL, "http://example.com/");

    /* Perform the request, 'res' holds the return code */
    res = curl_easy_perform(curl);
    /* Check for errors */
    if(res != CURLE_OK)
      fprintf(stderr, "curl_easy_perform() failed: %s\n",
              curl_easy_strerror(res));

    /* always cleanup */
    curl_easy_cleanup(curl);
  }
  return 0;
}
```
