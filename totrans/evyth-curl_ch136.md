# Reading email

There are two dominant protocols on the Internet for reading/downloading email from servers (at least if we do not count web based reading), and they are IMAP and POP3\. The former being the slightly more modern alternative. curl supports both.

## POP3

To list message numbers and sizes:

[PRE0]

To download message 1:

[PRE1]

To delete message 1:

[PRE2]

## IMAP

Get the mail using the UID 57 from mailbox ‘stuff’:

[PRE3]

Instead, get the mail with index 57 from the mailbox ‘fun’:

[PRE4]

List the mails in the mailbox ‘boring’:

[PRE5]

List the mails in the mailbox ‘boring’ and provide user and password:

[PRE6]

## TLS for emails

POP3 and IMAP can both be done over a secure connection and both can be done using either explicit or implicit TLS. The “explicit” method is probably the most common approach and it means that the client connects to the server using an insecure connection and *upgrades* it to TLS as it goes, using the `STARTTLS` command. You tell curl to attempt this with `--ssl` or if you want to *insist* on a secure connection you use `--ssl-reqd`. Like this:

[PRE7]

or

[PRE8]

“Implicit” SSL means that the connection gets secured already at first connect, which you make curl attempt by specifying a scheme in the URL that uses SSL. In this case either `pop3s://` or `imaps://`. For such connections, curl insists on connecting and negotiating a TLS connection already from the start, or it fails its operation.

The previous explicit examples done with implicit SSL:

[PRE9]

or

[PRE10]