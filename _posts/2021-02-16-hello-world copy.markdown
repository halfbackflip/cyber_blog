---
layout: post
title:  "Starting the CYSA"
date:   2021-02-19 05:58:00 +1100
categories: cysa
published: true
---

# Welcome! 

This is the first of a series of posts on my journey to pass the [CompTIA CySA+](https://www.comptia.org/certifications/cybersecurity-analyst).
I plan on taking my time and working through the material at a slow place. Sure, I could 
cram and pass the test in a few weeks... but would the certificate have any value if I didn't attain any of the knowledge associated with it?

These posts will focus on me completing the various lab excercises in the [CompTIA CySA+ Study Guide](https://www.amazon.com.au/CompTIA-CySA-Study-Guide-CS0-001/dp/1119348978)
If you are using these posts as a guide to your own studies, I will assume that you have fundamental knowledge about networking, virtual machines and security. 

I will also add a glossary of terms and some miscellaneous cloud/security/IT ramblings.
Who knows what could happen? 

# Lab Setup

Here is a diagram of the virtual environment I will be using. This will change as I progress. I have setup a Nat Network of 10.0.2.0/24 within VirtualBox, this will place the
VMs on their own segmented network while still being NAT'd out to my real network. Refer to the VirtualBox documentation [here](https://www.virtualbox.org/manual/ch01.html#globalsettings) for setting up a NAT network. 

![Lab Diagram](../assets/img/labdiagram.png)

## Required Software

Here are links for the main components of this architecture setup. Keep mind this can also
be run on VMWare, HyperV or in your own closet.

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - Type 2 Hypervisor software.

* [Kali Linux VirtualBox Image](https://www.offensive-security.com/ kali-linux-vm-vmware-virtualbox-image-download/) - Some kind of Linux security people use.

* [Metasploitable](https://sourceforge.net/projects/metasploitable/) - An intentionally vulnerable Linux virtual machine.

* [Windows Server 2019](https://www.microsoft.com/en-AU/windows-server/trial) - For testing Windows vulnerabilities.

Thats it. Now off we go. 