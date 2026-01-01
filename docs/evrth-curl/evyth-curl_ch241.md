# 进度条

libcurl 可以在标准错误输出中显示进度条。这个功能默认是禁用的，并且是那些名称中带有尴尬否定词的选项之一：`CURLOPT_NOPROGRESS` - 将其设置为 `1L` 以 *禁用* 进度条。设置为 `0L` 以启用它。

返回错误以停止传输

在代码中，它可能看起来像这样：

```sh
#include <stdio.h>
#include <curl/curl.h>

int main(void)
{
  CURL *curl;
  CURLcode res = CURLE_OK;

  curl = curl_easy_init();
  if(curl) {
    /* enable progress meter */
    curl_easy_setopt(curl, CURLOPT_NOPROGRESS, 0L);

    curl_easy_setopt(curl, CURLOPT_URL, "https://curl.se/");

    res = curl_easy_perform(curl);

    curl_easy_cleanup(curl);
  }

  return (int)res;
}
```
