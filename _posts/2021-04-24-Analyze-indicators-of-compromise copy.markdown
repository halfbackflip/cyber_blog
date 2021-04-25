---
layout: post
title:  "Analyze indicators of compromise"
date:   2021-04-24 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---

## Activity 12.1 Identify a Network Scan
In this excercise I will walk through how to initiate and then identify a network scan occurring on a Linux machine. 

1. I will startup both my Kali and Metaploitable VM. 
2. On Kali, open wireshark and a terminal window.
![Net Scan](\assets\img\netscan1.jpg)
3. Determine the IP address of the Metaploitable VM by running `ifconfig`. In my case it is 10.0.2.4
![Net Scan](\assets\img\netscan2.jpg)
4. Start the Wireshark Capture from the Kali machine using the active interface, in my case this is "eth0." Intially you may only see DHCP and ARP messages.
![Net Scan](\assets\img\netscan3.jpg)
5. Now we will start the nmap scan on the Kali Machine. Type in: 
`nmap -p 1-65535' <IP of the metasploitable VM>`
![Net Scan](\assets\img\netscan4.jpg)
6. Notice all the exciting ports which are open on the Metasploitable VM.
Lets try navigating to some of them via the ip address + port number in the
Kali Ice Weasel Browser.
7. Not surprisingly, the VM is running a web server on port 80.
![Net Scan](\assets\img\netscan5.jpg)
8. More surprisingly, the VM is running FTP, NFS, etc.
9. Stop the wireshark capture.
![Net Scan](\assets\img\netscan6.jpg)
10. Using filters, we can review the traffic captured.
11. Entering `tcp.port==80` will display the connections our Kali machine made to the Metasploitable VM.
![Net Scan](\assets\img\netscan7.jpg) 
12. Now lets investigate the scan we conducted.
13. Navigate to Edit > Preferences > Appearence > Columns. Adjust the layout to include both the "Src Port (unresolved)" and "Dest port (unresolved)"
14. This will allow us to easily see all the ports scanned in order. Enter the following filter: `ip.dist == <IP of the metasploitable VM>` Now sort the columns by time. Now we can sift through all our glorious sequential port scans. 
![Net Scan](\assets\img\netscan8.jpg)
  
## Glossary
A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.indicatorsofcompromise %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>