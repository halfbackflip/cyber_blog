---
layout: post
title:  "Containment, Eradication and Recovery"
date:   2021-04-26 05:58:00 +1100
categories: cysa 
permalink: /:categories/:title
published: true
---

## Activity 14.1 Incident Containment Options
In this section I have re-diagrammed the 3 main incident containment options.

1. Segmentation - Creating a seperate section of the existing network in order to contain the attacker or affected systems.
![Segmentation](\assets\img\segmentation.png) 
2. Isolation - Removing the attacker from internal network access, but permitting access to the internet. This technique can be used to monitor attacker communication with external servers.
![Isolation](\assets\img\isolation.png) 
3. Removal - Completely removing the affected network or attacker from any network access. 
![Removal](\assets\img\removal.png) 

## Activity 14.2 Incident Response Activities
Comptia categorizes incident response activities as the following:    
1. Containment: Containing the damage of the attack 
2. Eradication: Removing the attacker, all artifacts and malicious code caused by the attack.
3. Validation: Confirming the success of eradication.
4. Postincident Activities: Preparation of reports and lesson-learned sessions.

The following activities map to the Comptia categories:
<table>

  <tr bgcolor="grey" style="color:white;">
      <td>Activity</td>
      <td>Category</td>

  </tr>

  <tr>
    <td>Patching</td>
    <td>Validation</td>
  </tr>

  <tr>
    <td>Sanitization</td>
    <td>Eradication</td>
  </tr>
  
  <tr>
    <td>Lessons Learned</td>
    <td>Postincident Activities</td>
  </tr>

  <tr>
    <td>Reimaging</td>
    <td>Eradication</td>
  </tr>

  <tr>
    <td>Isolation</td>
    <td>Containment</td>
  </tr>

  <tr>
    <td>Scanning</td>
    <td>Validation</td>
  </tr>

  <tr>
    <td>Removal</td>
    <td>Containment</td>
  </tr>

  <tr>
    <td>Reconstruction</td>
    <td>Eradication</td>
  </tr>

  <tr>
    <td>Permission Verification</td>
    <td>Validation</td>
  </tr>

  <tr>
    <td>User Account Review</td>
    <td>Validation</td>
  </tr>

  <tr>
    <td>Segmentation</td>
    <td>Containment</td>
  </tr>

</table>

## Activity 14.3 Sanitization and Disposal Techniques
Here are the main three options fo media sanitization during the recovery phase of incident response.
* Clear: Uses software or hardware products to overwrite existing storage with non-sensitive data. This approach uses the standard read and write operations available to the device. In some devices the only option may be to restore a device to factory settings. 
* Purge: Application of media specific techniques to directly erase data such as overwrite, block erase and cryptographic erasure. The media is still useable after application of this technique.
* Destroy: The intent of destruction is to render the target data totally infeasible to retrieve. This may also result in the media being rendered useless for the future storage of data. Techniques include disintegration, melting, incineration and shredding.

The below diagram, re-created from [NIST SP 800-88: Guidelines for Media Santization](https://csrc.nist.gov/publications/detail/sp/800-88/rev-1/final) outlines the decision flow needed to determin the correct form of media sanitization.
![Sanitization](\assets\img\sanitizationflow.png)  

## Glossary
A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.containment_eradication_and_recovery %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>