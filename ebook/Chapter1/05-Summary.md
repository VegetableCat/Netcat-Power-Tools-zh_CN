# 总结

Netcat是一个网络程序，被用于读取，从使用IP协议栈的TCP和UDP连接中的数据。简单的说(more simply),Netcat是UNIX系统中的cat程序的网络版本。cat读写文件，Netcat读写网络连接。在过去十年中，尽管有更先进的工具被引入，Netcat因其简单而强大的功能，受到广泛用户喜爱。

简单却强大和本章的主题联系在一起。如你所见，Netcat的安装，无论是Windows，还是Linux(通过软件包或源码)，都很简单.只有少数几个常用的选项，使得学习命令行几乎不费吹灰之力。然而，无故障的安装和简单的命令行操作掩盖(belie)了事实，即Netcat确实是一个潜在的、功能强大的程序。

Netcat的简易可能会让一些人小看它。人们说，他们"低估(underestimated)"了Netcat的用处。其他人说他们在几年后"重新发现(rediscovering)"Netcat的用处了。不管是什么原因，回答好像总是...用Netcat吧！许多用户甚至推荐用Netcat来代替Telnet。Netcat是很有用的，所以在大多数用户的工具包中都占有一席之地。无论是为网络排除故障的管理员，评估(access)客户安全的渗透测试人员，或者只是一个想要学一些新东西的用户，Netcat都能帮到你。

   几年前，Mati Aharoni，BackTrack渗透测试CD的一位核心开发者，以及www.offensive-security.com的一位创始人(也就是Metasploit的作者)曾写过一篇短篇的安全论文，来示范一个完整的hack过程，从开始到结束。开始是一个端口扫描，接着抓取Banner信息，扫描应用程序的漏洞，放置后门，最后把一个文件传送到这台被占有的电脑。文件的内容只是一条简短的信息"你已经被黑了(You have been hacked!)"。如果你曾做到这步，你就知道，完成这次hack，从头到尾，只用了一个工具，Netcat。



## 链接

- [目录](../directory.md)