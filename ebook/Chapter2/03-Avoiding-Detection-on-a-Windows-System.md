## **测试出口防火墙**

在本节中，我们会讨论，如何测试出口防火墙的规则，来验证出站端口的过滤规则是否到位(in place)，并且正常工作。而重要的是，要验证防火墙上的控件(controls)是否正确过滤入站数据包，尤其是系统只关注入站数据包的过滤，而不测试出站数据的安全性，这又称为 *出口过滤*(egress filtering，详见<https://en.wikipedia.org/wiki/Egress_filtering>)。



​        为了方便出口防火墙的测试，我们将会用到两台计算机，一台被置于防火墙内(system A)，另一台被放置在防火墙的边缘外(system B).这次测试的目的是确定，哪些端口是被允许用于和防止在防火墙外的计算机通信的。一旦两个系统都配置好，我们会从系统A扫描系统B，来确定能够出口的TCP/UDP端口。

![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/1799046747.png)





### 

### **放置在防火墙外 -** **系统B**



系统B的作用是，监听所有的(all and any)端口，如果收到了入站请求连接，就发送一个响应数据包，返回到防火墙里面的(internal)系统。为了确定我们能够连接的TCP/UDP端口，我们会配置我们的外部(external)计算机，监听全部65535个TCP/UDP端口。



​        但是，用单独一个NC客户端，监听131070个端口是不现实的。取而代之，我们可以配置netcat监听两个端口，一个是tcp连接，另一个是udp连接。然后，我们用我们的包过滤设备，本质上，转发所有的TCP连接到我们的TCP的netcat监听端，以及所有的UDP流量到UDP的netcat监听端。[ 让nc接受所有端口访问的解决方案 ]



​        在本例中，在系统B上运行着Gentoo Linux，配置好了iptables，用于实现端口转发功能。TCP的netcat监听端 设置为 接受TCP/1234的连接，UDP的监听端会接受UDP/1234的连接。



**NOTE**



------

要了解在Gentoo Linux 平台上安装和内核配置、运行iptables的更多信息，参考以下链接：

<http://gentoo-wiki.com/HOWTO_Iptables_for_newbies>。

要了解关于Iptables的更多信息，你可以访问 <http://www.netfilter.org/。> 

------



​        当系统B 配置好iptables后，我们需要添加一下规则，来重定向入站连接到appropriate的Netcat监听端。要实现(implement)这个功能，我们会用到以下的iptables命令：[ 使用iptables技术，重定向所有端口的访问到NC监控的端口 ]











 

```
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 1:65535 -j REDIRECT --to-port 1234
iptables -t nat -A PREROUTING -i eth0 -p udp --dport 1:65535 -j REDIRECT --to-port 1234
```





​       为了验证iptables已经加载此规则，输入以下命令：











 

```
iptables –L –n –t nat
```





![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/1655874397.png)





​        iptables配置好了，我们就要开始这两个netcat监听端，在两个分开终端中输入如下命令：











 

```
nc –l –p 1234
nc –u –l –p 1234
```





​        此时，系统B也配置好，准备要接收从内部网络(system A)发来的，所有的65535的TCP端口，以及所有的65535 UDP端口的连接。



At this point, System B is set up and ready to accept connections on all 65,535 TCP ports, and all 65,535 UDP ports can set up the system on the internal network

(System A). ？？？

###  

### **放置在防火墙内** **-** **系统A**

系统A的功能是发起一个对系统B的端口扫描。在开始端口扫描之前，我们需要确认系统A的确位于防火墙内了。然后，你可以用。？？？为了演示扫描系统B的端口，我们会将netcat放在系统A上，执行一个全面的TCP端口扫描。Then you can use any system that is capable of doing a port scan to

function as System A

​        端口扫描的结果如下图。扫描发现了3个端口是被允许从系统A到系统B的。这意味着，出口防火墙测试是验证了的，防火墙是基于出口过滤的机制，阻断所有的出口TCP连接，但排除了TCP的443,80,53端口。





![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/1678218648.png)



**TIP**

------

如果你决定用不同的端口扫描器，比如Nmap，请确认使用的是SYN扫描，这将保证，攻击端发送的不是完整的TCP连接。如果执行的是完整TCP扫描，Netcat监听端会在第一次完整连接后退出(关闭连接)。



------







# **在Windows系统上避免检测**

在本节，我们会讨论，避免被Windows XP上的防火墙发现的方法，以及避免杀毒软件的检测。我们还会讨论多种方法来隐藏(obscure)我们的Netcat进程。





## **绕过Windows XP/Windows 2003 Server防火墙**

本节的目的是绕过(evade) Windows 防火墙的入站阻断技术。Windows 防火墙 / 网络连接共享 ( Internet Connection Sharing , ICS) 服务，包含Windows XP SP2，提供了一个用于基本实现入站数据包过滤的防火墙。

​        默认情况下，XP防火墙也会检测，阻止尝试开放 TCP/IP 堆栈(sockets) 的程序，监听入站连接。如果不了解(unaware of)这种Windows 防火墙的特性，对于渗透测试人员来说，当我们创建了一个监听入站连接的后门，这就是一个困难了。如下图所示，当我用Netcat尝试要创建一个TCP监听端时，Windows防火墙阻止了我的程序，并且触发(triggered)了一个Windows安全警告。

![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/390111004.png)



​        为了更好的理解我们接下来要做的事，让我们来看以下场景(scenario)。



### **示例**

你已经拿到(compromised???)了一台system权限的Windows系统，然而你想要安装一个后门，这样你就可以长期访问这台系统。你注意到了一个问题，那就是Windows的防火墙正在运行，可能会向管理员或者用户警告你的活动。因为这个exp可以让你访问系统，而且在开始的阶段(in the first place)不会被发现，我们想要保持我们的访问权限，避免防火墙的检测，并且能够随时回到这个系统。

​        要完成这点，我们需要修改Windows防火墙的规则，使其信任我们的程序。

### **用Netsh命令创建防火墙的例外**

*Netsh* 是Windows命令行工具，用于配置Windows防火墙。在上面的例子中，我们想用Netcat创建一个后门，这样我们就能长期拿到命令行的shell。使用几条netsh命令，在Windows防火墙里加入例外，就能保证我们的程序可以被允许接受入站连接。一个防火墙的例外，将允许程序或者协议通过网络通信(communicate over the network)。本小节的目的是，添加一个入口(port???)，让netcat能够在Windows的防火墙例外列表中运行。首先，我们需要确定Windows防火墙的状态，以及它是否设置为允许例外。





**确定防火墙的状态**

为了确定Windows防火墙的状态，我们将使用一个*netsh*命令。

以下命令会显示Windows防火墙是否生效(function)，以及是否设置为允许入站连接端口的例外。











 

```
netsh firewall show opmode
```





​        如下图所示，我们可以看到这条命令的输出。我们要特别注意查看profile下的设置，说明(current，现在)，因为这就是已经生效了的Windows 防火墙的profile。



![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/1049940485.png)

​        注意看Domain profile configuration设置，我们要注意Operation mode 和Exception mode 设置。

​        在例子中，配置是：









 

```
Operation mode = Enable
Exception mode = Enable
```





​        Operation mode设置是Enable，这意味着防火墙正在开启状态，并且阻断入站连接。Exception mode设置是Enable，这就告诉我们，Windows防火墙设置为允许例外。这个设置是很重要的，因为这就允许我们为Netcat监听端口添加一个例外。

​        如果netsh命令报告Exception mode是Disabled，意味防火墙不允许任何例外。这种情况下，我们需要配置防火墙，使之允许例外，使用以下命令：













 

```
Netsh firewall set opmode mode = enable exceptions = enable profile = all
```







​        当我们验证防火墙是设置为允许例外后，我们就能为Netcat监听创建一个例外。在下面的例子中，我们会添加一个例外到Windows防火墙，以允许Netcat监听端接受入站连接，并且不会触发Windows告警。我们的Netcat监听端会监听在TCP/1234.使用以下命令，我们添加TCP/1234到例外列表，并且设置例外的名称。













 

```
netsh firewall add portopening TCP 1234 “Windows Firewall Reporting Agent” enable all
```





​        一旦命令成功完成，你定义的端口信息就会添加到防火墙的例外列表，包括使用的协议、端口号、定义的名称。你可以验证规则是否已经添加到防火墙，使用以下命令：













 

```
netsh firewall show port opening
```





![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/1812928769.png)


​        这时，我们就可以把Netcat的监听端运行在TCP/1234，并且避免被Windows防火墙阻止，以及弹出Windows警告信息。



## **绕过杀毒软件的检测**

由于在此<http://www.vulnwatch.org/netcat/>  网站上的注明：

​        “12/15/05 - 赛门铁克(Symantec) 现在已经检测Netcat为黑客工具(Hacktool)。而诺顿(Norton)杀毒软件的默认操作是删除程序，所以要小心，别被删除了。Netcat不是(ni more)一个攻击工具，也不是(than)文件传输工具或是远程访问工具。Netcat没有利用(exploit)任何漏洞(vulnerability)。”

​        在撰写本文的时候，赛门铁克已经从病毒定义中移除了Netcat，并且不再会报告Netcat是一个黑客工具。为了避免以后的杀毒软件厂商，将Netcat检测为一个恶意(malicious)工具，我仍然建议编译一份修改后的Netcat。

​        有两种办法可以避免杀毒软件的检测。你可以修改源代码，并重新编译程序，或者你可以使用一个调试工具(debugger)，定位杀毒软件的特征码(signature)，修改二进制文件。这种方法主要用于，我们没有源代码的情况下。因为Netcat的源代码是公开的，我们将会修改Netcat的源代码，并重新编译程序。

​        

**重新编译Netcat**

在本节中，我们将讨论重新编译Netcat的Windows版本，这个版本是由Chris Wysopal移植到Windows的。你可以在<http://www.vulnwatch.org/netcat/> 获取这个版本，包括源码。

​        我们拥有Netcat的源代码后，我们还需要一个Windows的编译工具(compiler) 来建立(build)该程序。我们将使用Microsoft Visual Studio，这里面包含了一个命令行的编译器，*cl.exe*。这个编译器将会使用到*makefile*文件以及Netcat源代码。使用重新编译的方法，我们会修改Netcat的源代码文件。





![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/200556959.png)



​        添加一些注释到源文件中就已经足够了，所以，当程序被重新编译后，程序的特征码就和原来的程序不一样了。

​        makefile文件里包含了Netcat源码编译时所需的选项，也包含有编译器的变量(cc)，这需要被定义为你正在使用的编译器。编译器变量已经设置为*cl*了，因此，如果你是使用Visual Studio，你不需要修改其他的东西了。在命令行窗口输入make，然后一个新的、有着不同的特征码Netcat，就被创建出来了。





![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/1171715915.png)



不需要修改makefile，make命令就会编译出一个新的程序，叫做nc.exe，这就是一份新的重新编译过的Netcat，不会被杀毒软件查杀。



**NOTE**

------

> 如果你遇到如下错误：
>
> ```
> makefile:11: *** missing separator. Stop.
> ```
>
> 在makefile中移除多余的空格即可。



