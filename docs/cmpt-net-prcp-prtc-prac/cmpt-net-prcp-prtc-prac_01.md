# 第四版前言

> [原文](https://4ed.computer-networking.info/syllabus/default/preface.html)

与前几版相比，本教材的第四版引入了重大变化。前几版从原理开始讲解，然后解释了在部署的网络中使用的实际协议。从教学角度来看，这种方法的好处是让学生首先理解基本原理，然后详细探索现有协议是如何使用它们的。不幸的是，这种方法的主要缺点是学生花了半个学期的时间来理解抽象的协议。他们必须等到课程的下半部分才能理解他们每天使用的协议。这给学生带来了很多挫败感，并促使我们探索一种不同的方式来向学生解释计算机网络。这种方法在鲁汶大学进行了两年的测试，效果良好。我们将其应用于电子书的第四版。

电子书现在分为两个主要部分。第一部分将网络视为一个黑盒，重点关注主机之间的交互。在这一部分中，学生探索网络应用、安全、命名和寻址以及传输层的多数功能（除了拥塞控制）。网络和链路层被简要解释，重点是帧和包格式。电子书的第二部分打开黑盒，更详细地描述了控制平面协议，主要是路由协议（包括域内和域间路由），但也包括生成树协议和介质访问控制技术。我们还解释了可以在路由器上使用的流量控制技术以及拥塞控制如何在主机上操作。

这种方法的优点之一是学生可以在第一学期就快速进行与真实协议相关的动手练习。之前的方法更多地依赖于传统的纸笔练习。在[鲁汶大学](https://www.uclouvain.be)的学生在课程的同时进行两个项目。第一个项目侧重于主机部分。学生使用[Wireshark](https://www.wireshark.org)和类似工具来探索应用程序或浏览器在与网站交互时交换的数据包。这迫使他们更深入地了解主机使用的协议，并与课程并行进行。第二个项目基于由劳伦特·范贝弗及其在苏黎世联邦理工学院（ETH Zurich）的同事设计的[迷你互联网项目](https://github.com/nsg-ethz/mini_internet_project) [[HBR2020]](bibliography.html#hbr2020)

# 第一版前言

这本教科书源于第一作者的一种挫败感。许多作者选择编写教科书，要么是因为他们所在领域没有教科书，要么是因为他们对现有的教科书不满意。这种挫败感在计算机网络领域催生了多本优秀的教科书。在计算机网络教科书主要侧重于理论的时代，[道格拉斯·科默](https://www.cs.purdue.edu/people/comer)选择编写一本完全专注于 TCP/IP 协议套件的教科书[[科默 1988]](bibliography.html#comer1988)，这在当时是一个艰难的选择。后来，他通过描述一个完整的 TCP/IP 实现来扩展了他的教科书，为[[科默 1988]](bibliography.html#comer1988)中的理论描述增添了实际考虑。[理查德·斯蒂文斯](https://www.kohala.com/)像探险家一样看待互联网，通过观察在线路上交换的所有数据包来解释协议的运作[[斯蒂文斯 1994]](bibliography.html#stevens1994)。[吉姆·库罗斯](https://www-net.cs.umass.edu/personnel/kurose.html)和[基思·罗斯](https://engineering.nyu.edu/faculty/keith-ross)从学生使用的应用开始，重新发明了计算机网络教科书，并逐步通过移除每一层来解释互联网协议[[库罗斯罗斯 09]](bibliography.html#kuroseross09)。

促使这本书产生的挫败感是不同的。当我 1990 年代末开始教授计算机网络时，学生已经是互联网用户了，但他们的使用是有限的。学生仍然使用参考教科书，并在图书馆花时间。今天的学生完全不同。他们是热情且经验丰富的网络用户，在网络上能找到大量信息。这是一种积极的态度，因为他们可能比前辈们更好奇。多亏了互联网上可用的信息，他们可以检查或获取老师解释的课题的额外信息。这些丰富的信息给教师带来了几个挑战。直到 19 世纪末，教师从定义上比学生知识更丰富，学生很难验证老师所教的课程。今天，鉴于每个学生通过互联网指尖可获取的大量信息，验证一个课程或获取特定主题的更多信息有时只需几点击即可。例如，[维基百科](https://en.wikipedia.org)提供了各种主题的大量信息，学生经常查阅它们。不幸的是，这些网站上信息的组织并不适合让学生从中学习。此外，不同主题可用的信息和深度存在巨大差异。

第二个原因是计算机网络社区是开源运动中的积极参与者。今天，大多数网络协议都有高质量且广泛使用的开源实现。这包括作为 [linux](https://www.linux.org)、[freebsd](https://www.freebsd.org) 或在 8 位控制器上运行的 [uIP](https://www.sics.se/~adam/uip/index.php/Main_Page) 堆栈一部分的 TCP/IP 实现，也包括 [bind](https://www.isc.org/software/bind)、[unbound](https://www.unbound.net)、[apache](https://www.apache.org) 或 [sendmail](https://www.sendmail.org) 这样的服务器，以及 [xorp](https://en.wikipedia.org/wiki/XORP) 或 [quagga](https://www.quagga.net) 这样的路由协议实现。此外，定义几乎所有互联网协议的文档都是在互联网工程任务组 ([IETF](https://www.ietf.org)) 的一个开放过程中开发的。IETF 在公开可用的 [RFC](https://www.ietf.org/rfc.html) 中发布其协议规范，新的提案在 [Internet drafts](https://www.ietf.org/standards/ids/) 中描述。 

这本开源教科书旨在通过提供对指导互联网运作的关键原则的详细但具有教育意义的描述，来填补开源实现与开源网络规范之间的差距。本书在 [creative commons license](http://creativecommons.org/licenses/by/3.0/) 许可下发布。这种开源许可的动机有两个。第一个是希望这能让许多学生使用本书来学习计算机网络。第二个是希望其他教师能够重用、改编并改进它。时间将证明是否有可能建立一个贡献者社区，以进一步改进和发展本书。作为一个起点，第一个版本包含了为期一个学期的第一学期本科生或研究生网络课程的所有材料。

本电子书的[第一版](https://www.computer-networking.info/firstedition.html)是由 [Olivier Bonaventure](https://inl.info.ucl.ac.be/obo.html) 编写的。[Laurent Vanbever](https://inl.info.ucl.ac.be/lvanbeve.html)、[Virginie Van den Schriek](https://inl.info.ucl.ac.be/vvandens.html)、[Damien Saucez](https://inl.info.ucl.ac.be/dsaucez.html) 和 [Mickael Hoerdt](https://inl.info.ucl.ac.be/mhoerdt.html) 对练习做出了贡献。Pierre Reinbold 设计了代表交换机的图标，Nipaul Long 将许多图形重新绘制成 SVG 格式。Stéphane Bortzmeyer 向文本提出了许多建议和修正。

多年来，学生和同事对文本的部分内容做出了贡献，包括：

> +   Virginie Van den Schriek 对各种练习做出了贡献
> +   
> +   Laurent Vanbever 对各种练习做出了贡献
> +   
> +   Damien Saucez 对各种练习做出了贡献
> +   
> +   Mickael Hoerdt 对各种练习做出了贡献
> +   
> +   Pierre Reinbold 设计了代表路由器、交换机等图标，并提供了所有系统管理员支持以托管本书
> +   
> +   Nipaul Long 将大部分图表转换为 SVG 格式
> +   
> +   Daire O’Doherty 帮助改进了整本书的写作
> +   
> +   Quentin De Coninck 改进了文本和练习

电子书的第一个和第二个版本是在 GitHub 上开发的。第三版的大部分文本都是前两个版本的一部分。以下是这两版第一版的贡献者名单：

> +   Alexis Nootens
> +   
> +   Antoine Paris
> +   
> +   Benoît Legat
> +   
> +   Daire O’Doherty
> +   
> +   David Lebrun
> +   
> +   Diego Havenstein
> +   
> +   Eduardo Grosclaude
> +   
> +   Florian Knop
> +   
> +   Mathieu Jadin
> +   
> +   Juan Antonio Cordero
> +   
> +   Joris Van Hecke
> +   
> +   Léonard Julement
> +   
> +   Laurent Lantsogh
> +   
> +   Laurent Vanbever
> +   
> +   Marcel Waldvogel
> +   
> +   Matthieu Baerts
> +   
> +   Melanie Sedda
> +   
> +   Mickael Hoerdt
> +   
> +   motateko
> +   
> +   Nicolas Pettiaux
> +   
> +   Nipaul Long
> +   
> +   Olivier Tilmans
> +   
> +   Pablo Gonzalez
> +   
> +   Raphael Bauduin
> +   
> +   Robin Descamps
> +   
> +   Hélène Verhaeghe
> +   
> +   Virginie Vandenschriek

第三版的主要贡献者是[Olivier Bonaventure](https://inl.info.ucl.ac.be/obo.html)和[Quentin De Coninck](https://qdeconinck.github.io)。本版的其他贡献包括：

> +   Adrien Defer
> +   
> +   Anthony Gégo
> +   
> +   François Michel
> +   
> +   Benjamin Caudron
> +   
> +   Mohamed Elshawaf
> +   
> +   Amadéo David
> +   
> +   Fabien Duchene
> +   
> +   Florent Dardenne
> +   
> +   Nicolas Rosar
> +   
> +   Gauthier de Moffarts
> +   
> +   Marcin Wilk
> +   
> +   Greg Skinner

电子书的全部源代码可在[CNP3/ebook](https://github.com/CNP3/ebook)上找到。如果您发现任何错误、打字错误或想改进电子书，请添加问题或提出拉取请求。

电子书的 HTML 版本可在[`www.computer-networking.info`](https://www.computer-networking.info)找到。它包括在[`www.inginious.org/course/cnp3`](https://www.inginious.org/course/cnp3)平台上托管的多种在线练习。

该电子书仅涵盖计算机网络领域的很小一部分。为了鼓励读者探索该领域的其他方面，我们定期在 Networking Notes 博客上发布相关信息的链接，博客地址为[`blog.computer-networking.info`](https://blog.computer-networking.info)。您也可以通过[@cnp3_ebook](https://twitter.com/cnp3_ebook)在 Twitter 上关注我们。

注意

计算机网络：原理、协议与实践，(c) 2011-2024，[Olivier Bonaventure](https://inl.info.ucl.ac.be/obo.html)，[天主教鲁汶大学](https://www.uclouvain.be)（比利时）以及上述合作者，在 Saylor Foundation 的开放教科书挑战计划资助下使用 Creative Commons Attribution（CC BY）许可，以便将其纳入 Saylor.org 在[`www.saylor.org`](https://www.saylor.org)上提供的开放课程集合。完整的许可条款可在[`creativecommons.org/licenses/by/3.0/`](https://creativecommons.org/licenses/by/3.0/)查看。
