# 《计算机网络：原理、协议与实践》，第四版#

> 原文：[`4ed.computer-networking.info/syllabus/default/index.html`](https://4ed.computer-networking.info/syllabus/default/index.html)

![_images/cnp3.png](img/cnp3.png)

这本书第四版的草稿尚未润色。如果您发现任何错误或对文本有改进建议，请通过[CNP3/ebook#new](https://github.com/CNP3/ebook/issues/new)创建问题。

这本书第三版的草稿尚未润色。如果您发现任何错误或对文本有改进建议，请通过[CNP3/ebook#new](https://github.com/CNP3/ebook/issues/new)创建问题。

本版教科书的开发工作在[CNP3/ebook](https://github.com/CNP3/ebook)进行。

+   第四版序言

+   第一版序言

# 第一部分：主机#

+   简介

    +   物理层

    +   数据链路层

        +   帧

        +   处理传输错误

    +   网络层

        +   IP 版本 4 入门

        +   IP 版本 6 入门

    +   传输层

        +   无连接传输服务

        +   面向连接的传输服务

        +   请求-响应服务

    +   传输层

        +   无连接传输

        +   用户数据报协议

+   网络应用

    +   命名和寻址

        +   名称的好处

        +   域名系统

    +   电子邮件

        +   简单邮件传输协议

        +   邮局协议

    +   万维网

        +   超文本传输协议

        +   HTTP 版本 2.0

    +   远程过程调用

        +   数据编码

        +   到达被叫方

+   网络安全

    +   安全威胁

    +   加密原语

    +   加密协议

    +   密钥交换

    +   安全壳（ssh）

    +   传输层安全

        +   TLS 握手

        +   TLS 记录协议

        +   改进 TLS

    +   保护域名系统

+   可靠的传输协议

    +   一个简单的可靠协议

        +   在不可靠链路上可靠传输

        +   交替比特协议

        +   Go-back-n 和选择重传

    +   建立传输连接

    +   在传输连接上传输数据

    +   关闭传输连接

+   传输控制协议

    +   TCP 连接建立

    +   TCP 可靠数据传输

        +   分段传输策略

    +   TCP 窗口

    +   TCP 的重传超时

    +   高级重传策略

    +   TCP 连接释放

+   QUIC

    +   帧和包

    +   连接建立

        +   识别 QUIC 连接

        +   安全密钥

        +   QUIC 数据包头部

        +   0-RTT 数据

    +   关闭 QUIC 连接

        +   QUIC 连接的隐式终止

        +   QUIC 连接的显式终止

    +   通过 QUIC 连接交换数据

        +   QUIC 中的流量控制

        +   QUIC 丢包检测

        +   观察 QUIC 连接

            +   深入了解其他 QUIC 握手

            +   观察 QUIC 中的 0-RTT 数据

            +   QUIC 流

+   互联网协议

    +   IP 版本 4

        +   IPv4 数据包

    +   ICMP 版本 4

        +   IPv4 主机的操作

    +   IP 版本 6

        +   IPv6 寻址架构

        +   IPv6 数据包

        +   ICMP 版本 6

# 第二部分：网络#

+   构建网络

    +   数据报组织

        +   计算转发表

        +   平面或分层地址

        +   处理异构数据链路层

    +   虚拟电路组织

+   控制平面

    +   距离矢量路由

    +   链路状态路由

+   IP 网络中的路由

    +   域内路由

        +   RIP

        +   OSPF

+   域间路由

    +   边界网关协议

        +   BGP 决策过程

        +   BGP 收敛

+   数据链路层技术

    +   点对点协议

    +   以太网

        +   以太网交换机

        +   生成树协议（802.1d）

        +   虚拟局域网

    +   802.11 无线网络

+   资源共享

    +   带宽共享

    +   网络拥塞

    +   在网络中分配负载

    +   介质访问控制算法

        +   静态分配方法

        +   ALOHA

        +   载波侦听多路访问

        +   带有碰撞检测的载波侦听多路访问

        +   带有碰撞避免的载波侦听多路访问

        +   确定性介质访问控制算法

    +   拥塞控制

        +   基于窗口传输协议的拥塞控制

        +   拥塞控制

            +   在不丢失数据的情况下控制拥塞

            +   建模 TCP 拥塞控制

# 第三部分：实践#

# 附录#

+   术语表

+   参考文献

## 索引和表格#

+   索引

+   搜索页面

![Creative Commons BY](img/5f308f6c2de546ab7ce944bde590bb4d.png)(_images/cc-by.png)

## 索引和表格#

+   索引

+   搜索页面

![Creative Commons BY](img/5f308f6c2de546ab7ce944bde590bb4d.png)(_images/cc-by.png)
