# Download

The GET method is the default method libcurl uses when an HTTP URL is requested and no particular other method is asked for. It asks the server for a particular resource—the standard HTTP download request:

[PRE0]

Since options set in an easy handle are sticky and remain until changed, there may be times when you have asked for another request method than GET and then want to switch back to GET again for a subsequent request. For this purpose, there is the `CURLOPT_HTTPGET` option:

[PRE1]

## Download headers too

An HTTP transfer also includes a set of response headers. Response headers are metadata associated with the actual payload, called the response body. All downloads get a set of headers too, but when using libcurl you can select whether you want to have them downloaded (seen) or not.

You can ask libcurl to pass on the headers to the same stream as the regular body is, by using `CURLOPT_HEADER`:

[PRE2]

Or you can opt to store the headers in a separate download file, by relying on the default behaviors of the [write](ch213.xhtml#transfers__callbacks__write__md) and [header callbacks](ch216.xhtml#transfers__callbacks__header__md):

[PRE3]

If you only want to casually browse the headers, you may even be happy enough with just setting verbose mode while developing as that shows both outgoing and incoming headers sent to stderr:

[PRE4]