---
layout: post
title:  "Building an Incident Response Program"
date:   2021-04-18 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---
## Activity 11.1 Incident Severity Classification

Case Study: Your company is experiencing a prolonged DDOS attack on one of their crucial public facing web applications. The attack is preventing the company from selling its services to customers. Every day the attack is causing a loss of nearly two million. The attack is coming from multiple sources simultaneously. As you have exhausted all your options, you are now searching for a third-party to manage the incident response.

* Classify the incident
* Assign categorical ratings for funcitonal impact, economic impact, recoverability effort and information impact

### Incident Classification
The scope of an incident's impact can be defined as a both the degree of impairment caused to an organisation and the effort required to recover from the incident. As the DDOS attack is causing significant ecnomic impact, lacks a clear timeframe for resolution and requires engaging a third party -- the impact of this incident is high.

### Categorical Ratings
<table>
  <tr>
    <tr>Category</td>
    <tr>Rating</td>
    <tr>Justification</td>
  </tr>
  <tr>
    <tr>Functional Impact</td>
    <tr>HIGH</td>
    <tr>While under DDOS attack, the company has been completely unable to sell their products and provide services to their users.</td>
  </tr>
  <tr>
    <tr>Economic Impact</td>
    <tr>High</td>
    <tr>As the economic loss is estimated at over 2 million per day, this amount clears the NIST threshold of $500,000 per day. Since the DDOS attack is ongoing with no clear remediation ahead, the company will experience significant losses.</td>
  </tr>
  <tr>
    <tr>Recoverability Effort</td>
    <tr>Extended</td>
    <tr>The time to recover from this attack is not predictable. As in-house capabilities have been exhausted, the company is now searching for a third party provider to assist. Hiring a third party will also increase the costs associated with the incident.</td>
  </tr>
  <tr>
    <tr>Information Impact</td>
    <tr>None</td>
    <tr>As the attack is a DDOS attack, availability is the main effected aspect of the business. No information has been exfiltrated at this point.</td>
  </tr>
</table>

## Activity 11.2 Incident Response Phases
In this activity, I will identify the correct phase of the incdent response process which corresponds to the following:
<table>
  <tr>
    <tr>Activity</td>
    <tr>Phase</td>
  </tr>
  <tr>
    <tr>Conducting a lessons learned review session.</td>
    <tr>Post-incident activity</td>
  </tr>
  <tr>
    <tr>Recieving a report from a staff member about a malware infection.</td>
    <tr>Detection and analysis</td>
  </tr>
  <tr>
    <tr>Upgrading a firewall to block a new type of attack.</td>
    <tr>Preparation</td>
  </tr>
  <tr>
    <tr>Recovering normal operations after eradicating an incident.</td>
    <tr>Containment, Eradication, Recovery</td>
  </tr>
  <tr>
    <tr>Identifying the attackers and attacking systems</td>
    <tr>Containment, Eradication, Recovery</td>
  </tr>
  <tr>
    <tr>Interpreting log entries using a SIEM in order to identify a potential incident</td>
    <tr>Detection and analysis</td>
  </tr>
  <tr>
    <tr>Assembling the hardware and software required to conduct an incident investigation</td>
    <tr>Preparation</td>
  </tr>
</table>

## Activity 11.3 Develop an Incident Communications Plan

In this exercise I will imagine I am a CSIRT leader for a large ecommerce website. The website
has experienced a security incident where attackers used SQL injection attacks to steal transaction records
from the backend database. In response to this incident, I will develop a communications plan to
outline the methods for responding to all relevant stakeholders.

# Stakeholders 

XYZ company has several stakeholders, both internal and external which will need to be informed of the data breach.
Each stakeholder group requires a different approach to communicate information regarding the breach. 
The timing, content and approaches will differ for each group. Internal stakeholders which will need to be informed in order of 
priority are: executive management, board members, the legal team, IT department, the communications department and the general remainder of employees.
External stakeholders which will need to be informed, also in order of priority are: suppliers involved in the breech, shareholders, 
consumers and the general public. 

# Internal Stakeholders

A brief overview of the intended approach to inform identified internal stakeholders are listed below:

1. Executive Management
  - First priority 
  - Management team to meet with the CSIRT team and create the overall approach for 
    communicating the incident to all other stakeholders
  - Approach to be agreed on and documented by all management team members 
  - Timing and deadlines to be set
2. The Legal team
  - Review the approach developed by executive management
  - Alter any areas which may have significant legal implications
3. The Board 
  - Convene emergency board meeting
  - Recieve board approval and feedback for implementing communication plans
4. IT department
  - Inform all members of the IT department the technical details of the incident
  - IT team to increase monitoring and awareness posture
5. The Communications department
  - Present approach to divlugving the incident to the public and general employees
  - Communications team to develop strategies and select approrpiate channels, i.e. media, social media, email, etc
    for relevant communications
6. General empoyees
  - Explain incident according to approach laid out by communications team above.

# External Stakeholders
A brief overview of the intended approach to inform identified external stakeholders are listed below:

1. Suppliers
  - Create a priority list of any suppliers whose information may potentially have been divulged during the incident
  - Inform these suppliers and advise them of the nature of the information exfiltrated
  - Advise suplliers on status of company response and steps they can take
2. Shareholders
  - Send out general shareholder notice via email
  - Send at the same time as emailing general consumers
3. Consumers
  - Communicate breach first to consumers affected via official email
4. General Public
  - Notify the media and general public based on the approach outlined by the communications team
  - Use a multi-channel approach, i.e. twitter, facebook, television, email, etc.

# Conclusions

Before communicating the nature of a breach to stakeholders, careful consideration needs to be taken to 
develop the correct approach. This initial consideration requires significant input from executive management, the legal team and 
the communications department. Finally, once all stakeholders have been informed it is essential to review the effectiveness of the 
communcations and identify areas for improvement.

### Nature, Timing and Audiences to the internal and external stakeholders to be notified. 
## Glossary
A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.buildingincidentresponse %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>