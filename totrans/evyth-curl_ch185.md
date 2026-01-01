# Uploading with FTP

To upload to an FTP server, you specify the entire target file path and name in the URL, and you specify the local filename to upload with `-T, --upload-file`. Optionally, you end the target URL with a slash and then the file component from the local path is appended by curl and used as the remote filename.

Like:

[PRE0]

or to use the local filename as the remote name:

[PRE1]

curl also supports [globbing](ch068.xhtml#cmdline__globbing__md) in the `-T` argument so you can opt to easily upload a range of files:

[PRE2]

or a series of files:

[PRE3]

or

[PRE4]