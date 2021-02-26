---
layout: post
title:  "Today's Cybersecurity Analyst"
date:   2021-02-23 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---

## Activity 1.1: Create an Inbound Firewall Rule
Here is a short guide on how to create an inbound firewall that blocks file and printer sharing on 
Windows Server 2019. I am following the lab excercise steps for Activity 1.1 These instructions are 
abbreviated. After completing this in the GUI, I will include a powershell script.

1. Windows Key > Windows Defender Firewall

2. Select > "Allow an app or feature through Windows Defender Firewall"

3. Tick Box > "File and Printer Sharing" Click "OK" to apply the setting.
![File and printer sharing](\cyber_blog\assets\img\File and printer sharing.jpg)

4. Notice that File and printer sharing are is now listed as "Private" in the right column. This means only
file and printer sharing for systems on the same network are allowed.

5. If we navigate to Windows Defender Firewall > Advanced Settings > Outbound Rules > File and Printer Sharing. We can also see the Private Rules are now active with a green tick mark.
![File and printer sharing](\cyber_blog\assets\img\File and printer sharing 2.jpg)

6. This could become a very tedious task if performed at scale. Thankfully this is a one-liner in powershell. Run the below as admin:

    `Set-NetFirewallRule -DisplayGroup "File And Printer Sharing" -Enabled False -Profile Any`

7. Status can be then verified in powershell:

    `Get-NetFirewallRule -DisplayGroup "File and Printer Sharing`

8. Here is a sample screenshot. Magic!

![Lab Diagram](\cyber_blog\assets\img\File and printer sharing 3.jpg)

## Activity 1.2: Create a Group Policy Object

Next we are going to create a Group Policy Object and edit its contents to enforce an organization's password
policy. I currently have a domain running called "forestcity.com." You will need to setup a domain before completing this excercise. 

1. Open the Group Policy Management Console. Windows Key > Group Policy Management

2. Expand the folder corresponding to your AD forest

3. Open your domain folder

4. Right click the group policy objects > New

![Group Policy Object](\cyber_blog\assets\img\gpo.jpg)

5. Name the policy "Password Policy" > Click "Ok"

6. Expand the following folders > Policies > Windows Settings > Security Settings > Account Policies

![Group Policy Object](\cyber_blog\assets\img\gpo2.jpg)

7. Click the Password Policy

8. Double click max password age

9. Select Define This Policy Setting > Expiration value to 90 days

10. OK to close the window > Ok to Accept the change

11. Double-click the minimum Password Length Option

12. Click the box in the policy setting > Set the min length to 12 characters

13. Click OK to Close the window > Double-click the password must meet complexity requirements option

14. The policy should appear as per below. It can now be applied to users, groups or displayed as a trophy for our secure infraastructure. 

![Group Policy Object](\cyber_blog\assets\img\gpo3.jpg)

15. Finally, if we want to automate this... [maybe start here?](https://docs.microsoft.com/en-us/powershell/module/grouppolicy/new-gpo?view=win10-ps)


## Activity 1.3: Make a Penetration Testing Plan

Now, of course I will not actually do this. But this is what a plan might look like. I paid special attention to timing, scope and authorization. Missing one of these crucial aspects could cause serious issues, right?

The general standard for conducting a penetration test is contained in the [NIST SP 800-115: Technical Guide to
Information Security Testing and Assessment](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-115.pdf). NIST divides Penetration testing into the following phases:

![NIST Pen Testing Four Phases](\cyber_blog\assets\img\nistpentesting.jpg)

I will follow these four phases as per below, using a fictional company which I invented myself.
This fictional company comes straight from my brain.  

### Case Study

A start-up company has emerged to manage the distribution and delivery of vaccine levels on a global scale in response to the Covid 19 pandemic of 2020. The company has developed a SaaS solution for vaccine suppliers to monitor and manage their deliveries to healthcase instituions. The solution provides dashboards and analytics for tracking the delivery at every stage. In addition, the solution allows for the onboarding of outside suppliers, sends real-time notifications and allows the sharing of information to other vaccine suppliers. The goal of the company is to ensure the vaccine is distributed effeciently and equitably on a global scale. Currently, the company uses a multi-cloud enviroment to develop and test the software solution. They also operate several on-premises infraastrucutre for IT administration, i.e. Active Directory, Exchange, File Servers and VPN gateways.

### Pen Testing Plan

#### Planning

##### Timing

Perparation for the penetration test will require 1 month from commencement of the contract. The preparation for the test will involve the team conducting active reconaissance and performing discovery activities on the company's network.

##### Scope

The test scope has been determined to include the following:

* LAN: All routers, switches, WAPs, IOT Devices on premise at the company's single location are subject to the test.
* Cloud: The company runs workloads in both AWS and Azure. Production and development workloads are to be excluded from test. Test workloads, which have been tagged, are viable targets
* End-User Devices: Company end user devices will not be subject to any intrusive pentration techniques which could disrupt productivity
* Human Resources Requred: The penetration testing team is to conduct the entire test without assistance from internal IT staff. The IT staff have been made aware of the possibility of the event occurring within the next month. 

##### Authorization

Upon signing of the agreement, full authorisation will be provided to conduct the assessment.

#### Discovery

During the discovery phase, the penetration testing team will gather information about the network, systems, users and applications in the system. 

##### Network

Network recon will be the first step of the discovery phase. The penetration testing team will leverage port scanning, network traffic sniffing, exploits on network equipment in order to develop a layout of the company's network. Further recon on systems, users and applications wil be performed based on this layout.

##### Systems

The term systems will apply to both cloud and on-prem systems. On-prem systems will be subject to vulnerability scans and custom scripts designed to elicit system information. Cloud systems acessible from the on-prem network and from the internet will be probed using web vulnerability scanners. Systems which may hold backups, logs, crednetials and critical user information will be identified as crown jewels for the attack phase. 

##### Users

Key employees in the organisation will be identified and ranked according to their possible access. This ranking will determine which employees are targeted by generic phishing attacks or more targeted spear phishing attempts.

##### Application

Key IT infraastruce running the company's SaaS will be identified for additional reconaissance. This includes the database, front-facing web servers, jump boxes, firewalls, WAFs and so on.

#### Attack

In regards to timing, Management will be notified of a three week window in which an attack will occurr. The exact day and time of the attack will not be announced. Several attacks will take place across the network, systems, users and applications.

#### Reporting

Post reporting will be delivered one week after the test. The following information will be provided:

* List of attacked features
* Summary of attack results and vulnerabilities used
* Breakdown of attack methodology
* Quantitative and qualitative review of attack success
* Overall assessment of the comapny's security posture
* Reccomendations to improve security
* Final conclusions


<!-- ## Welcome! 

This is the first of a series of posts on my journey to pass the [CompTIA CySA+](https://www.comptia.org/certifications/cybersecurity-analyst).
I plan on taking my time and working through the material at a slow place. Sure, I could 
cram and pass the test in a few weeks... but would the certificate have any value if I lacked any of the knowledge associated with it?

These posts will focus on me completing the various lab excercises in this book [CompTIA CySA+ Study Guide](https://www.amazon.com.au/CompTIA-CySA-Study-Guide-CS0-001/dp/1119348978)
If you are using these posts as a guide to your own studies, I will assume that you have fundamental knowledge about networking, virtual machines and security. 

I will also add a glossary of terms and some miscellaneous cloud/security/IT ramblings.
Who knows what could happen? 

## Lab Setup

Here is a diagram of the initial virtual environment I will be using. This will change. I have setup a Nat Network of 10.0.2.0/24 within VirtualBox, this will place the
VMs on their own segmented network while still being NAT'd out to my real network. Refer to the VirtualBox documentation [here](https://www.virtualbox.org/manual/ch01.html#globalsettings) for setting up a NAT network. 

![Lab Diagram](/cyber_blog/assets/img/labdiagram.png)

## Required Software

Here are links for the main components of this architecture setup. Keep mind this can also
be run on VMWare, HyperV or in your own closet.

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - Type 2 Hypervisor software.

* [Kali Linux VirtualBox Image](https://www.offensive-security.com/ kali-linux-vm-vmware-virtualbox-image-download/) - Some kind of Linux security people use.

* [Metasploitable](https://sourceforge.net/projects/metasploitable/) - An intentionally vulnerable Linux virtual machine.

* [Windows Server 2019](https://www.microsoft.com/en-AU/windows-server/trial) - For testing Windows vulnerabilities.

Thats it. Now off we go.  -->