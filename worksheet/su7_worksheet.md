## ProLUG Security Engineering

# Unit 7 Worksheet

*Instructions*
*Fill out this sheet as you progress through the lab and discussions. Hold your worksheets until the end to turn them in as a final submission packet.*

---

## Discussion Questions:

### Unit 7 Discussion Post 1:

https://discord.com/channels/611027490848374811/1370814113755697284/1377377100021108846

Read about telemetry, logs, and traces.
There are many good sources, even from Microsoft:
https://microsoft.github.io/code-with-engineering-playbook/observability/log-vs-metric-vs-trace/

a.    *How does the usage guidance of that blog (at bottom) align with your understanding of these three items?*

> This helps me understand that installing a collector to receive various metris is of upmost importance.  Systems like Grafana, ELK & splunk allow us to properly track an occurence of an event.  We need the metrics to know where to start looking in the logs.

b.    *What other useful blogs or AI write-ups were you able to find?*

https://betterstack.com/community/guides/observability/logging-metrics-tracing/


c.    *What is the usefulness of this in securing your system?*

> All three of these are totally useful and necessary in securing ur systems.
The concept of traces does appear to be dependent on an application support team.  The other two are completely within the realm of a system administrator.


### Unit 7 Discussion Post 2:

https://discord.com/channels/611027490848374811/1370814206915379251/1377446719771512955

Unit 7 Discussion Post 2: When we think of our systems, sometimes an airgapped system is simple to think about because everything is closed in. The idea of alerting or reporting is the opposite. We are trying to get the correct, timely, and important information out of the system when and where it is needed.

 Read the summary at the top of: https://docs.google.com/document/d/199PqyG3UsyXlwieHaqbGiWVa8eMWi8zzAn0YfcApr8Q/edit?tab=t.0
 *a.    What is the litmus test for a page? (Sending something out of the system?)*

> Pages should be urgent, important, actionable, and real.




*b.    What is over-monitoring v. under-monitoring. Do you agree with the assessment of the paper? Why or why not, in your experience?*


> Err on the side of removing noisy alerts – over-monitoring is a harder problem to solve than under-monitoring.
Yes -in my experience the were always an operations team that receive the alerts first on their dashboard or email group.  Paging was only performed as an escalation by a person. The rule of over vs under applies first the the operations team and then also to the escalation procedure. As the article explained, distinguishing between the two, appears to be very difficult.



 *c.    What is cause-based v. symptom-based and where do they belong? Do you agree?*

> Include cause-based information in symptom-based pages or on dashboards, but avoid alerting directly on causes.
- Cause-based alerts are bad (but sometimes necessary)
   Alert on the data unavailability.  Alert on the symptom....
- Cause-based alerts are only necessary when you're walking off a cliff...
   e.g. running out of quota (set limits) or memory or disk I/O, etc.

---

## Definitions/Terminology
### Telemetry
> This allows IT teams to collect, record and transmit data.
> telemetry gathers and analyzes data from various remote sources so that organizations will have insight into their processes and systems.

### Tracing
> traces give us the big picture of what happens when a request is made to an application.
> traces are essential to understanding the full path a request takes in your application.

    - Span
	> a span in one of the steps within a trace.
	> a trace consists of multiple spans
	> a span is distinct unit of work within a trace (e.g. a db query, api call)
	> spans can be nested to accurately describe sub tasks

    - Label
	> a label represent the user interface items and inputs


### Time Series Database (TSDB)
> a TSDB is a database optimized for tie stamped data.
> the data is measurements or events that have been tracked and recorded over time
> the database becomes a history of what happened on a system.
> every entry is time stamped to  allow for querying and analysis of the data.


### Queue
> a queue is a list of jobs or objects waiting to be processed, first in first out.

### Upper control limit / Lower control limit (UCL/LCL)
> Control charts in Six Sigma are used to help optimize processes by identifying variations.
> The sigma refers to the standard deviation from the center line (meaning the baseline.)
> 3 sigmas above the baseline/center line is the upper control limit
> 3 sigmas below the baseline/center line is the lower control limit
> the upper and lower control lomits are the point where things have changed or deviated too much from expected ranges.

### Aggregation
> Aggregation is the collection and summarizing of raw data for later analysis.
> Aggregation is also a concept where two or more entities are combined to be treated as one unified entity.

### Service Level (SL)
- SLO
> Service Level Objectives are the target objectives (target value or target range of values).
> this is basically how organizations define the sub-goals to meet the SLA.


- SLA 
> Service Level Agreements are the formal contract defining what services will be provided to a client.
> this is basically the top : SLO's and SLI's help define the SLA.

- SLI
> Service Level Indicators are the actual values that have been measured to show the specific level of service achieved.
> this is the lowest level measured towards the goal of the SLO that is part of the SLA.

### Push v. Pull of data
> Pushing data is where the service or server pushes out / sends data to a collector.
> Pulling data, is a situation where a collector pulls in / contacts an actual service or server, asking it to give it data, metrics, et. al.

### Alerting rules
> These are rules that are defined to limit unnecessary escalation.
> These rules when properly defined allow us to automate escalation of various problems.
> Alerting rules allow us to prioritize alerts so that they only occur during the proper and relevant times /situations.
> Alerting rules will often rely on severity categories to determine what & how should be notified and during what time periods.

### Alertmanager
> this is an optional tool that is part of the Promethesus application.
    - Alert template
	> the alert manager email template in alertmanager.yml allows admins to properly configure alerts

    - Routing
    	> this is set to ensure alerts go to the correct email recipients
	> the route section of alertmanager.yml is where we can define custom email receivers to reduce unnecessary alerts.

    - Throttling
	> this is done when alerts are too frequent

### Monitoring for defensive operations
    - SIEM
	> Security information and event management
	> tools such as splunk, securonix, arcsight
	> these tools aggregate data from many different sources


    - Intrusion Detection Systems - IDS
	> these type of tools focus on monitoring network traffic for malicious activities
	> an IDS system can be placed before the firewall


    - Intrusion Prevention Systems - IPS
	> these tools intercept and analyze traffic looking for malicious traffic.
	> an IPS system can be placed after the firewall








## Notes During Lecture/Class:

Links:
- https://promlabs.com/promql-cheat-sheet/
- https://www.sans.org/information-security-policy/
- https://www.sans.org/blog/the-ultimate-list-of-sans-cheat-sheets/

Terms:

Useful tools:
- STIG Viewer 2.18
- SCC Tool (version varies by type of scan)
- OpenScap


Lab and Assignment
Unit7_Monitoring_and_Alerting - To be completed outside of lecture time.


Digging Deeper

1. Look into Wazuh: 
   Security Information and Event Management (SIEM). Real Time Monitoring | Wazuh: https://wazuh.com/platform/siem/

    a. What are their major capabilities and features? (what they advertise)
    b. What are they doing with logs that increases visibility and usefulness in the security space? 
       Log data analysis - Use cases · Wazuh documentation: https://documentation.wazuh.com/current/getting-started/use-cases/log-analysis.html

Reflection Questions
1. What do I mean when I say that security is an art and not an engineering practice?
2. What questions do you still have about this week?
3. How are you going to use what you've learned in your current role?
