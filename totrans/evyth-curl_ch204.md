# Set numerical options

Since `curl_easy_setopt()` is a vararg function where the 3rd argument can use different types depending on the situation, normal C language type conversion cannot be done. You **must** make sure that you truly pass a `long` and not an `int` if the documentation tells you so. On architectures where they are the same size, you may not get any problems but not all work like that. Similarly, for options that accept a `curl_off_t` type, it is **crucial** that you pass in an argument using that type and no other.

Enforce a `long`:

[PRE0]

Enforce a `curl_off_t`:

[PRE1]