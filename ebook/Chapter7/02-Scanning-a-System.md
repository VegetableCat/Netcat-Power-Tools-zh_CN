Scanning a System 

Whenever you encounter a problem on a system or network, often the best way to approach it is to start with a very broad examination of the problem, and eventually narrowing your search into smaller components.This makes sure you do not miss anything, and allows you to identify all parts of the problem.We’ve covered port scanning in previous chapters, but in this chapter, we’ll focus on port scanning for troubleshooting your network. 

Naturally, there are other tools that could be used for this step; the more popular among them is nmap. However, nmap contains a lot of functionality that we do not really need, and Netcat can perform the tasks we want for this step. Rather than load up a different tool, we can use Netcat and keep things simple. 

Let’s take a look again at the different switches available within Netcat, and talk a bit about the ones we will use in this step of our troubleshooting. 

- -e prog Program to exec after connect [dangerous!!] 
- -g gateway Source-routing hop point[s], up to 8 
- -G num Source-routing pointer: 4, 8, 12, ... 
- -h This cruft 
- -i secs Delay interval for lines sent, ports scanned 
- -l Listen mode, for inbound connects 
- -n Numeric-only Internet Protocol (IP) addresses, no Domain Name System (DNS) 
- -o file Hex dump of traffic 
- -p port Local port number 
- -r Randomize local and remote ports 
- -s addr Local source address 
- -t Enable Telnet negotiation 
- -u User Datagram Protocol (UDP) mode 
- -v Verbose [use twice to be more verbose] 
- -w secs Timeout for connects and final net reads 
- -z Zero-input/output (I/O) mode [used for scanning] 



Right away we see that the –z flag is used for scanning. By using this flag,
 we reduce any delays that we might experience with communicating with the target ports.Another way we can speed things up is with the actual port communication. By using the –w flag, we can tell Netcat how long we are willing to wait for a port to respond.When we scan a port that has nothing on it, we may have to wait, since Netcat is simply trying to connect as opposed to analyzing the port information. We will set this flag to one second. 



Tools & Traps... 

What is Zero-I/O Mode? 

For clarification, programs will often include delays since the application programmer does not know when the processor has scheduled the program’s requested process. By adding a delay, the programmer ensures that the program will allow enough time for the central processing unit (CPU) process to request and return the results. Depending on the load of the CPU, this process could take anywhere from milliseconds to seconds. Without adding a delay, the program might assume the worse and use empty results, or possibly crash. However, program delays are usually not necessary when communicating through hardware, so one trick a programmer can use is to set the delay to zero. This will provide the fastest results possible, and reduce our overall time scanning our target systems. 

Additionally, we want to see what is going on within the scan, so let’s turn on
verbose mode using the –v flag.This will allow us to catch any problems associated
with a connection problem or a problem with Netcat without having to wait for it
to return with no results. Also, if you know the IP address that you want to scan,
you can use the –n option. I use this extensively, because I do not always trust DNS
results.We could use the –u flag to communicate with UDP ports if we thought it
was worthwhile, but often it is a waste of time, since UDP communication typically
only works when a specific set of data is sent to the UDP port.Without the exact
data strings, applications using UDP usually just drop the packets and send no replies.



Warning 

When conducting troubleshooting against a system, you need to make sure you know what system you are connecting to. It is not uncommon for DNS to be pointing to a system different than where you intended to connect, causing a tremendous loss in productivity during any troubleshooting effort. If there is a discrepancy within the DNS, it should be resolved quickly. 

Figure 7.1 shows our initial scanning effort.We used the options previously discussed, and selected a target system that has an IP address of 192.168.1.100.
 In addition, we chose to scan ports 1–1024. If you notice, Netcat started its scanning at the highest port number. Rather than show you all port results, most of which have nothing on them producing the “connection timed out” message, Figure 7.1 just shows you the initial results.To see a cleaner picture, let’s run the scan again with just those ports that I know are opened from the previous scan. 

Figure 7.1 Initial Scan Against Target System 

Troubleshooting with Netcat • Chapter 7 229 

The scan request and the results can be seen in Figure 7.2.This scan tells us that we have multiple ports open, but doesn’t tell us what services are running on them. Other applications might provide a “best guess” as to what services are running on a particular port, but the results cannot always be trusted. In fact, these guesses are usually based on a list of what is supposed to be on a particular port (like a Web server is usually found on port 80), instead of conducting a real analysis through 

www.syngress.com 

230 Chapter 7 • Troubleshooting with Netcat 

banner grabbing or more in-depth analysis. In my own personal experience, I often validate the scan results anyway by using Netcat, so I know for sure exactly what services are available. I learned early on that I can spend a lot of time trying to troubleshoot a problem, just to find out I had the application version wrong or sometimes the wrong application entirely. 

Figure 7.2 Refined Scan Results Against Target System 

Notice in Figure 7.2 that we were able to specify exactly which ports we wanted to scan. If our initial troubleshooting effort was targeted against a particular service (for example, the Web server on port 80), and we found it to be closed, we would know there was a problem with the server and not the network, since we can see the services on the other ports. But since all ports are open, we could then proceed to the next step, which would be to check for latency problems in the network. 



