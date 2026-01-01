# Callbacks

Lots of operations within libcurl are controlled with the use of *callbacks*. A callback is a function pointer provided to libcurl that libcurl then calls at some point to get a particular job done.

Each callback has its specific documented purpose and it requires that you write it with the exact function prototype to accept the correct arguments and return the documented return code and return value so that libcurl performs the way you want it to.

Each callback option also has a companion option that sets the associated user pointer. This user pointer is a pointer that libcurl does not touch or care about, but just passes on as an argument to the callback. This allows you to, for example, pass in pointers to local data all the way through to your callback function.

Unless explicitly stated in a libcurl function documentation, it is not legal to invoke libcurl functions from within a libcurl callback.

*   [Write data](ch213.xhtml#transfers__callbacks__write__md)
*   [Read data](ch214.xhtml#transfers__callbacks__read__md)
*   [Progress information](ch215.xhtml#transfers__callbacks__progress__md)
*   [Header data](ch216.xhtml#transfers__callbacks__header__md)
*   [Debug](ch217.xhtml#transfers__callbacks__debug__md)
*   [sockopt](ch218.xhtml#transfers__callbacks__sockopt__md)
*   [SSL context](ch219.xhtml#transfers__callbacks__sslcontext__md)
*   [Seek and ioctl](ch220.xhtml#transfers__callbacks__seek__md)
*   [Network data conversion](ch221.xhtml#transfers__callbacks__conversions__md)
*   [Opensocket and closesocket](ch222.xhtml#transfers__callbacks__openclosesocket__md)
*   [SSH key](ch223.xhtml#transfers__callbacks__sshkey__md)
*   [RTSP interleaved data](ch224.xhtml#transfers__callbacks__rtsp__md)
*   [FTP wildcard matching](ch225.xhtml#transfers__callbacks__ftpmatch__md)
*   [Resolver start](ch226.xhtml#transfers__callbacks__resolver__md)
*   [Sending trailers](ch227.xhtml#transfers__callbacks__trailers__md)
*   [HSTS](ch228.xhtml#transfers__callbacks__hsts__md)
*   [Prereq](ch229.xhtml#transfers__callbacks__prereq__md)