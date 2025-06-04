ProLUG Security Engineering Unit 1 Worksheet


Instructions
Fill out this sheet as you progress through the lab and discussions. Hold your worksheets until the end to turn them in as a final submission packet.


Discussion Questions:
Unit 2 Discussion Post 1: The first question of this course is, "What is Security?"
1. Describe the CIA Triad.
2. What is the relationship between Authority, Will, and Force as they relate to security?
3. What are the types of controls and how do they relate to the above question?


##start text copied from discord posting

https://discord.com/channels/611027490848374811/1353052899047116912/1356016174869250228

1 Describe the CIA Triad.
Confidentiality is focused on limiting authorized to the specific information via specific access.
Integrity is about guarding information from tampering or improper modification.  Ensuring it is authentic,
Availability is focused on ensuring proper, timely and consistently reliable access to information.

2 What is the relationship between Authority, Will, and Force as they relate to security?
At first, I assumed authority, will and force described the attacker to a system.
That the attacker or threat lacks authority to enter parts of the system.
Had the will to bypass controls. 
And will use types of force to bypass controls.
But I see from everyone's responses 
-that it really describes the 'drive' of the administrator and his team to protect the systems and environments.

3 What are the types of controls and how do they relate to the above question?
Proper implementation of Access controls is essential for Confidentiality and Authority.
The integrity of a system can be protected by ensuring end to end encryption.
Availability of a system is essential for access.  The servers must be up and their load must be manageable. The administration and his team need to be proactive in protecting the system.
The proactive stance appears to directly connect to the architects, administrators and operators using their varied authority to define controls to protect the system. They require the will to follow the rules already set.  They need to follow up with all three to ensure rules aren’t broken.  To ensure open doors and windows are not ignored.

##end text copied from discord posting

https://discord.com/channels/611027490848374811/1353053002633711677/1360353373551067218

Unit 2 Discussion Post 2: Find a STIG or compliance requirement that you do not agree is necessary for a server or service build.


1. What is the STIG or compliance requirement trying to do?


2. What category and type of control is it?


3. Defend why you think it is not necessary. (What type of defenses do you think you could present?)

##start text copied from discord posting

Find a STIG or compliance requirement that you do not agree is necessary for a server or service build.
MariaDB Enterprise 10.x Security Technical Implementation Guide :: Version 2, Release: 3 Benchmark Date: 30 Jan 2025
Fourth from the top
•    Vul ID: V-253669           
•    Rule ID: SV-253669r960864_rule
•    Group Title: SRG-APP-000080-DB-000063


What is the STIG or compliance requirement trying to do?
•    This STIG describes a situation where users may be sharing account credentials.

What category and type of control is it?
•    This is an Operational category and appears to be a preventative control.

Defend why you think it is not necessary. 
•    This stig is not necessary because it is too vague.  It is essentially jus a CYA (cover your assets) entry.  It vaguely says don’t let database users share accounts – expecting the dba or admin to track down all the possible shared users.  Then perform their own investigation.  It gives leeway to simply decide that some shared accounts are ok and others are not ok.  The STIG then tells the admin to delete accounts.

(What type of defenses do you think you could present?)
•    My defense would be to contact the application managers for each of the related applications and ask for their documents on user access.  There is no practical way in any mid-to-large environment for the admin to just delete accounts because they think someone is sharing passwords.  
•    It’s likely that an application itself uses a shared account for various tasks.  This would be documented in the application managers’ documents.  It’s an app issue at heart. The admin can’t do anything about this stig and shouldn’t be removing accounts at will.

##end text copied from discord posting


Definitions/Terminology


CIA Triad:

The acronym stands for Confidentiality, Integrity and Availability which are the key principles of information and network secuirty.
Confidentiality - The goal is to prevent unauthorized access.  To ensure that information is accessed only by authorized individuals.
Integrity - ensure that data cannot be altered while in transit nor altered after saved to file, etc. Guarantee that data is accurate.
Availabilty - maintaining the systems to ensure they can be accessed when needed by users and other stakeholders. 

Regulatory Compliance:

Every organization must follow certain laws and regulations.  RC describes the policies taken to comply with these rules.

HIPAA:

This is a 1996 U.S. law designed to protect patient health information and ensure privacy in healthcare.A It allows for information to be used in various situations only if "de-identified."  That process requires removal of specified identifiers of the people whose records are being shared.

Industry Standards:

Anything that is widely accepted within a group of similar companies or organizations.

PCI/DSS:

Payment Card Industry Data Security Standard - rules for handling information related to money, credit cards, et. al.

Security Frameworks:

These are structured set of standards and best practices to implement cybsecurity goals.

CIS:

The Center for Internet Security defines a set of security controls that have become an industry standard. it is currently on version 8.1.  These controls allign with NIST, GDPR, HIPAA, ISO 27001 and PCIDSS frameworks.

STIG:

Security Technical Implementation Guides as defined by the US DOD (DISA).  STIG's appear to be the US governments version of inductury standard quarterly releases.  The CVE program by MITRE is another similar program. Befor that companies regulary release patches on a quartely basis under various names such as critical patch releases or updates.


Notes During Lecture/Class:
https://www.hhs.gov/hipaa/for-professionals/privacy/laws-regulations/index.html`


Links:
-  https://public.cyber.mil/stigs/downloads
-  https://excalidraw.com
-  https://www.open-scap.org
-  https://www.sans.org/information-security-policy
-  https://www.sans.org/blog/the-ultimate-list-of-sans-cheat-sheets




Terms:




Useful tools:
* STIG Viewer 2.18
* SCC Tool (version varies by type of scan)
* OpenScap






Lab and Assignment
Unit1_Build_Standards_and_Compliance - To be completed outside of lecture
time.


Digging Deeper
1. Research a risk management framework. https://csrc.nist.gov/projects/risk-management/about-rmf
   - What are the areas of concern for risk management?


2. Research the difference between quantitative and qualitative risks.
   - Why might you use one or the other?


3. Research ALE, SLE, and ARO.
   - What are these terms in relation to?
   - How do these help in the risk discussion?


Reflection Questions


1. What questions do you still have about this week?




2. How are you going to use what you've learned in your current role?
