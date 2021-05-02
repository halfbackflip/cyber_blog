---
layout: post
title:  "Performing Forensic Analysis and Techniques"
date:   2021-04-25 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---

## Activity 13.1 Create a Disk Image
In this excercise I will go over how to use the linux dd tool to create a disk image and then verify the image's integrity.

1. First, I will start my Kali Linux VM. (Although any linux distro can be used)
2. Next mount a USB flashdrive into the VM. In VirtualBox, flash drives can be mounted by clicking the little USB icon and selecting the drive you would like to mount.
![Forensics](\assets\img\forensics.jpg) 
3. If you have issues mounting the USB, you may need to install VirtualBox guest additions first.[https://www.virtualbox.org/manual/ch04.html](https://www.virtualbox.org/manual/ch04.html)
4. Once the USB file is mounted, it should appear on the desktop or filesystem. Verify the files are contained inside the USB drive.
![Forensics](\assets\img\forensics2.jpg)
5. Next make a directory to hold the copy of your disk image. I have created a directory called tmp in my home directory.
`mkdir ~/tmp` 
6. Next we will create an MD5 checksum of the USB drive before creating the image. This checksum will be used to validate the integrity of the drive. 
I will create an MD5 checksum called "...original.md5". Here is the command I used. 
Note: you may need to elevate your priveleges in order to run this command. Let the command run, as it could take some time to generate. 
![Forensics](\assets\img\forensics4.jpg)
7. Lets check the md5 generated. 
![Forensics](\assets\img\forensics5.jpg)
8. Now I will clone the the USB drive. Here is the command:
`dd if=/dev/disk/by-label/CYSA of=~/tmp/excercise7_1_disk.img bs=64k`
Breaking down each part of the command:
* `if=/dev/disk/by-label/CYSA` - Sets the input file by drive label name
* `of=~/tmp/excercise7_1_disk.img` - Sets the path of the output file
* `bs=64k` - Set the block size of the copy, if you know the block seize of the copy, this will result in a performance increase.
9. Wait until the command completes. There is not progress bar, but once the command completes, the following output will apppear.
![Forensics](\assets\img\forensics6.jpg)
10. Now I will create an md5 checksum of the clone. 
![Forensics](\assets\img\forensics7.jpg)
11. Last, I will compare md5 checksums the clone and original in order to cerify the integrity of the forensic copy.
```
more excercise7_1_clone.md5 
more excercise7_1_original.md5
```


## Activity 13.2 Conduct the NIST Rhino Hunt
In this excercise we will go on the great NIST Rhino Hunt. 
NIST provides numerous practice forensic images which can be downloaded to hone your skills.

1. First we will download SIFT. A forensics toolkit VM by the SANS institute. According to SANS SIFT is "The SIFT Workstation is a group of free open-source incident response and forensic tools designed to perform detailed digital forensic examinations in a variety of settings." After creating a free community membership on the SANS site, the SIFT can be downloaded as an OVA appliance from:
[https://digital-forensics.sans.org/community/downloads](https://digital-forensics.sans.org/community/downloads)
2. Once downloaded, the appliance can be imported into Virtualbox with File > Import Appliance
![Forensics](\assets\img\sift.jpg)
3. Start the VM. Login with a password of forensics. You should be greeted with a screen similar to this:
![Forensics](\assets\img\sift2.jpg) 
4. From within this VM, we will download and extract the SANS Rhino Hunt disk image. This can easily be downloaded from:
`wget http://www.cfreds.nist.gov/dfrws/DFRWS2005-RODEO.zip`
5. Navigate to the disk image's directory in the terminal. Now mount the Rhino USB image.
`sudo mount RHINOUSB.dd /mnt/usb`
6. Navigate to the folder to view the pictures. Note there are only two files in this directory. What has been deleted?
`ls /mnt/usb`
7. Lets recover the deleted files. Create a directory for the output
`mkdir output`
8. Run the foremost utility against the RHINOUSB utility. The foremost utility automatically recovers files based on headers and other info. If you recieve the processing image below, the utility has been successful.
`foremost -o output/ RHINOUSB.dd`
![Forensics](\assets\img\sift3.jpg) 
9. Navigate to the output folder and open the new document recovered from the USB image. 
![Forensics](\assets\img\sift4.jpg) 

## Glossary
A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.foresnicanalysis %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>