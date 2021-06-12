---
layout: post
title:  "Nmap, TCPDump and Grep"
date:   2021-06-11 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---
## Nmap
Nmap stands for "Network Mapper." It is an open source tool for network security, auditing and system administration. At the most basic level, nmap allows for scanning a network and returning which addresses are response and what ports are open.

## Nmap Discovery Scans
Discovery scans are used to footprint a network. 
Footprinting allows provides an overall, high-level view of a network. 
Using nmap you can perform footprinting of a network.

```
nmap 10.0.2.0/24
```
* Scans all IP addresses in the subnet
* Default scan sends a ping and TCP ACK to ports 80 and 443
* If the host responds, nmap will initiate a port scan of the top 1000 ports on the host. This scan will alert an IDS.

In this instance I have scanned my virtual subnet. 
The host 10.0.2.4 is running the metasploitable VM and returns the following open ports.
![nmap practice](\assets\img\nmap1.jpg)

```
nmap -sn 10.0.2.0/24
```
* Performs a host discovery scan
* Does not perform a port scan
![nmap practice](\assets\img\nmappractice2.jpg)

```
nmap -sL 10.0.2.0/24 
```
* Lists all IP addresses from the range
* Attempts to discovery any host names associated with the IP addresses
* Passive method

```
nmap -PS -p80 10.0.2.0/24
```
* TCP SYN ping. Probes specific ports from the list
* Uses a TCP SYN packet instead of ICMP, as some firewalls will block ICMP and IDS will alert on ICMP.
![nmap practice](\assets\img\nmappractice4.jpg)

```
map --scan-delay 10s -p22,23,80  10.0.2.4
```
* Issues a 10 second scan delay between ports when scanning
* Helps avoid detection
![nmap practice](\assets\img\nmappractice5.jpg)

```
nmap -T4 10.0.2.0/24
 ```
* Issues scanning probes with a timing pattern to avoid detection
* 0 is the slowest, 5 is the fastest.
![nmap practice](\assets\img\nmappractice3.jpg)

```
nmap -sI 10.0.2.0/24 
```
* A stealth scan method
* Makes the can appear as if it is coming from somewhere else.
* Hides the identity of the scanning machine

```
nmap 10.0.2.0/24 -oN 'Desktop/output.txt' 
nmap 10.0.2.0/24 -oX 'Desktop/output.xml'
nmap 10.0.2.0/24 -oG 'Desktop/output.txt'
```
* Outputs nmap to a text file, xml file or greppable output respectively. 
These file formats can then be ingested by a SIEM.

## Nmap Port Scans
Port scanning allows us to determine which services and which version of the services are in use by a given host.

```
nmap 10.0.2.4 -sS
```
* TCP SYN, sends a half-open scan to identify the port state
* Does not send an ACK packet afterwards.
* May require admin priveleges on the system
* More of a stealthy approach

```
nmap 10.0.2.4 -sT
```
* TCP Connect Scan
* Sends the full 3 way handshake, SYN and SYNACK
* Does not require raw packet priveleges on a workstation
* Establishes a connection with a connect system call.

```
nmap 10.0.2.4 -sN
```
* A null scan
* Conducts  scan by sending the header bit set to zero

```
nmap 10.0.2.4 -sF
```
* Conducts a scan by sending an unexpected FIN packed
* Not stealthy

```
nmap 10.0.2.4 -sX
```
* Christmas scan. Least stealthy, will set off alarm bells.
* Sends a packet by sending a packet with the FIN, PSH and URG flags set to one.

```
nmap 10.0.2.4 -sU
```
* UDP Scan
* Sends a UDP packet and waits for the packet to timeout, since there is no handshake.

```
nmap 10.0.2.4 -p80,22,23,443,53
```
* Scan a pre-specified port range.
![nmap practice](\assets\img\nmappractice6.jpg)

## Nmap Fingerprinting Scans
Fingerprinting is a technique for collecting detailed information about an individual target.
Nmap uses the Common Platfor Enumeration (CPE) scheme a standard scheme for identifying devices, operating systems and applications. Nmap compares responses to the CPE in order to determine the version of a service running.

```
nmap 10.0.2.4 -sV
```
* Provides basic versioning info for ports, services and OS
![nmap practice](\assets\img\nmappractice7.jpg)

```
nmap 10.0.2.4 -A
```
* Provides detailed versioning info

## Nmap Port States
When performing fingerprinting and scanning ports, nmap will return a range of different port states.

Standard States:
* Open - Application on the host is accepting connections
* Closed - Port not open; responds to probes with a rest RST packet
* Filtered - Nmap cannot probe the port; possibly due to a firewall

Other States:
* Unfiltered - Rare; Nmap can probe the port, but not determine if open or closed.
* Open|Filtered - Nmap cannot determine if a port is open or filtered
* Open|Closed - Nmap cannot determine if a port is closed or filtered when doing a TCP idle scan

## TCP Dump
TCP dump is a utility that records the contents of packets on a network interface. 
Here I will combine TCPdump with nmap in order to record the packet traces of a scan.

```
sudo tcpdump -i eth0
```
* Basic syntax to start tcpdump on the interface ethernet0. 

```
sudo tcpdump -i eth0 src 10.0.2.15
```
* Start tcpdump on the interface ethernet0. 
* Only collects packets with a source of 10.0.2.15, i.e. my current system

Here is an example of running tcpdump while conducting an nmap sV scan against the 10.0.2.0/24 subnet:
![nmap practice](\assets\img\nmappractice8.jpg)

```
sudo tcpdump -i eth0 dst 10.0.2.4
```
* Start tcpdump on the interface ethernet0. 
* Only collects packets with a destination of 10.0.2.15, i.e. a target VM system

```
sudo tcpdump -i eth0 dst 10.0.2.4 -w /home/kali/Desktop/metasploitableVM.pcap
```
* Start tcpdump on the interface ethernet0. 
* Only collects packets with a destination of 10.0.2.15, i.e. a target VM system
* Saves the capture to a file called "metasploitableVM.pcap" on my desktop

```
sudo tcpdump -r /home/kali/Desktop/metasploitableVM.pcap
```
* Reads the file just created via packet capture

```
sudo tcpdump dst port 23 -r /home/kali/Desktop/metasploitableVM.pcap
```
* Reads the file just created via packet capture
* Filters on traffic to port 23 for Telnet

```
sudo tcpdump dst port 23 -r /home/kali/Desktop/metasploitableVM.pcap
```
* Reads the file just created via packet capture
* Filters on traffic to port 23 for Telnet
* Shows the traffic in HEX and ASCII.

```
tcpdump -i en0 10.0.2.4
```
* Starts tcpdump on interface en0 and records all traffic going to 10.0.2.4

## Grep
Command line tool on unix-based systems that invokes simple string matching and 
regex syntax. Using grep we can retrieve specific lines from the nmap and tcpdump commands
run previously.

```
grep -F 23/tcp output.txt
```
* -F stands for simple search
* Searches all lines in output.txt for the '23/tcp', i.e. telnet.
![grep practice](\assets\img\greppractice.jpg)


```
grep '23' *
```
* Searches all files in the current working directory for '23', i.e. telnet
* Prints the output to the screen, showing which are files and which are directories.
![grep practice](\assets\img\greppractice2clear.jpg)

```
grep -r 10\.0\.2\.[0-255] output2grep.txt
```
* searches the output2grep.txt file for valid ip addresses in the above range
* -r is used for regular expressions
* \ is used as an escape character for regular expressions
![grep practice](\assets\img\greppractice5.jpg)

```
grep -i
```
* -i ignores case sensivity.
* grep is case sensitive by default

```
rep -v '80/tcp' output.txt
```
* returns non-matching lines
* Excludes every line with has tcp 80
![nmap practice](\assets\img\greppractice3.jpg)

```
grep -v -E '22|25|53|80' output.txt
```
* Returns all lines that do not match 22, 25, 53 or 80
* This could be used to narrow a search and look for specific ports.
![nmap practice](\assets\img\greppractice3.jpg)

```
grep -w
```
* treats search strings as distinct words

```
grep -c
```
* returns a count of matching words

```
grep -l
```
* returns names of files with matching lines

```
grep -L
```
* returns names of files without matching lines

## cut
A command that enables a user to specify which text on a line which can be removed

```
cut -c5 syslog.txt
```
Returns only the fifth character in each line from the syslog file

```
cut -c5-5 syslog.txt
```
Return only the 5-10 characters from each line in the file

## sort
Can be used to change the output order of a file

```
sort syslog.txt
```
Returns the file in alphabetical order

```
sort -r syslog.txt
```
Returns the file in reverse alphabetical order

```
sort -n syslog.txt
```
Returns the file in numerical order

```
sort -k 2 syslog.txt
```
Returns sorted based on the 2nd column

## head & tail
Returns the first 10 or last 10 lines of a file specified
```
head syslog.txt
tail syslog.txt
```



