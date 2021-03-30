---
layout: post
title:  "Cloud Security"
date:   2021-03-27 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---

## Activity 6.1: Run a ScoutSuite Assessment
In this step, I will download ScoutSuite and run the tool. 
ScoutSuite is a security auditing tool which works across any cloud environment. 

1. Download ScouteSuite from GitHub
[https://github.com/nccgroup/ScoutSuite](https://github.com/nccgroup/ScoutSuite)

2. Extract the zipped directory. Open a terminal window and navigate to the directory where scoutsuite is downloaded. Setup a virtual python env for this project. If you have never setup a virtual environment before, details are available here:
[https://docs.python.org/3/tutorial/venv.html](https://docs.python.org/3/tutorial/venv.html) 
`python scout.py`

3. I am going to run the scan against an existing AWS account which I have pre-configured to use the AWS CLI. For the purposes of this scan, I have defined a user in AWS with minimum policy permissions. The minmimum policy is provided on the Git Hub site.
[https://github.com/nccgroup/ScoutSuite/wiki/AWS-Minimal-Privileges-Policy](https://github.com/nccgroup/ScoutSuite/wiki/AWS-Minimal-Privileges-Policy) 

4. In order to start the scan, I will run:
`python scout.py aws`

5. Scoute suite will then use a range of API calls to scan all of the resources in your AWS account. Grab some popcorn and wait. 
![Scout Suite](\assets\img\scoutsuite.jpg)

6. When everything is finished ScoutSuite will generate a handy html report.
![Scout Suite](\assets\img\scoutsuite2.jpg)

7. Clicking on individual entries of the report will then drill down into reccommended security configurations.
![Scout Suite](\assets\img\scoutsuite3.jpg)


## Activity 6.2: Explort the Exploits Available with Pacu

In this lab activity I will download Pacu, a python script which is designed for offensive security testing against cloud environments. Pacu allows pen testers to leverage flaws within an AWS environment. Obviously, this tool should only be used against an account you have permission to access.

1. First we will navigate to the Github page and examine Pacu's repo.
[https://github.com/rhinosecuritylabs/pacu](https://github.com/rhinosecuritylabs/pacu)

2. Next we will follow the quick installation instructions in order to setup Pacu. I will be installing Pacu inside Kali Linux. The installation instructions are as follows:

```
> git clone https://github.com/RhinoSecurityLabs/pacu
  > cd pacu
  > bash install.sh
  > python3 pacu.py
```
3. We will then run it using the command `python3 pacu.py`
![Pacu](\assets\img\pacu.jpg)

4. Once Pacu is running, I will configure it to scan my AWS account. I will provide pacu with a pair or read-only keys in order to programatically access my AWS account.
Note: These keys no longer work ;)
![Pacu](\assets\img\pacu2.jpg)

5. In order to view a list of exploit modules we can run the list command.
![Pacu](\assets\img\pacu3.jpg)

6. Using the run command we can run any of the listed modules, for example if you wanted to perform 
recon and check the credentials in an account, you could run:

```run iam__get_credential_report```

The range of commands here are significant, such as checking the amount of spending in an account:
```aws aws__enum_spend```

## Activity 6.3: Scan an AWS Account with Prowler

Next we will check out an AWS account with Prowler.
Prowler can be downloaded from:
[https://github.com/toniblyx/prowler](https://github.com/toniblyx/prowler) 

As described on Github, Prowler is a tool that "Prowler is a security tool to perform AWS security best practices assessments, audits, incident response, continuous monitoring, hardening and forensics readiness."

1. I will again run prowler inside my Kali VM. 

2. As Prowler uses the AWS CLI under the hood, we wil need to first install the AWS CLI.
Follow the instructions here:
[https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html#cliv2-linux-prereq](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html#cliv2-linux-prereq)

3. Next install the python awscli lib and jq.
```sudo apt install jq```
```pip install awscli```

4. Clone prowler from Github and navigate to the directory.
```git clone https://github.com/toniblyx/prowler```
```cd prowler```

5. Now configure the AWS CLI. I will use my read-only keys from the previous session. 
![Prowler](\assets\img\prowler.jpg)

6. Next we can run prowler by typing: 
```./prowler```
Now prowler will run a full audit and check of our aws account.
Again, only run this against an account which you have access to. 
Here is a screenshot for example. The full output is much longer.

![Prowler](\assets\img\prowler2.jpg)

Once the checks are run, we can analyze the report and remediate any pressing vulnerabilities. Huzzah!

## Glossary

A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.cloudsecurity %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>