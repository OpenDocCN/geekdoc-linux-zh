# 停止缓慢传输

默认情况下，传输可能会停滞或以极慢的速度传输数据，而这种情况并不算错误。

如果在**M**秒内传输速度低于**N**字节/秒，则停止传输。使用`CURLOPT_LOW_SPEED_LIMIT`设置**N**，使用`CURLOPT_LOW_SPEED_TIME`设置**M**。

在实际代码中使用这些选项可能看起来像这样：

```sh
#include <stdio.h>
#include <curl/curl.h>

int main(void)
{
  CURL *curl;
  CURLcode res = CURLE_OK;

  curl = curl_easy_init();
  if(curl) {
    /* abort if slower than 30 bytes/sec during 60 seconds */
    curl_easy_setopt(curl, CURLOPT_LOW_SPEED_TIME, 60L);
    curl_easy_setopt(curl, CURLOPT_LOW_SPEED_LIMIT, 30L);

    curl_easy_setopt(curl, CURLOPT_URL, "https://curl.se/");

    res = curl_easy_perform(curl);

    curl_easy_cleanup(curl);
  }

  return (int)res;
}
```
