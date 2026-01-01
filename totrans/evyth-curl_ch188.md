# Directory traversing

When doing FTP commands to traverse the remote file system, there are a few different ways curl can proceed to reach the target file, the file the user wants to transfer.

## multicwd

curl can do one change directory (CWD) command for every individual directory down the file tree hierarchy. If the full path is `one/two/three/file.txt`, that method means doing three `CWD` commands before asking for the `file.txt` file to get transferred. This method thus creates quite a large number of commands if the path is many levels deep. This method is mandated by an early spec (RFC 1738) and is how curl acts by default:

[PRE0]

This then equals this FTP command/response sequence (simplified):

[PRE1]

## nocwd

The opposite to doing one CWD for each directory part is to not change the directory at all. This method asks the server using the entire path at once and is thus fast. Occasionally servers have a problem with this and it is not purely standards-compliant:

[PRE2]

This then equals this FTP command/response sequence (simplified):

[PRE3]

## singlecwd

This is the in-between the other two FTP methods. This makes a single `CWD` command to the target directory and then it asks for the given file:

[PRE4]

This then equals this FTP command/response sequence (simplified):

[PRE5]