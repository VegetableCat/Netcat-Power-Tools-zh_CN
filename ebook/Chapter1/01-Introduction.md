导读
=====

Netcat最早在1996年发行，作为一个网络管理程序，目的是可以读取和写入在TCP/IP协议栈上传输的TCP/UDP数据。Netcat常常被称为“瑞士军刀(Swiss Army knife)”，这当然有一定的理由的。就像瑞士军刀的多用途一样，Netcat的功能是很有用的，作为一个独立运行的程序和后端工具得到了广泛的应用。Netcat的用途包含：端口扫描，文件传输，获取banner，端口监听和重定向，还有一个比较邪恶的作用——后门程序。

关于 Netcat 的名字来源有些争议，但是其中一个最常见（比较让人信服的）的是 Netcat 是网络版的cat程序。Netcat从网络连接中读取信息，就像cat从文件中读写信息一样。更深层次的原因是，Netcat是特意要设计成像cat那样操作的。

最初的程序是在unix上，尽管没有按照一个规则来定期维护，但是Netcat已经被改写成一系列版本，它已经被移植到了很多的操作系统上，最常见的还是Linux的各种发行版和windows。




>## NOTE
>
>为了学习本章，我们会在 windows 和 Linux上使用Netcat。Windwos 本身是一类操作系统; UNIX 和 Linux 发行版在本质上是一样的，归为一类。此外，Linux的各个发行版的差距是很小的。另外要知道，还有至少两个略微不同的差别：在Unix发行版上的Netcat和最新版的GNU Netcat。
>
>在2006年的nmap黑客的邮件讨论组(Nmap-hackers mailing list)的调查中，在总体评分中，Netcat排名第4。实际上，在三次连续的调查(2000,2003,2006)中，尽管有大量的先进的强大的工具，Netcat仍然名列前茅，分别排名第2和第4和第4。在目前这个时代，当用户寻找最新的最强大的利器(edge tools),Netcat仍然深入人心(Netcat’s long reign continues)。
>
>本章的目的是为您提供关于Netcat的基本了解。为此(To that end),我们将开始进行安装和配置(Windows和UNIX/Linux),并跟进各个选项的注释和Netcat的基本操作的理解。在探讨Netcat的一些操作的时候，我们会介绍这本书的各章，涵盖更详细的操作。为此，把这个介绍性的章节作为出发点，开始你的旅程。enjoy！

