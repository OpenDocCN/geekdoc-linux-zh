# Preface for the fourth edition#

> 原文：[https://4ed.computer-networking.info/syllabus/default/preface.html](https://4ed.computer-networking.info/syllabus/default/preface.html)

The fourth edition of the textbook introduces a major change compared to the previous editions. There previous editions started from the principles before explaining the actual protocols that are used in deployed networks. From a pedagogical viewpoint, this approach had the benefit of allowing the students to first understand the basic principles and then explore in details how existing protocols use them. Unfortunately, the main drawback of this approach was that students spent half their semester to understand abstract protocols. They had to wait until the second half of the course to be able to understand the protocols that they use every day. This created a lot of frustration among students and motivated us to explore a different way to explain computer networking to students. This approach has been tested during two years at [UCLouvain](https://www.uclouvain.be) and worked well. We have adopted it for the fourth edition of the ebook.

The ebook is now divided in two main parts. The first part considers the network as a black box and focuses on the interactions between the hosts. During this part, student explore networked applications, security, naming and addressing and most functions of the transport layer except congestion control. The network and datalink layers are briefly explained with a focus on the frame and packet formats. The second part of the ebook opens the black box and describes in more details the control plane protocols, mainly the routing protocols (both intradomain and interdomain routing), but also the Spanning Tree Protocol and Medium Access Control techniques. We also explain the traffic control techniques which can be used on routers and how congestion control operates on hosts.

A key benefit of this approach is that students can quickly carry hands-on exercises with real protocols during the first semester. The previous approach relied more on traditional paper/pencil exercises. At [UCLouvain](https://www.uclouvain.be) students work on two projects in parallel with the course. The first project focuses on the host part. The students use [Wireshark](https://www.wireshark.org) and similar tools to explore the packets exchanged by an application or a browser when interacting with a website. This forces them to better understand the protocols used by hosts in more details in parallel with the course. The second project builds upon the [Mini Internet project](https://github.com/nsg-ethz/mini_internet_project) designed by Laurent Vanbever and his colleagues at ETH Zurich [[HBR2020]](bibliography.html#hbr2020)

# Preface for the first editions[#](#preface-for-the-first-editions "Link to this heading")

This textbook came from a frustration of first main author. Many authors choose to write a textbook because there are no textbooks in their field or because they are not satisfied with the existing textbooks. This frustration has produced several excellent textbooks in the networking community. At a time when networking textbooks were mainly theoretical, [Douglas Comer](https://www.cs.purdue.edu/people/comer) chose to write a textbook entirely focused on the TCP/IP protocol suite [[Comer1988]](bibliography.html#comer1988), a difficult choice at that time. He later extended his textbook by describing a complete TCP/IP implementation, adding practical considerations to the theoretical descriptions in [[Comer1988]](bibliography.html#comer1988). [Richard Stevens](https://www.kohala.com/) approached the Internet like an explorer and explained the operation of protocols by looking at all the packets that were exchanged on the wire [[Stevens1994]](bibliography.html#stevens1994). [Jim Kurose](https://www-net.cs.umass.edu/personnel/kurose.html) and [Keith Ross](https://engineering.nyu.edu/faculty/keith-ross) reinvented the networking textbooks by starting from the applications that the students use and later explained the Internet protocols by removing one layer after the other [[KuroseRoss09]](bibliography.html#kuroseross09).

The frustrations that motivated this book are different. When I started to teach networking in the late 1990s, students were already Internet users, but their usage was limited. Students were still using reference textbooks and spent time in the library. Today’s students are completely different. They are avid and experienced web users who find lots of information on the web. This is a positive attitude since they are probably more curious than their predecessors. Thanks to the information that is available on the Internet, they can check or obtain additional information about the topics explained by their teachers. This abundant information creates several challenges for a teacher. Until the end of the nineteenth century, a teacher was by definition more knowledgeable than his students and it was very difficult for the students to verify the lessons given by their teachers. Today, given the amount of information available at the fingertips of each student through the Internet, verifying a lesson or getting more information about a given topic is sometimes only a few clicks away. Websites such as [wikipedia](https://en.wikipedia.org) provide lots of information on various topics and students often consult them. Unfortunately, the organization of the information on these websites is not well suited to allow students to learn from them. Furthermore, there are huge differences in the quality and depth of the information that is available for different topics.

The second reason is that the computer networking community is a strong participant in the open-source movement. Today, there are high-quality and widely used open-source implementations for most networking protocols. This includes the TCP/IP implementations that are part of [linux](https://www.linux.org), [freebsd](https://www.freebsd.org) or the [uIP](https://www.sics.se/~adam/uip/index.php/Main_Page) stack running on 8bits controllers, but also servers such as [bind](https://www.isc.org/software/bind), [unbound](https://www.unbound.net), [apache](https://www.apache.org) or [sendmail](https://www.sendmail.org) and implementations of routing protocols such as [xorp](https://en.wikipedia.org/wiki/XORP) or [quagga](https://www.quagga.net) . Furthermore, the documents that define almost all of the Internet protocols have been developed within the Internet Engineering Task Force ([IETF](https://www.ietf.org)) using an open process. The IETF publishes its protocol specifications in the publicly available [RFC](https://www.ietf.org/rfc.html) and new proposals are described in [Internet drafts](https://www.ietf.org/standards/ids/).

This open textbook aims to fill the gap between the open-source implementations and the open-source network specifications by providing a detailed but pedagogical description of the key principles that guide the operation of the Internet. The book is released under a [creative commons license](http://creativecommons.org/licenses/by/3.0/). Such an open-source license is motivated by two reasons. The first is that we hope that this will allow many students to use the book to learn computer networks. The second is that I hope that other teachers will reuse, adapt and improve it. Time will tell if it is possible to build a community of contributors to improve and develop the book further. As a starting point, the first release contains all the material for a one-semester first upper undergraduate or a graduate networking course.

The [first edition](https://www.computer-networking.info/firstedition.html) of this ebook has been written by [Olivier Bonaventure](https://inl.info.ucl.ac.be/obo.html). [Laurent Vanbever](https://inl.info.ucl.ac.be/lvanbeve.html), [Virginie Van den Schriek](https://inl.info.ucl.ac.be/vvandens.html), [Damien Saucez](https://inl.info.ucl.ac.be/dsaucez.html) and [Mickael Hoerdt](https://inl.info.ucl.ac.be/mhoerdt.html) have contributed to exercises. Pierre Reinbold designed the icons used to represent switches and Nipaul Long has redrawn many figures in the SVG format. Stephane Bortzmeyer sent many suggestions and corrections to the text.

Over the years, students and colleagues contributed to parts of the text, including:

> *   Virginie Van den Schriek contributed to various exercises
>     
>     
> *   Laurent Vanbever contributed to various exercises
>     
>     
> *   Damien Saucez contributed to various exercises
>     
>     
> *   Mickael Hoerdt contributed to various exercises
>     
>     
> *   Pierre Reinbold designed the icons used to represent routers, switches, … and provided all the sysadmin support to host the book
>     
>     
> *   Nipaul Long converted most of the figures to SVG format
>     
>     
> *   Daire O’Doherty helped to improve the writing throughout the book
>     
>     
> *   Quentin De Coninck improved the text and exercises

The first and second versions of the e-book were developed on GitHub. A lot of text for the third edition was part of the two previous editions. Here is the list of contributors to these two first editions:

> *   Alexis Nootens
>     
>     
> *   Antoine Paris
>     
>     
> *   Benoît Legat
>     
>     
> *   Daire O’Doherty
>     
>     
> *   David Lebrun
>     
>     
> *   Diego Havenstein
>     
>     
> *   Eduardo Grosclaude
>     
>     
> *   Florian Knop
>     
>     
> *   Mathieu Jadin
>     
>     
> *   Juan Antonio Cordero
>     
>     
> *   Joris Van Hecke
>     
>     
> *   Léonard Julement
>     
>     
> *   Laurent Lantsogh
>     
>     
> *   Laurent Vanbever
>     
>     
> *   Marcel Waldvogel
>     
>     
> *   Matthieu Baerts
>     
>     
> *   Melanie Sedda
>     
>     
> *   Mickael Hoerdt
>     
>     
> *   motateko
>     
>     
> *   Nicolas Pettiaux
>     
>     
> *   Nipaul Long
>     
>     
> *   Olivier Tilmans
>     
>     
> *   Pablo Gonzalez
>     
>     
> *   Raphael Bauduin
>     
>     
> *   Robin Descamps
>     
>     
> *   Hélène Verhaeghe
>     
>     
> *   Virginie Vandenschriek

The main contributors to the third edition were [Olivier Bonaventure](https://inl.info.ucl.ac.be/obo.html) and [Quentin De Coninck](https://qdeconinck.github.io). Other contributions to this edition include:

> *   Adrien Defer
>     
>     
> *   Anthony Gégo
>     
>     
> *   François Michel
>     
>     
> *   Benjamin Caudron
>     
>     
> *   Mohamed Elshawaf
>     
>     
> *   Amadéo David
>     
>     
> *   Fabien Duchene
>     
>     
> *   Florent Dardenne
>     
>     
> *   Nicolas Rosar
>     
>     
> *   Gauthier de Moffarts
>     
>     
> *   Marcin Wilk
>     
>     
> *   Greg Skinner

The entire source code for the ebook is available on [CNP3/ebook](https://github.com/CNP3/ebook) If you spot any error, typo or want to improve the ebook, please add issues or suggest pull requests.

The HTML version of the ebook is available from [https://www.computer-networking.info](https://www.computer-networking.info) It includes various online exercises hosted on the [https://www.inginious.org/course/cnp3](https://www.inginious.org/course/cnp3) platform.

The ebook covers only a small subset of the Computer Networking domain. To encourage the readers to explore other aspects of this field, we regularly post pointers to relevant information on the Networking Notes blog at [https://blog.computer-networking.info](https://blog.computer-networking.info) You can also follow us on twitter via [@cnp3_ebook](https://twitter.com/cnp3_ebook).

Note

Computer Networking : Principles, Protocols and Practice, (c) 2011-2024, [Olivier Bonaventure](https://inl.info.ucl.ac.be/obo.html), [Université catholique de Louvain](https://www.uclouvain.be) (Belgium) and the collaborators listed above, used under a Creative Commons Attribution (CC BY) license made possible by funding from The Saylor Foundation’s Open Textbook Challenge in order to be incorporated into Saylor.org’ collection of open courses available at [https://www.saylor.org](https://www.saylor.org). Full license terms may be viewed at : [https://creativecommons.org/licenses/by/3.0/](https://creativecommons.org/licenses/by/3.0/)