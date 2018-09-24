# Netcat的命令选项

在本节中，我们将讨论Netcat的两种不用的操作模式，以及(as well as)一些最常用的选项。



## 操作模式



Netcat主要有两种主要的操作模式，作为客户端，或者作为服务端。在帮助页面上，开始的两行(在版本信息下面)说明了对于每种模式的正确语法:

连接到某处: ```nc [-options] hostname port[s] [ports] ...```

监听入站流量: ```nc –l –p port [options] [hostname] [port] ```

*连接到某处* 表示客户端模式的语法。通常，你在你的电脑上把nc作为客户端来获取另一台电脑上的信息。*监听入站流量*表示服务端模式的语法。注意使用`-l`让nc处于监听模式。此时，你将 Netcat 设置为了监听传入的连接。Netcat并不在意你使用是什么模式，但是不管你在什么模式下，都会按照你的命令去执行。



## 常用命令选项



在本节中，我们将会讨论，你可能会看到的最常用的用于nc的选项。这些选项在windows和Linux下都是一样的，除了少数例外(会预先在文中说明)。你可以在这本书中单独的章节参考(refer)Netcat更高级的用法，来完成(accomplish)你的任务。请记住，`-l`选项将会确定Netcat的操作模式。nc -l这个命令会将nc置于服务端/监听模式，而nc这个命令会将自身置于客户端。(command nc –l will put Netcat into server or listening mode, and nc by itself will run Netcat in client mode.尼玛这句话，看了三遍才看懂)



第一个可用的选项，-c，命令netcat在标准输入(stdin)的结尾处(EOF)关闭。此选项仅用于Linux的各种版本。

接下来是，-d，使Netcat从控制界面分开，在后台运行。如果你不希望运行Netcat的时候有个控制台窗口，这就特别有用(特别是如果有人可能会窥屏)(哈哈哈，真周到)。注意这个选项仅适用于Windwos版本。

   Netcat最强大的选项，毫无悬念的，`-e prog`。这个选项，只能在服务端模式使用，可以指定一个程序，当有客户端来连接的时候，让Netcat来执行。注意以下命令：

`nc -l -p 12345 -e cmd.exe`(windows)

`nc -l -p 12345 -e /bin/bash`(Linux)



   这两个命令是在做同样的事情，但是是在不同的系统上。第一个命令是在本地的12345端口运行netcat作为服务端模式，并会在有客户端连接的时候执行cmd.exe(windows的shell)。第二条命令也是(precisely)一样的，不一样的就是在Linux上执行了一个bash shell。为了测试这个选项，我们来启动Netcat的服务端模式。

![image-20180924145112283](../../images/1.7.png?raw=true)

另开一个窗口，然后让netcat作为客户端。



![image-20180924145136508](../../images/1.8.png?raw=true)



当你敲下回车键后，你就会看到Microsoft的banner信息和一个新的命令提示符。



这可能看起来有些震惊，但是这就是毫无疑问：你的确是通过Netcat来运行这个命令提示符。如果你是在一个网络里运行Netcat，而不是在同一台计算机上，你就直接有这台服务器的shell访问权限了。在命令提示符中输入`exit`，你会看到在第一个窗口的netcat的服务端关闭了。

在Linux上开启服务端的netcat，在终端输入`nc -l -p 12345 -e /bin/bash`。

现在，我们在Windows上打开命令提示符，然后把nc运行为客户端模式(见图1.9)。

Figure 1. Starting Netcat in Client Mode (Windows to Linux)

![image-20180924145359463](../../images/1.9.png?raw=true)





   不像先前连接到windows那样，Linux的bash shell不会返回字符到你的屏幕上。所以尝试用`uname -a`来显示系统信息。此时，这就证明(confirms)我们连接到了一台Linux机器,因为它接受了Linux的命令。另外，它还返回了相关系统信息:内核名称和版本，处理器(processor)信息，等等。



警告

   这还不能足够的突出出netcat的`-e`选项是多么强大。通过允许一个客户端来连入netcat，你可以给予他shell的访问权限。还有，这不需要用户认证或者是与此相关联的接入认证过程。重要的是，虽然你可能有正当的理由这样做，但无疑还有很多违法的用途。第五章：netcat的黑暗，将更详细的探讨这个选项。





`-g`和`-G`，可以让你配置netcat使用源站路由(source routing).使用源路由的方式，在一个网络中，发送方(the sender)指定路由让数据包通过。由于(since)大多数路由都阻止(block)源路由的数据包，所以这个选项多多少少有些过时(obsolete)。



`-h`,显示帮助信息，你懂的。



`-i`，设置延迟间隔(delay interval)(在发送数据和端口扫描之间)。在扫描端口时有速率限制非常有用。



`-l`，将netcat置于监听模式，或者像我们这章中说的，服务端模式。一般情况下(normally)，Netcat是一种一次性使用(single-use)的程序。或者说，一旦连接被关闭，Netcat也就关闭，不能再用了(no longer available)。然而，-L，在原来的连接关闭后，会用先前的命令打开nc：



`nc -l -p 12345 -e cmd.exe -L`



连接到这个实例(instance)的netcat会打开一个命令行到客户端。退出了命令行会关闭这个连接，但是-L会再次打开。



#### NOTE

   `-L`持久选项仅在Windows版本的netcat可用.(Linux版本的 `-L, --tunnel=ADDRESS:PORT  forward local port to remote address`)。不过，在Linux上用一些脚本程序就能克服(overcome)这个限制.为了解决某些问题，GNU版本的Netcat用-L来运行隧道链接(tunneling)。-L可以将本地端口转发(forward)到远程地址。





`-n`，使用只有数字(numeric-only)的ip地址，而且不做反向查找(reverse lookup)。如果你不加上`-n`，netcat就会做域名解析。但是(假设你加上了`-v` 用于显示更多的信息)，netcat会显示正向和反向的名称，还会用地址查找指定主机。我们来看下面这个例子。

![image-20180924145813986](../../images/1.10.png?raw=true)



加上了`-n`，netcat会接受一个数字的ip地址，并且不做反向查询(rDNS)。比较一下下面这个没有`-n`的。

![image-20180924145839394](../../images/1.11.png?raw=true)

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



#### 警告

单">" 重定向是要重定向输出到一个指定(specified)位置或者文件。但是要记住(keep in mind)，如果你使用的是相同的文件名，单">"重定向符号会覆盖(overwrite)原来的文件。如果你想要保留你的源文件，更安全的选择是使用双">"重定向来追加内容。如果指定文件不存在，双">"重定向也会自己创建一个。



"小于(less than)"重定向会冲将输入重定向:

nc -l -p 12345 < dumpfile



   当客户端连接到这台服务器，Netcat会发送dumpfile给客户端。或者说是，Netcat的客户端会从服务器上拉取(pull，ps:或者说是下载)文件。



   还有一个有用的重定向工具，管道(|),可以让一个命令的输出，作为第二个命令的输入(等等)。这些过程共同构成(constitute)了"管道行(pipeline)"。一些常用命令也可以和Netcat一起协作(in concert with)：cat(发送文件)，echo，tar(压缩并发送一个目录)。你甚至可以将Netcat运行两次，以建立一个中继(relay)。这些都可以。



## 链接

- [目录](../directory.md)