# Comptia+ Lab SSH
#1.0 Networking Concepts

This repo contains [Comptia+ Lab ssh](https://www.101labs.net) labs and tutorials authored both by Labs101, and as newbie and start to play with github and studing comptia+ just add lab101 materal. If you see what you like go to there site and they have lots of hands on books and video for a small subscribion fee. Just to let you guys know none of this is my own materal.

##Instructions:
Please follow the labs from start to finish. If you get stuck, do the next lab and come back to the problem lab later. There is a good chance you will work out the solution as you gain confidence and experience in configuring the software and using the commands. Before you attempt these labs, please use the free resources for software installation, Packet Tracer advice, and other tips at www.101labs.net/resources Please DO NOT configure these labs on a live network or on equipment belonging to private companies or individuals. You MUST be reading or have read a Network+ study guide. I don’t explain any theory in this book; it’s all hands-on labs. I presume you know (for example) when you need to use a crossover cable (router to router or PC to router or switch to switch) or a straight-through (PC to switch or router to switch). I don’t point this out in most of the network diagrams. For all of the labs on Cisco equipment using Packet Tracer any model of switch and router should work fine. I typically used an 1841 router and 2960 switch. Feel free to try other models which support different interface types. Do this after going through the lab a few times first. In the instructions I enclose commands you need to issue in single quotes (e.g., ‘ping 192.168.1.1’), but please don’t use them when issuing commands on network equipment. It’s impossible for me to give individual support to the thousands of readers of this book (sorry!), so please don’t contact me for tech support. Each lab has been testedby several tech editors from beginner to expert. 

###Video Training:
Each 101 Labs book has an associated video training course. You can watch the instructor configure each lab and talk you through the entire process step by step as well as share helpful tips for the real world of IT. Each course also has 200 exam-style questions to prepare you for the real thing. It’s certainly not necessary to take use this resource, but if you do, please use the coupon code ‘101book’ at the checkout page to get a big discount as a thank you for buying this book.

###Books and Videos Training Labs
Also from Reality Press Ltd. 
Cisco CCNA Simplified, Cisco CCDA Simplified, Cisco CCDP Simplified, Cisco CCNA in 60 Days, IP Subnetting—Zero to Guru, 101 Labs—CompTIA A+, (due 2019) 101 Labs—IP Subnetting,101 Labs—Cisco CCNA, 101 Labs—Cisco CCNP, (due 2019) 101 Labs—Wireshark, WCNA (due 2019),

###Lab Objective: 
The objective of this lab exercise is for you to learn and understand how to enable SSH access to a device—in this case, a Cisco router.

###Lab Purpose:
It’s never a good idea to permit Telnet access to network devices, especially in corporate settings. SSH is a secure way to connect to network devices. In order to configure SSH you need to: 
  
  *Create a hostname. 
  *Create a domain name. 
  *Generate a crypto key.

###Lab Tool: 
Packet Tracer 

###Lab Topology: 
Please use the following topology to complete this lab exercise:

#### LAB SSH Tutorial:
![image](https://github.com/Bigbear08/Lab-1-ssh-packet-tracer/assets/110280762/f461a561-2546-48fa-b70e-8a2e297783b5)

#Lab Walk through
##Task 1: 
Drag two routers onto the canvas. I don’t point this out in the labs because I presume you know this from reading your theory books, but connecting routers together requires a crossover cable (because we aren’t using a switch). I used 1941 models for this lab, but for most of the others I used 1841 models (which have Fast Ethernet interfaces). Configure the hostnames on routers Router0 and Router1 as illustrated in the topology. You must always answer no at the start because the routers will drop into a question-and-answer mode in an attempt to self-configure. I'll use R0 and R1 as hostnames. Here is how you do it on ROuter0:repeat the tasks on the other router, but give the hostname as R1.Continue with configuration dialog? [yes/no]: no Press RETURN to get started! Router>enable Router#config t Enter configuration commands, one per line. End with CNTL/Z. Router(config)#hostname R0 R0(config)# 

##Task 2:
Add an IP address to each Ethernet interface and ‘no shut’ them in order to bring them up. 
```
R0(config)#interface g0/0
R0(config-if)#ip address 192.168.1.1 255.255.255.0
R0(config-if)#no shut
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed
state to up 
```
#Over to Router1:
```
R1(config)#interface g0/0
R1(config-if)#ip address 192.168.1.2 255.255.255.0
R1(config-if)#no shut
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed
state to up 
R1(config-if)#end
```
Make sure you can ping across the link
```
R1#ping 192.168.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2
seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max =
0/0/0 ms
```
##Task 3: 
Secure Router1 so that it accepts SSH incoming connections. We need to set a domain name and generate keys. As options, we have set retries for the password to 2 attempts and a timeout of 60 seconds if there is no activity.
```
R1#conf t
Enter configuration commands, one per line.  End with
CNTL/Z.
R1(config)#ip domain-name 101labs.net
R1(config)#crypto key generate rsa
```
The name for the keys will be: R1.101labs.net Choose the size of the key modulus in the range of 360 to 2048 for your General Purpose Keys. Choosing a key modulus greater than 512 may take a few minutes.How many bits in the modulus [512]: 1024 % Generating 1024 bit RSA keys, keys will be non-exportable...[OK] 
```
R1(config)#ip ssh time-out 60
R1(config)#ip ssh authentication-retries 2
R1(config)#line vty 0 15
R1(config-line)#transport input ssh
R1(config-line)#password cisco
R1(config-line)#end
```
Next you can go to the router Telnet lines. There are 16 available lines on most Cisco devices, numbered 0 to 15 inclusive. You need to permit incoming SSH connections on these and ‘transport input ssh’ above does this.
```
R1#show ip ssh SSH Enabled - version 1.99
Authentication timeout: 60 secs; Authentication
retries:2
R1#
```
##Task 4:
Connect to Router1 from Router0 using SSH. You should be prompted for the password, which, as you can see above, is ‘cisco’. You can add a username for the connection, which I’ve done here. Use the letter ‘l’ below after ‘ssh’ not the number 1. 
```
R0#ssh -l paul 192.168.1.2
Open Password:
R1>
```
You can quit the session by holding down the Ctrl + Shift + 6keys at the same time, then letting go and pressing the X key. 

##Task 5:
Attempt to telnet from Router0 to Router1 to check that the connection is refused. 
```
R0#telnet 192.168.1.2
Trying 192.168.1.2 ...Open

[Connection to 192.168.1.2 closed by foreign host]
R0#
```
##Notes: 
Almost any model of router will do for this lab. Just make sure you connect the routers with a crossover cable because we aren’t using a switch in this lab. Ensure you have watched the video on how Packet Tracer works at [[www.101labs.net/resources.]] 









