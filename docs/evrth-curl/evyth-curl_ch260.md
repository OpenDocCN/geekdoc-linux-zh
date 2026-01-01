# 包含文件

当你想要使用 URL API 时，在你的代码中包含 `<curl/curl.h>`。

```sh
#include <curl/curl.h>

CURLU *h = curl_url();
rc = curl_url_set(h, CURLUPART_URL, "ftp://example.com/no/where", 0);
```
