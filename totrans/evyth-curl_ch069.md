# URL globbing

At times you want to get a range of URLs that are mostly the same, with only a small portion of it changing between the requests. Maybe it is a numeric range or maybe a set of names. curl offers “globbing” as a way to specify many URLs like that easily.

The globbing uses the reserved symbols [] and {} for this, symbols that normally cannot be part of a legal URL (except for numerical IPv6 addresses but curl handles them fine anyway). If the globbing gets in your way, disable it with `-g, --globoff`.

When using [] or {} sequences when invoked from a command line prompt, you probably have to put the full URL within double quotes to avoid the shell from interfering with it. This also goes for other characters treated special, like for example ‘&’, ‘?’ and ’*’.

While most transfer related functionality in curl is provided by the libcurl library, the URL globbing feature is not.

## Numerical ranges

You can ask for a numerical range with [N-M] syntax, where N is the start index and it goes up to and including M. For example, you can ask for 100 images one by one that are named numerically:

[PRE0]

and it can even do the ranges with zero prefixes, like if the number is three digits all the time:

[PRE1]

Or maybe you only want even-numbered images so you tell curl a step counter too. This example range goes from 0 to 100 with an increment of 2:

[PRE2]

## Alphabetical ranges

curl can also do alphabetical ranges, like when a site has sections named a to z:

[PRE3]

## List

Sometimes the parts do not follow such an easy pattern, and then you can instead give the full list yourself but then within the curly braces instead of the brackets used for the ranges:

[PRE4]

## Combinations

You can use several globs in the same URL which then makes curl iterate over those, too. To download the images of Ben, Alice and Frank, in both the resolutions 100 x 100 and 1000 x 1000, a command line could look like:

[PRE5]

Or download all the images of a chess board, indexed by two coordinates ranged 0 to 7:

[PRE6]

And you can, of course, mix ranges and series. Get a week’s worth of logs for both the web server and the mail server:

[PRE7]

## Output variables for globbing

In all the globbing examples previously in this chapter we have selected to use the `-O / --remote-name` option, which makes curl save the target file using the filename part of the used URL.

Sometimes that is not enough. You are downloading multiple files and maybe you want to save them in a different subdirectory or create the saved filenames differently. curl, of course, has a solution for these situations as well: output filename variables.

Each “glob” used in a URL gets a separate variable. They are referenced as `#[num]` - that means the single character `#` followed by the glob number which starts with 1 for the first glob and ends with the last glob.

Save the main pages of two different sites:

[PRE8]

Save the outputs from a command line with two globs in a subdirectory:

[PRE9]

## Using `[]{}` in URLs

When the globbing concept was introduced in curl in the 1990s, we all used the same Internet standard for how the URL syntax was defined, and in this standard these four symbols are documented as *reserved*. You had to URL-encode them in the URL if you wanted to use them (`%HH` style). Those symbols were therefore not used in URLs and were downright attractive to use for globbing purposes.

Later on, the URL syntax has gradually been relaxed and changed and these days every now and then we see URLs used where one of the four symbols `[]{}` are used as-is, without being URL-encoded. Passing such a URL to curl causes it to spew out syntax errors when the glob parser goes crazy.

To work around that problem, you have two separate options. You either encode the symbols yourself, or you switch off globbing.

Encode the symbols like this:

| symbol | encoding |
| --- | --- |
| `[` | `%5b` |
| `]` | `%5d` |
| `{` | `%7b` |
| `}` | `%7d` |

Or switch off globbing with `-g` or `--globoff`.