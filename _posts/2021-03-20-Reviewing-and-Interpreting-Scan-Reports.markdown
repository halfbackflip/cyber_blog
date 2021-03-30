---
layout: post
title:  "Reviewing and Interpreting Scan Reports"
date:   2021-03-20 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---

## Activity 5.1: Interpret a Vulnerability Scan
This exercise requires reviewing the results of the previous vulnerability scan and developing a remediation plan.

### Severity of Vulnerabilities
The report output lists a total of 165 vulnerabilities. The critical and high sections of the report list 18 vulnerabilities as critical and 26 as high. 
![Nessus Report](\assets\img\nessusreport.jpg)

Critical vulnerabilities score as a CVSS of 10.0 and higher. These will require immediate remediation. The critical vulnerabilities consist of Adobe Flash Player and missed Windows security updates. Remediating these issues will require downloading the latest Windows Server Updates. This process is straightforward. 
![Nessus Report](\assets\img\nessusreport2.jpg)

Within the console, under the the remediation tab, Nessus lists the remediation steps involved. 
![Nessus Report](\assets\img\nessusreport3.jpg)

## Activity 5.2: Analyze a CVSS Vector
First, I will provide an overview of CVSS. Using the CVSS information, I will examine a serious vulnerability present on my
Windows 10 system.

### CVSS Overview

The Common Vulnerability Scoring System or CVSS is an industry standard for scoring vulnerabilities. 
The score is then used to prioritize remediation actions. 

The CVSS attack vector metric is used to calculate a score for a given vulnerability. 
Version 3.0 is the latest version. In order to explain CVSS, I will review the CVSS score for
[CVE-2020-1472](https://nvd.nist.gov/vuln/detail/CVE-2020-1472) also known as Zero Logon.

CVE-2020-1472 has a base score of 10, the highest possible score. 
The CVSS vector is listed as "CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H"

But what does this mean?

Each abbreviation between each pair of "/.../" refers to a metric for evaluating the attack. I think a diagram best explains this...

![cvss diagram](\assets\img\cvssdiagram.png)

Luckily NIST provides a handy calculator which explains each metric, calculates the final score 
and explains the underlying equations.
[Nist Cacluator](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)

The equations are summarized as:

<div style="font-family: Times New Roman, font-size: 10">
CVSS v3.1 Equations

The CVSS v3.1 equations are defined below.
Base

The Base Score is a function of the Impact and Exploitability sub score equations. Where the Base score is defined as,
    If (Impact sub score <= 0)     0 else,
    Scope Unchanged4                 𝑅𝑜𝑢𝑛𝑑𝑢𝑝(𝑀𝑖𝑛𝑖𝑚𝑢𝑚[(𝐼𝑚𝑝𝑎𝑐𝑡 + 𝐸𝑥𝑝𝑙𝑜𝑖𝑡𝑎𝑏𝑖𝑙𝑖𝑡𝑦), 10])
    Scope Changed                      𝑅𝑜𝑢𝑛𝑑𝑢𝑝(𝑀𝑖𝑛𝑖𝑚𝑢𝑚[1.08 × (𝐼𝑚𝑝𝑎𝑐𝑡 + 𝐸𝑥𝑝𝑙𝑜𝑖𝑡𝑎𝑏𝑖𝑙𝑖𝑡𝑦), 10])

and the Impact sub score (ISC) is defined as,

    Scope Unchanged 6.42 × 𝐼𝑆𝐶Base
    Scope Changed 7.52 × [𝐼𝑆𝐶𝐵𝑎𝑠𝑒 − 0.029] − 3.25 × [𝐼𝑆𝐶𝐵𝑎𝑠𝑒 − 0.02]15

Where,

    𝐼𝑆𝐶𝐵𝑎𝑠𝑒 = 1 − [(1 − 𝐼𝑚𝑝𝑎𝑐𝑡𝐶𝑜𝑛𝑓) × (1 − 𝐼𝑚𝑝𝑎𝑐𝑡𝐼𝑛𝑡𝑒𝑔) × (1 − 𝐼𝑚𝑝𝑎𝑐𝑡𝐴𝑣𝑎𝑖𝑙)]

 And the Exploitability sub score is,

    8.22 × 𝐴𝑡𝑡𝑎𝑐𝑘𝑉𝑒𝑐𝑡𝑜𝑟 × 𝐴𝑡𝑡𝑎𝑐𝑘𝐶𝑜𝑚𝑝𝑙𝑒𝑥𝑖𝑡𝑦 × 𝑃𝑟𝑖𝑣𝑖𝑙𝑒𝑔𝑒𝑅𝑒𝑞𝑢𝑖𝑟𝑒𝑑 × 𝑈𝑠𝑒𝑟𝐼𝑛𝑡𝑒𝑟𝑎𝑐𝑡𝑖𝑜𝑛
Temporal
The Temporal score is defined as,

    𝑅𝑜𝑢𝑛𝑑𝑢𝑝(𝐵𝑎𝑠𝑒𝑆𝑐𝑜𝑟𝑒 × 𝐸𝑥𝑝𝑙𝑜𝑖𝑡𝐶𝑜𝑑𝑒𝑀𝑎𝑡𝑢𝑟𝑖𝑡𝑦 × 𝑅𝑒𝑚𝑒𝑑𝑖𝑎𝑡𝑖𝑜𝑛𝐿𝑒𝑣𝑒𝑙 × 𝑅𝑒𝑝𝑜𝑟𝑡𝐶𝑜𝑛𝑓𝑖𝑑𝑒𝑛𝑐𝑒)
Environmental
The environmental score is defined as,

    If (Modified Impact Sub score <= 0)     0 else,

    If Modified Scope is Unchanged           Round up(Round up (Minimum [ (M.Impact + M.Exploitability) ,10]) × Exploit Code Maturity × Remediation Level × Report Confidence)
    
    If Modified Scope is Changed               Round up(Round up (Minimum [1.08 × (M.Impact + M.Exploitability) ,10]) × Exploit Code Maturity × Remediation Level × Report Confidence)

And the modified Impact sub score is defined as,

    If Modified Scope is Unchanged 6.42 × [𝐼𝑆𝐶𝑀𝑜𝑑𝑖𝑓𝑖𝑒𝑑]
    
    If Modified Scope is Changed 7.52 × [𝐼𝑆𝐶𝑀𝑜𝑑𝑖𝑓𝑖𝑒𝑑 − 0.029]-3.25× [𝐼𝑆𝐶𝑀𝑜𝑑𝑖𝑓𝑖𝑒𝑑 × 0.9731 − 0.02] 13

Where,
    𝐼𝑆𝐶𝑀𝑜𝑑𝑖𝑓𝑖𝑒𝑑 = 𝑀𝑖𝑛𝑖𝑚𝑢𝑚 [[1 − (1 − 𝑀. 𝐼𝐶𝑜𝑛𝑓 × 𝐶𝑅) × (1 − 𝑀. 𝐼𝐼𝑛𝑡𝑒𝑔 × 𝐼𝑅) × (1 − 𝑀. 𝐼𝐴𝑣𝑎𝑖𝑙 × 𝐴𝑅)], 0.915]

The Modified Exploitability sub score is,

    8.22 × 𝑀. 𝐴𝑡𝑡𝑎𝑐𝑘𝑉𝑒𝑐𝑡𝑜𝑟 × 𝑀. 𝐴𝑡𝑡𝑎𝑐𝑘𝐶𝑜𝑚𝑝𝑙𝑒𝑥𝑖𝑡𝑦 × 𝑀. 𝑃𝑟𝑖𝑣𝑖𝑙𝑒𝑔𝑒𝑅𝑒𝑞𝑢𝑖𝑟𝑒𝑑 × 𝑀. 𝑈𝑠𝑒𝑟𝐼𝑛𝑡𝑒𝑟𝑎𝑐𝑡𝑖𝑜n

4 Where “Round up” is defined as the smallest number, specified to one decimal place, that is equal to or higher than its input. For example, Round up (4.02) is 4.1; and Round up (4.00) is 4.0.
</div>

## Activity 5.3: Remediate a Vulnerability
In the final step, I will select a vulnerabilitiy and remediate it. 
Lets see if CVE-2020-1472 is present on my Windows Server. 

1. Create a new scan. Scrolling down to the bottom of the scan templates, we can see "Zerologon Remote Scan" specifcally designed to detect the zero logon vulnerability. 
![Zero Logon](\assets\img\zerologon.jpg)

2. Input the ip address and then launch the scan.

3. Since my server is unpatched the scan will return positive.
![Zero Logon Confirmation](\assets\img\zerologon2.jpg)

### Remediaition Steps

Remediating Zerologon requires downloading the latest updates from microsoft.com.
Grab some popcorn. Download the updates. 
![Windows Update](\assets\img\windowsupdate.jpg)

### Confirmation of Remediation

Once the updates are downloaded, I re-run the scan... and it is gone!
![Windows Update](\assets\img\zerologon3.jpg)

## Glossary

A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.analyzingvulnerabilityscans %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>