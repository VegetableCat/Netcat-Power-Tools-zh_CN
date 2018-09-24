# 基本运用

在本章的剩余部分，我们将探讨一些基本的运用。



## 简易的聊天界面



我们在一开始(at the outset)就说Netcat是一个网络工具，被用于在网络中读取数据和写入。也许，最简单的办法来理解它是如何工作的，就是简单地建立一个服务器和客户端。你可以把这两个设置在同一台计算机上，也可以使用两台计算机。为了本次演示(demonstration)，我们在一台计算机上设置服务端和客户端。

  先打开一个终端窗口，启动服务端:

```
nc -l -p 12345
```

  在第二个终端，连接到服务端:

```
nc localhost 12345
```

   结果就是这样一个非常基础的聊天界面。在一端输入文字，敲下enter，文字会被发送另一端。需要注意的是，没有任何标志来表明(indicate)文本的来源，只会有输出显示。



Figure 1.12 Sending Data Across a Connection

![image-20180924144014308](../../images/1.12.png?raw=true)







## 端口扫描



虽然Netcat不一定是端口扫描的最佳选择(Nmap是受到广泛认可的最佳工具(the cream of the crop))，但是是有做一些基础(rudimentary)扫描的能力。就像Backtrak的开发人员Mati Aharoni曾经说的,"这不是它所擅长的方面，但是如果我只能用一个工具，我会带上Netcat(It’s not always

the best tool for the job, but if I was stranded on an island, I’d take Netcat with me)."我猜，很多人如果只能选一个工具，他们也会选择Netcat。



   端口扫描用在客户端模式。语法如下:

```
nc -[options] hostname [ports]
```

-w(网络超时)和-z，是用于端口扫描时最常用的选项，这两个都可能帮助加快你的扫描。其他的，还有，-i(设置间隔时间)，-n(禁用DNS查找)，-r(扫描随机端口)。参见图1.13的例子。



### TIP

端口扫描的时候，记得加上-v(详细)选项，或者是将输出重定向到一个文件。如果你不这样，Netcat也会正常扫描，但是不会给你任何显示。一般情况，常常加上-v是很好的选择。



当你列端口的时候，你可以加上一些选项。你可以列出一个单独的端口数字，或者一系列由逗号分割的端口，或者是一个范围(包含)。你甚至可以列出端口对应的服务名称。以下都可以:

```
nc –v 192.168.1.4 21, 80, 443

nc –v 192.168.1.4 1-200

nc –v 192.168.1.4 http
```

ps:

```
root@kali:~# nc -v 192.168.1.7 80

192.168.1.7 80 (http) open

^CExiting.

root@kali:~# nc -v 192.168.1.7 ssh

192.168.1.7 22 (ssh) open

SSH-2.0-OpenSSH_6.7p1 Raspbian-5

^CExiting.
```



   其中的常用端口，Netcat会告诉你那个端口和与其对应服务名称。在Windows中，常用的服务位于/WINDOWS/system32/drivers/etc/services。在Linux中，是在/etc/services文件。这些文件可以用于参考服务名称，以此来代替端口号。





在图1.13,Netcat运行在客户端模式在，并且含有以下选项:

显示详细信息，无DNS查询，随机端口顺序扫描，网络闲置超时3秒，关闭输入\输出。主机是192.168.1.4,端口扫描是21-25.Netcat返回了的信息是21端口开启，这很可能是用于FTP。有关端口扫描和Netcat的更多信息，请参见第10章，用Netcat来审计(audit)。



![image-20180924144014308](../../images/1.13.png?raw=true)



### NOTE

你也可以使用-u来扫描UDP端口，但是要知道(be aware)，即使端口开放，也可能"不会回复"。当然，这是有可能的，但并非大多数情况(circumstances)。



## 传输文件



Netcat的一个常见的用途就是传输文件。Netcat有上传和下载(pull and push)的功能。注意下面的例子：

```
nc -l -p 12345 < textfile
```

此时，Netcat开始在本地端口12345运行在服务端，并且主动提供了textfile文件。一旦有客户端连接到这台服务器，就会开始从服务器上拉取textfile文件并接收：

```
nc 192.168.1.4 12345 > textfile
```



#### Notes from the Underground ...

使用Netcat下载文件

   你可能会带着一个很充分的理由来怀疑，为什么要使用Netcat来传输文件，而不是使用更常见的文件传输协议(FTP,File Transfer Protocol).其实，在很多情况下，FTP可能是比较好的选择。但是，考虑到潜在威胁的情况，就是你有shell访问权限的目标计算机是在防火墙内。你要传输一些文件到目标，但是防火墙会阻止入站(inbound)流量。

   在这种情况下，你可以在本地运行Netcat的服务端模式，提供要传输的文件。然后，在目标计算机上运行Netcat的客户端。在多数情况下，防火墙会允许常见的出口(outbound)流量，所以你还可以在常用端口隐藏你的文件传输，比如80(HTTP).要获取更多的信息，请参见第5章，Netcat的黑暗，和第6章，用Netcat传输文件。



Netcat也能被用于发送(push)文件。如果你正在目的地(就是你想要收到文件的地方)运行Netcat，把Netcat置于服务器模式:

nc -l -p 12345 > textfile     >表示收

在源文件的计算机上，通过Netcat的客户端模式发送文件:

nc 192.168.1.4 12345 < textfile     <表示发



   然而，所有使用了Netcat的连接都不加密，包括文件传输咯。如果你在意用netcat传输的数据的隐私，可以考虑使用Cryptcat，Netcat的一个使用集成加密隧道的版本。Cryptcat和Netcat一样，使用相同的命令行语法，但是会使用两鱼加密法(twofish encryption，参考看雪一贴和维基百科)。还可以考虑在SSH(Secure Shell)连接内使用Netcat,这也是一种加密Netcat通信流量的方法。本小节意在介绍用Netcat传输文件的很基础的只是。要了解细节性的信息，尤其是参考加密和解密文件传输，参见第6章，用Netcat传输文件。



## 获取banner



抓取Banner属于一种穷举(enumeration)技术，目的是确定某个服务或应用程序的品牌(brand?)，版本，操作系统，或其他相关信息。如果你正在寻找某个版本的设备是否存在漏洞，这些信息就非常重要。

   抓取Banner的命令语法???The syntax of a banner grab is not unlike the standard Netcat command line.

   把Netcat运行在客户端模式，列出相应的(appropriate)主机名,最后列出相应服务的端口号。在某些情况，你可能没有输入信息(见图1.14)。在其他情况，你必须输入一个基于特定协议的有效命令。(见图1.15)



图1.14 SSH Banner Grabbing with Netcat

![image-20180924144014308](../../images/1.14.png?raw=true)

在图1.14,Netcat给我们两条信息：ip的主机名，运行在目标上的ssh服务的版本信息。



图1.15 HTTP Banner Grabing With Netcat

![image-20180924144014308](../../images/1.15.png?raw=true)



在图1.15,我们将Netcat置于客户端模式。我们的目标ip运行着一个Web服务。通过发出(issue)GET请求(先不论这个Bad Request的事实)，返回来的信息告诉我们目标的Web服务器软件和版本号。这还告诉我们Apache的这个版本可以运行在Windwos中。

更多详细的信息，请参考第4章，用Netcat抓取Banner。







## 端口和流量的重定向



Netcat可以被用于重定向端口和流量，这是一个有点阴暗(??darker shade)的操作。如果你要隐藏一次攻击的来源，这就很有用。我们的思路是，通过一个中间人(middle man)来运行，使得攻击看起来是从那个中间人发起的，而非真实的来源。下面的例子就很简单，但是会用到多个重定向。这个例子也需要你"获取(???own)"到中间人的权限，并且转发。这样的流量重定向被称为中继.

在源计算机：

```
nc <hostname of relay> 12345
```

windows和*nix都能测试成功



在中继：

```
nc -l -p 12345|nc <hostname of target> 54321
```



此简易的场景中，从源计算机(客户端模式)输入的会被发送到中继(服务端)。输出通过管道，作为第二个Netcat(客户端)的输入，最终(ultimately)从这里连接到目标计算机。第二点，Netcat的最初来源端口是12345,但是(yet)看起来攻击的来源端口是54321.这是port redirection(端口重定向)的一个简单例子。这种技术还可以用于隐藏Netcat在常用端口上的端口，以免被防火墙阻止。

   对于中继，有一个明显的限制。连接管道的数据(The piped data)是一个单向(one-way)连接。因此，源计算机不能接收到目标的响应。这个的解决方案是，在目标到源计算机之间，建立第二个中继(最好是通过另外一个中间人!)。

   要了解更多有关流量重定向的信息，请参考第5章，Netcat的黑暗，以及第7章，控制Netcat的流量。



## 其他用法



本节涵盖了Netcat的基本操作，唯一限制Netcat操作的是你的想象力。还有的潜力，或者说更高级的用法包含:

- 漏洞扫描(见第2章，Netcat和网络的渗透测试，和第3章，Netcat和应用的渗透测试)

- 通用网络的排错(troubleshooting)(见第8章，用Netcat来排错)

- 审计网络和设备(见第9章，用Netcat来审计)

- 备份文件，目录，甚至是驱动器

本书的其余部分会着重介绍这些和其他的用途。



## 链接

- [目录](../directory.md)