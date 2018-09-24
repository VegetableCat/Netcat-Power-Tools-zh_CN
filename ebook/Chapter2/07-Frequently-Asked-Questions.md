Frequently Asked Questions 

Q: How can I scan multiple hosts using Netcat?
 A: You can create a simple loop to read in a host list to your Netcat port scanning 

command.
 Q: How do I identify the type of application architecture supported by a Web server? 

A: Use Netcat to establish a connection to the Web server, issue a HEAD request, and view the header information, which contains the server version and application framework used. 

Q: The Windows Firewall detects and blocks my Netcat listener. How can I disable this block? 

A: You can use the Windows Firewall command line tool to create an exception for your listener and port. 

Q: In the past, Netcat was detected by antivirus as a HackTool. How can I avoid this from happening again? 

A: Currently, I am not aware of any antivirus definitions where Netcat is detected as a malicious tool.To avoid antivirus, it is best to recompile the program. 

Q: What is the purpose of Egress Firewall Testing?
 A: To test and verify that outbound port filtering is implemented and functioning 

properly. 

Q: How can I hide Netcat from a typical user on a system that I have already compromised? 

A: You can rename the executable to something that resembles another Windows process and put it in a covert location. 

Q: Why does the Netcat Windows Service Backdoor error appear when I try to start it? 

A: Netcat does not contain any code to know how to interact with the Windows Service Controller. 

www.syngress.com 

60 

Chapter 2 • Netcat Penetration Testing Features
 Q: How can I know when a Netcat backdoor will establish a connection to 

my system? 

A: The only way to know exactly when your backdoor will initiate a connection is by using Windows Task Scheduler to execute the backdoor commands at
 a specific day and time. 