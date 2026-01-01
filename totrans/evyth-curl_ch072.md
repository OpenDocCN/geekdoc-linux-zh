# Variables

This concept of variables for the command line and config files was added in curl 8.3.0.

A user sets a *variable* to a plain string with `--variable varName=content` or from the contents of a file with `--variable varName@file` where the file can be stdin if set to a single dash (`-`).

A variable in this context is given a specific name and it holds contents. Any number of variables can be set. If you set the same variable name again, it gets overwritten with new content. Variable names are case sensitive, can be up to 128 characters long and may consist of the characters a-z, A-Z, 0-9 and underscore.

Some examples below contain multiple lines for readability. The backslash (`\`) is used to instruct the terminal to ignore the newline.

## Setting variables

You can set variables at the command line with `--variable` or in config files with `variable` (no dashes):

[PRE0]

or in a config file:

[PRE1]

## Assigning contents from file

You can assign the contents of a plain text file to a variable, too:

[PRE2]

## Expand

Variables can be expanded in option parameters using `{{varName}}` when the option name is prefixed with `--expand-`. This makes the content of the variable `varName` get inserted.

If you reference a name that does not exist as a variable, a blank string is inserted.

Insert `{{` verbatim in the string by escaping it with a backslash:

`\{{`.

In the example below, the variable `host` is set and then expanded:

[PRE3]

For options specified without the `--expand-` prefix, variables are not expanded.

Variable content holding null bytes that are not encoded when expanded causes curl to exit with an error.

## Environment variables

Import an environment variable with `--variable %VARNAME`. This import makes curl exit with an error if the given environment variable is not set. A user can also opt to set a default value if the environment variable does not exist, using `=content` or `@file` as described above.

As an example, assign the `%USER` environment variable to a curl variable and insert it into a URL. Because no default value is specified, this operation fails if the environment variable does not exist:

[PRE4]

Instead, let’s use `dummy` as a default value if `%USER` does not exist:

[PRE5]

## Expand `--variable`

The `--variable` option itself can also be expanded, which allows you to assign variables to the contents of other variables.

[PRE6]

Or done in a config file:

[PRE7]

## Functions

When expanding variables, curl offers a set of *functions* to change how they are expanded. Functions are applied with colon + function name after the variable, like this: `{{varName:function}}`.

Multiple functions can be applied to the variable. They are then applied in a left-to-right order: `{{varName:func1:func2:func3}}`

These functions are available: `trim`, `json`, `url` and `b64`

## Function: `trim`

Expands the variable without leading and trailing white space. White space is defined as:

*   horizontal tabs
*   spaces
*   new lines
*   vertical tabs
*   form feed and carriage returns

This is extra useful when reading data from files.

[PRE8]

## Function: `json`

Expands the variable as a valid JSON string. This makes it easier to insert valid JSON into an argument (The quotes are not included in the resulting JSON).

[PRE9]

To trim the variable first, apply both functions (in this order):

[PRE10]

## Function: `url`

Expands the variable URL encoded. Also known as *percent encoded*. This function ensures that all output characters are legal within a URL and the rest are encoded as `%HH` where `HH` is a two-digit hexadecimal number for the ascii value.

[PRE11]

To trim the variable first, apply both functions (in this order):

[PRE12]

## Function: `b64`

Expands the variable base64 encoded. Base64 is an encoding for binary data that only uses 64 specific characters.

[PRE13]

To trim the variable first, apply both functions (in this order):

[PRE14]

Example: get the contents of a file called `$HOME/.secret` into a variable called `fix`. Make sure that the content is trimmed and percent-encoded sent as POST data:

[PRE15]