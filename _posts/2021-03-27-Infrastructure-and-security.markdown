---
layout: post
title:  "Infrastructure and Security Controls"
date:   2021-03-30 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---

## Activity 7.1: Review an Application Using the OWASP Attack Surface Analysis Cheat Sheet
In this excercise I will review an application against the [OWASP Attack Surface Analysis Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Attack_Surface_Analysis_Cheat_Sheet.html).

## Cheat Sheet Summary

The OWASP Attack Surface Analysis sheet focuses on documenting all externally vulnerable aspects of a system to protect it from attack. This analysis may omit internal or insider attacks since these forms of attack may require a seperate analysis. After the analysis, areas of a network which need to additional controls or testing are identified.

OWASP describes the attack surface for an application as:

* Sum of all paths for data/commands into and out of the application, and
the code that protects these paths. 
* All valuable data used in the application, including secrets and keys, intellectual property, critical business data, personal data and PII.
* The code that protects this data (including encryption and checksums, access auditing, and data integrity and operational security controls).

The OWASP approach also reccommends grouping each type of attack point into buckets or categories based on risk:
* External (or Internal)
* Purpose
* Implementaiton
* Design
* Technology

In order to map vulnerable parts of the application which are accessible over the web, the following web app testing tools can be used:
* [OWASP ZAP](https://www.zaproxy.org/)
* [Arachni](https://www.arachni-scanner.com/)
* [Skipfish](https://code.google.com/archive/p/skipfish/)
* [w3af](https://code.google.com/archive/p/skipfish/)

These tools will allow you to map entries points such as User interface (UI) forms and fields, HTTP headers and cookies, APIs, files, databases, local storage, email, messaging, runtime arguements and more.

Two final important points areas to consider when developing conculusions are backups and an application's attack surface growing over time. Backups of code, data and infraastructure are essential for a quick recovery in the event of any attack. Ideally multiple backups should be available, both online and offline. Second, application attack surfaces often increase over time. Proactive audits over time should be used to identify exposed parts of an application which are no longer used and can be removed.

## Building the Web App

For this excercise, I chose to not only analyze an app but also build the app myself. As most modern web apps are build on the cloud... and that cloud is either AWS or Azure... I will be building the web app listed in this [AWS project](https://aws.amazon.com/getting-started/hands-on/build-modern-app-fargate-lambda-dynamodb-python/). Before the web app is deployed, I will perform a brief analysis of both the external and internal attack surfaces. After my analysis, I will run a scan using Arachni in order to compare my findings with the scan report. 

The architecture is as follows:
![AWS Web App](\assets\img\awswebapp.png)

## Initial Analysis

Here is my analysis of the attack surface as presented by the diagram.  
<table>
    <tr bgcolor="grey" style="color:white;">
        <td>Attack Surface</td>
        <td>Vulnerable Aspects</td>
    </tr>
    <tr bgcolor="white">
        <td>External</td>
        <td>
            <ol>
                <li>Cognito - Password forms vulnerable to brute force, mitm or injection attacks</li>
                <li>API GW - Exposed APIs vulnerable to abuse, injection attacks, etc. </li>
                <li>S3 Bucket - Ensure CORS policy is set, set bucket policy to limit API calls to storage.</li>
                <li>Cloud9 - AWS console access vulnerable to brute force, injection attacks, mitm credenetial hijacking.</li>
                <li>Network Load Balancer - Vulnerable to DOS attacks</li>
            </ol>
        </td>
    </tr>
    <tr>
        <td>Internal</td>
        <td>
            <ol>
                <li>CodeCommit - Compromised users or users with high access can insert malicious code into the build.</li>
                <li>CodeBuild - Tests can be altered to allow malcious code into the build.</li>
                <li>DynamoDB - Data can be exfiltrated or altered by employees or compromised accounts.</li>
                <li>Lambda - Malicious code can be inserted into the Lambda function.</li>
                <li>Kinesis S3 - Data can be exfiltrated or altered by employees or compromised accounts.</li>
                <li>Fargate - Service can be disabled, deleted or used to exfiltrate incoming data.</li>
                <li>IAM - Users permissions or roles can be abused. Privelege escalation attacks my be possible.</li>
            </ol>
        </td>
    </tr>
</table>

## Scan Results
Now I will run a scan using Arachni. Once Archani is installed, here are the basic steps you
can follow:

1. Navigate to Scans > New
![Archni Scan](\assets\img\archani.jpg)

2. Enter URL of the website > Go!
![Arachni Scan](\assets\img\archani2.jpg)

3. Let the scan run. The scanner will perform several different across all pages on the web application. 
![Arachni Scan](\assets\img\archani3.jpg)

4. Once the scan is finished it will list all the identified issues with the web application.
![Arachni Scan](\assets\img\archani4.jpg)

## Conclusions
Compared to the several possible vulnerabilites I identified, Arachni only flagged a five possible issues with the web application. The most serious issue being a password form using the HTTP protocol, not HTTPS. As TLS certificates can easily be added to a web application to enable HTTPS this is a minor issue. To develop a complete picture of the web application's attack surface, I would test it with a range of different tools. As this analysis only used one tool, the full attack surface of the application cannot be verified. 

## Activity 7.2: Review a NIST Security Architecture

In this exercise I will review a NIST Security Architecture Diagram!
Specifically, I will be looking at a diagram found in [NIST SP1800-5b](https://csrc.nist.gov/publications/detail/sp/1800-5/final).

Here is the architecture which shows an example of an enterprise security architecture. 
![Nist Diagram](\assets\img\nistdiagram.jpg)

Lets mark this diagram up! I will be paying extra close attention to areas where: 1. A single failure could be possible 2. Sections where I could apply additional controls
![Nist Diagram](\assets\img\diagramanalysis.png)

## Glossary

A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.infrastructureandsecurity %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>