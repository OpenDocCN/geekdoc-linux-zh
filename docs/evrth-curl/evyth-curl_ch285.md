# 通过 HTTP 提交登录表单

通过 HTTP 提交登录通常是一个确定在 POST 中提交哪些数据以及将其发送到哪个目标 URL 的问题。

登录成功后，如果使用了正确的 cookies，可以获取目标 URL。由于许多登录系统使用 HTTP 重定向，我们要求 libcurl 在收到重定向时跟随。

一些登录表单使得过程更加复杂，并要求您从显示登录表单的页面获取 cookies 等，因此如果您需要这些，可能需要稍微扩展一下此代码。

通过传递一个不存在的 cookie 文件，此示例启用了 cookie 解析器，以便在登录响应的响应到达时存储传入的 cookies，然后后续的资源请求使用这些 cookies 并向服务器证明我们确实已经正确登录。

```sh
#include <stdio.h>
#include <string.h>
#include <curl/curl.h>

int main(void)
{
  CURL *curl;
  CURLcode res;

  static const char *postthis = "user=daniel&password=monkey123";

  curl = curl_easy_init();
  if(curl) {
    curl_easy_setopt(curl, CURLOPT_URL, "https://example.com/login.cgi");
    curl_easy_setopt(curl, CURLOPT_POSTFIELDS, postthis);
    curl_easy_setopt(curl, CURLOPT_FOLLOWLOCATION, 1L); /* redirects */
    curl_easy_setopt(curl, CURLOPT_COOKIEFILE, ""); /* no file */
    res = curl_easy_perform(curl);
    /* Check for errors */
    if(res != CURLE_OK)
      fprintf(stderr, "curl_easy_perform() failed: %s\n",
              curl_easy_strerror(res));
    else {
      /*
       * After the login POST, we have received the new cookies. Switch
       * over to a GET and ask for the login-protected URL.
       */
      curl_easy_setopt(curl, CURLOPT_URL, "https://example.com/file");
      curl_easy_setopt(curl, CURLOPT_HTTPGET, 1L); /* no more POST */
      res = curl_easy_perform(curl);
      /* Check for errors */
      if(res != CURLE_OK)
        fprintf(stderr, "second curl_easy_perform() failed: %s\n",
                curl_easy_strerror(res));
    }
    /* always cleanup */
    curl_easy_cleanup(curl);
  }
  return 0;
}
```
