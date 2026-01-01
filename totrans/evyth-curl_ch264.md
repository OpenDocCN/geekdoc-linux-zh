# Get a URL

The `CURLU *` handle represents a single URL and you can easily extract that full URL or its individual parts with `curl_url_get`:

[PRE0]

If the handle does not have enough information to return the part that is being asked for, it returns error.

A returned string must be freed with `curl_free()` after you are done with it.