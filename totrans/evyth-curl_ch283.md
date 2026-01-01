# Get a simple HTTP page

This example just fetches the HTML from a given URL and sends it to stdout. Possibly the simplest libcurl program you can write.

By replacing the URL this is able to get contents over other supported protocols as well.

Getting the output sent to stdout is a default behavior and usually not what you actually want. Most applications instead install a [write callback](ch213.xhtml#transfers__callbacks__write__md) to have receive the data that arrives.

[PRE0]