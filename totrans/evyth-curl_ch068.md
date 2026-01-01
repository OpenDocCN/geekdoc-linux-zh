# trurl

*trurl* is a separate command line tool with the sole purpose of parsing, manipulating and outputting URLs and parts of URLs. A companion tool to curl for your command lines and scripting needs.

trurl uses libcurl’s URL parser. This ensures that curl and trurl always have the same opinion about URLs and that both tools parse them identically and consistently.

## Usage

Typically you pass in one or more URLs to trurl and specify what components you want output. Possibly while modifying the URL(s) as well.

trurl knows URLs and every URL consists of up to ten separate and independent *components*. These components can be extracted, removed and updated with trurl.

## trurl example command lines

**Replace the hostname of a URL:**

[PRE0]

**Create a URL by setting components:**

[PRE1]

**Redirect a URL:**

[PRE2]

**Change port number:**

[PRE3]

**Extract the path from a URL:**

[PRE4]

**Extract the port from a URL:**

[PRE5]

**Append a path segment to a URL:**

[PRE6]

**Append a query segment to a URL:**

[PRE7]

**Read URLs from stdin:**

[PRE8]

**Output JSON:**

[PRE9]

**Remove tracking tuples from query:**

[PRE10]

**Show a specific query key value:**

[PRE11]

**Sort the key/value pairs in the query component:**

[PRE12]

**Work with a query that uses a semicolon separator:**

[PRE13]

**Accept spaces in the URL path:**

[PRE14]

## More

Everything you want to know about trurl is found at [https://curl.se/trurl](https://curl.se/trurl). It is probably already available for your Linux distribution of choice.