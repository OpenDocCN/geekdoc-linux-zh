# Rate limit

Lets an application set a speed cap. Do not transfer data faster than a set number of bytes per second. libcurl then attempts to keep the average speed below the given threshold over a period of multiple seconds.

There are separate options for receiving (`CURLOPT_MAX_RECV_SPEED_LARGE`) and sending (`CURLOPT_MAX_SEND_SPEED_LARGE`).

Here is an example source code showing it in use:

[PRE0]