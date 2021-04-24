---
layout: post
title:  "Security Operations and Monitoring"
date:   2021-04-18 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---
## Activity 10.1 Analyze a Network Capture File
In this excercise, I will analyze network capture data.
The tool I will use for analyzing the data will be [wireshark](https://www.wireshark.org/download.html). 

### Part 1: Select a packet capture
[Malware Traffic Analysis](https://www.malware-traffic-analysis.net/training-exercises.html) as the name suggests, provides samples of malware traffic for analysis and training purposes. 

I will be analyzing the traffic in this excercise:
[https://www.malware-traffic-analysis.net/training-exercises.html](https://www.malware-traffic-analysis.net/training-exercises.html)

Also, I highly reccommend setting up your columns in Wireshark properly:
[https://www.malware-traffic-analysis.net/tutorials/wireshark/index.html](https://www.malware-traffic-analysis.net/tutorials/wireshark/index.html)
### Part 2: Analyze the Traffic

## Scenario
The alert provided in this task is as follows:
![alert](\assets\img\2021-02-08-traffic-analysis-exercise-alerts.jpg)

LAN Data:
LAN segment range:  10.2.8.0/24 (10.2.8.0 through 10.2.8.255)
Domain:  ascolimited.com
Domain controller:  10.2.8.2 - AscoLimited-DC
LAN segment gateway:  10.2.8.1
LAN segment broadcast address:  10.2.8.255

## Timeline

* 2021-02-08 15:59 UTC
    - 10.2.8.101 communicates with 162.241.149.1 over 443. The external IP appears to be using a Lets Encrypt Free SSL Certificate
* 2021-02-08 16:00 UTC
    - Tordal/Hancitor/Chanitor Checkin: This is a known malware downloader utility
    - src_ip: 10.2.8.101
    - dest_ip: 54.235.147.252
    - URL retrieved: http://api.ipify.org/
    - This activity can be observed with the following wireshark filter by destination:
    ![alert](\assets\img\wireshark.jpg)
* 2021-02-08 16:00 UTC
    - Shellcode is detected from 198.211.10.238
* 2021-02-08 16:00 UTC
    - 198.211.10.238 connects via a reverse shell to 10.2.8.101
* 2021-02-08 16:00 UTC
    - 198.211.10.238 initiates Cobalt Strike
    - Cobalt strike deploys the 'Beacon' agent
    - Cobalt strike allows for: command execution, key logging, file transfer, SOCKS proxying, privilege escalation, mimikatz, port scanning and lateral movement, etc. 
    - Cobalt Strike is a paid malware package
* 2021-02-08 16:00 UTC
    - Cobalt Strike initiates a DLL or EXE download from 8.208.10.147
* 2021-02-08 16:00 UTC
    - "Flicker Stealer" is activated. Flicker stealer is a "an MaaS (Malware as a Service) stealer that is sold on hacking forums. Its main goal is to steal sensitive information cached by the user - specifically browser passwords -  and send it back to the virus’ owner."
* 2021-02-08 16:01 UTC
    - Flicker Stealer sends sensitive info back to 198.211.10.238
* 2021-02-08 16:01 UTC
    - Compromised system 10.2.8.101 begins communicating with the domain controller 10.2.8.1
    - Files sent to server over SMB2
* 2021-02-08 16:01 UTC 
    - First appearence of the RPC_NETLOGON protocol.
    - Attempted exploitation of the domain controller. Requesting domain info.
    - [https://www.crowdstrike.com/blog/cve-2020-1472-zerologon-security-advisory/](https://www.crowdstrike.com/blog/cve-2020-1472-zerologon-security-advisory/)
    - [https://frsecure.com/blog/cve-2020-1472/](https://frsecure.com/blog/cve-2020-1472/)

## Findings
Executive Summary:
From 17:59 UTC, the host at IP address 10.2.8.101 was infected with Hancitor, Cobalt Strike and Flicker Stealer Malware. 
After being infected with Flicker Stealer, the attacker also attempted to compromise the domain controller. 

Details:
IP Address: 10.2.8.101
Host name: DESKTOP-MGVG60Z
Windows Account: bill.cook

Indicators of Commpromise:

Tordal/Hancitor/Chanitor Checkin:
- 45.124.85.55 intiates download of Malware at 15:59 UTC
- 54.235.147.252 sends beaconing signals after download.

Cobalt Strike:
- 198.211.10.238 port 8080 installs Cobalt strike at 16:00 UTC

Flicker Stealer:
- 8.208.10.147 port 80 intiates a download of Flicker Strike DLL / EXE at 16:01 UTC

## Activity 10.2 Analyze a Phishing Email

For this activity, I will analyze a phishing email I pulled from my spam inbox. 
![Phishing](\assets\img\phishing.jpg)

### Part 1: Manually Analyze an email header
First I will manually analyze the email header of the phishing email. 
Note I have replaced my actual email with XXXX@email.com.
Here is the full header information:

```
Delivered-To: XXXX@email.com
Received: by 2002:a5e:8909:0:0:0:0:0 with SMTP id k9csp859350ioj;
        Wed, 31 Mar 2021 13:19:37 -0700 (PDT)
X-Google-Smtp-Source: ABdhPJwLcyjVC5HJ1WGEuysDkoz5/zl1HJsd7Ly+Gpe6ZxDkaXRxdJkR7ivoJA2MXu6IUv9byxmt
X-Received: by 2002:a5d:58fc:: with SMTP id f28mr5422737wrd.180.1617221977399;
        Wed, 31 Mar 2021 13:19:37 -0700 (PDT)
ARC-Seal: i=1; a=rsa-sha256; t=1617221977; cv=none;
        d=google.com; s=arc-20160816;
        b=VpIhnFI3KKR2jedeOKsDfD8/3tQVNlmsYUmd3JB9kQDcUv5Sr9TxL87jV+aUw2Ds/o
         KkXsoOyOumlV0MtPlDrU1WE0a/SmxK+MxPBkWcG/30H4yQitg8/mk9/ET3wxemE93ESi
         N56/un+G7aQl64Hn2LDTv+5C2yLsFy/nvoKtvHIOD4fBiVFYYjZIYMO6Z8aLXl20ZN6A
         QmqYLJ0TcXtCpksNZn79b3mgbdzBkoKOtBrgNBNh3yKjWcTeLuWN6/oAvYo0Zmr3vO6M
         9RJCX1VL2hnCxOLZ2uodIAUnXNFNeOS4OEVKkM+CaKK7iz3+YihNr88Wr80TRmvh5pAV
         5EOQ==
ARC-Message-Signature: i=1; a=rsa-sha256; c=relaxed/relaxed; d=google.com; s=arc-20160816;
        h=mime-version:content-transfer-encoding:subject:to:from:message-id
         :errors-to:date;
        bh=ypHEL79sTaQyvaL6eeLNKJnRKsWRdUMDRD1VKmvoREU=;
        b=bzoDnoG54kwDEkHQd2u1tn9HKmyCAXrnVaD2wykeg3aHHORb3X+8EEgBqjK9wTzytd
         7qgnLVHrVKTceNiDKeYnM0Ec+XOLged9GV8GgdkOPsdGmgR02Euwq2XcpoCA9U7vR0Ct
         MUxaPrLUynBfH0mO4bJcf/exxN8S5n1dzn6UiUmli7dn+iL1+stqea4WNWUeHKPtEhWH
         MDapTgqY44EyHO7fR2YroLAsOZtqr4hX2YYxdxOqfkt6eZ6NAoo2T3NWcUwrTZVk1TPu
         W7N+l9waKMTYAIXVGeQtGafMkRVMUZonUessqUv6E2RnyLq+sQubWl83upGPpqJi8rSS
         wCQg==
ARC-Authentication-Results: i=1; mx.google.com;
       spf=pass (google.com: domain of return.qoxmjm2gjnxmtl4ajn0qtl2adm0kjn5ato@hgak7fvtf158dp.r7e3-f3e2.rwhtilbp.ga designates 2602:fed2:7300:548:7e3:f3e2:0:1 as permitted sender) smtp.mailfrom=return.QOxMjM2gjNxMTL4AjN0QTL2ADM0kjN5ATO@hgak7fvtf158dp.r7e3-f3e2.rwhtilbp.ga
Return-Path: <return.QOxMjM2gjNxMTL4AjN0QTL2ADM0kjN5ATO@hgak7fvtf158dp.r7e3-f3e2.rwhtilbp.ga>
Received: from 2602fed27300054807e3f3e200000001.rwhtilbp.ga (2602fed27300054807e3f3e200000001.rwhtilbp.ga. [2602:fed2:7300:548:7e3:f3e2:0:1])
        by mx.google.com with ESMTPS id r5si3160359wrz.349.2021.03.31.13.19.36
        for <XXXX@email.com>
        (version=TLS1_2 cipher=ECDHE-ECDSA-AES128-GCM-SHA256 bits=128/128);
        Wed, 31 Mar 2021 13:19:37 -0700 (PDT)
Received-SPF: pass (google.com: domain of return.qoxmjm2gjnxmtl4ajn0qtl2adm0kjn5ato@hgak7fvtf158dp.r7e3-f3e2.rwhtilbp.ga designates 2602:fed2:7300:548:7e3:f3e2:0:1 as permitted sender) client-ip=2602:fed2:7300:548:7e3:f3e2:0:1;
Authentication-Results: mx.google.com;
       spf=pass (google.com: domain of return.qoxmjm2gjnxmtl4ajn0qtl2adm0kjn5ato@hgak7fvtf158dp.r7e3-f3e2.rwhtilbp.ga designates 2602:fed2:7300:548:7e3:f3e2:0:1 as permitted sender) smtp.mailfrom=return.QOxMjM2gjNxMTL4AjN0QTL2ADM0kjN5ATO@hgak7fvtf158dp.r7e3-f3e2.rwhtilbp.ga
Date: Wed, 31 Mar 2021 19:49:15 GMT
Return-Path: return.QOxMjM2gjNxMTL4AjN0QTL2ADM0kjN5ATO@hgak7fvtf158dp.r7e3-f3e2.rwhtilbp.ga
Errors-To: return.QOxMjM2gjNxMTL4AjN0QTL2ADM0kjN5ATO@hgak7fvtf158dp.r7e3-f3e2.rwhtilbp.ga
Message-Id: <21fbd7f483f47e3028870175d435b0@hgak7fvtf158dp.r7e3-f3e2.rwhtilbp.ga>
From: "'SmileDirectClub Publisher'" <sdc44608@hgak7fvtf158dp.w7e3-f3e2.rwhtilbp.ga>
To: XXXX@email.com
Subject: Clear Aligners are All the Rage - Get Your At-Home Kit Today
Content-Type: text/html; charset=utf-8
Content-Transfer-Encoding: quoted-printable
MIME-Version: 1.0
```
There are two main aspects which point to a phishing email inside the header.

1. Email originates from a suspicious sender domain name which does not match the display name of 'SmileDirectClub Publisher'
    ```sdc44608@hgak7fvtf158dp.w7e3-f3e2.rwhtilbp.ga```

2. The email passes the SPF check, but it desgingates an IPV6 address as the return address. Looking up this address, provides... nothing.
    ```Received-SPF: pass (google.com: domain of return.qoxmjm2gjnxmtl4ajn0qtl2adm0kjn5ato@hgak7fvtf158dp.r7e3-f3e2.rwhtilbp.ga designates 2602:fed2:7300:548:7e3:f3e2:0:1 as permitted sender) ```

### Part 2: Analyze the email content

Checking over the email content is the next step. 
Note: I am not responsible if you visit one of these links... and get rolled. 
The content for this email is as follows:

```
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body style=3D=22 padding: 0;margin: 0; background-color:#ffffff; =
font-size: 14px;=22>
    <table cellpadding=3D=220=22 cellspacing=3D=220=22 style=3D=22width: =
620px; background-color:#5701ff;font-size: 14px; font-family: sans-serif; =
margin:0 auto; border: 1px solid #000;=22>
      <tbody>
        <tr>
          <td>
            <table border=3D=220=22 cellpadding=3D=220=22 =
cellspacing=3D=220=22 style=3D=22padding: 22px;=22>
              <tbody>
                <tr style=3D=22background-color: #ffffff;  =22>
                  <td style=3D=22border-radius:10px 10px 0  0 ;=22>
                    <table>
                      <tbody>
                        <tr>
                          <td style=3D=22 padding-left: 30px; padding-top: =
50px; padding-right: 50px;=22><a href=3D=22http://hgak7fvtf158dp.w7e3-f3e2[.]=
rwhtilbp.ga/cMGUAAKTDAAC2neMSx3UFeA/=22 style=3D=22 text-decoration:none; =
display: inline-block; color: #5901ff;font-size: 4em;line-height:1; =
font-weight: bold; =22>free 30-second smile assessment </a>
                          </td>
                        </tr>
                        <tr>
                          <td style=3D=22 padding-left: 30px; padding-top: =
25px; color: #5600ff; font-size: 1.9em;line-height:1;letter-spacing: =
1px;=22>
                            <p style=3D=22 padding: 0; margin: 0;=22>get =
started on the smile you&#39;ll
                            </p>
                            <p style=3D=22 padding: 0; margin: 0; =
padding-top: 10px;letter-spacing: 1px;=22>love in just three easy steps.
                            </p>
                          </td>
                        </tr>
                        <tr>
                          <td style=3D=22padding-left: 30px; padding-top: =
50px; padding-bottom: 40px;=22><a href=3D=22http://hgak7fvtf158dp[.]w7e3-f3e2=
.rwhtilbp.ga/cMGUAAKTDAAC2neMSx3UFeA/=22 style=3D=22 text-decoration:none; =
display: inline-block; background-color: #6400ff; color: #ffffff;font-size:=
 1.4em;line-height:1; font-weight: bold; border-radius: 5px; padding: 12px =
15px;=22>ORDER YOUR IMPRESSION KIT &gt;&gt; </a>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </td>
                </tr>
                <tr>
                  <td style=3D=22background-color: #d9d9d7; border-radius:0=
  0 10px 10px  ;=22>
                    <table>
                      <tbody>
                        <tr>
                          <td style=3D=22padding-left: 30px;=22>
                            <p style=3D=22 padding-top: 30px; font-size: 1.=
2em;line-height:1;color: #3a3a3a;letter-spacing: 1px;=22>which one best =
describes your teeth crowding=3F
                            </p><a href=3D=22http://hgak7fvtf158dp[.]=
w7e3-f3e2.rwhtilbp.ga/cMGUAAKTDAAC2neMSx3UFeA/=22 style=3D=22 =
text-decoration:none; display: inline-block;=22><img alt=3D=22=22 =
src=3D=22http://hgak7fvtf158dp[.]w7e3-f3e2.rwhtilbp.ga/img-cMGUAAKTDAAC2neMSx=
3UFeA/tooth1.jpg=22 width=3D=22519=22 height=3D=22106=22 =
style=3D=22display:block;=22 /></a>
                          </td>
                        </tr>
                        <tr>
                          <td style=3D=22padding-left: 30px; =
padding-bottom: 50px;=22>
                            <p style=3D=22 padding-top: 30px; font-size: 1.=
2em;line-height:1;color: #3a3a3a;letter-spacing: 1px;=22>which one best =
describes your teeth spacing=3F
                            </p><a href=3D=22http://hgak7fvtf158dp[.]=
w7e3-f3e2.rwhtilbp.ga/cMGUAAKTDAAC2neMSx3UFeA/=22 style=3D=22 =
text-decoration:none; display: inline-block; =22><img alt=3D=22=22 =
src=3D=22http://hgak7fvtf158dp[.]w7e3-f3e2.rwhtilbp.ga/img-cMGUAAKTDAAC2neMSx=
3UFeA/tooth2.jpg=22 width=3D=22520=22 height=3D=22107=22 =
style=3D=22display:block;=22 /> </a>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </td>
                </tr>
                <tr>
                  <td style=3D=22color: #ffffff; padding-left: 30px; =
font-size: 2em;line-height:1;=22>
                    <p style=3D=22 padding: 0; margin: 0;padding-top: 30px;=
 =22>use code <span style=3D=22font-weight: bold;=22> <a =
href=3D=22http://hgak7fvtf158dp[.]w7e3-f3e2.rwhtilbp.ga/cMGUAAKTDAAC2neMSx3UF=
eA/=22 style=3D=22 text-decoration:none; font-weight: bold;color: =
#ffffff;=22>SDCAFF50</a> </span> for <span style=3D=22font-weight: =
bold;=22> 50% off</span>
                    </p>
                    <p style=3D=22padding: 0; margin: 0;padding-top: 10px; =
=22>impression kit
                    </p>
                  </td>
                </tr>
                <tr>
                  <td style=3D=22padding: 50px 0 50px 30px;=22><a =
href=3D=22http://hgak7fvtf158dp[.]w7e3-f3e2.rwhtilbp.ga/cMGUAAKTDAAC2neMSx3UF=
eA/=22 style=3D=22 text-decoration:none; display: inline-block;=22><img =
alt=3D=22=22 src=3D=22http://hgak7fvtf158dp[.]w7e3-f3e2.rwhtilbp.=
ga/img-cMGUAAKTDAAC2neMSx3UFeA/logosmlt.gif=22 width=3D=22122=22 =
height=3D=2291=22 style=3D=22display:block;=22 /></a>
                  </td>
                </tr>
              </tbody>
            </table>
          </td>
        </tr>
      </tbody>
    </table><center>
    <p align=3D=22center=22>=F0=9D=98=9B=F0=9D=98=A9=F0=9D=98=AA=
=F0=9D=98=B4 =F0=9D=98=AA=F0=9D=98=B4 =F0=9D=98=A2=F0=9D=98=AF =
=F0=9D=98=A2=F0=9D=98=A5=F0=9D=98=B7=F0=9D=98=A6=F0=9D=98=B3=F0=9D=98=B5=
=F0=9D=98=AA=F0=9D=98=B4=F0=9D=98=A6=F0=9D=98=AE=F0=9D=98=A6=F0=9D=98=AF=
=F0=9D=98=B5. =F0=9D=98=9B=F0=9D=98=A9=F0=9D=98=AA=F0=9D=98=B4 =
=F0=9D=98=A6=F0=9D=98=AE=F0=9D=98=A2=F0=9D=98=AA=F0=9D=98=AD =
=F0=9D=98=B8=F0=9D=98=A2=F0=9D=98=B4 =F0=9D=98=B4=F0=9D=98=A6=F0=9D=98=AF=
=F0=9D=98=B5 =F0=9D=98=A3=F0=9D=98=BA =F0=9D=98=A2 3=F0=9D=98=B3=
=F0=9D=98=A5 =F0=9D=98=B1=F0=9D=98=A2=F0=9D=98=B3=F0=9D=98=B5=F0=9D=98=BA =
=F0=9D=98=AE=F0=9D=98=A2=F0=9D=98=B3=F0=9D=98=AC=F0=9D=98=A6=F0=9D=98=B5=
=F0=9D=98=AA=F0=9D=98=AF=F0=9D=98=A8 =F0=9D=98=B1=F0=9D=98=A2=F0=9D=98=B3=
=F0=9D=98=B5=F0=9D=98=AF=F0=9D=98=A6=F0=9D=98=B3.
      <br />=F0=9D=98=90=F0=9D=98=AF =F0=9D=98=B0=F0=9D=98=B3=F0=9D=98=A5=
=F0=9D=98=A6=F0=9D=98=B3 =F0=9D=98=B5=F0=9D=98=B0 =F0=9D=98=B3=
=F0=9D=98=A6=F0=9D=98=AE=F0=9D=98=B0=F0=9D=98=B7=F0=9D=98=A6 =
=F0=9D=98=BA=F0=9D=98=B0=F0=9D=98=B6=F0=9D=98=B3=F0=9D=98=B4=F0=9D=98=A6=
=F0=9D=98=AD=F0=9D=98=A7 =F0=9D=98=A7=F0=9D=98=B3=F0=9D=98=B0=F0=9D=98=AE =
=F0=9D=98=A7=F0=9D=98=B6=F0=9D=98=B5=F0=9D=98=B6=F0=9D=98=B3=F0=9D=98=A6 =
=F0=9D=98=A6=F0=9D=98=AE=F0=9D=98=A2=F0=9D=98=AA=F0=9D=98=AD=F0=9D=98=B4, =
<a href=3D=22http://hgak7fvtf158dp[.]w7e3-f3e2.rwhtilbp.=
ga/cMGUAAKTDAAC2neMSx3UFeA/unsub=22>=F0=9D=98=B1=F0=9D=98=AD=F0=9D=98=A6=
=F0=9D=98=A2=F0=9D=98=B4=F0=9D=98=A6 =F0=9D=98=A8=F0=9D=98=B0 =
=F0=9D=98=A9=F0=9D=98=A6=F0=9D=98=B3=F0=9D=98=A6</a>.
      <br />=F0=9D=98=96=F0=9D=98=B3 =F0=9D=98=B8=F0=9D=98=B3=F0=9D=98=AA=
=F0=9D=98=B5=F0=9D=98=A6 =F0=9D=98=B5=F0=9D=98=B0: 2803 =
=F0=9D=98=97=F0=9D=98=A9=F0=9D=98=AA=F0=9D=98=AD=F0=9D=98=A2=F0=9D=98=A5=
=F0=9D=98=A6=F0=9D=98=AD=F0=9D=98=B1=F0=9D=98=A9=F0=9D=98=AA=F0=9D=98=A2 =
=F0=9D=98=97=F0=9D=98=AA=F0=9D=98=AC=F0=9D=98=A6, =F0=9D=98=9A=
=F0=9D=98=B6=F0=9D=98=AA=F0=9D=98=B5=F0=9D=98=A6 =F0=9D=98=89 #258, =
=F0=9D=98=8A=F0=9D=98=AD=F0=9D=98=A2=F0=9D=98=BA=F0=9D=98=AE=F0=9D=98=B0=
=F0=9D=98=AF=F0=9D=98=B5, =F0=9D=98=8B=F0=9D=98=8C 19703
    </p>
    <div style=3D=22min-height:179px;height:199px;margin-bottom:59px;=22>
&nbsp;
    </div>
    <p>&nbsp;
    </p>
    <p>&nbsp;
    </p>
    <p>&nbsp;
    </p>
    <p>&nbsp;
    </p>
    <p>&nbsp; <img alt=3D=22=22 border=3D=220=22 class=3D=22ComingLike=22 =
src=3D=22http://hgak7fvtf158dp.w7e3-f3e2.rwhtilbp[.]ga/cMGUAAKTDAAC2neMSx3UFe=
A/t.gif=22 style=3D=22height:1px;width:1px;=22 /> <a =
href=3D=22http://hgak7fvtf158dp.w7e3-f3e2.rwhtilbp[.]ga/cMGUAAKTDAAC2neMSx3UF=
eA/optt=22><img alt=3D=22=22 src=3D=22http://hgak7fvtf158dp.w7e3-f3e2.=
rwhtilbp.ga/img-cMGUAAKTDAAC2neMSx3UFeA/prfott.jpg=22 width=3D=22314=22 =
height=3D=2226=22 /></a>
    </p></center>
  </body>
</html>
```

The mail indicator that something is very wrong with this email is the same dodgey malicious link 
sprayed in the body, images and footer. If the email was even close to legitimate, different sections in 
the email would link to different parts of a website, right?
```http://hgak7fvtf158dp[.]=w7e3-f3e2.rwhtilbp.ga/cMGUAAKTDAAC2neMSx3UFeA/=22```

Another strange aspect of this email is the disclaimer is encoded. The html text below which clearly appears in the above screenshot:
```
𝘛𝘩𝘪𝘴 𝘪𝘴 𝘢𝘯 𝘢𝘥𝘷𝘦𝘳𝘵𝘪𝘴𝘦𝘮𝘦𝘯𝘵. 𝘛𝘩𝘪𝘴 𝘦𝘮𝘢𝘪𝘭 𝘸𝘢𝘴 𝘴𝘦𝘯𝘵 𝘣𝘺 𝘢 3𝘳𝘥 𝘱𝘢𝘳𝘵𝘺 𝘮𝘢𝘳𝘬𝘦𝘵𝘪𝘯𝘨 𝘱𝘢𝘳𝘵𝘯𝘦𝘳.
<br />𝘐𝘯 𝘰𝘳𝘥𝘦𝘳 𝘵𝘰 𝘳𝘦𝘮𝘰𝘷𝘦 𝘺𝘰𝘶𝘳𝘴𝘦𝘭𝘧 𝘧𝘳𝘰𝘮 𝘧𝘶𝘵𝘶𝘳𝘦 𝘦𝘮𝘢𝘪𝘭𝘴, <a href="http://hgak7fvtf158dp[.]w7e3-f3e2.rwhtilbp.ga/cMGUAAKTDAAC2neMSx3UFeA/unsub">𝘱𝘭𝘦𝘢𝘴𝘦 𝘨𝘰 𝘩𝘦𝘳𝘦</a>.
<br />𝘖𝘳 𝘸𝘳𝘪𝘵𝘦 𝘵𝘰: 2803 𝘗𝘩𝘪𝘭𝘢𝘥𝘦𝘭𝘱𝘩𝘪𝘢 𝘗𝘪𝘬𝘦, 𝘚𝘶𝘪𝘵𝘦 𝘉 #258, 𝘊𝘭𝘢𝘺𝘮𝘰𝘯𝘵, 𝘋𝘌 19703
```
Is fully encoded with several lines similar to:
```=F0=9D=98=9B=F0=9D=98=A9=F0=9D=98=AA=[...]```

This is presumably to avoid detectiond by Google's automated spam detection tools.
Note the URL in the above message is again the same dodgey one as before.

### Part 3: Use an automated tool

There are several automated tools which can also be used to analyze email headers.

Here are a few:
[https://toolbox.googleapps.com/apps/messageheader/](https://toolbox.googleapps.com/apps/messageheader/)
[https://mxtoolbox.com/EmailHeaders.aspx](https://mxtoolbox.com/EmailHeaders.aspx)
[https://www.whatismyip.com/email-header-analyzer/](https://www.whatismyip.com/email-header-analyzer/)

A tool such as the mxtoolbox analyzer will output a clearly organized table with all the headers displayed.
The tool can also check the email for DMARC compliance. 

![alert](\assets\img\emailheaderanalyze.jpg)

## Glossary
A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.securityoperationsandmonitoring %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>