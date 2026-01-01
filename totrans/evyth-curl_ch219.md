# sockopt

The sockopt callback is set with `CURLOPT_SOCKOPTFUNCTION`:

[PRE0]

The `sockopt_callback` function must match this prototype:

[PRE1]

This callback function gets called by libcurl when a new socket has been created but before the connect call, to allow applications to change specific socket options.

The **clientp** pointer points to the private data set with `CURLOPT_SOCKOPTDATA`:

[PRE2]

This callback should return:

*   CURL_SOCKOPT_OK on success
*   CURL_SOCKOPT_ERROR to signal an unrecoverable error to libcurl
*   CURL_SOCKOPT_ALREADY_CONNECTED to signal success but also that the socket is in fact already connected to the destination