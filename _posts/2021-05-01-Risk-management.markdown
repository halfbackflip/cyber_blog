---
layout: post
title:  "Risk Management"
date:   2021-04-27 05:58:00 +1100
categories: cysa  
permalink: /:categories/:title
published: true
---

## 15.1 Risk Management Strategies
Risk management consists of four main risk strategies

<table>

  <tr bgcolor="grey" style="color:white;">
      <td>Activity</td>
      <td>Category</td>
  </tr>

  <tr>
    <td>Risk Mitigation</td>
    <td>Implementing security controls to reduce the likelihood of the risk occurring and lower the magnitude of the impact.</td>
  </tr>

  <tr>
    <td>Risk Avoidance</td>
    <td>Changing business activities to completely eliminate the risk; eliminating the risk often comes with significant drawbacks.</td>
  </tr>

  <tr>
    <td>Risk Transference</td>
    <td>Transfering the risk and potential impact to another organization or third party.</td>
  </tr>

  <tr>
    <td>Risk Acceptance</td>
    <td>Deliberately choosing to continue business operations as normal despite the potential risk.</td>
  </tr>

</table>

## 15.2 Risk Identification and Assessment
In this section I will think of a business process critical to the operation of an organization and identify the risks
associated to that process. Once risks are identified, I will select one and conduct a risk assessment. 

### Risk Identification
For this risk assessment, I will consider a university in the United States. A critical business process for any university is
achieving or exceeding enrollments. Here is a diagram brainstorming some risks associated with this. 
![Risk Management](\assets\img\risk_management.png) 

### Risk Assessment
The risk I will focus on is the damage to a university's reputation. I will conduct a rough quantitative risk estimate here.  I will select Yale University as a case study to narrow the scope of this exercise. How many times over the past 10 years has Yale experienced significant damage to their reputation? The more difficult question answer will be: have these events effected Yale's enrolment?

Note: Obviously this is not an actual quantitative assessment as I do not have the data and resources required to properly conduct one. Rather, I see this as a rough-draft-sprint-pratice excercise for thinking about risk.

* 2021
    - [Student shot to death near campus](https://chicago.suntimes.com/2021/4/13/22382251/interpol-search-qinxuan-pan-murder-yale-student-kevin-jiang)
* 2020
    - [Yale Law professor suspended for sexual harassment](https://nymag.com/intelligencer/2020/08/yale-professor-jed-rubenfeld-suspended-for-sexual-harassment.html)
    - [Justice Department sues Yale](https://www.insidehighered.com/admissions/article/2020/10/12/justice-department-sues-yale-over-admissions)
* 2019 
    - [Bribery scam for student to ensure admittance](https://www.bbc.com/news/world-us-canada-47709546)
    - [Professor sexually preys on students for decades](https://www.washingtonpost.com/local/education/yale-missteps-allowed-professor-to-prey-on-students-for-decades-report-finds/2019/08/22/ecf25d12-c451-11e9-850e-c0eef81a5224_story.html)
* 2018
    - [Yale students robbed at gunpoint](https://yaledailynews.com/blog/2018/04/24/td-students-robbed-at-gunpoint/)
* 2016
    -[Basketball Captain Expelled for sexual assault](https://www.theguardian.com/education/2016/sep/17/yale-denies-expelling-student-to-prove-tough-on-sexual-assault)
* 2015
    - [Halloween costume controversy](https://www.theatlantic.com/politics/archive/2015/11/the-new-intolerance-of-student-activism-at-yale/414810/)
    - [Halloween rape case](https://www.nytimes.com/2018/03/07/nyregion/yale-student-not-guilty-saifullah-khan.html)
* 2014
    - [Students expelled for sexual assault](https://www.nytimes.com/2014/02/22/nyregion/two-sexual-assaults-are-reported-at-yale.html)
* 2013
    - [Yale fined for crime under-reporting](https://www.chronicle.com/article/yale-u-is-fined-165-000-under-crime-reporting-law/)
* 2012
    - [Conviction of 2009 Murder of Yale Student](https://abcnews.go.com/US/raymond-clark-pleads-guilty-murder-yale-grad-student/story?id=13158057)

### Quantitative Risk Assessment 
1. Determine the Asset Value (AV) of the asset affected by the risk. In 2021 Yale admitted 1322 students. The average yearly tuition is 55,500. This results in an AV of 73,371,000 annually. 
2. Determine the Annualized Rate of Occurrence (ARO), i.e. how likely the risk will occur over a given year. As per above, a damaging PR event occurred 12 (by my estimate after 30 minutes of googling) times over 10 years. Dividing these two numbers produces an ARO of 1.2.
3. Determine the percent of damage which will occur to the asset if the risk happens. This referred to as the Exposure Factor (EF). Past scandals appear to have little effect on enrolment. I will set the EF at 0.001, although I imagine it could be set even lower. 
4. Determine the single loss expectancy (SLE), the amount of damage expected each time a risk materializes. The SLE is calculated by multiplying EF and AV, so 0.001 X 73,371,000 results in 73,371. 
5. Calculate the annualized loss expectancy (ALE), the amount of damage expected from a risk in a given year. This is calculated by multiplying the ARO and SLE, so 1.2 x 73,371 results in an Annual Loss Expectancy of 88,045.


## 15.3 Risk Management
Now that I have conducted a risk assessment, I will look at how the previously identified risk management strategies could be used to address this risk:
* Risk mitigation: Develop several PR strategies and playbooks for handling common reputation events to handle an actual event.
* Risk avoidance: Drastically reduce enrolment numbers and rely on other sources of cash flow to fund the university.
* Risk acceptance:  Set aside the ALE amount in budget every year for handling damaging PR events.
* Risk transference: Is this... possible?


## Glossary
A glossary of all the terms, acronyms and slang I run across for this chapter.

<table>
{% for item in site.data.riskmanagement %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>