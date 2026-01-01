# Command line concepts

curl started out as a command-line tool and it has been invoked from shell prompts and from within scripts by countless users over the years.

## Garbage in gives garbage out

curl has little will of its own. It tries to please you and your wishes to a large extent. It also means that it tries to play with what you give it. If you misspell an option, it might do something unintended. If you pass in a slightly illegal URL, chances are curl still deals with it and proceeds. It means that you can pass in crazy data in some options and you can have curl pass on that crazy data in its transfer operation.

This is a design choice, as it allows you to really tweak how curl does its protocol communications and you can have curl massage your server implementations in the most creative ways.

*   [Differences](ch051.xhtml#cmdline__differences__md)
*   [Command line options](ch052.xhtml#cmdline__options__md)
*   [Options depend on version](ch053.xhtml#cmdline__versions__md)
*   [URLs](urls/)
*   [URL globbing](ch068.xhtml#cmdline__globbing__md)
*   [List options](ch069.xhtml#cmdline__listopts__md)
*   [Config file](ch070.xhtml#cmdline__configfile__md)
*   [Variables](ch071.xhtml#cmdline__variables__md)
*   [Passwords](ch072.xhtml#cmdline__passwords__md)
*   [Progress meter](ch073.xhtml#cmdline__progressmeter__md)
*   [Version](ch074.xhtml#cmdline__curlver__md)
*   [Persistent connections](ch075.xhtml#cmdline__persist__md)
*   [Exit code](ch076.xhtml#cmdline__exitcode__md)
*   [Copy as curl](ch077.xhtml#cmdline__copyas__md)