# Host

The hostname part of the URL is, of course, simply a name that can be resolved to a numerical IP address, or the numerical address itself.

[PRE0]

When specifying a numerical address, use the dotted version for IPv4 addresses:

[PRE1]

…and for IPv6 addresses the numerical version needs to be within square brackets:

[PRE2]

When a hostname is used, the conversion of the name to an IP address is typically done using the system’s resolver functions. That normally lets a sysadmin provide local name lookups in the `/etc/hosts` file (or equivalent).

## International Domain Names (IDN)

curl knows how to deal with IDN names and you just pass them on like you would a normal name:

[PRE3]