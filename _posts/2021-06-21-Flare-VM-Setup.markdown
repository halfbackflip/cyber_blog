---
layout: post
title:  "Flare VM Setup"
date:   2021-06-21 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---

## Overview

This is a walkthrough for installing Flare VM in Windows 10 on VirtualBox.
FlareVM is a free Windows-based malware analysis toolkit by FireEye! Lets get started.

Additional Info about Flare VM can be found here:
[https://www.fireeye.com/blog/threat-research/2017/07/flare-vm-the-windows-malware.html](https://www.fireeye.com/blog/threat-research/2017/07/flare-vm-the-windows-malware.html)

Github Repo:
[https://github.com/fireeye/flare-vm/blob/master/README.md](https://github.com/fireeye/flare-vm/blob/master/README.md)

## Requirements
* Type 2 Hypervisor (VirtualBox will be used for this walkthrough, although VMWare, Hyper, KVM, etc. can also be used.)
* Virtual Machine Settings:
    - 80 GB Virtual Hard Disk
    - 4 GB RAM (8 Reccommended)
    - 1 CPU (2 Reccommended)

## VM Download and Preparation
* Download a free Windows VM from Microsoft:
[https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/)

* Select Windows 10 w/Edge and use the following settings for VirtualBox.
![Win 10 VM](\assets\img\flarevm.png)

* In VirtualBox, create a NAT Network to place the Flare VM on. Network settings can be modified later when conducting Malware analysis. 
 File > Preferences > Network > +
![Win 10 VM](\assets\img\flarevm1.png)

* Import the Windows 10 OVF into VirtualBox. File > Import Appliance.  Leave the settings as default. 

* In VirtualBox, make the following setting changes:
    - Display > Screen > Graphics Controller > VBoxSVGA
    - Display > Screen > Video Memory > 60 MB
    - Right Click > Settings > Network > Attached to: "NAT Network" > Select the NAT Network Created Earlier.
![Win 10 VM](\assets\img\flarevm2.png)

By Default, the Windows VM may not come with sufficient storage.
* In VirtualBox Naviagate to Settings > Storage. Check if the storage is at least 80 gb. 
![Win 10 VM](\assets\img\flarevm3.png)

* If the storage is under 80 GB, copy the location to your VDI in the settings, open command prompt and run the following commands to re-size the disk:
    - Navigate to the directory where VirtualBox is installed. In my case:
    ```
    cd C:\Program Files\Oracle\VirtualBox
    ```
    - Execute VBoxManage.exe to resize the disk. 
    ```
    VBoxManage.exe modifyhd <path to vdi> --resize <amount>
    VBoxManage.exe modifyhd "C:\Users\DavidReiling\VirtualBox VMs\FlareVM - Win10\MSEdge - Win10-disk001.vdi" --resize 81920
    ```
    - Verify the VM was resized in settings. 

* Settings > Storage. Insert an Optical Drive in the VM and add "the Guest Additions CD Image". Once ran, Guest additions will allow for copy/pasting into the VM and setting display scaling.
![Win 10 VM](\assets\img\flarevm4.png)

* Start the VM. The default username/password should be "IEUser" and "Passw0rd!"

* If you resized the disk earlier, open Disk Management. Right click > C:\ > Extend Volume and extend the volume into the unallocated space.

* From the Virtual Box Manager Window > Devices > Optical Drive > VBoxGuestAdditions.iso. Run the guest additions .iso and restart it.

* Once the VM is restarted, from the Virtual Box Manager Window > Devices > Optical Drive > Remove Disk

## Disabling Windows Defender
Next we will disable Windows Defender and other Windows Security Features inside this VM in order to execute malware.

* Follow the below instructions and add the C:/ drive as an exclusion:
[https://support.microsoft.com/en-us/windows/add-an-exclusion-to-windows-security-811816c0-4dfd-af4a-47e4-c301afe13b26](https://support.microsoft.com/en-us/windows/add-an-exclusion-to-windows-security-811816c0-4dfd-af4a-47e4-c301afe13b26)

* Here is a screenshot of the completed settings:
![Win 10 VM](\assets\img\flarevm7.png)

* From the Virus & threat protection settings page, perform the following actions:
    - Disable real-time protection
    - Disable cloud-delivered protection
    - Disable automatic sample submission
    - Add the directory C:\ to the exclusion list
![Win 10 VM](\assets\img\flarevm6.png)

* Finally, you may need to disable Windows Defender via the Group Policy Editor. [Follow these excellent instructions](https://samsclass.info/126/proj/PMA40.htm) Below is a summary of these instructions.
    - Open a command prompt as administrator. Enter gpedit.msc.
    - Navigate to: Computer Security Policy > Computer Configuration > Adminstrative Templates > Windows Components > Windows Defender Antivirus. Double click "Windows Defender Antivirus"
    - Double-click "Turn off Windows Defender Antivirus" > in the "Turn off Windows Defender Antivirus" box, click Enabled.
    ![Win 10 VM](\assets\img\flarevm8.png)
    - Disable Windows Defender SmartScreen. Follow the following path: Local Computer Security Policy > Computer Configuration > Adminstrative Templates > Windows Components > File Explorer > double-click "Configure Windows Defender Smartscreen".
    - Double-click "Configure Windows Defender Smartscreen" in the "Configure Windows Defender Smartscreen" box, click Disabled, as shown below and then click OK. 
    ![Win 10 VM](\assets\img\flarevm9.png)

## Installing Flare VM Tools
* Before install, create a snapshot. VirtualBox > Machine > Tools > Snapshots > Take
* Download and extract the github code from the FlareVM repo inside your VM:
[https://github.com/fireeye/flare-vm/blob/master/README.md](https://github.com/fireeye/flare-vm/blob/master/README.md)
* Run Powershell as administrator and navigate to the extracted directory. 
* Follow the readme instructions on the FlareVM GitHub page or downloaded repo for setup. As of this writing the approximate instructions are:
    - Download and copy install.ps1 onto your new VM
    - Open PowerShell as an Administrator
    - Unblock the install file by running:
    - Unblock-File .\install.ps1
    - Enable script execution by running:
    - Set-ExecutionPolicy Unrestricted
    - Finally, execute the installer script as follow: .\install.ps1
    - You can also pass your password as an argument: .\install.ps1 -password <password>
* Flare VM setup scripts will now run. This may take some time (1-3 hours) and involve some restarts of the VM, grab some popcorn. 
* Once completed the flare VM desktop wallpaper should appear.
* IMPORTANT: Once everything is finished take a snapshot as a baseline for the VM.
* There should be a shortcut on the desktop. According to the FlareVM page, here is a list of the malware analysis tools installed on the VM:
    - Disassemblers:
        * IDA Free 5.0 and IDA Free 7.0
        * Binary Ninja
        * Radare2 and Cutter
        * Debuggers:
        * OllyDbg and OllyDbg2
        * x64dbg
        * Windbg
    - File Format parser:
        * CFF Explorer, PEView, PEStudio
        * PdfStreamdumper, pdf-parser, pdfid
        * ffdec
        * offvis and officemalscanner
        * PE-bear
    - Decompilers:
        * RetDec
        * Jd-gui and bytecode-viewer
        * dnSpy
        * IDR
        * VBDecompiler
        * Py2ExeDecompiler
    - Monitoring tools:
        * SysInternal suite
        * RegShot
    - Utilities:
        * Hex Editors (010 editor, HxD and File Insight)
        * FLOSS (FireEye Labs Obfuscated String Solver)
        * Fakenet-NG
        * Yara
        * Malware Analyst Pack

## Guidelines for opening malicious stuff.
* Always have a baseline snapshot to restore to afterwards
* Before opening something dodgey, adjust your network settings:
    * Change the network adapter to "Host Only" if you want a competely isolated environment with no network connection.
    * Change the network adapter to "" in order to allow internet access only
* Files can be dragged/dropped by Devices > Drag and Drop > Bi Directional
    * After dragging a malcious file into the VM, disable this feature.