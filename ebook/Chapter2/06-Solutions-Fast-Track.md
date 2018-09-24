## Solutions Fast Track 

### Port Scanning and Service Identification 

- [x] Netcat is a powerful utility for port scanning, banner grabbing, and unknown service identification. 
- [x] You can identify a type of Web server and version information using Netcat to send the HTTP HEAD command. 
- [x] Creating a script to automate banner identification is a necessity when performing a penetration test against thousands of Web servers. 

### Egress Firewall Testing

- [x] The objective of egress testing is to test outbound firewall rules to verify outbound port filtering is working properly. 
- [x] Two systems are required for this test.A system located on the inside of the firewall will attempt to make a connection through the firewall, to a system on the outside on every TCP and UDP port.

### Avoid Detection on a Windows System 

- [x] The Windows firewall detects and, by default, blocks programs from opening TCP/IP sockets and listening for incoming connections. 
- [x] You can bypass the Windows Firewall blocking and alerting features by manually adding exceptions to the firewall. 
- [x] Recompiling Netcat from source will ensure Antivirus programs will not identify and remove it from your target system. 

### Creating a Netcat Backdoor on a Windows XP or Windows 2003 Server
- [x] Two methods are used to communicate with our Netcat backdoor, initiating a connection to the backdoor or triggering the backdoor to make a connection to us. 
- [x] Executing the backdoor using a Registry Entry triggers the Netcat connection to our listener when a user logs on to the system. 
- [x] The Windows Task Scheduler Service can be used to send us a Windows command prompt at any day/time interval you choose. 