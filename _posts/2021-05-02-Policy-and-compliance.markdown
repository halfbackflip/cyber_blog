---
layout: post
title:  "Policy and Compliance"
date:   2021-05-01 05:58:00 +1100
categories: cysa  
permalink: /:categories/:title
published: true
---

## 16.1 Policy Documents
  <tr bgcolor="grey" style="color:white;">
      <td>Type</td>
      <td>Definition</td>
  </tr>

  <tr>
    <td>Policy</td>
    <td>High level requirements for an overall security program. Compliance is mandatory.</td>
  </tr>

  <tr>
    <td>Standard</td>
    <td>Provides detailed requirements for implementing a cybersecuriy program and policy.</td>
  </tr>

  <tr>
    <td>Guideline</td>
    <td>Best practice on achieving a security goal(s). Compliance is not mandatory.</td>
  </tr>

  <tr>
    <td>Procedure</td>
    <td>Outlines a step-by-step process for carrying out an operation.</td>
  </tr>

</table>

## 16.2 Using a Cybersecurity Framework
In this excercise, I will review a subcategory of the NIST [Framework for Improving 
Critical Infrastructure Cybersecurity](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.04162018.pdf). 
This framework is designed to provide a structuted approach when reviewing an existing cybersecurity program. The Framework Core provides five key outcomes, Identify, Protect, Detect, Respond and Recover.
The framework then breaks these outcomes into categories and subcategories while providing the titles of relevant reference documents.
![Risk Management](\assets\img\framework.jpg)

In this excercise, select a specific category and then subcategory. Below is a breakdown of how an organization could addresses these subcategories.
<html>
  <table bgcolor="White">
    <tr bgcolor="DarkGrey" style="color:white;">
      <td>Function</td>
      <td>Category</td>
      <td>Subcategory</td>
      <td>Implementation</td>
    </tr>
    <tr>
      <td bgcolor="Indigo" style="color:white;" rowspan="5">PROTECT (PR)</td>
      <td rowspan="5">Awareness and Training (PR.AT): The organization’s personnel and partners are 
      provided cybersecurity awareness education and are trained to 
      perform their cybersecurity related duties and responsibilities 
      consistent with related policies, 
      procedures, and agreements.
      </td>
      <td>PR.AT-1: All users are informed and trained</td>
      <td>
          <ul>
              <li>Every user must complete mandatory phishing training</li>
              <li>New hires are required to attend a security introduction</li>
              <li>Employees are made aware when security policy changes are enacted</li>
              <li>Easy to read security policy snapshots are provided</li>
              <li>Employees are involved in fake phishing campaigns</li>
              <li>Actual phishing campaign emails are circulated to employees as an example</li>
              <li>Monthly town hall meetings discuss organisational security posture</li>
              <li>Employees are given the opporunity to provide feedback and reccommednations</li>
          </ul>
      </td>
    </tr>
    <tr>
      <td>PR.AT-2: Privileged users understand their 
  roles and responsibilities </td>
      <td>
          <ul>
              <li>Higher level users are briefed on the scope of their priveleges</li>
              <li>Admin users are required to follow Priveleged Access Management PIM</li>
              <li>Changes to user responsiblities are communicated</li>
              <li>Role reference is provided to all employees, listing the name and descriptions of all roles</li>
          </ul>
      </td>
    </tr>
    <tr>
      <td>PR.AT-3: Third-party stakeholders (e.g., 
  suppliers, customers, partners) understand 
  their roles and responsibilities</td>
      <td>
          <ul>
              <li>Third party stakeholders are provided with least privelege access</li>
              <li>Roles and responsibilities are clearly outlined in contracts</li>
              <li>Changes to responsiblities are communicated</li>
          </ul>
      </td>
    </tr>
    <tr>
      <td>PR.AT-4: Senior executives understand 
  their roles and responsibilities</td>
      <td>
          <ul>
              <li>Senior Executives are given clear roles and responsibilities within the cybersecurity program</li>
              <li>Critical roles, i.e. Incident Response, are involved in excercises and practice runs</li>
              <li>Senior Executive roles are communicated to the rest of the organisation</li>
              <li>Changes to responsiblities are communicated</li>
          </ul>
      </td>
    </tr>
    <tr>
      <td>PR.AT-5: Physical and cybersecurity 
  personnel understand their roles and
  responsibilities </td>
      <td>
          <ul>
              <li>A clear organisational hiearchy diagram is provided</li>
              <li>Physical and cybersecurty personnel are fully briefed on their roles and responsibilites</li>
              <li>A clear process for incident escalation is provided</li>
              <li>All roles are involved in frequent practice excercises to encourage preparedness and improvement</li>
              <li>Roles are flexible and modified when organizational change occurrs</li>
              <li>Changes to responsiblities are communicated</li>
          </ul>
      </td>
    </tr>
  </table>
</html>
## 16.3 Compliance Auditing Tools
In this section I will review an aspect of the the Payment Card Industry (PCI) [Data Security Standard](https://www.pcisecuritystandards.org/document_library). Specifically, I will analyze the requirements of password construction from section 8.2.3 and how to 
determine wether an organization is in compliance with this requirement. 

According to section 8.2.3 passwords must contain a minimum length of seven characters and contain both numeric and alphabetic characters. In order to verify these settings there are several measures which can be employed. The first step would be to verify the organisation's password policy. This policy should be an easily accessible and version controlled document. After checking the policy document, the next step is check compliance in all areas the password policy would be employed. 

Verifying the password policy is deployed at the user layer is a key step. Assuming the environment is using Active Directory, the default password policy can be checked with the following powershell command:

```Get-ADDefaultDomainPasswordPolicy```

Depending on the environment, Fine-Grained Password Policies (FGPP), a feature allowing different password policies to be set based on active directory security group members could apply. FGPP password policies can be listed with the following powershell command:

```get-adfinegrainedpasswordpolicy -filter *```

Once passsword complexity is assessed within Active Directory, the next step is to verify password compliance on other platforms. For example, checking network devices, third-party login portals, software tools, mobile devices and application logins. The range of platforms to check here would vary significantly depending on the organisation. 

## Glossary
A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.policyandcompliance %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>