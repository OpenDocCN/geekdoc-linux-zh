# libcurl HTTP

HTTP is by far the most commonly used protocol by libcurl users and libcurl offers countless ways of modifying such transfers. See the [HTTP protocol basics](ch024.xhtml#protocols__http__md) for some basics on how the HTTP protocol works.

## HTTPS

Doing HTTPS is typically done the same way as for HTTP as the extra security layer and server verification etc is done automatically and transparently by default. Just use the `https://` scheme in the URL.

HTTPS is HTTP with TLS on top. See also the [TLS transfer options](ch205.xhtml#transfers__options__tls__md) section.

## HTTP proxy

See [using Proxies with libcurl](ch235.xhtml#transfers__conn__proxies__md)

## Sections

*   [HTTP responses](ch245.xhtml#libcurl-http__responses__md)
*   [HTTP requests](ch246.xhtml#libcurl-http__requests__md)
*   [HTTP versions](ch247.xhtml#libcurl-http__versions__md)
*   [HTTP ranges](ch248.xhtml#libcurl-http__ranges__md)
*   [HTTP authentication](ch249.xhtml#libcurl-http__auth__md)
*   [Cookies with libcurl](ch250.xhtml#libcurl-http__cookies__md)
*   [Download](ch251.xhtml#libcurl-http__download__md)
*   [Upload](ch252.xhtml#libcurl-http__upload__md)
*   [Multiplexing](ch253.xhtml#libcurl-http__multiplexing__md)
*   [HSTS](ch254.xhtml#libcurl-http__hsts__md)
*   [alt-svc](ch255.xhtml#libcurl-http__alt-svc__md)