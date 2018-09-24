Testing Network Latency 

In the previous section, we scanned our target system to see if all expected services were running on the system. However, before we can move onto a more detailed analysis of those services, we need to make sure the network is not the cause of our troubleshooting effort.There are times when latency within a network is too large, which can cause applications connecting to our target system to drop the connection and generate an error message.This can happen even though the service on our target system is properly working. 

To test network latency, we could search the Internet for a tool designed specifically for that task. Or, we can just use Netcat again. Since what we want to examine is transfer speed of data between two systems, it only makes sense that we should turn to Netcat, which was specifically designed to establish data channels. In this next example, 



we will set up a way to test latency just using Netcat, and will use a couple of different applications to assist us. 

The first thing we need to do is find an application that can provide us with some timing functionality.Within Linux, the application we will use is called “time,” which will calculate the total running time of any command, as you will see in our next example. 

The second thing we need is data.We could create a file that contains a lot of data, but that is time consuming and not worth our time.We can use another appli- cation to do this task for us, called “yes.” The way yes works is it will write the letter “y” and append it with the newline command, and will keep doing so until the program is terminated. Not very fancy, but just the right tool for what we need. 

Using Netcat as a 

Listener on Our Target System 

Now that we have a way to generate data to send over a communication channel, and a method to calculate transfer speed, all we need is the data channel.This first example will use Netcat as a listener on our target system. In Figure 7.3, we have logged onto our target system and ran Netcat to listen on port 4321. Since we did this at a high port number, we do not need to have root privileges to perform this task, just access to the server. 

Figure 7.3 Setting Up a Listener on Our Target System 

In Figure 7.3, you can see that we are sending any received data to a file in the /tmp directory called speed_test. I am doing this only so that we can see exactly what we sent to our server using the yes application. Normally, you would want to redirect any incoming data to /dev/null to save on disk space. 

Now, let’s launch our latency test. In Figure 7.4, we again use Netcat to establish a connection on our target system.You can also see that we included the time and 

Troubleshooting with Netcat • Chapter 7 231 

www.syngress.com 

232 Chapter 7 • Troubleshooting with Netcat 

yes applications within the command string. One problem with our command is that there is no mechanism to terminate the process. In other words, unless we stop the process manually, the yes application will continue to create data until our target system no longer has any drive space.We terminate the process using the Command-C key sequence in Linux. 

Tip 

The more data we send and the longer we wait before terminating the process, the more accurate our transfer speed results will be. However, for most purposes, a few seconds is sufficient. In Figure 7.4, notice that we also used our verbose command twice, which is required to have Netcat tell us exactly how much data is sent or received across the channel.Without this information, it will be impossible to do our latency calculations. 

Figure 7.4 Launching Our Latency Test 

As you can see in Figure 7.4, we ran our command for about six seconds, which gave the yes application enough time to generate and transmit 42 megabytes of data to our target system. Just to make sure our calculations are correct, let’s examine the file size of our /tmp/speed_test file. In Figure 7.5, we see that the size of the data capture file is the same size as displayed in Figure 7.4. Now that we have confirmed the amount of data we transmitted, we can do our calculations. 



Figure 7.5 Validating the Amount of Data Sent to the Target System 

To determine the transmission speed, we simply need to do some quick math: 

Transmission speed = (size of data sent) ÷ (“real” transmission time) 

In our example above, we can calculate that our transmission speed is
 42544128 bytes/6.019 seconds, which translates into a speed of 7.07 Mb/s. For our troubleshooting effort, this should be adequate for any application attempting to con- nect to our target system, so we can probably rule out network latency as a problem. 

Before we move on to the next scenario in determining network latency, let’s take a quick look at the data sent to our target system by the yes application. In Figure 7.6, we see a few lines of the /tmp/speed_test file. As you can see, the file simply consists of the letter “y” on separate lines. At this point, we should delete the file, since we no longer need it. 

Tip 

Figure 7.6 Output of the Sent Data Using the yes Application 

If you plan on using this method regularly, it may make sense to create a file of a set size, such as one 10 Mb in size. That way, you can quickly identify changes in your network simply by looking at the transfer speed, saving the need to remember the formula and doing the calculations each time. 

www.syngress.com 

234 Chapter 7 • Troubleshooting with Netcat 

Using a Pre-existing 

Service on Our Target System 

So what are our options if we do not have access to the target system to set up Netcat as a listener? We will have to use a service that already exists on our target system, and calculate the time it takes to create a session and transmit data with
 that service. In this scenario, we will use the Web service on our target system. It is important to keep in mind that what we will be asking for in this case is a Web page, which typically is very small compared to the data we sent in our previous example. However, if we do not have an alternative, this can at least give us something to work with.To increase accuracy, we could conduct this test multiple times, to account for any minor variations we might experience in the network. 

Using a UDP Service 

In Figure 7.7, we use a command similar to our previous example, but this time
 we are going to target a service that uses UDP.We did not do a scan for active UDP ports on our target system, but I will target a port that I know exists on our target system, port 37, which provides time service. Since we are targeting a UDP port,
 we need to add the –u flag. One other important step is we want to redirect any return traffic into the /dev/null directory, so we do not clutter up our command window with useless data. Also, just like before, we need to manually terminate the test, since yes will not terminate on its own. Again, we use the Control-C sequence to accomplish this step. 

Figure 7.7 Performing Latency Test Against UDP Port 37 

Using the formula in our previous scenario, we can calculate the network speed to be (49316864 bytes + 160004 bytes)/7.421s, or 6.67 Mb/s.This result is very close to our previous results when we used Netcat to listen on our target system. 



Some of the discrepancy between the two results could be response time within the remote system application, or fluctuations in network speeds due to congestion or different network routing path selections. Overall, the difference between 7.01 Mb/s and 6.67 Mb/s is nothing to be concerned with, and should rule out any problems with network latency if this were our results during a troubleshooting session. 

Warning 

Do not use just one method to determine latency issues. Make sure you have multiple results as well as a solid baseline before presenting your evidence to the network administrators. There can be many reasons for results indicating latency problems, including issues with the system you performed the tests on such as high CPU or low memory. In other words, the network might be fine; the problem could be your system. 

Using a TCP Service 

In this next scenario, we will target a service using TCP, specifically the Web service we identified in our previous scan. Just like before in the UDP example, we redirect any returning data (in this case, a Web page) to the /dev/null file, effectively dumping anything we receive. Using the formula from the previous scenario, we can calculate our transmission speed as (1988 bytes)/0.012 seconds, or around 165 Kb/s. 

Figure 7.8 Performing Latency Test Against TCP Port 80 

Normally, a network speed of 200 Kb/s would be serious cause for alarm, but let’s take a second and analyze what is happening. Our system requests a Web page from our target system, which then has to process it through the Web server application
 in order to send us back to our requested page.The 0.12 seconds includes a lot of overhead on the remote server that we cannot simply remove from our calculation. 

Troubleshooting with Netcat • Chapter 7 235 

www.syngress.com 

236 Chapter 7 • Troubleshooting with Netcat 

So how can we use this data to our advantage to determine if there is any latency problem? We have to perform this test multiple times over several days or weeks to get a baseline statistic. If we run this command regularly during times when there are no connectivity problems, and we know it takes on average 0.012–0.030 seconds to grab the Web page from the target system, but later discover it takes 2–3 seconds during our troubleshooting session, we know we have a problem. 

Which scenario is more accurate? The first two, either using Netcat or a UDP service as a listener on the target system, are much more reliable to determine trans- mission time over a network. However, if we have time to generate a baseline, we can use the third scenario, where we use a TCP service (such as Hypertext Transport Protocol [HTTP]) to gauge any latency issues on a network, which is what we are really after during a troubleshooting session. Regardless of which method you use, the more important thing to remember is that many factors can cause your transmis- sion speeds to fluctuate, and baselines should be created even when using Netcat or UDP as a listener. 