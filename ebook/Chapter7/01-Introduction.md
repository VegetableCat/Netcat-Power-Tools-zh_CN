导读
====== 

In this chapter, we will be tackling problems within our network, and solving them using Netcat. As has already been mentioned throughout this book, Netcat has a strong advantage over other tools in that it does not alter the data stream between systems.We will make use of this extensively in this chapter, as we communicate with applications in their own “native tongue” to identify and fix problems. 

Many of you might be familiar with one of the more popular tools used by administrators, which is Telnet. Similar to Netcat,Telnet can be configured to communicate with any port on a target system, which then provides a conduit for communication with the target system’s applications.While Telnet is a very useful tool, it has its problems.The primary one for our topic at hand is that Telnet will inject its own messages into the data stream. Not only that,Telnet constantly exam- ines the traffic for control commands, and will proceed to remove whatever it thinks is data meant for it, and act according to this perceived command.This could have seriously negative consequences, especially if encrypted data is being transmitted through the data stream. It is not unusual to have within encrypted data, strings of data that could be misinterpreted to be special characters, and if one piece of that data looks like Telnet’s disconnect string, so much for your connectivity. 

Usually, administrators do not encounter this situation, because they use Telnet primarily using only plaintext, thereby avoiding special characters that might be accidentally interpreted by Telnet. Unfortunately, this limits any troubleshooting efforts, and prevents the administrator from doing more within their network.This is where Netcat steps in. Netcat does not inject or react to data passing between two systems, so you can use it extensively, knowing that the data stream will remain uncorrupted, even if that data stream contains strings that look like special characters. 

At the beginning of this chapter, we will discuss how to examine remote systems using Netcat’s scanning ability. Once we finish with that subject, we will move on
 to the system by testing open ports to see if they really are active and to see what protocols are on those ports. After that, we move in and directly communicate with different applications to determine what problems might exist, which will give us insight into how to solve them. 

By the end of this chapter, we will have discussed how to communicate with multiple applications, but not every possible application you might encounter. However, after you have concluded this chapter, you will understand how to conduct troubleshooting sessions against additional services you might encounter, based on what you learn here. 



