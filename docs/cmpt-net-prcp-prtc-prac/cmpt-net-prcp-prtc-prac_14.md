# 术语表

> 原文：[`4ed.computer-networking.info/syllabus/default/glossary.html`](https://4ed.computer-networking.info/syllabus/default/glossary.html)

AIMD#

加法增加，乘法减少。一种速率适配算法，TCP 特别使用，当网络未拥塞时，主机以加法方式增加其传输速率，当检测到拥塞时以乘法方式减少。

任何播送（anycast）

一种传输模式，其中信息从一个源发送到一个属于指定组的接收器

API#

应用程序编程接口

ARP#

地址解析协议（ARP）是 IPv4 设备用于获取局域网中与 IPv4 地址相对应的数据链路层地址的协议。ARP 在[**RFC 826**](https://datatracker.ietf.org/doc/html/rfc826.html)中定义。

ARPANET#

高级研究计划署（ARPA）网络是由美国国防部 ARPA 资助下，美国网络科学家构建的网络。ARPANET 被认为是当今互联网的鼻祖。

ASCII#

美国信息交换标准代码（ASCII）是一种字符编码方案，它定义了字符的二进制表示。ASCII 表包含可打印字符和控制字符。ASCII 字符以 7 位编码，仅包含用于书写英文文本所需的字符。后来还开发了其他字符集，如 Unicode，以支持所有书写语言。

ASN.1#

抽象语法表示法 1（ASN.1）是由 ISO 和 ITU-T 设计的。它是一种标准且灵活的表示法，可用于描述表示、编码、传输和解码应用程序之间数据的数据结构。它最初是为了在 OSI 参考模型的表示层中使用而设计的，但现在也用于其他协议，如 SNMP。

ATM#

异步传输模式

BGP#

边界网关协议（BGP）是全球互联网中使用的域间路由协议。

BNF#

巴科斯-诺尔范式（BNF）是使用语法和词汇规则描述语言的一种正式方法。BNF 经常用于定义编程语言，也用于定义网络应用程序之间交换的消息。[**RFC 5234**](https://datatracker.ietf.org/doc/html/rfc5234.html) 解释了如何编写 BNF 以指定互联网协议。

广播#

一种传输模式，其中相同的信息被发送到网络中的所有节点

CIDR#

无类别域间路由是 IP 版本 4 的当前地址分配架构。它在[**RFC 1518**](https://datatracker.ietf.org/doc/html/rfc1518.html)和[**RFC 4632**](https://datatracker.ietf.org/doc/html/rfc4632.html)中定义。

拨号线路#

正常电话线的同义词，即可以用来拨打任何电话号码的线路。

DNS#

域名系统是一个分布式数据库，主机可以通过它查询将名称映射到 IP 地址。它在[**RFC 1035**](https://datatracker.ietf.org/doc/html/rfc1035.html)中定义。

eBGP#

eBGP 会话是两个属于不同自治系统的直接连接的路由器之间的 BGP 会话。也称为外部 BGP 会话。

EGP#

外部网关协议。域间路由协议的同义词

EIGRP#

增强型内部网关路由协议（EIGRP）是一种常在企业网络中使用的专有域内路由协议。EIGRP 使用在[[Garcia1993]](bibliography.html#garcia1993)中描述的 DUAL 算法。

以太网#

最广泛使用的局域网技术。

文件传输#

一种允许用户通过网络从远程服务器发送或接收文件的服务。文件传输协议 FTP 曾经是一种流行的服务。现在它已经被 HTTP/HTTPs 或更安全的协议，如[SSH 文件传输协议](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol)所取代。

frame#

一个帧是数据链路层信息传输的单位。

Frame-Relay#

一种由电信运营商部署的，使用虚电路的广域网技术。

ftp#

在 HTTP [**RFC 2616**](https://datatracker.ietf.org/doc/html/rfc2616.html)广泛采用之前，定义在[**RFC 959**](https://datatracker.ietf.org/doc/html/rfc959.html)的文件传输协议（FTP）一直是互联网上交换文件的既定协议。

FTP#

文件传输协议（FTP）在[**RFC 959**](https://datatracker.ietf.org/doc/html/rfc959.html)中定义。

hosts.txt#

包含所有互联网主机列表的原始文件。这个文件已经被弃用，但 Unix 变体仍然维护一个包含名称和 IP 地址映射的本地`/etc/hosts`文件。有关 Linux 上此文件格式的描述，请参阅[`man7.org/linux/man-pages/man5/hosts.5.html`](http://man7.org/linux/man-pages/man5/hosts.5.html)。

HTML#

超文本标记语言指定了在万维网上交换的文档的结构和语法。HTML 由 W3C 的[HTML 工作组](https://www.w3.org/html/wg/)维护

HTTP#

超文本传输协议在[**RFC 2616**](https://datatracker.ietf.org/doc/html/rfc2616.html)中定义

集线器#

在物理层运行的中继器。

iBGP#

一个 iBGP 会话是同一自治系统内两个路由器之间的 BGP。也称为内部 BGP 会话。

ICANN#

互联网名称与数字地址分配机构（ICANN）负责协调域名、IP 地址和 AS 号码以及协议参数的分配。它还负责 DNS 根域名服务器的运行和演进。

IETF#

互联网工程任务组是一个非营利组织，负责制定互联网中使用的协议标准。IETF 主要涵盖传输和网络层。IETF 内部也标准化了几个应用层协议。IETF 的工作以工作组的形式组织。大部分工作是通过电子邮件交换完成的，每年有三次 IETF 会议。任何人都可以参加。请参阅[`www.ietf.org`](https://www.ietf.org)

IGP#

内部网关协议。同义词为域内路由协议

IGRP#

内部网关路由协议（IGRP）是一个专有域内路由协议，使用距离矢量。IGRP 支持每条路由的多个度量标准，但已被 EIGRP 取代

IMAP#

互联网消息访问协议（IMAP），在[**RFC 3501**](https://datatracker.ietf.org/doc/html/rfc3501.html)中定义，是一个应用层协议，允许客户端访问和操作存储在服务器上的电子邮件。使用 IMAP，电子邮件消息保留在服务器上，不会下载到客户端。

互联网#

公共互联网，即由运行 IPv4 或 IPv6 的不同网络组成的网络

互联网#

互联网是一个互联网，即由不同网络组成的网络。大写的互联网对应于我们今天使用的全球网络，但在路径中已经使用了其他互联网。

互联网分配数字权威机构#

IANA#

互联网分配数字权威机构（IANA）负责 DNS 根、IP 地址和其他互联网协议资源的协调

互联网协议#

IP#

互联网协议是 TCP/IP 协议套件中网络层协议的通用术语。IP 版本 4 被广泛使用，但 IP 版本 6 正在全球范围内部署。

互联网服务提供商#

互联网服务提供商#

ISP#

为用户提供互联网接入的公司

反向查询#

对于 DNS 服务器和解析器，反向查询是针对与给定 IP 地址对应的域名的查询。

IP 子网#

一组具有共同前缀的 IP 地址。

IP 版本 4#

IPv4#

是互联网协议的第四版，是目前互联网中使用的无连接网络层协议。IPv4 地址被编码为 32 位字段。

IP 版本 6#

IPv6#

是互联网协议的第六版，是一种无连接网络层协议，旨在取代 IPv4。IP 版本 6 地址被编码为 128 位字段。

IS-IS#

中间系统-中间系统。最初是为 ISO CLNP 协议定义的链路状态内部域路由，后来扩展以支持 IPv4 和 IPv6。IS-IS 常用于 ISP 网络。它在[[ISO10589]](bibliography.html#iso10589)中定义。

ISN#

TCP 连接的初始序列号是客户端（或服务器）选择的序列号，在建立 TCP 连接时放置在 SYN（或 SYN+ACK）段中。

ISO#

国际标准化组织是联合国的一个机构，总部位于日内瓦，负责在多个主题上制定标准。在 ISO 中，各国代表投票批准或拒绝标准。有关 ISO 的更多信息，可以从[`www.iso.org`](https://www.iso.org)获取。

ISO-3166#

一个 ISO 标准，用于定义表示国家和其子单位的代码。请参阅[`www.iso.org/iso/country_codes.htm`](http://www.iso.org/iso/country_codes.htm)。

ISP#

互联网服务提供商，即向其客户提供互联网接入的网络。

ITU#

国际电信联盟是联合国的一个机构，其目的是为电信行业制定标准。它最初是为了标准化基本电话系统而创建的，但后来扩展到了数据网络。ITU 的工作主要由电信行业的网络专家（运营商和供应商）完成。更多信息请见[`www.itu.int`](https://www.itu.int)。

IXP#

互联网交换点。不同域的路由器连接到同一局域网以建立对等会话和交换数据包的位置。有关 IXP 的部分列表，请参阅[`www.euro-ix.net/`](http://www.euro-ix.net/)或[`en.wikipedia.org/wiki/List_of_Internet_exchange_points_by_size`](https://en.wikipedia.org/wiki/List_of_Internet_exchange_points_by_size)。

承租线路#

两个端点之间永久可用的电话线路。

局域网#

局域网#

LAN#

局域网。例如包括以太网、Wi-Fi、令牌环、FDDI 等。

MAC 地址#

IP 地址#

地址#

一个用于在网络层或数据链路层标识网络接口的比特串。大多数地址具有固定长度，例如 IPv4 为 32 位，IPv6 为 128 位，以太网和其他相关局域网为 48 位。

MAN#

城域网。覆盖大城市地理区域的网络。

中继访问控制#

一种调节对共享介质访问的技术。

MIME#

在[**RFC 2045**](https://datatracker.ietf.org/doc/html/rfc2045.html)中定义的多用途互联网邮件扩展（MIME）是一组对电子邮件消息格式的扩展，允许在邮件消息中使用非 ASCII 字符。一个 MIME 消息可以由几个不同的部分组成，每个部分都有不同的格式。

MIME 文档#

MIME 文档是一个使用 MIME 格式编码的文档。

小型计算机#

小型计算机是一种多用户系统，通常在 20 世纪 60 年代/70 年代用于为部门提供服务。有关更多信息，请参阅相应的维基百科文章：[`en.wikipedia.org/wiki/Minicomputer`](https://en.wikipedia.org/wiki/Minicomputer)。

调制解调器#

调制解调器（modulator-demodulator）是一种通过调制（resp. 解调）模拟信号来编码（resp. 解码）数字信息的设备。调制解调器通常用于通过电话线和无线电链路传输数字信息。有关各种调制解调器的概述，请参阅[`en.wikipedia.org/wiki/Modem`](https://en.wikipedia.org/wiki/Modem)。

MSS#

TCP 实体在 SYN 段中使用的 TCP 选项，用于指示它能够接收的最大段大小。

多播#

一种传输模式，其中信息被有效地发送到属于给定组的所有接收者。

nameserver#

实现 DNS 协议并可以回答其自身域内名称查询的服务器。

NAT#

网络地址转换器是一个中间盒，用于转换 IP 数据包。

NBMA#

非广播多路访问网络是一个子网络，它支持多个主机/路由器，但不提供向子网络中所有设备发送广播帧的有效方式。ATM 子网络是 NBMA 网络的例子。

网络字节序#

互联网协议允许传输字节序列。这些字节序列足以携带 ASCII 字符。网络字节序指的是 16 位和 32 位整数的 Big-Endian 编码。有关端序的更多信息，请参阅[`en.wikipedia.org/wiki/Endianness`](https://en.wikipedia.org/wiki/Endianness)。

NFS#

网络文件系统在[**RFC 1094**](https://datatracker.ietf.org/doc/html/rfc1094.html)中定义。

NNTP#

网络新闻传输协议#

定义在[**RFC 3977**](https://datatracker.ietf.org/doc/html/rfc3977.html)中的用于传输新闻组消息的协议。

NTP#

网络时间协议在[**RFC 1305**](https://datatracker.ietf.org/doc/html/rfc1305.html)中定义。

OSI#

开放系统互联。由 ISO 开发的一套网络标准，包括 7 层 OSI 参考模型。

OSPF#

开放最短路径优先。一种常在企业网络和 ISP 网络中使用的链路状态内部域路由协议。OSPF 在[**RFC 2328**](https://datatracker.ietf.org/doc/html/rfc2328.html)和[**RFC 5340**](https://datatracker.ietf.org/doc/html/rfc5340.html)中定义。

数据包#

数据包#

数据包是网络层信息传输的单位。

PBL#

基于问题学习是一种依赖于问题的教学方法。

POP#

邮政协议（POP），由[**RFC 1939**](https://datatracker.ietf.org/doc/html/rfc1939.html)定义，是一种应用层协议，允许客户端下载存储在服务器上的电子邮件消息。

协议#

一组规则（语义和语法），指定节点可以交换的消息以进行通信。

远程登录#

一种允许用户通过网络连接到远程服务器的服务。过去，定义在[**RFC 854**](https://datatracker.ietf.org/doc/html/rfc854.html)中的 Telnet 和定义在[**RFC 1282**](https://datatracker.ietf.org/doc/html/rfc1282.html)中的 BSD rlogin 服务很受欢迎。由于安全原因，它们已被弃用，并由 ssh 取代。

解析器#

实现 DNS 协议并可以解析查询的服务器。解析器通常为一组客户端（例如，校园中的所有主机或给定 ISP 的所有客户端）提供服务。它代表其客户端向所有名称服务器发送 DNS 查询，并将收到的答案存储在其缓存中。解析器必须知道根名称服务器的 IP 地址。

RIP#

路由信息协议。一种基于距离向量的域内路由协议，有时在企业网络中使用。RIP 在[**RFC 2453**](https://datatracker.ietf.org/doc/html/rfc2453.html)中定义。

RIR#

区域互联网注册局。代表 IANA 管理 IP 地址和 AS 号码的组织。

根名称服务器#

负责域名层次结构根的名称服务器。目前有十几个根名称服务器，每个 DNS 解析器。有关这些根服务器的更多信息，请参阅[`www.root-servers.org/`](http://www.root-servers.org/)。

往返时间#

往返时间（RTT）是段传输与在传输协议中接收相应确认之间的延迟。

路由器#

在网络层中操作的中继。

RPC#

已经定义了多种远程过程调用类型。在[**RFC 5531**](https://datatracker.ietf.org/doc/html/rfc5531.html)中定义的 RPC 机制被诸如 NFS 等应用程序使用。

SDU（服务数据单元）#

服务数据单元是应用程序之间传输信息的单位。

段#

段是传输层信息传输的单位。

SMTP#

简单邮件传输协议（SMTP）在[**RFC 821**](https://datatracker.ietf.org/doc/html/rfc821.html)中定义。

SNMP#

**简单网络管理协议（SNMP）**是为 TCP/IP 网络定义的管理协议。

socket#

一个最初在伯克利 Unix 上定义的低级 API，允许程序员开发客户端和服务器。

欺骗数据包#

当数据包的发送者使用与其自己的地址不同的源地址时，称该数据包为欺骗数据包。

ssh#

**安全壳（SSH）传输层协议**在[**RFC 4253**](https://datatracker.ietf.org/doc/html/rfc4253.html)中定义。

standard query#

对于 DNS 服务器和解析器，一个标准查询是对 A 或 AAAA 记录的查询。此类查询通常返回一个 IP 地址。

switch#

在数据链路层操作的**中继**。

SYN Cookie#

**SYN Cookie**是一种用于计算初始序列号（ISN）的技术。

TCB#

**传输控制块（TCB）**是 TCP 实现为每个建立的 TCP 连接维护的一组变量。

TCP#

**传输控制协议（TCP）**是 TCP/IP 协议套件中传输层的协议，它在上层 IP 上提供了一种可靠的字节流连接服务。

TCP/IP#

指的是 TCP 和 IP 协议。

telnet#

**telnet 协议**在[**RFC 854**](https://datatracker.ietf.org/doc/html/rfc854.html)中定义。

TLD#

**顶级域名**。顶级域名有两种类型。ccTLD 是与两个字母的 ISO-3166 国家代码相对应的 TLD。gTLD 是不分配给任何国家的通用 TLD。

TLS#

**传输层安全性（TLS）**在[**RFC 5246**](https://datatracker.ietf.org/doc/html/rfc5246.html)中定义，是一种用于为互联网应用程序提供通信安全的加密协议。此协议在传输服务之上使用，但其详细描述超出了本书的范围。

UDP#

**用户数据报协议（UDP）**是 TCP/IP 协议套件中传输层的协议，它提供了一种不可靠的无连接服务，包括检测损坏的机制。

单播#

一种信息从一个源发送到一个接收者的**传输模式**。

VNC#

vnc#

一个网络应用程序，允许远程访问计算机的图形用户界面。见[`en.wikipedia.org/wiki/Virtual_Network_Computing`](https://en.wikipedia.org/wiki/Virtual_Network_Computing)。

W3C#

为了标准化在万维网上使用的协议和机制，成立了世界范围的网络联盟。因此，它专注于应用层的一个子集。请参阅[`www.w3.org`](https://www.w3.org)

WAN#

广域网。一个大型的网络。

X.25#

一种使用虚拟电路的广域网技术，由电信运营商部署。

X11#

XWindow 系统和相关的协议在[[SG1990]](bibliography.html#sg1990)中定义。

XML#

可扩展标记语言（XML）是一种灵活的文本格式，源自 SGML。它最初是为电子出版行业设计的，但现在被广泛用于需要交换结构化数据的各种应用。XML 规范由[多个工作组](https://www.w3.org/XML/)的 W3C 维护。
