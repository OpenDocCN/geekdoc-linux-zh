# 概念

A libcurl application can do WebSocket using one of these two different approaches below.

## 1. 回调方法

It can decide to use the regular write callback to receive incoming data, and respond to that data in or outside of the callback with `curl_ws_send`. Thereby treating the entire session as a form of download from the server.

Within the write callback, an application can call `curl_ws_meta()` to retrieve information about the incoming WebSocket data.

## 2. 仅连接的方法

The other way to do it, if using the write callback is not suitable, is to set `CURLOPT_CONNECT_ONLY` to the value `2L` and let libcurl do a transfer that only sets up the connection to the server, does the WebSocket upgrade and then is considered complete. After that *connect-only* transfer, the application can use `curl_ws_recv()` and `curl_ws_send()` to receive and send WebSocket data over the connection.

## 升级或死亡

Doing a transfer with a `ws://` or `wss://` URL implies that libcurl makes a successful upgrade to the WebSocket protocol or an error is returned. An HTTP 200 response code which for example is considered fine in a normal HTTP transfer is therefore considered an error when asking for a WebSocket transfer.

## 自动 `PONG`

If not using raw mode, libcurl automatically responds with the appropriate `PONG` response for incoming `PING` frames and does not expose them in the API.
