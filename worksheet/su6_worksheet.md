## ProLUG Security Engineering 
# Unit 6 Worksheet 
 
*Instructions*
*Fill out this sheet as you progress through the lab and discussions. Hold your worksheets until the end to turn them in as a final submission packet.*

---

## Discussion Questions:

### Unit 6 Discussion Post 1: 
Review chapter 15 of the SRE book: 


https://discord.com/channels/611027490848374811/1368299025648058498/1369478670623703163

Security Unit 6 Discussion Post 1:  
Review chapter 15 of the SRE book: https://google.github.io/building-secure-and-reliable-systems/raw/ch15.html#collect_appropriate_and_useful_logs. 
There are 14 references at the end of the chapter. Follow them for more information. One of them:  https://jvns.ca/blog/2019/06/23/a-few-debugging-resources/  should be reviewed for question “c”.
a.    What are some concepts that are new to you?
**Data anonymization or pseudonymization**

> This is essential in financial and health service industries.  It allows for PII (Personally Identifiable Information) to be kept secret.  This is a function of privacy.


**Encryption (public-private key )**

> Encryption is a process to change the form of data /messages to so that only specific people can read it.  Also called assymmetric encryption.  Different keys are used to encrypt and decrypt messages. The public key encrypts the message.  The private key decrypts the message.
Symmetric key encryption uses a single key for both encryption and decryption.  Like a password….  This is used for large amounts of data.

**Cloud access security brokers (CASBs)**

> CASB are proxies between end users and cloud services to enforce security controls and provide logging.


**full packet captures vs netflow data**

> Full packet captures contain full-fidelity data- everything.
NetFlow data contains a summarization of the data.
The full data is kept for a short amount of time while the summary is kept for much longer.



### Unit 6 Discussion Post 2: 


https://discord.com/channels/611027490848374811/1368299171282681928/1369429034399830128

Unit 6 Discussion Post 2: Read sre.google/sre-book/monitoring-distributed-systems/ 
*What interesting or new things do you learn in this reading? *

> White-box monitoring
Black-box monitoring
Four golden signals (latency, traffic, errors, saturation)


*What may you want to know more about? *

> Ansible vs puppet vs chef


*What are the “4 golden signals”?*

> Four golden signals (latency, traffic, errors, saturation)


*After reading these, why is immutability so important to logging?*

> logs need to simple and not overly verbose.  Logs need to provide the relevant information. *edit* immutable refers to protecting the logs from being changed, so that what has been recorded can be trusted. The importance of immutability is that the log should be a permanent and unchangeable record of the past.  We keep logs to record security and historical events.  But if a bad actor could change things in our system... if a bad actor can change the logs too... then we have no security.  As in the video- logs should be 'chiseled in stone.'
 

~~In this case, if the format is immutable ( unchanging set of permanent categories), then action can be taken on the output.  The four signals are the categories that need to be focused on so that problems can be identified and resolved via logs.~~



*What do you think the other required items are for logging to be effective?*

> Correct timing of an error.  Knowing when `it` happened is essential.  This means ensuring the clocks of all the servers in sync with each other.  It is important to have tools that allow for comparing output in different time zones easily when supporting a global environment.  This is done via services like NTP


---

## Definitions/Terminology 

Types of logs :
	- Host 
	- Application 
	- Network 
	- DB

    > Log files are software-generated files of actions or errors.  This creates a historical record of what happened during execution.  The best logs will record descriptive information such as timestamps to give context to the rocrds. the above list describe different functions, network vs db vs host logs, each with their own relevant record of what happened.

Immutable:

    > Immutable logs are files that have not been tampered with and can be trusted as a honest source of what happened furing any period of time. The best way to ensure logs are immutable is to push/pull them to another server in real-time.  That way they can be accessed if the machine goes down and they can be trusted in situations where the server has been compromised.  Any superuser could change the logs on a system to cover their tracks or erase evidence of their mistakes so ensuring logs are immutable are a promise that must be kept by trusted individuals.

Structure of Logs :
	- RFC 3164 BSD Syslog 
	- RFC 5424 IETF Syslog 
	- Systemd Journal 

    > syslog is a standard for system logs.  The two listed rfc's are different formats for how the logs are written.  The BSD standard is older than the IETF standard.  One example change is that the IETF adds the year to the timestamp. fyi -  they are called syslog messages... at least since the old days.... it has changed since they are now being sent to remote servers for immutability.


Log rotation:

    > LOg rotation has many benefits.  It ensures that log files do not become to large.  Troubleshooting outages usually involves only recent log messages so rotation of logs is a useful policy. Outages are also referred to as incident response and it usually focuses on the last few hours of logs.  Rotating logs is essentially breaking the logs up into smaller files based of date or size.

Rsyslog:

    > rsyslog is an updated version of the syslog daemon.  It introduces the features that allow us to send syslog messages off the specific server to a centralized syslog server that can collects logs from many different machines.  It supports various protocols that allow for logs to be sent to different types of collection servers.

Log aggregation :
	- ELK	-> an opensource set of programs called the ELK Stack
		--> Elasticsearch stores logs (indexing)
		--> Logstash provides data processing (data aggregation)
		--> Kibana provides visualization (analysis)
		The order/flow is: server log -> logstash -> elasticsearch -> kibana
		(But I guess ELK sounds better than LEK :-)
		NOTE: Beats then Kafka is sometimes used before the above in a stack
	- Splunk -> a propietary software suite that is complete out of the box (and sponsor of F1 :-)
	- Graylog ->  Opensource with paid support options uses Elasticsearch
	- Loki 	  ->  utilizes data in object storage (AWS S3, Google cloud, Azure Kubernetes Servive AKS )


SIEM :

    > Security Information and Event Management






Notes During Lecture/Class: 



Links: 
- https://grafana.com/docs/loki/latest/query/analyzer/  
- https://www.sans.org/information-security-policy/ 
- https://www.sans.org/blog/the-ultimate-list-of-sans-cheat-sheets/ 
- https://public.cyber.mil/stigs/downloads/ 

Terms:  
 
Useful tools: 
- STIG Viewer 2.18 
- SCC Tool (version varies by type of scan) 
- OpenScap 
 
Lab and Assignment 
Unit6_Logs_and_Parsing - To be completed outside of lecture time. 

Digging Deeper
 
1. Find a cloud service and see what their logging best practices are for security 
incident response. Here is AWS: https://aws.amazon.com/blogs/security/logging-
strategies-for-security-incident-response/ 
	a. What are the high level concepts mentioned? 
	b. What are the tools available and what actions do they take? 
	c. What are the manual and automated query capabilities provided, and how 
	   do they help you rapidly get to a correct assessment of the logged events? 

2. Open up that STIG Viewer and filter by "logging" for any of the previous STIGs we've 
worked on. (Mariadb has some really good ones.) 

a. What seems to be a common theme? 

b. What types of activities MUST be logged in various applications and 
operating systems? 
	i. Does it make sense why all logins are tracked? 
	ii. Does it make sense why all admin actions, even just attempted admin 
	    actions, are logged? 

Reflection Questions 

1. What architectures have you used in your career?  
	a. If you haven't yet worked with any of these, what do you think you would 
	   architect in the ProLUG lab (~60 virtual machines, 4 physical machines, 1 
	   NFS share, and 2 Windows laptops?) 
 
2. What questions do you still have about this week? 
 
3. How are you going to use what you've learned in your current role? 
 
 
 
