Phase 1.
 
- Rockstar Corp has five offices in Hollywood, their IP addresses are 15.199.94.91, 203.0.113.32, 161.35.96.20, 15.199.95.91 and 192.0.2.0.  I determined that the IP 161.35.96.20 is currently accepting connections.  The IPs 15.199.94.91, 203.0.113.32, 15.199.95.91 and 192.0.2.0 were all unreachable using the fping command, which runs on the OSI layer 3.  To test these IP addresses we used:
       
        "fping 15.199.95.91 15.199.94.91 203.0.113.32 161.35.96.20 192.0.2.0"
- ![HW8_Phase_1.png](/HW-8/Image/HW8_Phase_1.png)
 
Phase 2.
 
 - Using Nmap on IP 161.35.96.20 to run a syn scan we got the results back that port 22 is currently open for SSH.  Having your SSH port open is a concerning vulnerability and Rockstar Corp should definitely close this port as soon as possible in order to mitigate the risk.  A syn scan occurs on the transport layer, layer 4, of the OSI model and to run this syn scan we simply use the command:
 
        "sudo nmap -sS 161.35.96.20"
- ![HW8_Phase_2.png](/HW-8/Image/HW8_Phase_2.png)
 
Phase 3.
 
- To prove that having an open SSH port is a bad idea, we successly remotely connected to the rockstar server in hollywood using SSH (OSI Layer 7) and the default user information. We simply had to use the following command:
 
         "ssh jimi@161.35.96.20 -p 22"
- As a result we were able to access the hollywood server.
- ![](/HW-8/Image/HW8_Phase_3.png)
 
- Upon examination of the host computer we discovered that a hacker had managed to poison the DNS cache of the computer by modifying /etc/hosts. The hacker had changed the default IP address for rollingstone.com to 98.137.246.8 for this computer.
- ![](/HW-8/Image/HW8_Phase_3-2.png)
 
- After exiting the computer we verified that the IP address in the /etc/hosts was not registered to the same domain as rollingstone.com using nslookup.
 
        "nslookup 98.137.246.8"
- We recieved confirmation that this address the computer was automatically being sent to was not registered to rollingstone.com. Nslookup returned it as unknown. Obviously the DNS poisoning needs to be removed from /etc/hosts and again the ssh on port 22 needs to be closed. This all seems to have occured on the OSI layer 7.
- ![](/HW-8/Image/HW8_Phase_3-3.png)
 
Phase 4.
 
- I was directed to search /etc because that is where we found the hosts file that had been DNS poisoned. After running the find command to look for text files in /etc we found a text file titled 'packetcaptureinfo.txt'. Opening the file with cat we found that the file linked to a google drive folder.
 
        "find /etc -type f -name '*.txt'
- ![](/HW-8/Image/HW_Phase_4.png)
 
        "cat /etc/packetcaptureinfo.txt"
- ![](/HW-8/Image/HW_Phase_4-2.png)
- I opened the google folder and downloaded the pcap file secretlogs.pcapng to open in wireshark.
- ![](/HW-8/Image/HW8_Phase_4-3.png)
- I found two things that were concerning within the packet file. The first thing that concerned me was that it appeared that one of the mac addresses were spoofed, which would allow the network traffic to be spied on. See picture below. The best way to prevent this from happening would be to create static ARP entries, instead of allowing dynamic ARP entries. Assigning static ARP entries would lock every device to one mac/IP address on the network. ARP occurs on the second layer of the OSI networking model.
- ![](/HW-8/Image/HW8_Phase_4-4.png)
- The final thing that I found in the packet capture occurred in the HTTP protocol packets on the application layer of the OSI model. It connected and downloaded multiple things including accessing a logging agent and getting a javascript application. Because it happened on http and not https it isn't secure and http shouldn't be enabled at all. More importantly the downloading of a potentially malicious application that could have logging capabilities is very concerning. Rockstar corp should probably look at disabling or at least limiting the access to unsecure downloading of apps. Picture of the wireshark packet is below.
![](/HW-8/Image/HW8_Phase_4-5.png)

© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.