# Command line options

When telling curl to do something, you invoke curl with zero, one or several command-line options to accompany the URL or set of URLs you want the transfer to be about. curl supports over two hundred different options.

## Short options

Command line options pass on information to curl about how you want it to behave. Like you can ask curl to switch on verbose mode with the -v option:

[PRE0]

-v is here used as a “short option”. You write those with the minus symbol and a single letter immediately following it. Many options are just switches that switch something on or change something between two known states. They can be used with just that option name. You can then also combine several single-letter options after the minus. To ask for both verbose mode and that curl follows HTTP redirects:

[PRE1]

The command-line parser in curl always parses the entire line and you can put the options anywhere you like; they can also appear after the URL:

[PRE2]

and the two separate short options can of course also be specified separately, like:

[PRE3]

## Long options

Single-letter options are convenient since they are quick to write and use, but as there are only a limited number of letters in the alphabet and there are many things to control, not all options are available like that. Long option names are therefore provided for those. Also, as a convenience and to allow scripts to become more readable, most short options have longer name aliases.

Long options are always written with *two* minuses (or *dashes*, whichever you prefer to call them) and then the name and you can only write one option name per double-minus. Asking for verbose mode using the long option format looks like:

[PRE4]

and asking for HTTP redirects as well using the long format looks like:

[PRE5]

## Arguments to options

Not all options are just simple boolean flags that enable or disable features. For some of them you need to pass on data, like perhaps a username or a path to a file. You do this by writing first the option and then the argument, separated with a space. Like, for example, if you want to send an arbitrary string of data in an HTTP POST to a server:

[PRE6]

and it works the same way even if you use the long form of the option:

[PRE7]

When you use the short options with arguments, you can, in fact, also write the data without the space separator:

[PRE8]

## Arguments with spaces

At times you want to pass on an argument to an option, and that argument contains one or more spaces. For example you want to set the user-agent field curl uses to be exactly `I am your father`, including those three spaces. Then you need to put quotes around the string when you pass it to curl on the command line. The exact quotes to use varies depending on your shell/command prompt, but generally it works with double quotes in most places:

[PRE9]

Failing to use quotes, like if you would write the command line like this:

[PRE10]

… makes curl only use ‘I’ as a user-agent string, and the following strings, `am`, `your` and `father` are instead treated as separate URLs since they do not start with `-` to indicate that they are options and curl only ever handles options and URLs.

To make the string itself contain double quotes, which is common when you for example want to send a string of JSON to the server, you may need to use single quotes (except on Windows, where single quotes do not work the same way). Send the JSON string `{ "name": "Darth" }`:

[PRE11]

Or if you want to avoid the single quote thing, you may prefer to send the data to curl via a file, which then does not need the extra quoting. Assuming we call the file ‘json’ that contains the above mentioned data:

[PRE12]

## Negative options

For options that switch on something, there is also a way to switch it off. You then use the long form of the option with an initial `no-` prefix before the name. As an example, to switch off verbose mode:

[PRE13]