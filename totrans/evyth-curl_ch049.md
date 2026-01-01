# TLS libraries

To make curl support TLS based protocols, such as HTTPS, FTPS, SMTPS, POP3S, IMAPS and more, you need to build with a third-party TLS library since curl does not implement the TLS protocol itself.

curl is written to work with a large number of TLS libraries:

*   AmiSSL
*   AWS-LC
*   BearSSL
*   BoringSSL
*   GnuTLS
*   libressl
*   mbedTLS
*   OpenSSL
*   rustls
*   Schannel (native Windows)
*   Secure Transport (native macOS)
*   WolfSSL

When you build curl and libcurl to use one of these libraries, it is important that you have the library and its include headers installed on your build machine.

## configure

Below, you learn how to tell configure to use the different libraries. The configure script does not select any TLS library by default. You must select one, or instruct configure that you want to build without TLS support using `--without-ssl`.

### OpenSSL, BoringSSL, libressl

[PRE0]

configure detects OpenSSL in its default path by default. You can optionally point configure to a custom install path prefix where it can find OpenSSL:

[PRE1]

The alternatives [BoringSSL](ch049.xhtml#build__boringssl__md) and libressl look similar enough that configure detects them the same way as OpenSSL. It then uses additional measures to figure out which of the particular flavors it is using.

### GnuTLS

[PRE2]

configure detects GnuTLS in its default path by default. You can optionally point configure to a custom install path prefix where it can find gnutls:

[PRE3]

### WolfSSL

[PRE4]

configure detects WolfSSL in its default path by default. You can optionally point configure to a custom install path prefix where it can find WolfSSL:

[PRE5]

### mbedTLS

[PRE6]

configure detects mbedTLS in its default path by default. You can optionally point configure to a custom install path prefix where it can find mbedTLS:

[PRE7]

### Secure Transport

[PRE8]

configure detects Secure Transport in its default path by default. You can optionally point configure to a custom install path prefix where it can find Secure Transport:

[PRE9]

### Schannel

[PRE10]

configure detects Schannel in its default path by default.

(WinSSL was previously an alternative name for Schannel, and earlier curl versions instead needed `--with-winssl`)

### BearSSL

[PRE11]

configure detects BearSSL in its default path by default. You can optionally point configure to a custom install path prefix where it can find BearSSL:

[PRE12]

### Rustls

[PRE13]

When told to use rustls, curl is actually trying to find and use the rustls-ffi library - the C API for the rustls library. configure detects rustls-ffi in its default path by default. You can optionally point configure to a custom install path prefix where it can find rustls-ffi:

[PRE14]