# Customize headers

In an HTTP request, after the initial request-line, there typically follows a number of request headers. That is a set of `name: value` pairs that ends with a blank line that separates the headers from the following request body (that sometimes is empty).

curl passes on a few default headers by default on its own account in requests, like for example `Host:`, `Accept:`, `User-Agent:` and a few others that may depend on what the user asks curl to do.

All headers set by curl itself can be replaced, by the user. You just then tell curl’s `-H` or `--header` the new header to use and it then replaces the internal one if the header field matches one of those headers, or it adds the specified header to the list of headers to send in the request.

To change the `Host:` header, do this:

[PRE0]

To add a `Elevator: floor-9` header, do this:

[PRE1]

If you just want to delete an internally generated header, just give it to curl without a value, just nothing on the right side of the colon.

To switch off the `User-Agent:` header, do this:

[PRE2]

Finally, if you then truly want to add a header with no contents on the right side of the colon (which is a rare thing), the magic marker for that is to instead end the header field name with a *semicolon*. Like this:

[PRE3]