# FAQ 常见问答

Q：我还没有开始下载Netcat，我的杀毒软件发现Netcat是一个木马！我该怎么办？

A：如果你从没下载安装过，你可能摊上事啦！除了Netcat的普通版(the vanilla version)，还有其他编译的版本会自动设置他们的特定端口(ncx.exe运行在80端口,而ncx99.exe运行在99端口)。



Q：我的杀毒软件不让我 下载/安装/使用 Netcat。这是什么原因？

A：至少有两个大型的防病毒厂商(也许更多)把Netcat标志为一个问题。在一些测试案例中，其中之一是从实质上阻止下载，因为Netcat被包含在一个更大的安装包内。第二步，隔离(quarantine)掉Netcat正在运行的"自我保护"功能。这里有几种方案，通常这会涉及到修改"默认"参数( they typically involve modifying “default” parameters)。首先，你可以禁用实时保护(live protection)，至少在你下载Netcat的这短暂时间内。其次，你可以为Netcat创建一个特殊的目录(甚至连同其他类似的可能会被杀掉的工具一起)，并设置你的实时保护/自动防御选项，忽略这个目录。最后，你可以从你的正常(narmal)扫描，或是日常(scheduled)扫描中，排除(exclude)此目录。



Q：Netcat已经安装在我的系统中了。为啥我要重新安装？

A：Netcat在Linux发行版的很多预装的包都是"安全"编译，没有GAPING_SECURITY_HOLE选项。如果没有这个功能，Netcat就不能执行程序。因为Netcat的强大就在于这个选项，所以如果你需要这个功能，你应该重新编译和安装。



Q：我怎么知道Netcat是编译了-e选项呢？

A：在Windows上，Netcat已经编译的此选项，不需要更多的操作了。在Linux上，只需要输入nc -h显示帮助信息。GNU Netcat(版本0.7.1)是已经编译了此选项，所以，也不需要更多的操作。原始UNIX版本的Netcat(通常版本是1.10)编译了此选项，帮助信息会显示(ps:我原来的nc就有-e的)。在Mac上，Netcat默认没有编译这个选项。



Q：我怎么知道Netcat是在客户端模式还是在服务端模式下运行？

A：-l意味着(denote)监听(listening)，或者称为服务端模式。没有-l就表明是客户端模式咯。



Q：我断开连接的时候，Netcat也就关闭了服务端。但是我希望连接是持久的。这能行吗？

A：当然。在Windows，用-L选项，当Netcat关闭的时候，会用相同的命令重新打开它。这种特殊的选项在Linux不可用，但是你可以写一个简单的脚本来弥补。



Q：如果能做[über-leet的功能]，Netcat还可以变的更吊(cooler)。我该怎么做呢？

A：Netcat是开源的。这意味着你可以下载源码，修改以满足你的需求(to your delight)，然后编译你的über-leet功能。



Q：我在哪能找到有关Netcat的更多信息咧？

A：首先，请参考本书的剩余章节。本丛书作者(the contributing authors)是知识渊博，在各自领域的专家。其次，Google it。在互联网上，有大量的(a wide range of)有关Netcat的文档和教程(tutorials)。第三，找到一个有关的论坛，贴出问题。如果你知道怎么样问问题(如何问好一个问题)，那里会有很多人愿意帮助你。