# Submit a login form over HTTP

A login submission over HTTP is usually a matter of figuring out exactly what data to submit in a POST and to which target URL to send it.

Once logged in, the target URL can be fetched if the proper cookies are used. As many login-systems work with HTTP redirects, we ask libcurl to follow such if they arrive.

Some login forms makes it more complicated and require that you got cookies from the page showing the login form etc, so if you need that you may want to extend this code a little bit.

By passing in a non-existing cookie file, this example enables the cookie parser so incoming cookies are stored when the response from the login response arrives and then the subsequent request for the resource uses those and prove to the server that we are in fact correctly logged in.

[PRE0]