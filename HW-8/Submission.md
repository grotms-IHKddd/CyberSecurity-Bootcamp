Phase 1:
        
        Rockstar Corp has five offices in Hollywood, their IP addresses are 15.199.94.91, 203.0.113.32, 161.35.96.20, 15.199.95.91 and 192.0.2.0. I determined that the IP 161.35.96.20 is currently accepting connections. The IPs 15.199.94.91, 203.0.113.32, 15.199.95.91 and 192.0.2.0 were all unreachable using the fping command, which runs on the OSI layer 3. To test these layers we used:
        "fping 15.199.95.91 15.199.94.91 203.0.113.32 161.35.96.20 192.0.2.0"
![HW8_Phase_1.png](/HW-8/Image/HW8_Phase_1.png)

Phase 2:

        Using Nmap on IP 161.35.96.20 to run a syn scan we got the results back that port 22 is currently open for SSH. Having your SSH port open is a concerning vulnerability and Rockstar Corp should definitely close this port as soon as possible in order to mitigate the risk. A syn scan occurs on the transport layer, layer 4, of the OSI model and to run this syn scan we simply use the command:
        "sudo nmap -sS 161.35.96.20"
![HW8_Phase_2.png](/HW-8/Image/HW8_Phase_2.png)

Phase 3:

        To prove that having an open SSH port is a bad idea, we successly remotely connected to the rockstar server in hollywood using SSH (OSI Layer 7) and the default user information. We simply had to use the following command:
        "ssh jimi@161.35.96.20 -p 22"
        As a result we were able to access the hollywood server. 
![](/HW-8/Image/HW8_Phase_3.png)

        Upon examination of host computer we discovered that a hacker had managed to poison the DNS cache of the computer by modifying /etc/hosts. The hacker had changed the default IP address for rollingstone.com to 98.137.246.8 for this computer.
![](/HW-8/Image/HW8_Phase_3-2.png)

        After exiting the computer we verified that the IP address in the /etc/hosts was not registered to the same domain as rollingstone.com using nslookup. We recieved confirmation that this address the computer was automatically being sent to was not registered to rollingstone.com. Nslookup returned it as unknown. Obviously the DNS poisoning needs to be removed from /etc/hosts and again the ssh on port 22 needs to be closed. This all seems to have occured on the OSI layer 7.
![](/HW-8/Image/HW8_Phase_3-3.png)