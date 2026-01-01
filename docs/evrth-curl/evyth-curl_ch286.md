# 获取 FTP 目录列表

此示例仅从给定的 URL 获取 FTP 目录输出并将其发送到 stdout。URL 中的尾部斜杠使得 libcurl 将其视为目录。

```sh
#include <curl/curl.h>

int main(void)
{
  CURL *curl;
  CURLcode res;

  curl_global_init(CURL_GLOBAL_DEFAULT);

  curl = curl_easy_init();
  if(curl) {
    /*
     * Make the URL end with a trailing slash
     */
    curl_easy_setopt(curl, CURLOPT_URL, "ftp://ftp.example.com/");

    res = curl_easy_perform(curl);

    /* always cleanup */
    curl_easy_cleanup(curl);

    if(CURLE_OK != res) {
      /* we failed */
      fprintf(stderr, "curl told us %d\n", res);
    }
  }

  curl_global_cleanup();

  return 0;
}
```
