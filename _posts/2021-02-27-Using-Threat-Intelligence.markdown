---
layout: post
title:  "Using Threat Intelligence"
date:   2021-03-03 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---

## Activity 2.1 Explore the ATT&CK Framework

In this activity I will analyze a threat using the [MITRE ATT&CK framework](https://attack.mitre.org/). For this
example, I will use the 2019 Capital One Data Breach.

### Threat Profile

On July 29 2019, Capital One Bank announced a data breach which affected approximately 100 million individuals
in the USA and 6 million in Canada. The personal information of individuals who applied for Credit Cards was exfiltrated in the breach. The vulnerability leading to the incident was stated by Capital One as a specific configuration vulnerability in their infrastructure. This vulnerability was leveraged by an individual to extract info on March 22 to 23 2019. Capital One was made aware of the breach from an anonymous email to their reponsible disclosure program. Data was encrypted at rest, however the perpetrator was able to decrypt some of the data. Data fields such as Social security numbers and account numbers remained encrypted.

The threat actor was determined to by Paige Thompson, a former Amazon Web Services Engineer. Thompson leveraged a misconfiguration in the bank's ModSecurity Web Application Firewall in order to execute commands on the firewall to exfiltrate the data from Capital One's Amazon S3 buckets. The misconfiguration of the WAF was key in the exfiltration of the data. Specifically, the WAF was assigned excessive permissions which allowed it to read and list the files in any S3 bucket. The type of vulnerability exploited is a "Server Side Request Forgery" or SSRF. This is attack manipulates a server or in this instance the WAF into running unintended commands. The attack then ran an S3 sync command in order to copy the data to their server. Logs from Capital One show Thompson connected several time from both TOR exit nodes and a VPN service in order to cover her tracks.

In regards to evidence, Thompson was confirmed to have a list of over 700 AWS buckets. Logs at the bank were also able to show attempted connections from TOR exit nodes which listed bucket content. Other commands executed on the buckets originated from a VPN service which Thompson subscribes too. After the breach, Thompson also posted to several public forums, i.e. Slack and GitHub boasting about the breach. 
Thompson was arrested by the FBI. During the raid, FBI agents seized several storage devices containing copies of the breached data


### Analysis

The following is a mapping of the above threat profile to the MITRE ATT&CK framework. 

<table>

    <tr bgcolor="grey" style="color:white;">
        <td><b>Stage</b></td> 
        <td><b>Step of the attack</b></td>
        <td><b>ATT&CK</b></td>
    </tr>

    <tr>
        <td>Initial Access</td> 
        <td>Utilize the AWS Command Line Credentials, AccessKeyID and SecretAccessKey, in order to execute commands</td>
        <td>Valid Accounts</td>
    </tr>

    <tr>
        <td>Intial Access</td> 
        <td>Use previous work experience building Capital One Cloud Infraastrucutre in order to plan attack</td>
        <td>Trusted Relationship</td>
    </tr>

    <tr>
        <td>Execution</td> 
        <td>Execute several AWS CLI commands in order to exfiltrate data</td>
        <td>Exploitation for Client Execution</td>
    </tr>

    <tr>
        <td>Credential Access</td> 
        <td>Leverage misconfiguration of WAF to relay commands to AWS services in order to retrieve credentials</td>
        <td>Exploitation for Credential Access</td>
    </tr>

    <tr>
        <td>Discovery</td> 
        <td>run CLI commands in order to list content of AWS storage prior to exfiltration</td>
        <td>Cloud Infrastructure Discovery</td>
    </tr>

    <tr>
        <td>Exfiltration</td> 
        <td>Use the sync command to copy AWS data to local storage</td>
        <td>Exfiltration Over Alternative Protocol </td>
    </tr>

</table>

This diagram breaks down the approximate timeline the attacker followed when exfiltrating the data.

![Cap One Timeline](\assets\img\capone-timeline.png)

### Sources
[http://web.mit.edu/smadnick/www/wp/2020-16.pdf](http://web.mit.edu/smadnick/www/wp/2020-16.pdf)<br>
[https://krebsonsecurity.com/2019/08/what-we-can-learn-from-the-capital-one-hack/](https://krebsonsecurity.com/2019/08/what-we-can-learn-from-the-capital-one-hack/)<br>
[https://www.zdnet.com/article/100-million-americans-and-6-million-canadians-caught-up-in-capital-one-breach/](https://www.zdnet.com/article/100-million-americans-and-6-million-canadians-caught-up-in-capital-one-breach/)<br>
[https://www.capitalone.com/about/newsroom/capital-one-announces-data-security-incident/](https://www.capitalone.com/about/newsroom/capital-one-announces-data-security-incident/)<br>
[https://www.justice.gov/usao-wdwa/pr/seattle-tech-worker-arrested-data-theft-involving-large-financial-services-company](https://www.justice.gov/usao-wdwa/pr/seattle-tech-worker-arrested-data-theft-involving-large-financial-services-company)<br>

## Activity 2.2 Set Up a STIX/TAXII Feed

In this acitivity I will setup the Anomali STAXX in order to process STIXX feeds for threat intel. 
Anomali STAXX allows you to connect to STIX/TAXII servers and poll threat intelligence from them. 
Using STAXX, you are able to run keyword searches on vulnerabilities.

1. Installation

For this setup you will need either VMWare Workstation or Oracle VirtualBox. 
Anomali STAXX can be downloaded from [here](https://www.anomali.com/resources/staxx/download-staxx).

Once downloaded open the virtual appliance and be sure to assign it 4GB of RAM and 2 CPUs. If you are using VirtualBox, 
check that the network adapter is set to Bridged. Start the VM.

2. Before logging in to Anomali and gathering threat data, first I will set the timezone. Login to the VM server with the username/password displayed at the top of the VM.

![Anomali Password Change](\assets\img\anomali1.jpg)

Reset the password as prompted.

![Anomali Password Change](\assets\img\anomali2.PNG)

Once logged in, use the following command to change your timezone.

sudo timedatectl set-timezone (your timezone)

Timezones can be listed with the folllowing command

`timedatectl list-timezones`


3. The VM will then display an IP address for you to connect to locally. 

Login to the interface with a webrowser using the following credentials:

User name: admin
Password: changeme

After that, set your own password -- and you are good to go. 

2. After logging in, agree use the Limo service to gather data for your first threat feeds.
The dashboard will then begin to populate with threat feeds!

![Anomali Limo Accept](\assets\img\anomali3.jpg)


## Glossary

A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.todayscybersecurityanalyst %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>