# Iterate over headers

[PRE0]

This function lets the application iterate over all available headers from within the given **origins** that arrived in the **request**.

The **request** argument tells libcurl from which request you want headers from.

If **previous** is set to NULL, this function returns a pointer to the first header. The application can then use that pointer as an argument to the next call to iterate over all available headers within the same **origin** and **request** context.

When this function returns NULL, there are no more headers within the context.

See [Header struct](ch278.xhtml#helpers__headerapi__struct__md) for details on the `curl_header` struct that function this returns a pointer to.