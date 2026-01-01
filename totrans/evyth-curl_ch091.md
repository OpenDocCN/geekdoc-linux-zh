# Multiple downloads

As curl can be told to download many URLs in a single command line, there are, of course, times when you want to store these downloads in nicely named local files.

The key to understanding this is that each download URL needs its own “storage instruction”. Without said “storage instruction”, curl defaults to sending the data to stdout. If you ask for two URLs and only tell curl where to save the first URL, the second one is sent to stdout. Like this:

[PRE0]

The “storage instructions” are read and handled in the same order as the download URLs so they do not have to be next to the URL in any way. You can round up all the output options first, last or interleaved with the URLs. You choose.

These examples all work the same way:

[PRE1]

The `-O` is similarly just an instruction for a single download so if you download multiple URLs, use more of them:

[PRE2]

## Parallel

Unless told otherwise, curl downloads all given URLs in a serial fashion, one by one. By using `-Z` (or `--parallel`) curl can instead do the transfers [in parallel](ch066.xhtml#cmdline__urls__parallel__md): several ones at once.