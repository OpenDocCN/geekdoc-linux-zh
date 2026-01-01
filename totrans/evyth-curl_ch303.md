# Test file format

Each curl test is designed in a single text file using an XML-like format.

Labels mark the beginning and the end of all sections, and each label must be written in its own line. Comments are either XML-style (enclosed with `<!--` and `-->`) or shell script style (beginning with `#`) and must appear on their own lines and not alongside actual test data. Most test data files are syntactically valid XML, although a few files are not (lack of support for character entities and the preservation of carriage return and linefeed characters at the end of lines are the biggest differences).

All tests must begin with a `<testcase>` tag, which encompasses the remainder of the file. See below for other tags.

Each test file is called `tests/data/testNUMBER` where `NUMBER` is a unique numerical test identifier. Each test has to use its own dedicated number. The number has no meaning other than identifying the test.

The test file defines exactly what command line or tool to run, what test servers to invoke and how they should respond, exactly what protocol exchange that should happen, what output and return code to expect and much more.

Everything is written within their dedicated tags like this when the name is set:

[PRE0]

## keywords

Every test has one or more `<keywords>` set in the top of the file. They are meant to be “tags” that identify features and protocols that are tested by this test case. `runtests.pl` can be made to run only tests that match (or do not match) such keywords.

## Preprocessed

Under the hood, each test input file is *preprocessed* at startup by `runtests.pl`. This means that variables, macros and keywords are expanded and a temporary version of the file is stored in `tests/log/testNUMBER` - and *that* file is then used by all the test servers etc.

This processing allows the test format to offer features like `%repeat` to create really big test files without bloating the input files correspondingly.

## Base64 Encoding

In the preprocess stage, a special instruction can be used to have runtests.pl base64 encode a certain section and insert in the generated output file. This is in particular good for test cases where the test tool is expected to pass in base64 encoded content that might use dynamic information that is unique for this particular test invocation, like the server port number.

To insert a base64 encoded string into the output, use this syntax:

[PRE1]

The data to encode can then use any of the existing variables mentioned below, or even percent-encoded individual bytes. As an example, insert the HTTP server’s port number (in ASCII) followed by a space and the hexadecimal byte 9a:

[PRE2]

## Hexadecimal decoding

In the preprocess stage, a special instruction can be used to have runtests.pl generate a sequence of binary bytes.

To insert a sequence of bytes from a hex encoded string, use this syntax:

[PRE3]

For example, to insert the binary octets 0, 1 and 255 into the test file:

[PRE4]

## Repeat content

In the preprocess stage, a special instruction can be used to have runtests.pl generate a repetitive sequence of bytes.

To insert a sequence of repeat bytes, use this syntax to make the `<string>` get repeated `<number>` of times. The number has to be 1 or larger and the string may contain `%HH` hexadecimal codes:

[PRE5]

For example, to insert the word hello a 100 times:

[PRE6]

## Conditional lines

Lines in the test file can be made to appear conditionally on a specific feature (see the “features” section below) being set or not set. If the specific feature is present, the following lines are output, otherwise it outputs nothing, until a following `else` or `endif` clause. Like this:

[PRE7]

It can also check for the inverse condition, so if the feature is *not* set by the use of an exclamation mark:

[PRE8]

You can also make an “else” clause to get output for the opposite condition, like:

[PRE9]

**Note** that there can be no nested conditions. You can only do one conditional at a time and you can only check for a single feature in it.

## Variables

When the test is preprocessed, a range of variables in the test file are replaced by their content at that time.

Available substitute variables include:

*   `%CLIENT6IP` - IPv6 address of the client running curl
*   `%CLIENTIP` - IPv4 address of the client running curl
*   `%CURL` - Path to the curl executable
*   `%FILE_PWD` - Current directory, on windows prefixed with a slash
*   `%FTP6PORT` - IPv6 port number of the FTP server
*   `%FTPPORT` - Port number of the FTP server
*   `%FTPSPORT` - Port number of the FTPS server
*   `%FTPTIME2` - Timeout in seconds that should be just sufficient to receive a response from the test FTP server
*   `%FTPTIME3` - Even longer than `%FTPTIME2`
*   `%GOPHER6PORT` - IPv6 port number of the Gopher server
*   `%GOPHERPORT` - Port number of the Gopher server
*   `%GOPHERSPORT` - Port number of the Gophers server
*   `%HOST6IP` - IPv6 address of the host running this test
*   `%HOSTIP` - IPv4 address of the host running this test
*   `%HTTP6PORT` - IPv6 port number of the HTTP server
*   `%HTTPPORT` - Port number of the HTTP server
*   `%HTTP2PORT` - Port number of the HTTP/2 server
*   `%HTTPSPORT` - Port number of the HTTPS server
*   `%HTTPSPROXYPORT` - Port number of the HTTPS-proxy
*   `%HTTPTLS6PORT` - IPv6 port number of the HTTP TLS server
*   `%HTTPTLSPORT` - Port number of the HTTP TLS server
*   `%HTTPUNIXPATH` - Path to the Unix socket of the HTTP server
*   `%SOCKSUNIXPATH` - Absolute Path to the Unix socket of the SOCKS server
*   `%IMAP6PORT` - IPv6 port number of the IMAP server
*   `%IMAPPORT` - Port number of the IMAP server
*   `%MQTTPORT` - Port number of the MQTT server
*   `%TELNETPORT` - Port number of the telnet server
*   `%NOLISTENPORT` - Port number where no service is listening
*   `%POP36PORT` - IPv6 port number of the POP3 server
*   `%POP3PORT` - Port number of the POP3 server
*   `%POSIX_PWD` - Current directory somewhat mingw friendly
*   `%PROXYPORT` - Port number of the HTTP proxy
*   `%PWD` - Current directory
*   `%RTSP6PORT` - IPv6 port number of the RTSP server
*   `%RTSPPORT` - Port number of the RTSP server
*   `%SMBPORT` - Port number of the SMB server
*   `%SMBSPORT` - Port number of the SMBS server
*   `%SMTP6PORT` - IPv6 port number of the SMTP server
*   `%SMTPPORT` - Port number of the SMTP server
*   `%SOCKSPORT` - Port number of the SOCKS4/5 server
*   `%SRCDIR` - Full path to the source dir
*   `%SSHPORT` - Port number of the SCP/SFTP server
*   `%SSHSRVMD5` - MD5 of SSH server’s public key
*   `%SSHSRVSHA256` - SHA256 of SSH server’s public key
*   `%SSH_PWD` - Current directory friendly for the SSH server
*   `%TESTNUMBER` - Number of the test case
*   `%TFTP6PORT` - IPv6 port number of the TFTP server
*   `%TFTPPORT` - Port number of the TFTP server
*   `%USER` - Login ID of the user running the test
*   `%VERSION` - the full version number of the tested curl