# 安装



   Netcat是一个小而简单的程序，怪不得(no wonder)安装那么简单(straightward),不管你使用的是什么操作系统。对于windows，Netcat已经有编译好了二进制形式的软件包，所以没有真正安装的必要(no true installation required)。就像前面的note，有两种常用的UNIX/Linux的安装方式( implementations)：原始的UNIX版本和GNU版本。几乎(Virtually)所有的UNIX/Linux自带了其对应的已经编译好的版本;然而，学习怎么安装还是有必要的。此外，如果想要达到你的特定要求，你可能需要重新编译netcat来实现。



## 在Windows上安装



   windows上的安装真的是不能在简单了。只需要从  [download](http://www.vulnwatch.org/netcat/nc111nt.zip) 下（已失效）现在为http://joncraton.org/files/nc111nt.zip 密码:nc）下载zip文件。解压到你想要的目录，就搞定啦。(具体看图1.1)。还有一些重要的文件需要查看一下：hobbit.txt是原始文档，readme.txt是版本1.10-1.11安全修订的注释，还有license.txt是指GPL许可(GNU General Public License).



## NOTE



   netcat是一个命令行工具。双击nc.exe，只是简单的运行netcat，没有指定参数，然后会显示一个命令提示符(prompt)。你要在这里运行netcat，但是当实例完成以后，窗口就会马上关闭。嘛。就什么都没有看到。这没用(helpful)啊，你要看到反馈啊。一个简单的方式，直接从命令行启动。开始-运行-cmd.exe nc -h，你就会看到帮助界面进行进一步的引导。








我的杀毒软件(anti-virus)说Netcat是一个木马程序(trojan)！



   Netcat的强力(potent)通信能力不受限于网络管理员。渗透测试(Penetration testers)使用Netcat来测试目标系统的安全性(举个例子，Netcat被包括在Metasploit框架里)。恶意(Malicious)用户用Netcat(或者它的变种中的一个)作为获取远程访问系统的途径。在这种方面，可以理解为什么很多杀毒软件把Netcat标记(label)为"木马"或者"黑客工具(hacktool)".

   一些杀毒软件可能会试图阻止(prevent..from)你安装Netcat，或者甚至试图阻止你下载Netcat或是其他包含Netcat的程序。与几乎所有的工具，没有道德规范要限制它的使用仅用于合法的目的。你的想法就简单的决定了Netcat是否是由你下载并安装的(这样就不会是威胁)，或是暗中(surreotitiously)被恶意用户用于恶意(nefatious)用途安装的。

   你可以考虑配置你的杀毒软件，当它扫描或自动防御的时候，排除安装了Netcat的特定目录。当然啦，你需要了解与此相关的危险。



## 在Linux上安装



   很多主流(mainstream)Linux发行版自带了已经编译和安装好的Netcat。其他的(非主流？)系统也至少有一个或者多个Netcat的版本作为预编译(pre-compiled)的软件包可供下载(available)。要确定(determine这个词用的好)Netcat的版本，只需要输入`nc -h`或者`netcat -h`，最初的UNIX版本会返回[v1.10]的一个版本行，而GNU版本会返回GNU Netcat 0.7.1,这是一份网络工具的重写版本。即使Netcat已经预装在你的系统上，你可能也不希望跳过这一节吧。许多在Linux发行版上，预装(pre-installed)、预编译(pre-compiled)或者软件包版本的Netcat，编译是没有包含所谓的GAPING_SECURITY_HOEL选项(这允许Netcat用-e参数来执行程序)。这些通常是从Netcat的源代码中'安全'的编译出来的。GNU版本的Netcat自动编译了启用-e选项，因此这个版本的安装无需额外的必要配置。尽管如此，最初的Netcat的所有其他功能都保持不变(remains intact).当然啦，能够执行程序使Netcat成为了一个强大的工具。在这本书中，很多示范(demonstrations)都很好的利用了-e选项，所以如果你想照着学习(follow along)，你可能要考虑重新编译Netcat。



### TIP

   如果您已经安装了Netcat，但是不确定它是否编译了-e选项，直接(simplely)运行netcat -h显示帮助信息(help screen)。如果包含-e选项，那么Netcat就有。如果没有-e选项，那么你就需要重新编译Netcat，或者使用GNU版本。





### 从软件包安装



   绝大多数发行版都把Netcat预编译成一个软件包(package)。有一些发行版可能有多于一个的版本，或者多个有个不同功能的安装程序(implementations)。记住(note),就像我们上面做的那样(as we did above),这些软件包也许没有执行程序的选项(一般都是因为一个什么原因)。比如，在Debian系统上，要从一个预编译的包安装Netcat，输入apt-get install netcat







### TIP



   确保你的软件源是最新的，虽然说这个超出(beyond)了本书的范围(scope)。比如说，Debian和APT，软件源列在/etc/apt/sources.list文件中。此外，一定要保持你的列表里的软件也是更新了的，用apt-get update来更新。对于其他发行版，查看你的文档，然后更新软件包列表。






   图1.2显示了简单的Netcat软件包安装的过程。注意，在这种情况下(notice that in this case),Netcat没有与其他软件包的依赖关系(dependecies),即使是在Debian的最小化安装(minimalist install)中.还要注意下包名netcat_1.10-32_i386.deb。这里的关键是1.10,这代表版本信息。这就证实(comfirms)了这个包实际上是从最初的UNIX Netcat编译的，而不是(as opposed to)GNU Netcat。还有就是(Furthermore),nc -h表明(reveals)这个包是预编译了万能(all-powerful) -e 选项。



### NOTE

   要通过(via)软件包的方式安装到另类的(other flavors)Linux发行版上，请参考(consult)你的文档上安装预编译包的具体方法。



### 从源码安装



   如果你想要编译源代码，你有两个版本可供选择，这两个大致(more or less)都是一样的，但是有一点重要的不同(exception)。第一个选择是原始的UNIX Netcat版本，在此处查看 www.vulnwatch.org/netcat 。第二个选择是GNU Netcat版本，在这里 netcat.sourceforge.net 。Netcat的这两个版本的关键区别在于，原始的Netcat需要手动(manual)配置来编译-e选项，而GNU Netcat自动就编译好了。手动配置也不复杂，但是如果你不习惯看源代码的话，就会非常棘手(tricky).

   如果你对于Linux还比较(relatively)小白(new)，从源码编译一个程序让你感觉害怕(daunting)，放心(rest easy).整个(entire)安装过程是简单容易的，只需要花费几分钟。为了达到安装的目的(sake)，所以我们安装没有手动配置-e选项的Netcat，我们会下载GNU版本的Netcat，并且配置和编译。



快速安装nc

```shell
wget http://sourceforge.net/projects/netcat/files/netcat/0.7.1/netcat-0.7.1.tar.gz
tar –xzf netcat-0.7.1.tar.gz
cd netcat-0.7.1
./configure
make
make install

```



原文wget给的链接已经失效了，不过我已修正为可用的地址了。



   第一步是下载源码。你可以选择简单的wget命令行工具(utility)，也可以用浏览器或者其他下载方式。









   接着，解压(un-tar)文件(archive)并放到一个新创建的Netcat目录。然后，配置Netcat。这个configure脚本会创建一个叫makefile的配置文件。







   make命令会从上步骤的Makefile里面建立二进制文件(Netcat的可执行文件)。使用make install命令会把Netcat安装到你的系统中。注意，运行make install需要root权限(privileges).对的!(That's it)你会发现，往往来说(more often than not)，这就是个比较普通的，在Linux上的源代码安装软件的过程。



#### NOTE



   如果你在安装过程中遇到(encounter)任何错误，这可能是发生在最后两步。如果是，可能是你没有安装正确的软件包来编译。最有可能是你使用了最小化安装。一定要检查软件包的来源(references)，以确保安装正确 (proper)。



   你安装的可执行二进制文件可能是叫nc或者是netcat，这取决于netcat的版本。为了一致性，在本章，我们将使用nc。





## 确认安装成功



   无论(Regardless of)你用的是Windows版本的Netcat还是Linux版本的，需要确认Netcat是否正确安装，输入nc -h或者netcat -h来显示帮助信息。注意，有些选项是存在差异的。比如，在windows版本，-L选项表示开启持久(persistent)的监听(listening)模式(以后会讲)，而在Linux版本表示隧道模式(我的根本没有-L选项呢)。

   还有就是，Linux版本包括-V选项(注意是大写字母)，用来显示版本信息(这个也没有)。而Windows版本没有(lacks)这个选项。最后，Linux版本包括-x(hexdump incoming and outgoing traffic 显示进出流量的十六进制)选项，但Windows没有-x，取而代之的是-o。







windows版的版本v1.11 NT









linux(bt)版的版本是GNU netcat0.7.1,a rewrite of the networking tool

而我的是1.10-41



确实多了很多选项 先前没有的如下 还有一些kali上nc有的GNU版没有的 不列举了

-L,--tunnel=ADDRESS:PORT forward local port to remote address

-V,--version output version information and exit

-x,--hexdump hexdump incoming and outgoing traffic



然后我安装了GNU版的nc了。。

果然，如果只输入netcat或者nc，就进入命令行模式了。

显示cmd line: