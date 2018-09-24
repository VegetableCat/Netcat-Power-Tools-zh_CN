# **端口扫描和服务标识**

在渗透测试过程中，端口扫描和服务识别起到了很大的作用。如果你不能确定一个系统运行的服务，或者服务器版本，这就很难确定与它相关的任何潜在漏洞的信息。在本小节，我会讨论如何使用 Netcat 的端口扫描，识别Web服务器的版本信息，以及识别可疑的或者未知的服务。



## 作为端口扫描工具

在大多数情况下，如今 Netcat已经不是最强大的端口扫描工具，但是它仍旧可以处理这个任务。Netcat的所有选项，包含端口扫描，默认使用TCP协议。下表显示了Netcat的端口扫描相关选项。

 

| **Netcat选项** | **描述**                              |
| -------------- | ------------------------------------- |
| –i secs        | 扫描间隔时间                          |
| –r             | 使来源和目的端口随机                  |
| –u             | UDP模式                               |
| –v             | 显示详细信息(用 –vv 显示两倍)         |
| –z             | 关闭输入/输出(不会进行一个完整的连接) |
| Target         | 扫描的目标IP/主机                     |
| Port-range     | 扫描的端口号或者范围                  |



​        端口扫描的示例如下图。本例中，Netcat将尝试连接到1-1024的TCP端口，并显示结果。以下命令就是扫描TCP端口的：









 

```
nc -v -z target port-range
```









![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/1.JPG)



​        如上图所示，Netcat发现，在目标系统上，开放了多个TCP端口。另外(Additionally)，如果要对目标系统进行UDP端口扫描，你需要将Netcat放置在UDP模式下：









 

```
nc -v –u -z target port-range
```





​        再者(Furthermore)，如果你发现你自己被自动阻断技术所屏蔽，尝试使用-i 选项，来调整Netcat的延迟时间间隔。那些触发(trigger)屏蔽机制的是有特定的特征，定时的阀值(timed threshold)，或者是连续的(sequential)扫描端口。一种方法来确定阀值，就是调整扫描每个端口的间隙。还有，用 -r 选项来随机扫描范围的端口。

## 抓取Banner

Netcat的一个有用的功能就是，在试图连接到服务时，通过从服务的Banner触发(trigger)一个响应，来确定版本信息。抓取Banner可以被应用到很多不同的服务上。在这节，我会展示给你看，如何发出(issue)几个命令，来识别一个Web服务器的版本信息。

​        在下面的例子中，我们会发送一个HTTP HEAD请求，来确定Web服务器的版本。HEAD方法即是，允许客户端请求HTTP header头部信息。从HEAD请求输出的信息，会帮助我们确定有关服务器的重要信息，包括正在运行的服务器类型和版本。要执行(perform)一个HEAD请求，我们需要让Netcat和目标Web服务器建立连接，命令如下：









 

```
nc -v www.microsoft.com 80
```





​        这就简单的建立了一个到Web服务器的TCP连接。一旦连接建立，你需要发送下面的信息到Netcat的窗口：









 

```
HEAD / HTTP/1.0
```





​        当你敲击enter两次后，我们会从Web服务器，得到如下的响应(http头信息)。

​        结果如图所示，[www.microsoft.com](http://www.microsoft.com)  竟然(surprisingly)是运行在一个Microsoft-IIS/7.0的Web服务器上，使用的是ASP.NET的Web应用框架。

![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/a28ce482-19e5-4d1e-a144-1f44f25bd783.JPG)



### 使用脚本让Netcat识别多个Web服务器的Banner

在渗透测试中，接触到大量的Web应用程序是很常见的。如果你没有一个自动化收集信息的方案，要去确定这样大量的应用的类型和Web服务器的版本就会是一项让人生畏的(daunting)任务。使用获取Banner的命令，我们将之加入到一个脚本，这样就能进行自动化抓取Banner。



​        下面就是一个Linux的shell脚本的样本，用来获取Web服务器的banner：













 

```
for i in `cat hostlist.txt `;do
nc -q 2 -v $i 80 < request.txt
done
```





​        这种简单的循环会读取hostlist.txt文件，里面包含了IP地址或者目标的域名。然后使用Netcat命令和使用管道发送HEAD请求，到已经建立连接的Web服务器。在示例中，-q 2选项是需要注意的。如果Web服务器不是一个真正的Web服务器，而是一个Netcat监听端，你没使用-q选项，你的连接可能永远不会中断。-q 2 确保连接在请求后的2秒会被中断。在request.txt文件中包含着HEAD 请求，**\*HEAD/HTTP/1.0/n/n***.

Banner抓取不仅仅适用于识别一个Web 服务器的类型和版本的时候。Netcat还能用于获取服务的版本信息，比如文件传输协议FTP,Telnet,SSH,POP,IMAP,以及SMTP。

（在第四章查看Banner抓取的更多信息。）







### 服务识别 



Netcat还可以被用在 帮助识别在系统上运行着的一个未知的或是可疑的服务。例如说（Say for instance），你做了个扫描，发现TCP/65522是开放的，然而你的扫描器报告说这个服务是未知的。我们可以用Netcat发起一个简单的连接，到那个端口，尝试得到服务器的响应，这就会帮助我们识别未知的服务。我们的目标是得到所有那个服务可以提供的信息。下图展示了，Netcat连接到目标系统的65522端口的详细连接信息。

![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/7b559958-30de-4611-a763-8a7cc6d18a39.png)



如你所见，未知的服务被确定为是一个运行在65522端口的SSH服务端。