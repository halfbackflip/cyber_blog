---
layout: post
title:  "Software and Hardware Development Security"
date:   2021-04-10 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---
## Activity 9.1 Review an Application Using the OWASP Application Security Architecture Cheat Sheet
In this excercise I will download BurpSuite and scan a sample site. MORE SCANNING!

### Part 1: Download and Installation
Burpsuite provides a free community edition. Their scanner can be downloaded from [here](https://portswigger.net/burp/releases/professional-community-2021-3-1?requestededition=community)

https://subgraph.com/vega/

OWASP maintains an excellent list of scanning utilities:
[https://owasp.org/www-community/Vulnerability_Scanning_Tools](https://owasp.org/www-community/Vulnerability_Scanning_Tools)

### Part 2: Select an application and scan it
As Burpsuite is a dedicated web application scanner... I need a web application to scan. Obviously running around the world wide web scanning is not legal. However many organisations run intentionally vulnerable web applications for cyber security practice purposes. Here is a short list of some of these vulnerable sites:

[http://www.google-gruyere.appspot.com/](http://www.google-gruyere.appspot.com/)
A vulnerable web app provided by Google. Clicking start generates a unique ID, creating a personal website for your to hack away at.

[http://www.itsecgames.com/](http://www.itsecgames.com/)
Purposedly buggy web applicaiton. Available as stand-alone code or a VM.

[http://damnvulnerableiosapp.com/](http://damnvulnerableiosapp.com/)
A vulnerable IOS application.

[https://github.com/webpwnized/mutillidae)](https://github.com/webpwnized/mutillidae)
A vulnerable web app created by OWASP. 

[https://owasp.org/www-project-webgoat/](https://owasp.org/www-project-webgoat/)
Webgoat! An insecure Java application maintained by OWASP.Available as a handy docker container. 

[https://dvwa.co.uk/](https://dvwa.co.uk/)
Damn vulnerable web application. A PHP/MysQL vulnerable web application for testing.

For this example I will start with Google's Gruyere. It is very easy to get started: since it requires no local installation or setting up a VM. Simply visit the site and click start to get going. 
![gruyere](\assets\img\gruyere.jpg)

Scanning steps:
1. Start up Burpsuite. Select > New Scan
This performs an end-to-end managed scan. 
![burpsuite](\assets\img\burpsuite.jpg)

2. On the scan type page, I will enter the gruyere URL and leave "crawl and audit" selected. Also, I will leave "Scan using HTTP & HTTPS" selected to scan across both protocols. Click OK to start the scan.
![burpsuite](\assets\img\burpsuite2.jpg)

3. Grab some popcorn.
![burpsuite](\assets\img\burpsuite3.jpg)

5. As the scan progresses, discovered vulnerabilities will appear in the Issue Activity section. Clicking them will reveal more details about the issues found. 
![burpsuite](\assets\img\burpsuite4.jpg) 

4. Once the scan completes, detailed info of specific vulnerabilities can be found in the Target tab.
![burpsuite](\assets\img\burpsuite5.jpg)  

5. In order to generate a report, select indvidual issues or select all and right click > Report Selected Issues.
![burpsuite](\assets\img\burpsuite6.jpg)

6. You can then generate a fancy html report as ouput. As I chose to generate a report from all the issues, my report has length of over a hundred pages. Selecting the vulnerabilities to include in the report will result in a shorter length.
![burpsuite](\assets\img\burpsuite7.jpg) 

### Part 3: Analyze the scan results

### Cross Site Scripting
Definition:
The scan identified 6 confirmed issues of reflected cross site scripting with a severity of high. A reflected cross site scripting attack is when data is copied from a request and echoed into an application's immediate response. This attack can allow an attacker to execute Javascript code suppllied by the attacker within the user's browser. The code can provide actions such as stealing user credentials, logging key strokes and so on. 

Remediation:
1. Input Validataion. For example, a field request a year should accept only numbers and exactly four digits
2. User input should be HTML encoded when it is copied into a response. For example, HTML metacharacters such as <> and = should not be included in a response. 

Issue Breakdown:
* Note the higlighted section showing how the request is sent and the response delivered.
![xss.jpg](\assets\img\xss.jpg) 

### Clear Text Submission of Password
Definition: 
The scan confirmed 2 vulnerable issues of passwords being submitted in cleartext. Passwords are currently transmitted over unencrypted connections. This makes them vulnerable to interception by an attacker.

Remediation:
1. Applications should use the latest version of TLS, Transport Level Encryption, enabling HTTPS. This ensures all communications between a client and server are encrypted. Nearly all modern web applications use HTTPS and web browsers will frequently display a warning to a user before allowing them to communication with an application using HTTP.

Issue Breakdown:
* Note the highlighted section in the response showing the password returned in cleartext. This response could easily be captured by an attacker.
![cleartext](\assets\img\cleartext.jpg) 

### Password submitted using GET method
Definition: The scan confirmed 2 vulnerable issues of GET methods being used to submit passwords. This vulnerability is a low severity. Using a GET method, passwords are transmitted within the query string of the requested URL. The password can then be logged by reverse proxies, displayed on screen or even revealed to a thirdy party via a refferer header when a user clicks on an off-site link.

Remediation:
1. Forms submitting passwords should use the POST method. The application code may need to be refactored to specify the method attribute of the FORM tag as method="POST"

Issue Breakdown:
![get-method](\assets\img\get-method.jpg)

### Frameable response (potential Clickjacking)
Definition: Five potential clicking issuers were detected. If a page does not set a proper X-Frame-Options or Content-Security-Policy HTTP header, an attacker could load an iframe within the page. This could enable a click jacking attack. A clickjacking attack is when an attacker overlays fake page or interface over the normal page. The user will then unknowingly click or perform keystrokes on the page, performing unintended actions or revealing credentials. 

Remediation:
The application can return a response header with the name X-Frame-Options and a value of DENY to prevent all forms of framing. Alternatively the value SAMEORIGIN can be set to allow framing only by pages on the same origin as the response itself.

Issue Breakdown:
![clickjacking](\assets\img\clickjacking.jpg)

## Activity 9.2 Learn About Web Application Exploits from WebGoat

### Part 1: Download Docker
As mentioned above, Web Goat is a purposedly vulnerable Java web application maintained by OWASP.
I will run Web Goat in a docker container on my Kali Linux VM. First, lets download and install docker. 
I will follow the official instructions located here: 
[Docker](https://docs.docker.com/engine/install/debian/)

As per the below instructions, the commands needed to install docker are:
`kali@kali:~$ sudo apt update`
`kali@kali:~$ sudo apt install -y docker.io`
`kali@kali:~$ sudo systemctl enable docker --now`
`kali@kali:~$ docker`


In order to start Web Goat, we will use the all-in-one docker container. 
All the necessarry files can be downloaded from Github:
[https://github.com/WebGoat/WebGoat](https://github.com/WebGoat/WebGoat)

1. Clone into the Repositiory:
![Docker](\assets\img\docker-repo.jpg)

2. Navigate to Web Goat > Docker or alternatively run "https://hub.docker.com/r/webgoat/goatandwolf"

3. Run the docker commands to run the container. Be sure to set your timezone appropriately.
`docker run -p 8080:8080 -p 9090:9090 -e TZ=Europe/Amsterdam webgoat/goatandwolf`

4. Open your browser. WebGoat will be located at: http://127.0.0.1:8080/WebGoat and WebWolf will be located at: http://127.0.0.1:9090/WebWolf
![webgoat](\assets\img\webgoatwebwolf.jpg). 

5. There are serveral lessons in Web Goat. I will skip the first few lessons and go through the section on SQL injection.

### Part 2: SQL Injection
Definition: 
SQL injection is inserting SQL commands into forms and other exposed areas of a web application to elicit a response from the web application's underlying database. This can result in displaying information, which is not intended to be displayed, modifying the database, shutting down the database, adding privileges to the database and compromising database integrity.

Lesson 2 in Web Goat provides the most basic example of an SQL injection, which results in dumping employee information.
![webgoat](\assets\img\webgoat.jpg)

SQL injection involves inserting DML, "Data Maniuplation Language" commands into inputs of a web application. 

Core DML commands are:
* SELECT retrieves data
* INSERT adds data to a table
* UPDATE updates data in a a table
* DELETE Delete all records in a table

As lesson 3 in Web Goat demonstrates, SQL injection can violate both confindentiality and integrity.
Here we are able to both reveal confidential employee info and change it with a DML command.
![webgoat](\assets\img\webgoat2.jpg)

SQL injection commands can also leverage DDL, "Data Definition Language" commands. These commands can change the structure of the database, violating data integrity.

Core DDL commands are:
* CREATE create a database and/or related objects
* ALTER change the database structure
* DROP delete objects from the database

In Lesson 4, we modify the table schema by adding an additional column to the database.
![webgoat](\assets\img\webgoat3.jpg)

DCL, "Data Control Language" is used to manage priveleges. An attacker using DCL can grant themselves unwanted priveleges to the
datbase.

Core DCL commands are:
* GRANT allow user access to a table
* REVOKE remove user access

Here is an example in the Web Goat excercises of granting the right to alter tables:
![webgoat](\assets\img\webgoat4.jpg)

SQL Injection can be remediated through input validation, i.e. checking that user supplied input is not an SQL command.
Without proper input validation, crafted statments such as this can result in dumping all the users in a database:
![webgoat](\assets\img\webgoat5.jpg)

As '1' = '1' always evaluates to true this will result in data being dumped from the table.
The next two excercises provide further examples of this:
![webgoat](\assets\img\webgoat6.jpg)
![webgoat](\assets\img\webgoat7.jpg)

Query chaining allows you to append one or more queries to the end of an actual query by using the ";". This function allows for ever further mischief... In the next Web Goat Excercise, you can chain the following query:
`'; UPDATE employees SET salary = 10000000 WHERE auth_tan = '3SL99A`
This query attaches to the underlying query of the web application, allowing you to adjust the salary of anyone in the database.
![webgoat](\assets\img\webgoat8.jpg)

The last example of Web Goat provides an example of deleting an access log table to cover your tracks.
This example also leverages query chainging. Inserting the following query will delete the log table completely.
`'; DROP TABLE access_log --`

## Glossary
A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.softwareandhardwaredevelopmentsecurity.csv %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>