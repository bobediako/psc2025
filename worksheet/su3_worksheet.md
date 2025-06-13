# ProLUG Security Engineering
## Unit 3 Worksheet

* Instructions
Fill out this sheet as you progress through the lab and discussions. Hold your worksheets until
the end to turn them in as a final submission packet. *

Discussion Questions:

Unit 3 Discussion Post 1: There are 16 Stigs that involve PAM for RHEL 9. Read the guide from Rocky Linux here: https://docs.rockylinux.org/guides/security/pam/

https://discord.com/channels/611027490848374811/1360636294673465626/1362539851420799078

Security - Unit 3 Discussion Post 1: There are 16 Stigs that involve PAM for RHEL 9. Read the guide from Rocky Linux
### What are the mechanisms and how do they affect PAM functionality?
- account
- auth
- password
- session
### Review /etc/pam.d/sshd on a Linux system, what is happening in that file relative to these functionalities?

Here are some of the combinations of **mechanism** plus **control flags** 

auth       substack
> authentication with password &include other config settings
auth       include 
>  authentication with password & include configs
account    required  
>  authentication is required to continue.  If it fails don’t complain until then end when all other checks are done.
account    include 
> auth based on time restrictions, etc. & include other configs
password   include 
> update credentials if auth token expires or changes & include other configs
session    required 
> this relates to the initial session setup like logging and termination at the end of the session.  It is required and must be successful.
session    optional 
> this relates to the session… but if it fails the result is ignored and does not prevent login.
session    include      
### What are the common PAM modules?

```
pam_sepermit.so
pam_nologin.so
pam_selinux.so 
pam_loginuid.so
pam_namespace.so
pam_keyinit.so
pam_motd.so
password-auth
postlogin
```
### Review /etc/pam.d/sshd on a Linux system, what is happening in that file relative to these functionalities?

```
Here are 5 
pam_sepermit - PAM module to allow/deny login depending on SELinux enforcement state
pam_nologin - Prevent non-root users from login
pam_selinux - PAM module to set the default security context
pam_loginuid - Record user's login uid to the process attribute
pam_namespace - PAM module for configuring namespace for a session
Blog 
https://www.kuppingercole.com/blog/guest/10-use-cases-for-universal-privilege-management
```

### Unit 3 Discussion Post 2: Read about active directory (or LDAP) configurations of Linux via sssd here:

https://discord.com/channels/611027490848374811/1360640634548912430/1362897695643275306

Security - Unit 3 Discussion Post 2: Read about active directory (or LDAP) configurations of Linux via sssd here: https://docs.rockylinux.org/guides/security/authentication/active_directory_authentication/ 
1.    Why do we not want to just use local authentication in Linux? Or really any system?
```
Because most enterprises use windows as their desktop it makes sense to share the same username password with the Linux environment.  We have learned from the video that Microsoft AD has 90% of the market – so that product is the standard to connect to Linux servers.  This allows us to implement single sign on SSO across the environment using the username / password that users are most likely to remember… the one they use for their desktop.  This also helps administrators because they no longer have to maintain accounts for every single separate machine.  Another term that is used to describe this setup is Federated authentication.
```
2.    There are 4 SSSD STIGS.
a.    What are they? b. What do they seek to do with the system?
```
V-258019 : Allow users to manually invoke a session lock when leaving their terminal/desk.
Add an entry to the file /etc/dconf/db/local.d/00-security-settings
V-258122 : enable smart card MultiFactor authentication.
Add entry to"/etc/sssd/sssd.conf"
V-258123 : Enable certificate status checks for multi factor authentication 
Add an entry to the file "/etc/sssd/conf.d/certificate_verification.conf"
V-258133 :  Prohibit the use of cached authentication after one day.
Add an entry to the file /etc/sssd/sssd.conf
```




## Definitions/Terminology

PAM:

    > Pluggable Authentication Modules are a set of libraries that provide modular security features to systems. The related config file is pam.conf

AD:

    > Microsoft Active Directory.  A domain controller is used to authenticate and authorize users.  It's large market share in windows means that it is now the predominant service.  Using AD to authenticate users for both Microsoft and Linux servers allows admins to create networks with a centralized authentication system.  Apparently Micosoft uses a proprietary version of Kerberos to implement some of its protocols.  It also uses LDAP.

LDAP:

    > Lightweight Directory Access Protocol. This is a protocol and can  be used with various directory services.

sssd:

    > this is an extra set of packages and libraries.  It installs a daemon that will allow the host to act as a client for services provided by different machines on the network.  These different services are various remote identity providers suh as LDAP, Active Directory, Kerberos and FreeIPA.  These providers are centrally controlled directory services; sssd allows our client machine to use this information locally for login and identity management. The information is cached and is not permanent on the client.

oddjob:

    > oddjob in the movies is the henchman and driver for a bond villian.  oddjob in linux appears to be the henchman for redhat systems allowing unpriveleged processes to take privelged actions.

krb5:

    > Kerberos is a network authentication protocol created by MIT.  krb5 is the current version. Kerberos can be installed as either a client or a server.  As a client we can install the krb5-workstation package. /etc/krb5.conf is the config file.

realm/realmd:

    > realmd is part of the kerberos suite.  Realms in Kerberos are like domains in DNS.  Realms are how a goup of machines are organized -meaning a group of machines belong to a realm.  The can be other groups of machines organized into different realms.  Users can be authenticated within their own realm.  They can also be authenticated into other realms using cross realm kerberos trusts.  Realms are logical groups. Also note that kerberos 4 is different than kerberos 5.

wheel (system group in RHEL):

    > This is the super user group zero in all un*x systems since the 1980's...



Notes During Lecture/Class:

Links:
- https://www.sans.org/information-security-policy/
- https://www.sans.org/blog/the-ultimate-list-of-sans-cheat-sheets/
- https://docs.rockylinux.org/guides/security/pam/
- https://docs.rockylinux.org/guides/security/authentication/active_directory_authentication/
- https://docs.rockylinux.org/books/admin_guide/06-users/

Terms:
Useful tools:
- STIG Viewer 2.18
- SCC Tool (version varies by type of scan)
- OpenScap
Lab and Assignment
Unit3_Identity_and_Access_Management - To be completed outside of lecture time.
Digging Deeper

1. How does /etc/security/access.conf come into play with pam_access? Read up on it
here: https://man7.org/linux/man-pages/man8/pam_access.8.html
    a. Can you find any other good resources?
    b. What is the structure of the access.conf file directives?
    2. What other important user access or user management information do you learn by
reading this? https://docs.rockylinux.org/books/admin_guide/06-users/
a. What is the contents of the /etc/login.defs file? Why do you care?

Reflection Questions

1. What questions do you still have about this week?
2. How are you going to use what you've learned in your current role?
