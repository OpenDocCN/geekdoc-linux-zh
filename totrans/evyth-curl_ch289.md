# libcurl internals

libcurl is never finished and is not just an off-the-shelf product. It is a living project that is improved and modified on almost a daily basis. We depend on skilled and interested hackers to fix bugs and to add features.

This chapter is meant to describe internal details to aid keen libcurl hackers to learn some basic concepts on how libcurl works internally and thus possibly where to look for problems or where to add things when you want to make the library do something new.

*   [Easy handles and connections](ch289.xhtml#internals__easy__md)
*   [Everything is multi](ch290.xhtml#internals__multi__md)
*   [State machines](ch291.xhtml#internals__statemachines__md)
*   [Protocol handler](ch292.xhtml#internals__handler__md)
*   [Backends](ch293.xhtml#internals__backends__md)
*   [Caches and state](ch294.xhtml#internals__caches__md)
*   [Timeouts](ch295.xhtml#internals__timeouts__md)
*   [Windows vs Unix](ch296.xhtml#internals__windows-vs-unix__md)
*   [Memory debugging](ch297.xhtml#internals__memory-debugging__md)
*   [Content Encoding](ch298.xhtml#internals__content-encoding__md)
*   [Structs](ch299.xhtml#internals__structs__md)
*   [Resolving host names](ch300.xhtml#internals__resolving__md)
*   [Tests](tests/)