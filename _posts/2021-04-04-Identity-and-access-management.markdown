---
layout: post
title:  "Identity and Access Management Security"
date:   2021-04-04 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---

## Activity 8.1 Federated Security Scenario
In this exercise I will examine two federated identity scenarios. For each I will research the technology
used and brainstorm any security issues related with the implementation. 

## Part 1: Google OAuth integeration
### Summary: 
A company has implemented an OAuth integration with Google using their own internally developed code. The integration will be implmented on their server via HTTP. 

### Security Issues:
This implementation faces numerous security issues.
1. Using HTTP makes the system vulnerable to traffic capture. HTTPS should be used in place.
2. The latest version, OAuth 2.0 should be used when possible
3. Potential for AS Mix-up Attack if multiple authorization servers are used
4. Staff may be vulnerable to phishing email attacks as per 2017 OAuth-based phishing attack
5. Instead of internally developed code, the organization should utilize the provided OAuth libraries
6. MFA and strong passwords need to be enforced on internal Google Accounts
7. Storage of user secrets needs to be secure, i.e. segmented network, encryption at rest/in transit.

## Part 2: High security federation incident response
### Summary: 
A company is considering using Facebook Login to allow users to login to their customer support site. 
As a benefit the company will no longer be responsible for handling identity management in their application. 
![Login with Facebook](\assets\img\loginfbook.png)

### Reccomendations:
Below are recommendations for the implementation team:
1. Verify Graph API Calls as per Facebook recommendations .
2. Utilize the Facebook provided SDK. Do not create your own library.
3. Consider implementing Google OAuth or additional providers for redundancy.
4. Use HTTPS.
5. Check implementation against Facebook's [checklist](https://developers.facebook.com/docs/facebook-login/security/)
6. Do not implement tracking scripts which pull info from Facebook's login API.

### Incident Response Plan:
Here are some points which would need to be included in an incident response plan if using Facebook login:
1. Create a process if the identity provider (Facebook) is compromised.
2. Anticipate small-lever incidents, i.e. if the identity provider is offline or a small portion of accounts are compromised.
3. A procedure on how to handle bots, fake or compromised accounts which may be logging in to the app via Facebook Login.

## Part 3: Analyze your responses
In regards to Google OAuth, my reccommendations can be checked against the [OWASP Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)

### OWASP's main reccomendations are:
1. Use OAuth 1.0a or OAuth 2.0 since the first version (OAuth1.0) has been found to be vulnerable to session fixation.
2. OAuth 2.0 uses HTTPS and should be used over OAuth 1.0a
3. Audit the OAuth 2.0 code; developer mistakes can cause serious vulnerabilities.
4. OAuth should be used for authorization - not authentication

As for the second activity, information for developing a federation incident response can be located at:
[Federated SecurityIncident Response Policy](https://www.btaa.org/docs/default-source/technology/federated_security_incident_response.pdf)

The btaa provides an example response to a security incident where Federated Identity is involved. The main point of difference being "instead of interacting directly with the user...interacts with the user's home institution (the IdP), who take the lead with user interactions and forensic investigation." Once the compromise of a user's account is discovered, the Federated Identity Provider will be informed of the compromised credentials and provided with the evidence. The provider will then need to reset the user's account and restore their credentials. Finally, the provider will inform the organization of the successful reset. Information discovered during the investigation of the account compromise should be shared between both parties. For example, if the compromise is determined to originate from a specific netblock of IP addresses, the organization may be able to block these addresses.

Sources:
1. [https://oauth.net/code/](https://oauth.net/code/)
2. [https://en.wikipedia.org/wiki/OAuth](https://en.wikipedia.org/wiki/OAuth)
3. [https://ldapwiki.com/wiki/OAuth%202.0%20Threat%20Model%20and%20Security%20Configurations](https://ldapwiki.com/wiki/OAuth%202.0%20Threat%20Model%20and%20Security%20Configurations)
4.  [https://developers.facebook.com/docs/facebook-login/overview](https://developers.facebook.com/docs/facebook-login/overview)
5. [https://www.wired.com/story/security-risks-of-logging-in-with-facebook/](https://www.wired.com/story/security-risks-of-logging-in-with-facebook/)
5. [https://freedom-to-tinker.com/2018/04/18/no-boundaries-for-facebook-data-third-party-trackers-abuse-facebook-login/](https://freedom-to-tinker.com/2018/04/18/no-boundaries-for-facebook-data-third-party-trackers-abuse-facebook-login/)
6. [https://developers.facebook.com/docs/facebook-login/security/](https://developers.facebook.com/docs/facebook-login/security/)
7. [https://owasp.org/www-pdf-archive/OWASP-NL_Chapter_Meeting201501015_OAuth_Jim_Manico.pdf](https://owasp.org/www-pdf-archive/OWASP-NL_Chapter_Meeting201501015_OAuth_Jim_Manico.pdf)
8. [https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
9 .[https://www.btaa.org/docs/default-source/technology/federated_security_incident_response.pdf](https://www.btaa.org/docs/default-source/technology/federated_security_incident_response.pdf)

## Activity 8.2: On-site Identity Issues Scenario
In this exercise I will examine two local identity scenarios. As per the last exercise, for each I will research the technology and highlight any security issues related with the implementation.

## Part 1: Emergency privelege escalation
### Summary: 
A companies’ administrative accounts are created and managed via a central IDAM solution. This solution, along with the company's central AAA servers are hosted in redundant data centres. Site-to-site VPNs then connect these servers to multiple locations around the country. A recent internet connectivity outage resulted in engineers being unable to access privileged accounts. They require a solution in order to allow for emergency, on-demand privileged local access during an outage when central AAA servers are unavailable. The solution should be secure and flexible enough to enable authentication for network devices, servers and workstations.

### Solution: 
I propose utilizing a local, read-only domain controller RODC at local sites. According to Microsoft, RODCs allow users to logon and complete tasks "even when there is no network connectivity to hub sites." The RODCs at the branch sites would replicate directory information from the central hub Domain controllers. As the RODCs are read-only they would allow greater security, preventing an attacker from writing to the domain controller should they compromise a local site. Network connections between the RODC and the central Domain controllers would be tunnelled through the existing site-to-site VPNs. In the event of an outage, AAA services would automatically fail over to the RODC and failback once service is restored.

### Sample Architecture Diagram:
![RODCArchitecture.png](\assets\img\RODCArchitecture.png)

## Part 2: Managing privelege creep
### Summary: 
A recent audit revealed many long-term employees possessed broader rights to files and folders needed to accomplish their jobs.  Some employees could even access sensitive data. How can you handle the current issue of privilege creep and ensure it does not happen in the future?
The solution cannot disrupt company operations.

### Solution:
From a procedural perspective there are several policies which can prevent privilege creep. These include enforcing the principle of least privilege, conducting regular access audits, reviewing the access granting process and reducing the number of departments which can manage user privileges. In this situation, user rights not relevant to an individual's job role need to be removed. Removing access rights to sensitive and confidential data should be prioritized. The next step would be to revert user permissions to their default settings. To avoid causing any widespread disruptions, this process will be performed in stages or in small groups. In the event of any issues, permissions could easily be restored. The last stage would involve taking a baseline measurement of user rights. This baseline could then be fed into an automation workflow. The workflow could generate a report comparing user permissions to the baseline to measure privilege creep. During regularly scheduled audits, the governance team could review these reports and determine which user permissions drifted too far from the baseline.

## Glossary

A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.identityandaccessmgmt %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>