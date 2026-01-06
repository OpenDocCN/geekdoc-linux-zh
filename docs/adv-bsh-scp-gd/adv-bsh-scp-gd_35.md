# 第三十章. 网络编程

> 原文：[`tldp.org/LDP/abs/html/networkprogramming.html`](https://tldp.org/LDP/abs/html/networkprogramming.html)

|   |  **网络就像一头大象和一头白象拍卖会：它从不忘记，而且总是垃圾。*

*--Nemo**  |

Linux 系统有相当多的工具用于访问、操作和故障排除网络连接。我们可以将这些工具集成到脚本中--这些脚本可以扩展我们对网络的知识，实用的脚本可以方便网络的管理。

这里是一个简单的 CGI 脚本，演示了连接到远程服务器。

**示例 30-1\. 打印服务器环境**

```sh
#!/bin/bash
# test-cgi.sh
# by Michael Zick
# Used with permission

# May have to change the location for your site.
# (At the ISP's servers, Bash may not be in the usual place.)
# Other places: /usr/bin or /usr/local/bin
# Might even try it without any path in sha-bang.

# Disable filename globbing.
set -f

# Header tells browser what to expect.
echo Content-type: text/plain
echo

echo CGI/1.0 test script report:
echo

echo environment settings:
set
echo

echo whereis bash?
whereis bash
echo

echo who are we?
echo ${BASH_VERSINFO[*]}
echo

echo argc is $#. argv is "$*".
echo

# CGI/1.0 expected environment variables.

echo SERVER_SOFTWARE = $SERVER_SOFTWARE
echo SERVER_NAME = $SERVER_NAME
echo GATEWAY_INTERFACE = $GATEWAY_INTERFACE
echo SERVER_PROTOCOL = $SERVER_PROTOCOL
echo SERVER_PORT = $SERVER_PORT
echo REQUEST_METHOD = $REQUEST_METHOD
echo HTTP_ACCEPT = "$HTTP_ACCEPT"
echo PATH_INFO = "$PATH_INFO"
echo PATH_TRANSLATED = "$PATH_TRANSLATED"
echo SCRIPT_NAME = "$SCRIPT_NAME"
echo QUERY_STRING = "$QUERY_STRING"
echo REMOTE_HOST = $REMOTE_HOST
echo REMOTE_ADDR = $REMOTE_ADDR
echo REMOTE_USER = $REMOTE_USER
echo AUTH_TYPE = $AUTH_TYPE
echo CONTENT_TYPE = $CONTENT_TYPE
echo CONTENT_LENGTH = $CONTENT_LENGTH

exit 0

# Here document to give short instructions.
:<<-'_test_CGI_'

1) Drop this in your http://domain.name/cgi-bin directory.
2) Then, open http://domain.name/cgi-bin/test-cgi.sh.

_test_CGI_
```

为了安全起见，识别计算机访问的 IP 地址可能会有所帮助。

**示例 30-2\. IP 地址**

```sh
#!/bin/bash
# ip-addresses.sh
# List the IP addresses your computer is connected to.

#  Inspired by Greg Bledsoe's ddos.sh script,
#  Linux Journal, 09 March 2011.
#    URL:
#  http://www.linuxjournal.com/content/back-dead-simple-bash-complex-ddos
#  Greg licensed his script under the GPL2,
#+ and as a derivative, this script is likewise GPL2.

connection_type=TCP      # Also try UDP.
field=2           # Which field of the output we're interested in.
no_match=LISTEN   # Filter out records containing this. Why?
lsof_args=-ni     # -i lists Internet-associated files.
                  # -n preserves numerical IP addresses.
		  # What happens without the -n option? Try it.
router="[0-9][0-9][0-9][0-9][0-9]->"
#       Delete the router info.

lsof "$lsof_args" &#124; grep $connection_type &#124; grep -v "$no_match" &#124;
      awk '{print $9}' &#124; cut -d : -f $field &#124; sort &#124; uniq &#124;
      sed s/"^$router"//

#  Bledsoe's script assigns the output of a filtered IP list,
#  (similar to lines 19-22, above) to a variable.
#  He checks for multiple connections to a single IP address,
#  then uses:
#
#    iptables -I INPUT -s $ip -p tcp -j REJECT --reject-with tcp-reset
#
#  ... within a 60-second delay loop to bounce packets from DDOS attacks.

#  Exercise:
#  --------
#  Use the 'iptables' command to extend this script
#+ to reject connection attempts from well-known spammer IP domains.
```

网络编程的更多示例：

1.  从 *nist.gov* 获取时间

1.  下载 URL

1.  GRE 隧道

1.  检查互联网服务器是否运行

1.  示例 16-41

1.  示例 A-28

1.  示例 A-29

1.  示例 29-1

参见系统和管理命令章节中的网络命令以及外部过滤器、程序和命令章节中的通信命令。
