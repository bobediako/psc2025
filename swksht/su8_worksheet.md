# ProLUG Security Engineering

# Unit 8 Worksheet

## Instructions

Fill out this sheet as you progress through the lab and discussions. Hold your worksheets until
the end to turn them in as a final submission packet.


## Discussion Questions:

### Unit 8 Discussion Post 1:

https://discord.com/channels/611027490848374811/1373377997582893066/1377518418961633300

Security
Unit 8 Discussion Post 1:
Read about configuration management here: https://en.wikipedia.org/wiki/Configuration_management

a.    *What overlap of terms and concepts do you see from this week’s meeting?*

- Configuration Management originated in the United States Department of Defense in the 1950s as a technical management discipline for hardware material items—and it is now a standard practice in virtually every industry.

 - a standardized, systematic approach that ensures consistency


b.    *What are some of the standards and guidelines organizations involved with configuration management?*

- ISO 10007 Quality management systems – Guidelines for configuration management


c.    *Do you recognize them from other IT activities?*

- ISO 10007 includes change control and configuration audits


### Unit 8 Discussion Post 2:
https://discord.com/channels/611027490848374811/1373378163211505724/1377527916606390433

Security
Unit 8 Discussion Post 2:
Review the SRE guide to treating configurations as code. Read as much as you like, but focus down on the “Practical Advice” section: https://google.github.io/building-secure-and-reliable-systems/raw/ch14.html#treat_configuration_as_code


a.    *What are the best practices that you can use in your configuration management adherence?*

- the best practices regarding code versioning and change review
- requiring that configuration changes be checked in, reviewed, and tested prior to deployment


b.    *What are the security threats and how can you mitigate them?*
- security vulnerability scans prevent the release of code with known vulnerabilities
- a deployment policy requiring packages and code come from approved source repositories
- separate privileges so that custom build scripts do not have access to the signing key
- explicitly specify the choice of compiler and build tools so that binaries are safe



c.    *Why might it be good to know this as you design a CMDB or CI/CD pipeline?*
- we need to "Verify Artifacts, Not Just People"...

- we need to ensure the safety of the SOURCE -> BUILD -> TEST -> DEPLOY pipeline


## Definitions/Terminology

- System Lifecycle
> NIST defines System Lifecycle as the period that begins when a system is conceived and ends when the system is no longer available for use.
	> Planning -> Purchasing -> Deployment -> Maintenance -> Replacement -> Recycling
	> Plan -> Develop/Acquire -> Integrate -> Maintain/Upgrade -> Retire

> System Lifecycle is sometimes referred to as the systems development lifecyce (SDLC)
> SDLC often has 6 to 9 stages in a cycle consisting of
	> Analysis -> Planning/Design -> Dev -> Test -> Deployment/Implementation -> Maintenance
	> Planning -> Analysis -> Design -> Implementation -> Testing & Integration -> Maintenance


- Configuration Drift
> this is the phenomenon where the system gradually changes over time.
> As so as a system is newly really into production it immediately begins to drift and change from the original release
> systems are usually built to match a specific documented state; but individual changes occur over time so that two identical systems eventually differ from each other in significant ways

- Change management activities
    - CMDB
	> Configuration Management DataBase is a centralized repository for an organizations hardware and software configurations items.
	> service now is a popular product in this area
	> bmc is another provider in this area

    - CI
	> Configuration Items are main elements / entries of a CMDB 
	> CI's are hardware, software, networks, databases
	> they are also sometimes details of facilities, locations, etc.
	> the will common properties such as a unique name, ownership, id#

    - Baseline
	> A CMDB baseline is a snapshot of the CI at a specific point in time.
	> The baseline snapshot servers as a reference to track and manage changes
	> When reviewing changes over time, you will be comparing the configuration from one point in time / now, against your baseline.

- Build book
	> this another term under this umbrella of ITIL terms
	> this describes the build method - the steps to build a system from scratch
	> the build book is how the environment / system is created
	> it is typically a step by step guide for how to setup a system 


- Run book
	> Runbook aka playbook are the steps that a team must follow for a specific task
	> it is typically a step by step guide for daily / weekly / occasional tasks

- Hashing
    - md5sum
	> use md5sum for quick non criticalfile integrity checks where security is not the top concern.
	> faster than sha-2 family of hashes

    - sha<x>sum
	> use sha256sum for secure & reliable hashing for encrypting sensitive documents
	> slower but more secure than md5sum 

- IaC
	> Infrastructure as code

- Orchestration
	> automated configuration, management, coordination of releases 
	> automating a series of tasks to work together and be performed as one unit

- Automation
	> creating clear, consistent and repeatable processes to replace manual tasks.

- AIDE
	> ?Advanced Intrusion Detection Environment?
	> compares the baseline in the DMDB against a running system to detect changes for security






## Notes During Lecture/Class:

Links:
- https://google.github.io/building-secure-and-reliable-systems/raw/ch14.html#treat_configuration_as_code
- https://en.wikipedia.org/wiki/Configuration_management
- https://www.sans.org/information-security-policy/
- https://www.sans.org/blog/the-ultimate-list-of-sans-cheat-sheets/

Terms:

Useful tools:
- STIG Viewer 2.18
- Ansible
- Killercoda

Lab and Assignment
Unit8-Configuration-drift-remediation - To be completed outside of lecture time.

Digging Deeper

1. Review more of the SRE books from Google: https://sre.google/books/ to try to find 
   more useful change management practices and policies.


Reflection Questions

1. How does the idea of control play into configuration management? Why is it so
important?

2. What questions do you still have about this week?

3. How are you going to use what you’ve learned in your current role?
