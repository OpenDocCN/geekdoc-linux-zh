# FTP type

This is not a feature that is widely used.

URLs that identify files on FTP servers have a special feature that allows you to also tell the client (curl in this case) which file type the resource is. This is because FTP is a little special and can change mode for a transfer and thus handle the file differently than if it would use another mode.

You tell curl that the FTP resource is an ASCII type by appending `;type=A` to the URL. Getting the `foo` file from the root directory of `example.com` using ASCII could then be made with:

[PRE0]

And while curl defaults to binary transfers for FTP, the URL format allows you to also specify the binary type with type=I:

[PRE1]

Finally, you can tell curl that the identified resource is a directory if the type you pass is D:

[PRE2]

…this can then work as an alternative format, instead of ending the path with a trailing slash as mentioned above.