# Non-HTTP redirects

Browsers support more ways to do redirects that sometimes make life complicated to a curl user as these methods are not supported or recognized by curl.

## HTML redirects

If the above was not enough, the web world also provides a method to redirect browsers by plain HTML. See the example `<meta>` tag below. This is somewhat complicated with curl since curl never parses HTML and thus has no knowledge of these kinds of redirects.

[PRE0]

## JavaScript redirects

The modern web is full of JavaScript and as you know, JavaScript is a language and a full runtime that allows code to execute in the browser when visiting websites.

JavaScript also provides means for it to instruct the browser to move on to another site - a redirect, if you will.