# Trace options

There are times when `-v` is not enough. In particular, when you want to store the complete stream including the actual transferred data.

For situations when curl does encrypted file transfers with protocols such as HTTPS, FTPS or SFTP, other network monitoring tools (like Wireshark or tcpdump) are not able to do this job as easily for you.

For this, curl offers two other options that you use instead of `-v`.

`--trace [filename]` saves a full trace in the given filename. You can also use ‘-’ (a single minus) instead of a filename to get it passed to stdout. You would use it like this:

[PRE0]

When completed, there is a ‘dump’ file that can turn out pretty sizable. In this case, the 15 first lines of the dump file looks like:

[PRE1]

Every single sent and received byte gets displayed individually in hexadecimal numbers. Received headers are output line by line.

If you think the hexadecimals are not helping, you can try `--trace-ascii [filename]` instead, also this accepting ‘-’ for stdout and that makes the 15 first lines of tracing look like:

[PRE2]

## Time stamps

The `--trace-time` option prefixes all verbose/trace outputs with a high resolution timer for when the line is printed. It works with the regular `-v / --verbose` option as well as with `--trace` and `--trace-ascii`.

An example could look like this:

[PRE3]

The lines are all the local time as hours:minutes:seconds and then number of microseconds in that second.

## Identify transfers and connections

As the trace information flow showing on screen or to a file using these options is a continuous stream even though your command line might make curl use a large number of separate connections and different transfers, there are times when you want to see to which specific transfers or connections the various information below to. To better understand the trace output.

You can then add `--trace-ids` to the line and you see how curl adds two numbers to all tracing: the connection number and the transfer number. They are two separate identifiers because connections can be reused and multiple transfers can use the same connection.

## More data

If the amount of tracing data is not enough. Like when you suspect and want to debug a problem in a more fundamental lower protocol level, curl provides the `--trace-config` option for you.

With this option you tell curl to also include logging about components that it otherwise does not include by default, such as details about TLS, HTTP/2 or HTTP/3 protocol bits. It also has convenience options for adding the connection and transfer identifiers and time stamps.

The `--trace-config` option accepts an argument where you specify a comma-separated list with the areas you want it to trace. For example, include identifiers and show me HTTP/2 details:

[PRE4]

The exact set of options varies, but here are some ones to try:

| area | description |
| --- | --- |
| `ids` | the same identifiers as `--trace-ids` provides |
| `time` | the same time output as `--trace-time` provides |
| `all` | show everything possible |
| `tls` | TLS protocol exchange details |
| `http/2` | HTTP/2 frame information |
| `http/3` | HTTP/3 frame information |
| `*` | additional ones in future versions |

Doing a quick run with `all` is often a good way to get to see which specific areas that are shown, as then you can do follow-up runs with more specific areas set.