# Netcat的命令选项

   在本节中，我们将讨论Netcat的两种不用的操作模式，以及(as well as)一些最常用的选项。



## 操作模式



   Netcat有两种主要的操作模式，要么作为客户端，要么作为服务端。在帮助页面上，开始的两行(在版本信息下面)说明了对于每种模式的正确语法(syntax):

连接到某处: ```nc [-options] hostname port[s] [ports] ...```

监听入口流量(inbound): ```nc –l –p port [options] [hostname] [port] ```

   连接到某处表示客户端模式的语法。通常(Typically)，你在你的电脑上把nc作为客户端来获取(obtain)另一台电脑上的信息。监听入口流量表示服务端模式的语法。注意使用-l让nc处于监听模式。此时，你将nc设置为了监听传入的连接。Netcat并不在意你使用是什么模式，但是不管你在什么模式下，都会按照你的命令去执行。



## 常用命令选项



   在本节中，我们将会讨论，你可能会看到的最常用的用于nc的选项。这些选项在windows和Linux下都是一样的，除了少数例外(会预先在文中说明)。你可以在这本书中单独的章节参考(refer)Netcat更高级的用法，来完成(accomplish)你的任务。请记住，-l选项将会确定Netcat的操作模式。nc -l这个命令会将nc置于服务端/监听模式，而nc这个命令会将自身置于客户端。(command nc –l will put Netcat into server or listening mode, and nc by itself will run Netcat in client mode.尼玛这句话，看了三遍才看懂)



   第一个可用的选项，-c，命令netcat在标准输入(stdin)的结尾处(EOF)关闭。此选项仅用于Linux的各种版本。

   接下来是，-d，使Netcat从控制界面分开，在后台运行。如果你不希望运行Netcat的时候有个控制台窗口，这就特别有用(特别是如果有人可能会窥屏)(哈哈哈，真周到)。注意这个选项仅适用于Windwos版本。

   Netcat最强大(powerful)的选项，毫无悬念(undoubtedly)，-e prog。这个选项，只能在服务端模式使用，可以指定一个程序，当有客户端来连接的时候，让Netcat来执行。注意以下命令：

`nc -l -p 12345 -e cmd.exe`(windows)

`nc -l -p 12345 -e /bin/bash`(Linux)



   这两个命令是在做同样的事情，但是是在不同的系统上。第一个命令是在本地的12345端口运行netcat作为服务端模式，并会在有客户端连接的时候执行cmd.exe(windows的shell)。第二条命令也是(precisely)一样的，不一样的就是在Linux上执行了一个bash shell。为了测试这个选项，我们来启动Netcat的服务端模式。



c:\netcat>nc -l -p 12345 -e cmd.exe





另开一个窗口，然后让netcat作为客户端。



c:\netcat>nc localhost 12345







当你敲下(hit) enter，你就会看到(are greeted with)Microsoft的banner信息和一个新的命令提示符。



Microsoft Windows XP [Version 5.1.2600]

(C) Copyright 1985-2001 Microsoft Corp.

c:\netcat>_







   这可能看起来有些震惊(underwhelming)，但是这就是毫无疑问(make no mistake about it)：你的确是通过Netcat来运行这个命令提示符。如果你是在一个网络里运行Netcat，而不是在同一台计算机上，你就直接有这台服务器的shell访问权限了。在命令提示符中输入exit，你会看到在第一个窗口的netcat的服务端关闭了。



   在Linux上开启服务端的netcat，在终端(box)输入nc -l -p 12345 -e /bin/bash.现在，我们在Windows上打开命令提示符，然后把nc运行为客户端模式。







   不像(unlike)先前连接到windows那样，Linux的bash shell不会返回字符到你的屏幕上。所以要用uname -a来显示系统信息。此时，这就证明(confirms)我们连接到了一台Linux机器(box),因为它接受了Linux的命令。另外(furthermore)，它返回了相关(relevant)系统信息:内核(kernel)名称和版本，处理器(processor)信息，等等(so forth)。



C:\netcat>nc -v 192.168.1.10 12345

192.168.1.10: inverse host lookup failed:h_errno 11004: NO_DATA

(UNKNOWN) [192.168.1.10] 12345(?) open

uname -a

Linux bt 2.6.21.5 #2 SMP Sat Aug 25 19:01:21 GMT 2007 i686 AMD Turion(tm) 64 Mobile Technology ML-34 AuthenticAMD GNU/Linux





warning

   这还不能足够的突出(stressed)出netcat的-e选项是多么强大。通过允许一个客户端来连入netcat，你可以给予他shell的访问权限。还有，这不需要用户认证或者是与此相关联的接入认证过程。重要的是，虽然你可能有正当的理由这样做，但无疑还有很多违法的用途。第五章：netcat的黑暗，将更详细的探讨这个选项。





`-g`和`-G`，可以让你配置netcat使用源站路由(source routing).使用源路由的方式，在一个网络中，发送方(the sender)指定路由让数据包通过。由于(since)大多数路由都阻止(block)源路由的数据包，所以这个选项多多少少有些过时(obsolete)。



`-h`,显示帮助信息，你懂的。



`-i`，设置延迟间隔(delay interval)(在发送数据和端口扫描之间)。在扫描端口时有速率限制非常有用。



`-l`，将netcat置于监听模式，或者像我们这章中说的，服务端模式。一般情况下(normally)，Netcat是一种一次性使用(single-use)的程序。或者说，一旦连接被关闭，Netcat也就关闭，不能再用了(no longer available)。然而，-L，在原来的连接关闭后，会用先前的命令打开nc：



`nc -l -p 12345 -e cmd.exe -L`



连接到这个实例(instance)的netcat会打开一个命令行到客户端。退出了命令行会关闭这个连接，但是-L会再次打开。



#### NOTE

   `-L`持久选项仅在Windows版本的netcat可用.(Linux版本的 `-L, --tunnel=ADDRESS:PORT  forward local port to remote address`)。不过，在Linux上用一些脚本程序就能克服(overcome)这个限制.为了解决某些问题，GNU版本的Netcat用-L来运行隧道链接(tunneling)。-L可以将本地端口转发(forward)到远程地址。





`-n`，使用只有数字(numeric-only)的ip地址，而且不做反向查找(reverse lookup)。如果你不加上`-n`，netcat就会做域名解析。但是(假设你加上了`-v` 用于显示更多的信息)，netcat会显示正向和反向的名称，还会用地址查找指定主机。我们来看下面这个例子。

```
C:\netcat>nc -v -n 64.233.169.103 80

(UNKNOWN) [64.233.169.103] 80 (?) open
```





加上了`-n`，netcat会接受一个数字的ip地址，并且不做反向查询(rDNS)。比较一下下面这个没有`-n`的。

```
C:\netcat>nc -v 64.233.169.103 80

yo-in-f103.google.com [64.233.169.103] 80 (http) open
```





没有`-n`，netcat就会执行反向查询(rDNS)，然后告诉我们，指定的ip属于google。当做一个正向或者反向的dns查询Netcat显示警告，这并非罕见。这些警告往往涉及(relate)不匹配的dns记录的可能。



`-o filename`，可以将netcat的流量的16进制dump存到一个文件里。



`-p port`，指定netcat监听本机某个端口(服务端):

`nc -l -p 12345`



本例中，netcat作为服务端运行，并且在12345端口监听传入连接。



在客户端模式下，Netcat还可以扫描端口。你可以指定多个端口(以逗号分割)，范围(全部也可以)，甚至是常见端口的名称。当在客户端模式下指定了端口，可以不用-p选项。只需要列出主机名，后面跟着端口号或者是端口范围。如果你是指定了一个范围的端口，Netcat会在范围里的高端口(top)的向低端口(bottom)扫描。因此，如果你让Netcat扫描20-30端口，它会从30开始扫描到20。



`-r`，随机扫描端口。如果你用netcat来扫描端口，-r会让netcat以相对随机的方式(manner)从标准的大到小来扫描。此外，-r也可以用在当服务端模式下时随机打开你本地的源端口。



`-s`，改变数据包的源地址，可以用于源地址的欺骗(spoofing)。这又是一个过时的命令，由于智能路由器会丢弃这些数据包，所以其有效性(usefulness)已经随着时间的推移(over time)而退化(degraded)。还有一个明显的限制就是，回复会发送到伪造(spoofed)的地址，而不是真实的地址。



`-T,--telnet`，设置netcat来回应telnet的协商(negotiation)。换句话说，Netcat可以设置为一个简单的Telnet服务器。注意下面的命令:

nc -l -p 12345 -e cmd.exe -T      这里有错哦 原来是-t -t是tcp



需要注意的是，上面的(previous)命令是运行在windows的netcat服务端。如果你是运行netcat在linux的服务器上，你需要把cmd.exe换成/bin/bash。

使用Netcat，telnet，或者其他的像PUTTY这类的客户端链接到服务器，通过telnet你就会拥有shell的权限。



#### warning

你要记住(Recall)，Netcat的通信没有加密。而且，telnet是明文协议(clear-text protocol).同样(likewise)，在这条链路上的所有通信都可以被监听。



`-u`，使用UDP代替(rather than)默认的TCP。因为UDP是不需要建立连接(connectionless)的协议，因此建议你使用超时(-w)和此选项搭配使用。



`-v`，和很多常见的命令行工具一样，调节唠叨(verbosity)程度，或者说是显示给用户的信息量。没有这个选项，netcat一样可以完美运行，但是netcat将会静默运行，并且只有当发生了错误时，才会显示信息。同样，和其他大多数程序都一样，你可以使用多个v来增加显示信息的等级(`-v` `-v`或者`-vv`都可以)。



#### TIP

强烈建议(highly recommended)每次用netcat的时候都加上`-v`，以便于直观的看到它正在做的事。很多用户还可以结合`-v`和`-w`一起使用。



`-V`，在GNU版本下，显示版本信息，然后退出。



`-w secs`，设置网络闲置(network inactivity,或者idle)的超时。用于本应断开连接，但你的服务器没有自动断开，还可以加快你的请求速度。一般时间为3秒。

`-z`，不显示输入/输出，用于端口扫描。当使用-z时，Netcat不会在TCP连接中发送数据，而是在UDP连接中发送有限的数据。(ps:嘛。。。我也不知道这什么意思。。。所以要慢慢看咯)



#### TIP

Netcat的选项(switches)可以单独(individually)使用，也可以连用。比如说，你要启动Netcat的服务端模式，在12345端口监听，并且要显示详细信息(verbose)。这样的命令是`nc -v -l -p 12345`。然而，你还可以把多个(multiple)选项放在一起，就像这样`nc -vlp 12345`。(ps:其实Linux本身也支持的。。。



## 重定向工具



最后，还有一些基本的UNIX重定向与netcat兼容。

其中最有帮助的是>,>>,<和管道(|).



   单"大于(greater than)"重定向符号会直接输出:

nc -l -p 12345 > dumpfile

   这个命令会将所有收到的信息重定向(redirect...into...)到一个文件。收到的信息可能是对方输入的文字，或者甚至是发送一个文件。换句话说，不管是向服务端发送了什么东西，都会被重定向到dumpfile这个文件里。





   双"大于"重定向符号会将重定向输出，但是是追加(append rather than replace)到文件：

nc -l -p 12345 >> dumpfile



#### warning

单">" 重定向是要重定向输出到一个指定(specified)位置或者文件。但是要记住(keep in mind)，如果你使用的是相同的文件名，单">"重定向符号会覆盖(overwrite)原来的文件。如果你想要保留你的源文件，更安全的选择是使用双">"重定向来追加内容。如果指定文件不存在，双">"重定向也会自己创建一个。



"小于(less than)"重定向会冲将输入重定向:

nc -l -p 12345 < dumpfile



   当客户端连接到这台服务器，Netcat会发送dumpfile给客户端。或者说是，Netcat的客户端会从服务器上拉取(pull，ps:或者说是下载)文件。



   还有一个有用的重定向工具，管道(|),可以让一个命令的输出，作为第二个命令的输入(等等)。这些过程共同构成(constitute)了"管道行(pipeline)"。一些常用命令也可以和Netcat一起协作(in concert with)：cat(发送文件)，echo，tar(压缩并发送一个目录)。你甚至可以将Netcat运行两次，以建立一个中继(relay)。这些都可以。



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



图1.13

```
C:\netcat>nc -v -n -r -w3 -z 192.168.1.4 21-25

(UNKNOWN) [192.168.1.4] 25 (?): TIMEOUT

(UNKNOWN) [192.168.1.4] 24 (?): TIMEOUT

(UNKNOWN) [192.168.1.4] 22 (?): TIMEOUT

(UNKNOWN) [192.168.1.4] 21 (?)open

(UNKNOWN) [192.168.1.4] 23 (?): TIMEOUT
```





在图1.13,Netcat运行在客户端模式在，并且含有以下选项:

显示详细信息，无DNS查询，随机端口顺序扫描，网络闲置超时3秒，关闭输入\输出。主机是192.168.1.4,端口扫描是21-25.Netcat返回了的信息是21端口开启，这很可能是用于FTP。有关端口扫描和Netcat的更多信息，请参见第10章，用Netcat来审计(audit)。



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

```
C:\netcat>nc -v 192.168.1.5 22

RAVENCLAW [192.168.1.5] 22 (?) open

SSH-2.0-OpenSSH_3.8.1p1

_
```







ps:

```
root@kali:~# nc -v 192.168.1.7 ssh

192.168.1.7 22 (ssh) open

SSH-2.0-OpenSSH_6.7p1 Raspbian-5
```



在图1.14,Netcat给我们两条信息：ip的主机名，运行在目标上的ssh服务的版本信息。



图1.15 HTTP Banner Grabing With Netcat

```
C:\netcat>nc -v 192.168.1.5 80

RAVENCLAW [192.168.1.5] 80 (http) open

GET / HTTP/1.1



HTTP/1.1 400 Bad Request

Date: Mon, 17 Mar 2008 00:59:36 GMT

Server: Apache/2.2.8 (Win32)

Content-Length:226

Connection:close

Content-Type: text/html; charset=iso-8859-1



....接着是网页内容....
```





   在图1.15,我们将Netcat置于客户端模式。我们的目标ip运行着一个Web服务。通过发出(issue)GET请求(先不论这个Bad Request的事实)，返回来的信息告诉我们目标的Web服务器软件和版本号。这还告诉我们Apache的这个版本可以运行在Windwos中。

更多详细的信息，请参考第4章，用Netcat抓取Banner。







## 端口和流量的重定向



Netcat可以被用于重定向端口和流量，这是一个有点阴暗(??darker shade)的操作。如果你要隐藏一次攻击的来源，这就很有用。我们的思路是，通过一个中间人(middle man)来运行，使得攻击看起来是从那个中间人发起的，而非真实的来源。下面的例子就很简单，但是会用到多个重定向。这个例子也需要你"获取(???own)"到中间人的权限，并且转发。这样的流量重定向被称为中继(relay).

在源计算机:

nc <hostname of relay> 12345         windows和*nix都能测试成功

在中继:

nc -l -p 12345|nc <hostname of target> 54321



   此简易方案(scenario)中，从源计算机(客户端模式)输入的会被发送到中继(服务端)。输出通过管道，作为第二个Netcat(客户端)的输入，最终(ultimately)从这里连接到目标计算机。第二点，Netcat的最初来源端口是12345,但是(yet)看起来攻击的来源端口是54321.这是port redirection(端口重定向)的一个简单例子。这种技术还可以用于隐藏Netcat在常用端口上的端口，以免被防火墙阻止。

   对于中继，有一个明显的限制。连接管道的数据(The piped data)是一个单向(one-way)连接。因此，源计算机不能接收到目标的响应。这个的解决方案是，在目标到源计算机之间，建立第二个中继(最好是通过另外一个中间人!)。

   要了解更多有关流量重定向的信息，请参考第5章，Netcat的黑暗，以及第7章，控制Netcat的流量。



## 其他用法



本节涵盖了Netcat的基本操作，唯一限制Netcat的操作是你的想象力。还有的潜力，或者说更高级的用法包含:

■ 漏洞扫描(见第2章，Netcat和网络的渗透测试，和第3章，Netcat和应用的渗透测试)

■ 通用网络的排错(troubleshooting)(见第8章，用Netcat来排错)

■ 审计网络和设备(见第9章，用Netcat来审计)

■ 备份文件，目录，甚至是驱动器

本书的其余部分就特地(dedicated to)介绍这些，以及其他的什么用途。

