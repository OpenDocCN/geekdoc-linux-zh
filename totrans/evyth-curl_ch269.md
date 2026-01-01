# CURLOPT_CURLU

As a convenience to applications, they can pass in an already parsed URL to libcurl to work with, as an alternative to `CURLOPT_URL`.

You pass in a `CURLU` handle instead of a URL string with the `CURLOPT_CURLU` option.

Example:

[PRE0]