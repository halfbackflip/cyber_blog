---
layout: post
title:  "Reconnaissance and Intelligence Gathering"
date:   2021-03-07 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---

## Activity 3.1: Port Scanning

In this lab I will use my Kali Linux VM to perform a port scan on my intentionally vulnerable Metasploitable VM.
I will record the results and use the scan to determine the operating system's version.

The IP addresses of the two VMs are as follows:
Metasploitable: 10.0.2.4
Kali Linux: 10.0.2.15

Performing a simple port scan is... quite simple. 
`nmap 10.0.2.4`

The result is as follows. Oh my, look at all these exposed ports! 
![Nmap Output](\assets\img\nmap.jpg)

This is just a quick scan, we can also use nmap to scan ALL open ports and determine the operationg system. This will command will need to be with administrator permissions.
`nmap 10.0.2.4 -O -p 1-65535`

Note the difference in the results below. The full scan adds a section below determining the OS type of the target and additional ports were scanned. 
![Nmap Output2](\assets\img\nmap2.jpg)

It is also worth exploring some additional options with nmap. We can re-run our scan and include -sV in order to determine the service versions running on the target host. This can take significantly longer. It is best used if you are looking at specific hosts.
![Nmap Output3](\assets\img\nmap3.jpg)

In several situations you may need at output file for further anaylsis or to put into another script. Adding the "oA" option will output to .nmap, .xml and .gmap formats at once.
`nmap 10.0.2.4 -oA`

## Activity 3.2: Write an Intelligence Gathering Plan

In this excercise, I will write a brief passive and active intelligence gathering plan for an organisation. I will create a fictional company for this example. For developing this plan, I will refer guidelines set out in the [Open Source Security Test Methodology Manual](https://www.isecom.org/OSSTMM.3.pdf). 

## Target Summary
A leading manufacturer of construction and plumbing supplies. The company employs over 2000 full-time staff with. headquarters located in Illinois. In addition to their Headquarters, the company also operates branch offices in Texas, Florida and North Carolina. On an international scale, the company operates a global manufacturing and distribution network. 

## Passive Data Reconnaissance 

Passive Intelligence Gathering: Relying on existing information that is avilable about the organization, systems or network. 

### Methods

At this stage, we will assume we have no internal network access. First we will gather non-technical information from publicly available sources. These sources will include: the company website, office address information, satellite imagery of locations, public legal information, quarterly financial reports, company social media accounts, downloading mobile applications, social media accounts of key employees, cold calling office locations, checking DNS records, viewing previous version of the website using WayBackMachine, analyses of website document metadata and business registration information along with other sources.

After this intel is compiled, the following documentation will be produced:

* List of key business locations with opening hours, satellite imagery and snapshot of building security
* Organisational charts for company management and departments, i.e. finance, IT and HR
* Social media graphs for identified company employees
* Summary of key legal, business and financial info
* DNS info

### Tools

The following tools will be used in order to perform passive intel gathering:
* Web Browser
* Google Earth
* Relevant public legal, financial and business info
* Creepy geolocation tool
* Metasploit tools for social engineering
* Dig & Whois DNS lookups
* Web Browser Developer Tools

## Active Data Reconnaissance 

Active Intelligence Gathering: Intentionally deploying host scanning tools in order to gain info about systems, services
and vulnerabilities.

### Methods

In this part of the intelligence gathering exercise, we will assume that we obtained non-privileged access to the company network. 

The initial step will involve mapping the network. We will use tools such as nmap, zenmap and Angry IP scanner to create a Network Topology map. The network discovery scans will not check for open ports and system information. Once a draft topology has been created, we will perform the network discovery scan from several different hosts on the network. As firewalls and other network devices can impede scanning, performing discovery scans from hosts on different points in the network will allow us to fill in the gaps.

Once the topology and IP address ranges are mapped out, we will then attempt to identify the key hosts on the network, such as servers, domain controllers, firewalls, routers and switches. Once the key hosts are identified, we will then target our scans at these hosts specifically. Scans on these hosts will look to determine: 1. Open TCP & UDP Ports 2. Versions of services running on scanned ports 3. OS version on each host 4. Host Type: On-prem? Virtual Machine? Cloud? Printer? Other? During the intrusive scanning process, alarms, notifications, alerts or logging may be triggered. Any applications or systems which produce this data will be recorded and marked on the network topology map.

As this company operates several manufacturing and logistics warehouses, we anticipate discovering numerous embedded systems or SCADA systems. As embedded systems can be particularly vulnerable since they often do not receive patching or frequent updates, any scanned host which appears to belong in this category will be flagged for additional investigation.

After this intel stage is completed, the following documentation will be produced:

After this intel stage is compelted, the following documentation will be produced:
1. Network Topology Map
2. Identification of Key Hosts
3. Profile of Key Hosts, i.e. open ports, os type and service versions
4. List of SCADA and embedded systems

### Tools
The following tools will be used in order to perform active intel gathering:
* Angry IP Scanner
* Nmap
* Zenmap

### Final Analysis
After passive and active intelligence gathering is performed, the final step will be to analyze combined data returned from the active and passive intelligence gathering. This procedure will be completed using the Combined Trifecta and 4 Point Process as outlined in the OSSTMM. 

![Trifecta Process](\assets\img\trifectaprocess.jpg)

### Sources
[https://www.isecom.org/OSSTMM.3.pdf](https://www.isecom.org/OSSTMM.3.pdf)

## Glossary

A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.reconnaissanceintelligencegathering %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>