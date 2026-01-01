# Get a response into memory

This example is a variation of the former that instead of sending the received data to stdout (which often is not what you want), this example instead stores the incoming data in a memory buffer that is enlarged as the incoming data grows.

It accomplishes this by using a [write callback](ch213.xhtml#transfers__callbacks__write__md) to receive the data.

This example uses a fixed URL string with a set URL scheme, but you can of course change this to use any other supported protocol and then get a resource from that instead.

[PRE0]