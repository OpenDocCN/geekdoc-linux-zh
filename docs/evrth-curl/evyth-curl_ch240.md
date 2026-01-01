# 速率限制

允许应用程序设置速度上限。不要以每秒超过设定字节数的速度传输数据。libcurl 随后会尝试在多个秒内将平均速度保持在给定的阈值以下。

有单独的选项用于接收（`CURLOPT_MAX_RECV_SPEED_LARGE`）和发送（`CURLOPT_MAX_SEND_SPEED_LARGE`）。

下面是一个示例源代码，展示了其使用方法：

```sh
#include <stdio.h>
#include <curl/curl.h>

int main(void)
{
  CURL *curl;
  CURLcode res = CURLE_OK;

  curl = curl_easy_init();
  if(curl) {
    curl_off_t maxrecv = 31415;
    curl_off_t maxsend = 67954;

    curl_easy_setopt(curl, CURLOPT_MAX_RECV_SPEED_LARGE, maxrecv);
    curl_easy_setopt(curl, CURLOPT_MAX_SEND_SPEED_LARGE, maxsend);

    curl_easy_setopt(curl, CURLOPT_URL, "https://curl.se/");

    res = curl_easy_perform(curl);

    curl_easy_cleanup(curl);
  }

  return (int)res;
}
```
