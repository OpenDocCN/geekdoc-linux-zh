# Progress meter

libcurl can be made to output a progress meter on stderr. This feature is disabled by default and is one of those options with an ones awkward negation in the name: `CURLOPT_NOPROGRESS` - set it to `1L` to *disable* progress meter. Set it to `0L` to enable it.

Return error to stop transfer

It can look something like this in code:

[PRE0]