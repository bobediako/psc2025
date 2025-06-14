# Unit 3 Lab - User Access and System Integration

### Does any of it look familiar to you?

Here are some of the combinations of mechanism plus control flags 

auth       substack
> authentication with password &include other config settings
auth       include 
> authentication with password & include configs
account    required
> authentication is required to continue.
> If it fails don’t complain until the end when all other checks are done.
account    include 
> auth based on time restrictions, etc. & include other configs
password   include 
> update credentials if auth token expires or changes & include other configs
session    required 
> this relates to the initial session setup like logging and termination
> (at the end of the session).  It is required and must be successful.
session    optional 
> this relates to the session… 
> but if it fails the result is ignored and does not prevent login.
session    include
> this relates to the session… include other config


---


## Filter by pam and see how many STIGS you have. (Why is it really only 16?)

> It is less than 16 because one of the STIGS refer to spam, etc.


## Examine STIG V-257986
### What is the problem?
> 



### What is the fix?

> the fix is to edit the entry for UsePAM in /etc/ssh/sshd_config

```bash
[root@hammer14 ~]# ls -l  /etc/ssh/sshd_config
-rw-------. 1 root root 3668 Aug 17  2024 /etc/ssh/sshd_config
[root@hammer14 ~]# grep -C 3 UsePAM /etc/ssh/sshd_config
# If you just want the PAM account and session checks to run without
# PAM authentication, then enable this but set PasswordAuthentication
# and KbdInteractiveAuthentication to 'no'.
# WARNING: 'UsePAM no' is not supported in RHEL and may cause several
# problems.
#UsePAM no

#AllowAgentForwarding yes
#AllowTcpForwarding yes
```


### What type of control is being implemented?
>  Pr.. Te..




### Is it set properly on your system?

> No it was disabled


### Can you remediate this finding?

> It has been fixed / remediated using the following

```bash
[root@hammer14 ~]# cp -p /etc/ssh/sshd_config /etc/ssh/.sshd_config.`date +%F`
[root@hammer14 ~]# perl -pi -e "s/^#UsePAM no/UsePAM yes/" /etc/ssh/sshd_config
[root@hammer14 ~]# diff /etc/ssh/sshd_config /etc/ssh/.sshd_config.`date +%F`
96c96
< UsePAM yes
---
> #UsePAM no

```


---


## Check and remediate STIG V-258055


### What is the problem?

> This is a request tolock out the root account upon 3 wrong tries

### What is the fix?

> the suggested fix is to use the faillock feature

### What type of control is being implemented?

> Te Pr

### Are there any major implications to think about with this change on your system? Why or why not?

> Yes this appears to be a very dangerous and disruptive setting. Any wrong attemps will directly impact a sysadmin's ability to work on the system.

### Is it set properly on your system?

> No it is disabled

```bash
[root@hammer14 ~]# grep -C 3 "# even_deny"  /etc/security/faillock.conf
#
# Root account can become locked as well as regular accounts.
# Enabled if option is present.
# even_deny_root
#
# This option implies the `even_deny_root` option.
# Allow access after n seconds to root account after the
```

### How would you go about remediating this on your system?

> I would fight this STIG... then I would give in and implement as follows...

```bash
[root@hammer14 ~]# cp -p  /etc/security/faillock.conf  /etc/security/.faillock.conf.`date +%F`    ;   perl -pi -e "s/^# even_deny_root/even_deny_root/"  /etc/security/faillock.conf  ;  diff /etc/security/faillock.conf  /etc/security/.faillock.conf.`date +%F`
49c49
< even_deny_root
---
> # even_deny_root
```




---


## Check and remediate STIG V-258098

### What is the problem?
> The STIG requests a specific level of passowrd complexity

### What is the fix?
> The fix is to enable the pam module pam_pwquality

### What type of control is being implemented?
> Tech Prev

### Is it set properly on your system?
> Yes and no... It is currently set to be stricter than the STIG with the requisite control."
```
[root@hammer14 ~]# grep pam_pwquality /etc/pam.d/system-auth
password    requisite     pam_pwquality.so try_first_pass local_users_only retry=3 authtok_type=
```

---

## Filter by "password complexity"


### How many are there?
> 14

### What are the password complexity rules?
> Listed here
```
[root@hammer14 ~]# grep  pam_pwquality /etc/pam.d/system-auth /etc/pam.d/password-auth
/etc/pam.d/system-auth:password    requisite     pam_pwquality.so try_first_pass local_users_only retry=3 authtok_type=
/etc/pam.d/password-auth:password    requisite     pam_pwquality.so try_first_pass local_users_only retry=3 authtok_type=
```

### Are there any you haven't seen before?
> not familiar with any


---


## Filter by sssd
### How many STIGS do you see?
> 4

### What do these STIGS appear to be trying to do? What types of controls are they?
> Tech Deterrent



---

## OpenLDAP Setup
### Install and configure OpenLDAP
### 1. Stop the warewulf client

```bash
[root@hammer14 ~]# systemctl status wwclient
● wwclient.service - Warewulf node runtime overlay update
     Loaded: loaded (/etc/systemd/system/wwclient.service; enabled; preset: dis>
     Active: active (running) since Thu 2025-05-22 23:12:55 MST; 16h ago
   Main PID: 792 (wwclient)
      Tasks: 8 (limit: 24363)
     Memory: 7.5M
        CPU: 2.074s
     CGroup: /system.slice/wwclient.service
             └─792 /warewulf/wwclient

May 23 14:02:59 hammer14 wwclient[792]: 2025/05/23 14:02:59 Updating system
May 23 14:12:59 hammer14 wwclient[792]: 2025/05/23 14:12:59 Updating system
May 23 14:22:59 hammer14 wwclient[792]: 2025/05/23 14:22:59 Updating system
May 23 14:32:59 hammer14 wwclient[792]: 2025/05/23 14:32:59 Updating system
May 23 14:42:59 hammer14 wwclient[792]: 2025/05/23 14:42:59 Updating system
May 23 14:52:59 hammer14 wwclient[792]: 2025/05/23 14:52:59 Updating system
May 23 15:02:59 hammer14 wwclient[792]: 2025/05/23 15:02:59 Updating system
May 23 15:12:59 hammer14 wwclient[792]: 2025/05/23 15:12:59 Updating system
May 23 15:22:59 hammer14 wwclient[792]: 2025/05/23 15:22:59 Updating system
May 23 15:32:59 hammer14 wwclient[792]: 2025/05/23 15:32:59 Updating system
[root@hammer14 ~]# systemctl stop wwclient
```

### 2. Edit your /etc/hosts file
### Look for and edit the line that has your current server

```bash
[root@hammer14 ~]# cp -p /etc/hosts /etc/.hosts.`date +%F`
[root@hammer14 ~]# perl -pi -e "s/hammer14 hammer14-default/hammer14 hammer14-default ldap.prolug.lan ldap/" /etc/hosts
[root@hammer14 ~]# diff /etc/hosts /etc/.hosts.`date +%F`                       19c19
< 192.168.200.164 hammer14 hammer14-default ldap.prolug.lan ldap                
---
> 192.168.200.164 hammer14 hammer14-default
```

### 3. Setup dnf repo

```bash
[root@hammer14 ~]# dnf repolist | wc
      6      43     318
[root@hammer14 ~]# dnf repolist
repo id             repo name
appstream           Rocky Linux 9 - AppStream
baseos              Rocky Linux 9 - BaseOS
epel                Extra Packages for Enterprise Linux 9 - x86_64
epel-cisco-openh264 Extra Packages for Enterprise Linux 9 openh264 (From Cisco) - x86_64
extras              Rocky Linux 9 - Extras
[root@hammer14 ~]# dnf config-manager --set-enabled plus
[root@hammer14 ~]# dnf repolist
repo id             repo name
appstream           Rocky Linux 9 - AppStream
baseos              Rocky Linux 9 - BaseOS
epel                Extra Packages for Enterprise Linux 9 - x86_64
epel-cisco-openh264 Extra Packages for Enterprise Linux 9 openh264 (From Cisco) - x86_64
extras              Rocky Linux 9 - Extras
plus                Rocky Linux 9 - Plus
[root@hammer14 ~]# dnf repolist | wc
      7      49     359
[root@hammer14 ~]# dnf -y install openldap-servers openldap-clients openldap
Rocky Linux 9 - Extras                          6.3 kB/s | 2.9 kB     00:00
Rocky Linux 9 - Plus                            4.9 kB/s | 4.5 kB     00:00
Package openldap-2.6.6-3.el9.x86_64 is already installed.
Dependencies resolved.
================================================================================
 Package                Architecture Version              Repository       Size
================================================================================
Installing:
 openldap-clients       x86_64       2.6.6-3.el9          baseos          173 k
 openldap-servers       x86_64       2.6.6-3.el9          epel            2.3 M
Installing dependencies:
 libtool-ltdl           x86_64       2.4.6-46.el9         appstream        35 k

Transaction Summary
================================================================================
Install  3 Packages

Total download size: 2.5 M
Installed size: 6.3 M
Downloading Packages:
(1/3): libtool-ltdl-2.4.6-46.el9.x86_64.rpm      65 kB/s |  35 kB     00:00
(2/3): openldap-clients-2.6.6-3.el9.x86_64.rpm  278 kB/s | 173 kB     00:00
(3/3): openldap-servers-2.6.6-3.el9.x86_64.rpm  2.3 MB/s | 2.3 MB     00:01
--------------------------------------------------------------------------------
Total                                           1.3 MB/s | 2.5 MB     00:01
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1
  Installing       : libtool-ltdl-2.4.6-46.el9.x86_64                       1/3
  Running scriptlet: openldap-servers-2.6.6-3.el9.x86_64                    2/3
  Installing       : openldap-servers-2.6.6-3.el9.x86_64                    2/3
  Running scriptlet: openldap-servers-2.6.6-3.el9.x86_64                    2/3
  Installing       : openldap-clients-2.6.6-3.el9.x86_64                    3/3
  Running scriptlet: openldap-clients-2.6.6-3.el9.x86_64                    3/3
  Verifying        : openldap-servers-2.6.6-3.el9.x86_64                    1/3
  Verifying        : openldap-clients-2.6.6-3.el9.x86_64                    2/3
  Verifying        : libtool-ltdl-2.4.6-46.el9.x86_64                       3/3

Installed:
  libtool-ltdl-2.4.6-46.el9.x86_64       openldap-clients-2.6.6-3.el9.x86_64
  openldap-servers-2.6.6-3.el9.x86_64

Complete!
```

### 4. Start slapd systemctl

```bash
[root@hammer14 ~]# systemctl start slapd
[root@hammer14 ~]# systemctl status slapd
● slapd.service - OpenLDAP Server Daemon
     Loaded: loaded (/usr/lib/systemd/system/slapd.service; disabled; preset: d>
     Active: active (running) since Fri 2025-05-23 16:18:24 MST; 29s ago
       Docs: man:slapd
             man:slapd-config
             man:slapd-mdb
             file:///usr/share/doc/openldap-servers/guide.html
    Process: 11575 ExecStartPre=/usr/libexec/openldap/check-config.sh (code=exi>
    Process: 11595 ExecStart=/usr/sbin/slapd -u ldap -h ldap:/// ldaps:/// ldap>
   Main PID: 11596 (slapd)
      Tasks: 2 (limit: 24363)
     Memory: 5.1M
        CPU: 190ms
     CGroup: /system.slice/slapd.service
             └─11596 /usr/sbin/slapd -u ldap -h "ldap:/// ldaps:/// ldapi:///"

May 23 16:18:24 hammer14 systemd[1]: Starting OpenLDAP Server Daemon...
May 23 16:18:24 hammer14 runuser[11578]: pam_unix(runuser:session): session ope>
May 23 16:18:24 hammer14 runuser[11578]: pam_unix(runuser:session): session clo>
May 23 16:18:24 hammer14 slapd[11595]: @(#) $OpenLDAP: slapd 2.6.6 (Jul 26 2024>
                                               openldap
May 23 16:18:24 hammer14 slapd[11596]: slapd starting
May 23 16:18:24 hammer14 systemd[1]: Started OpenLDAP Server Daemon.
[root@hammer14 ~]# ss -ntulp | egrep "(^N|slapd)"
Netid State  Recv-Q Send-Q Local Address:Port Peer Address:PortProcess          
tcp   LISTEN 0      2048         0.0.0.0:636       0.0.0.0:*    users:(("slapd",pid=11596,fd=9))
tcp   LISTEN 0      2048         0.0.0.0:389       0.0.0.0:*    users:(("slapd",pid=11596,fd=7))
tcp   LISTEN 0      2048            [::]:636          [::]:*    users:(("slapd",pid=11596,fd=10))
tcp   LISTEN 0      2048            [::]:389          [::]:*    users:(("slapd",pid=11596,fd=8))
```

### 5. Allow ldap through the firewall
```bash
[root@hammer14 ~]# firewall-cmd --add-service={ldap,ldaps} --permanent
success
[root@hammer14 ~]# firewall-cmd --reload
success
[root@hammer14 ~]# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources:
  services: cockpit dhcpv6-client ldap ldaps ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:


```

### 6. Generate a password (Our example uses testpassword) This will return a salted SSHA password. Save this password and salted hash for later input

```bash
[root@hammer14 ~]# echo "NOTE passwd helloworld"
NOTE passwd helloworld
[root@hammer14 ~]#
[root@hammer14 ~]# slappasswd
New password:
Re-enter new password:
{SSHA}picyvG7ylr/VVSiSfv4GoF8LvqcDMwPc
```


### 7. Change the password 

```bash
[root@hammer14 ~]# cat changerootpass.ldif
dn: olcDatabase={0}config,cn=config
changetype: modify
replace: olcRootPW
olcRootPW: {SSHA}picyvG7ylr/VVSiSfv4GoF8LvqcDMwPc
```

```bash
[root@hammer14 ~]# cat changerootpass.ldif
dn: olcDatabase={0}config,cn=config
changetype: modify
replace: olcRootPW
olcRootPW: {SSHA}picyvG7ylr/VVSiSfv4GoF8LvqcDMwPc
[root@hammer14 ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f changerootpass.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
modifying entry "olcDatabase={0}config,cn=config"

```

### 8. Generate basic schemas

```bash
[root@hammer14 ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldiff
/etc/openldap/schema/cosine.ldiff: No such file or directory
[root@hammer14 ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
adding new entry "cn=cosine,cn=schema,cn=config"

[root@hammer14 ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
adding new entry "cn=nis,cn=schema,cn=config"

[root@hammer14 ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
adding new entry "cn=inetorgperson,cn=schema,cn=config"

[root@hammer14 ~]#

```

### 9. Set up the domain (USE THE PASSWORD YOU GENERATED EARLIER)

*vi setdomain.ldif*

```bash
[root@hammer14 ~]# cat setdomain.ldif
dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth" read by dn.base="cn=Manager,dc=prolug,dc=lan" read by * none

dn: olcDatabase={2}mdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=prolug,dc=lan

dn: olcDatabase={2}mdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=Manager,dc=prolug,dc=lan

dn: olcDatabase={2}mdb,cn=config
changetype: modify
add: olcRootPW
olcRootPW:  {SSHA}picyvG7ylr/VVSiSfv4GoF8LvqcDMwPc

dn: olcDatabase={2}mdb,cn=config
changetype: modify
add: olcAccess
olcAccess: {0}to attrs=userPassword,shadowLastChange by dn="cn=Manager,dc=prolug,dc=lan" write by anonymous auth by self write by * none
olcAccess: {1}to dn.base="" by * read
olcAccess: {2}to * by dn="cn=Manager,dc=prolug,dc=lan" write by * read

```

### 10. Run it

```bash
[root@hammer14 ~]# ldapmodify -Y EXTERNAL -H ldapi:/// -f setdomain.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
modifying entry "olcDatabase={1}monitor,cn=config"

modifying entry "olcDatabase={2}mdb,cn=config"

modifying entry "olcDatabase={2}mdb,cn=config"

modifying entry "olcDatabase={2}mdb,cn=config"

modifying entry "olcDatabase={2}mdb,cn=config"

[root@hammer14 ~]# cat -n setdomain.ldif
     1  dn: olcDatabase={1}monitor,cn=config
     2  changetype: modify
     3  replace: olcAccess
     4  olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth" read by dn.base="cn=Manager,dc=prolug,dc=lan" read by * none
     5
     6  dn: olcDatabase={2}mdb,cn=config
     7  changetype: modify
     8  replace: olcSuffix
     9  olcSuffix: dc=prolug,dc=lan
    10
    11  dn: olcDatabase={2}mdb,cn=config
    12  changetype: modify
    13  replace: olcRootDN
    14  olcRootDN: cn=Manager,dc=prolug,dc=lan
    15
    16  dn: olcDatabase={2}mdb,cn=config
    17  changetype: modify
    18  add: olcRootPW
    19  olcRootPW:  {SSHA}picyvG7ylr/VVSiSfv4GoF8LvqcDMwPc
    20
    21  dn: olcDatabase={2}mdb,cn=config
    22  changetype: modify
    23  add: olcAccess
    24  olcAccess: {0}to attrs=userPassword,shadowLastChange by dn="cn=Manager,dc=prolug,dc=lan" write by anonymous auth by self write by * none
    25  olcAccess: {1}to dn.base="" by * read
    26  olcAccess: {2}to * by dn="cn=Manager,dc=prolug,dc=lan" write by * read

```


### 11. Search and verify the domain is working.

```bash
[root@hammer14 ~]# ldapsearch -H ldap:// -x -s base -b "" -LLL "namingContexts"
dn:
namingContexts: dc=prolug,dc=lan

[root@hammer14 ~]#

```

### 12. Add the base group and organization.

*vi addou.ldif*

```bash
[root@hammer14 ~]# cat addou.ldiff
dn: dc=prolug,dc=lan
objectClass: top
objectClass: dcObject
objectclass: organization
o: My prolug Organisation
dc: prolug

dn: cn=Manager,dc=prolug,dc=lan
objectClass: organizationalRole
cn: Manager
description: OpenLDAP Manager

dn: ou=People,dc=prolug,dc=lan
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=prolug,dc=lan
objectClass: organizationalUnit
ou: Group

```
*run it*
*enter the password from earlier helloworld*
```bash
[root@hammer14 ~]# ldapadd -x -D cn=Manager,dc=prolug,dc=lan -W -f addou.ldif
Enter LDAP Password:
adding new entry "dc=prolug,dc=lan"

adding new entry "cn=Manager,dc=prolug,dc=lan"

adding new entry "ou=People,dc=prolug,dc=lan"

adding new entry "ou=Group,dc=prolug,dc=lan"

[root@hammer14 ~]#

```
### 13. Verifying

```bash
root@hammer14 ~]# ldapsearch -H ldap:// -x -s base -b "" -LLL "+"
dn:
structuralObjectClass: OpenLDAProotDSE
configContext: cn=config
monitorContext: cn=Monitor
namingContexts: dc=prolug,dc=lan
supportedControl: 2.16.840.1.113730.3.4.18
supportedControl: 2.16.840.1.113730.3.4.2
supportedControl: 1.3.6.1.4.1.4203.1.10.1
supportedControl: 1.3.6.1.1.22
supportedControl: 1.2.840.113556.1.4.319
supportedControl: 1.2.826.0.1.3344810.2.3
supportedControl: 1.3.6.1.1.13.2
supportedControl: 1.3.6.1.1.13.1
supportedControl: 1.3.6.1.1.12
supportedExtension: 1.3.6.1.4.1.4203.1.11.1
supportedExtension: 1.3.6.1.4.1.4203.1.11.3
supportedExtension: 1.3.6.1.1.8
supportedExtension: 1.3.6.1.1.21.3
supportedExtension: 1.3.6.1.1.21.1
supportedFeatures: 1.3.6.1.1.14
supportedFeatures: 1.3.6.1.4.1.4203.1.5.1
supportedFeatures: 1.3.6.1.4.1.4203.1.5.2
supportedFeatures: 1.3.6.1.4.1.4203.1.5.3
supportedFeatures: 1.3.6.1.4.1.4203.1.5.4
supportedFeatures: 1.3.6.1.4.1.4203.1.5.5
supportedLDAPVersion: 3
entryDN:
subschemaSubentry: cn=Subschema

[root@hammer14 ~]#
[root@hammer14 ~]# ldapsearch -x -b "dc=prolug,dc=lan" ou
# extended LDIF
#
# LDAPv3
# base <dc=prolug,dc=lan> with scope subtree
# filter: (objectclass=*)
# requesting: ou
#

# prolug.lan
dn: dc=prolug,dc=lan

# Manager, prolug.lan
dn: cn=Manager,dc=prolug,dc=lan

# People, prolug.lan
dn: ou=People,dc=prolug,dc=lan
ou: People

# Group, prolug.lan
dn: ou=Group,dc=prolug,dc=lan
ou: Group

# search result
search: 2
result: 0 Success

# numResponses: 5
# numEntries: 4
[root@hammer14 ~]#

```


### 14. Add a user
### *Generate a password (use testuser1234)*

```bash
[root@hammer14 ~]# slappasswd
New password:
Re-enter new password:
{SSHA}02Q/6vBxzKt9E4+j32ZUSuJtsX2F04Q5
[root@hammer14 ~]#

```
*vi adduser.ldif*
```bash
[root@hammer14 ~]# cat adduser.ldif
dn: uid=testuser,ou=People,dc=prolug,dc=lan
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
cn: testuser
sn: temp
userPassword: {SSHA}yb6e0ICSdlZaMef3zizvysEzXRGozQOK
loginShell: /bin/bash
uidNumber: 15000
gidNumber: 15000
homeDirectory: /home/testuser
shadowLastChange: 0
shadowMax: 0
shadowWarning: 0

dn: cn=testuser,ou=Group,dc=prolug,dc=lan
objectClass: posixGroup
cn: testuser
gidNumber: 15000
memberUid: testuser

```

### 15. run it
*I used the original helloworld pssword*
```bash
[root@hammer14 ~]# ldapadd -x -D cn=Manager,dc=prolug,dc=lan -W -f adduser.ldif
Enter LDAP Password:
adding new entry "uid=testuser,ou=People,dc=prolug,dc=lan"

adding new entry "cn=testuser,ou=Group,dc=prolug,dc=lan"


```

### 16. Verify that your user is in the system.

```bash
[root@hammer14 ~]# ldapsearch -x -b "ou=People,dc=prolug,dc=lan"
# extended LDIF
#
# LDAPv3
# base <ou=People,dc=prolug,dc=lan> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# People, prolug.lan
dn: ou=People,dc=prolug,dc=lan
objectClass: organizationalUnit
ou: People

# testuser, People, prolug.lan
dn: uid=testuser,ou=People,dc=prolug,dc=lan
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
cn: testuser
sn: temp
loginShell: /bin/bash
uidNumber: 15000
gidNumber: 15000
homeDirectory: /home/testuser
shadowMax: 0
shadowWarning: 0
uid: testuser

# search result
search: 2
result: 0 Success

# numResponses: 3
# numEntries: 2
[root@hammer14 ~]#

```

### 17. Secure the system with TLS (accept all defaults)
```bash
[root@hammer14 ~]# openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/tls/ldapserver.key -out /etc/pki/tls/ldapserver.crt
.+..........+........+....+...........+....+..+.+..+.+..+....+........+.+..............+......+.+.....+...+......+.+......+.....+...+.+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*...........+...+...+.....+............+....+...+..+......+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.......+.....+....+...+............+......+............+........+.........+...+.............+.....+....+.....+.+...........+....+......+...............+......+...+.....+......+.+..............+.+.....+...+...............+....+.....+.+..............+.........................+..+....+...........+...+..........+........+...+....+...+.....+...+.........+.......+...+..................+.....+...+.+.....+.+........+......+.+...+......+...+.....+..................+.........+.+..............+..........+...+......+..............+.......+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
......+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.+..+...+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*...+......+.+.........+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:
State or Province Name (full name) []:
Locality Name (eg, city) [Default City]:
Organization Name (eg, company) [Default Company Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (eg, your name or your server's hostname) []:
Email Address []:
[root@hammer14 ~]#
[root@hammer14 ~]# chown ldap:ldap /etc/pki/tls/{ldapserver.crt,ldapserver.key}
[root@hammer14 ~]# ls -l /etc/pki/tls/ldap*
-rw-r--r--. 1 ldap ldap 1237 May 23 19:57 /etc/pki/tls/ldapserver.crt
-rw-------. 1 ldap ldap 1708 May 23 19:56 /etc/pki/tls/ldapserver.key

```
*vi tls.ldif*

```bash
[root@hammer14 ~]# cat tls.ldif
dn: cn=config
changetype: modify
add: olcTLSCACertificateFile
olcTLSCACertificateFile: /etc/pki/tls/ldapserver.crt

add: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/pki/tls/ldapserver.key

add: olcTLSCertificateFile
olcTLSCertificateFile: /etc/pki/tls/ldapserver.crt

```

```bash
[root@hammer14 ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f tls.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
modifying entry "cn=config"

[root@hammer14 ~]#

```

### 18. Fix the /etc/openldap/ldap.conf to allow for certs

*vi /etc/openldap/ldap.conf*

```
[root@hammer14 ~]# grep -v ^# /etc/openldap/ldap.conf | uniq

TLS_CACERT /etc/pki/tls/ldapserver.crt
TLS_REQCERT never

SASL_NOCANON    on

[root@hammer14 ~]# cat /etc/openldap/ldap.conf
#
# LDAP Defaults
#

# See ldap.conf(5) for details
# This file should be world readable but not world writable.

#BASE   dc=example,dc=com
#URI    ldap://ldap.example.com ldap://ldap-master.example.com:666

#SIZELIMIT      12
#TIMELIMIT      15
#DEREF          never

# When no CA certificates are specified the Shared System Certificates
# are in use. In order to have these available along with the ones specified
# by TLS_CACERTDIR one has to include them explicitly:
#TLS_CACERT     /etc/pki/tls/cert.pem
TLS_CACERT /etc/pki/tls/ldapserver.crt
TLS_REQCERT never

# System-wide Crypto Policies provide up to date cipher suite which should
# be used unless one needs a finer grinded selection of ciphers. Hence, the
# PROFILE=SYSTEM value represents the default behavior which is in place
# when no explicit setting is used. (see openssl-ciphers(1) for more info)
#TLS_CIPHER_SUITE PROFILE=SYSTEM

# Turning this off breaks GSSAPI used with krb5 when rdns = false
SASL_NOCANON    on

[root@hammer14 ~]#

```

## SSSD Configuration and Realmd join to LDAP

### 1. Install sssd, configure, and validate that the user is seen by the system

```bash
[root@hammer14 ~]# dnf install openldap-clients sssd sssd-ldap oddjob-mkhomedir authselect
Last metadata expiration check: 2:30:18 ago on Fri May 23 17:40:04 2025.
Package openldap-clients-2.6.6-3.el9.x86_64 is already installed.
Dependencies resolved.
================================================================================
 Package                    Arch       Version              Repository     Size
================================================================================
Installing:
 authselect                 x86_64     1.2.6-2.el9          baseos        140 k
 oddjob-mkhomedir           x86_64     0.34.7-7.el9         appstream      26 k
 sssd                       x86_64     2.9.5-4.el9_5.4      baseos         27 k
 sssd-ldap                  x86_64     2.9.5-4.el9_5.4      baseos        159 k
Upgrading:
 glibc                      x86_64     2.34-125.el9_5.8     baseos        1.9 M
 glibc-common               x86_64     2.34-125.el9_5.8     baseos        290 k
 glibc-minimal-langpack     x86_64     2.34-125.el9_5.8     baseos         16 k
Installing dependencies:
 authselect-libs            x86_64     1.2.6-2.el9          baseos        236 k
 c-ares                     x86_64     1.19.1-2.el9_4       baseos        110 k
 cyrus-sasl-gssapi          x86_64     2.1.27-21.el9        baseos         26 k
 dbus-tools                 x86_64     1:1.12.20-8.el9      baseos         50 k
 glibc-gconv-extra          x86_64     2.34-125.el9_5.8     baseos        1.6 M
 libdhash                   x86_64     0.5.0-53.el9         baseos         29 k
 libipa_hbac                x86_64     2.9.5-4.el9_5.4      baseos         35 k
 libldb                     x86_64     2.9.1-2.el9          baseos        182 k
 libsmbclient               x86_64     4.20.2-2.el9_5.1     baseos         72 k
 libsss_certmap             x86_64     2.9.5-4.el9_5.4      baseos         90 k
 libsss_idmap               x86_64     2.9.5-4.el9_5.4      baseos         41 k
 libsss_nss_idmap           x86_64     2.9.5-4.el9_5.4      baseos         45 k
 libtalloc                  x86_64     2.4.2-1.el9          baseos         30 k
 libtdb                     x86_64     1.4.10-1.el9         baseos         50 k
 libtevent                  x86_64     0.16.1-1.el9         baseos         47 k
 libwbclient                x86_64     4.20.2-2.el9_5.1     baseos         41 k
 oddjob                     x86_64     0.34.7-7.el9         appstream      63 k
 samba-client-libs          x86_64     4.20.2-2.el9_5.1     baseos        5.2 M
 samba-common               noarch     4.20.2-2.el9_5.1     baseos        167 k
 samba-common-libs          x86_64     4.20.2-2.el9_5.1     baseos         99 k
 sssd-ad                    x86_64     2.9.5-4.el9_5.4      baseos        215 k
 sssd-client                x86_64     2.9.5-4.el9_5.4      baseos        161 k
 sssd-common                x86_64     2.9.5-4.el9_5.4      baseos        1.6 M
 sssd-common-pac            x86_64     2.9.5-4.el9_5.4      baseos         96 k
 sssd-ipa                   x86_64     2.9.5-4.el9_5.4      baseos        281 k
 sssd-krb5                  x86_64     2.9.5-4.el9_5.4      baseos         72 k
 sssd-krb5-common           x86_64     2.9.5-4.el9_5.4      baseos         94 k
 sssd-nfs-idmap             x86_64     2.9.5-4.el9_5.4      baseos         39 k
 sssd-proxy                 x86_64     2.9.5-4.el9_5.4      baseos         72 k
Installing weak dependencies:
 adcli                      x86_64     0.9.2-1.el9          baseos        125 k

Transaction Summary
================================================================================
Install  34 Packages
Upgrade   3 Packages

Total download size: 13 M
Is this ok [y/N]: y
Downloading Packages:
(1/37): samba-common-libs-4.20.2-2.el9_5.1.x86_ 164 kB/s |  99 kB     00:00
(2/37): c-ares-1.19.1-2.el9_4.x86_64.rpm        175 kB/s | 110 kB     00:00
(3/37): samba-common-4.20.2-2.el9_5.1.noarch.rp 567 kB/s | 167 kB     00:00
(4/37): adcli-0.9.2-1.el9.x86_64.rpm            129 kB/s | 125 kB     00:00
(5/37): libwbclient-4.20.2-2.el9_5.1.x86_64.rpm 398 kB/s |  41 kB     00:00
(6/37): libsmbclient-4.20.2-2.el9_5.1.x86_64.rp 294 kB/s |  72 kB     00:00
(7/37): authselect-libs-1.2.6-2.el9.x86_64.rpm  636 kB/s | 236 kB     00:00
(8/37): dbus-tools-1.12.20-8.el9.x86_64.rpm     345 kB/s |  50 kB     00:00
(9/37): authselect-1.2.6-2.el9.x86_64.rpm       394 kB/s | 140 kB     00:00
(10/37): libdhash-0.5.0-53.el9.x86_64.rpm       241 kB/s |  29 kB     00:00
(11/37): libtdb-1.4.10-1.el9.x86_64.rpm         338 kB/s |  50 kB     00:00
(12/37): sssd-proxy-2.9.5-4.el9_5.4.x86_64.rpm  110 kB/s |  72 kB     00:00
(13/37): sssd-nfs-idmap-2.9.5-4.el9_5.4.x86_64.  59 kB/s |  39 kB     00:00
(14/37): sssd-ldap-2.9.5-4.el9_5.4.x86_64.rpm   315 kB/s | 159 kB     00:00
(15/37): sssd-krb5-common-2.9.5-4.el9_5.4.x86_6 152 kB/s |  94 kB     00:00
(16/37): sssd-krb5-2.9.5-4.el9_5.4.x86_64.rpm   341 kB/s |  72 kB     00:00
(17/37): sssd-common-pac-2.9.5-4.el9_5.4.x86_64 491 kB/s |  96 kB     00:00
(18/37): sssd-ipa-2.9.5-4.el9_5.4.x86_64.rpm    242 kB/s | 281 kB     00:01
(19/37): sssd-client-2.9.5-4.el9_5.4.x86_64.rpm 211 kB/s | 161 kB     00:00
(20/37): sssd-ad-2.9.5-4.el9_5.4.x86_64.rpm     372 kB/s | 215 kB     00:00
(21/37): sssd-2.9.5-4.el9_5.4.x86_64.rpm        281 kB/s |  27 kB     00:00
(22/37): libsss_nss_idmap-2.9.5-4.el9_5.4.x86_6 243 kB/s |  45 kB     00:00
(23/37): libsss_idmap-2.9.5-4.el9_5.4.x86_64.rp 238 kB/s |  41 kB     00:00
(24/37): libsss_certmap-2.9.5-4.el9_5.4.x86_64. 327 kB/s |  90 kB     00:00
(25/37): libipa_hbac-2.9.5-4.el9_5.4.x86_64.rpm 220 kB/s |  35 kB     00:00
(26/37): sssd-common-2.9.5-4.el9_5.4.x86_64.rpm 499 kB/s | 1.6 MB     00:03
(27/37): libtalloc-2.4.2-1.el9.x86_64.rpm       213 kB/s |  30 kB     00:00
(28/37): libtevent-0.16.1-1.el9.x86_64.rpm      201 kB/s |  47 kB     00:00
(29/37): cyrus-sasl-gssapi-2.1.27-21.el9.x86_64 124 kB/s |  26 kB     00:00
(30/37): libldb-2.9.1-2.el9.x86_64.rpm          254 kB/s | 182 kB     00:00
(31/37): oddjob-mkhomedir-0.34.7-7.el9.x86_64.r 210 kB/s |  26 kB     00:00
(32/37): oddjob-0.34.7-7.el9.x86_64.rpm         212 kB/s |  63 kB     00:00
(33/37): glibc-minimal-langpack-2.34-125.el9_5. 110 kB/s |  16 kB     00:00
(34/37): glibc-common-2.34-125.el9_5.8.x86_64.r 239 kB/s | 290 kB     00:01
(35/37): glibc-gconv-extra-2.34-125.el9_5.8.x86 315 kB/s | 1.6 MB     00:05
(36/37): glibc-2.34-125.el9_5.8.x86_64.rpm      453 kB/s | 1.9 MB     00:04
(37/37): samba-client-libs-4.20.2-2.el9_5.1.x86 333 kB/s | 5.2 MB     00:16
--------------------------------------------------------------------------------
Total                                           799 kB/s |  13 MB     00:17
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1
  Running scriptlet: samba-common-4.20.2-2.el9_5.1.noarch                  1/40
  Installing       : samba-common-4.20.2-2.el9_5.1.noarch                  1/40
  Running scriptlet: samba-common-4.20.2-2.el9_5.1.noarch                  1/40
  Upgrading        : glibc-minimal-langpack-2.34-125.el9_5.8.x86_64        2/40
  Upgrading        : glibc-common-2.34-125.el9_5.8.x86_64                  3/40
  Running scriptlet: glibc-2.34-125.el9_5.8.x86_64                         4/40
  Upgrading        : glibc-2.34-125.el9_5.8.x86_64                         4/40
  Running scriptlet: glibc-2.34-125.el9_5.8.x86_64                         4/40
  Installing       : glibc-gconv-extra-2.34-125.el9_5.8.x86_64             5/40
  Running scriptlet: glibc-gconv-extra-2.34-125.el9_5.8.x86_64             5/40
  Installing       : libtalloc-2.4.2-1.el9.x86_64                          6/40
  Installing       : libtevent-0.16.1-1.el9.x86_64                         7/40
  Installing       : libtdb-1.4.10-1.el9.x86_64                            8/40
  Installing       : libldb-2.9.1-2.el9.x86_64                             9/40
  Installing       : libdhash-0.5.0-53.el9.x86_64                         10/40
  Running scriptlet: libwbclient-4.20.2-2.el9_5.1.x86_64                  11/40
  Installing       : libwbclient-4.20.2-2.el9_5.1.x86_64                  11/40
  Installing       : samba-client-libs-4.20.2-2.el9_5.1.x86_64            12/40
  Installing       : samba-common-libs-4.20.2-2.el9_5.1.x86_64            13/40
  Installing       : libsss_idmap-2.9.5-4.el9_5.4.x86_64                  14/40
  Installing       : libsss_certmap-2.9.5-4.el9_5.4.x86_64                15/40
  Installing       : cyrus-sasl-gssapi-2.1.27-21.el9.x86_64               16/40
  Installing       : adcli-0.9.2-1.el9.x86_64                             17/40
  Installing       : libsmbclient-4.20.2-2.el9_5.1.x86_64                 18/40
  Installing       : c-ares-1.19.1-2.el9_4.x86_64                         19/40
  Running scriptlet: authselect-libs-1.2.6-2.el9.x86_64                   20/40
  Installing       : authselect-libs-1.2.6-2.el9.x86_64                   20/40
  Installing       : dbus-tools-1:1.12.20-8.el9.x86_64                    21/40
  Installing       : sssd-nfs-idmap-2.9.5-4.el9_5.4.x86_64                22/40
  Installing       : libsss_nss_idmap-2.9.5-4.el9_5.4.x86_64              23/40
  Installing       : sssd-client-2.9.5-4.el9_5.4.x86_64                   24/40
  Running scriptlet: sssd-client-2.9.5-4.el9_5.4.x86_64                   24/40
  Running scriptlet: sssd-common-2.9.5-4.el9_5.4.x86_64                   25/40
  Installing       : sssd-common-2.9.5-4.el9_5.4.x86_64                   25/40
  Running scriptlet: sssd-common-2.9.5-4.el9_5.4.x86_64                   25/40
Created symlink /etc/systemd/system/multi-user.target.wants/sssd.service → /usr/lib/systemd/system/sssd.service.

  Installing       : sssd-krb5-common-2.9.5-4.el9_5.4.x86_64              26/40
  Installing       : sssd-common-pac-2.9.5-4.el9_5.4.x86_64               27/40
  Installing       : sssd-ad-2.9.5-4.el9_5.4.x86_64                       28/40
  Installing       : sssd-ldap-2.9.5-4.el9_5.4.x86_64                     29/40
  Installing       : sssd-krb5-2.9.5-4.el9_5.4.x86_64                     30/40
  Installing       : sssd-proxy-2.9.5-4.el9_5.4.x86_64                    31/40
  Installing       : libipa_hbac-2.9.5-4.el9_5.4.x86_64                   32/40
  Installing       : sssd-ipa-2.9.5-4.el9_5.4.x86_64                      33/40
  Installing       : sssd-2.9.5-4.el9_5.4.x86_64                          34/40
  Installing       : oddjob-0.34.7-7.el9.x86_64                           35/40
  Running scriptlet: oddjob-0.34.7-7.el9.x86_64                           35/40
  Installing       : oddjob-mkhomedir-0.34.7-7.el9.x86_64                 36/40
  Running scriptlet: oddjob-mkhomedir-0.34.7-7.el9.x86_64                 36/40
  Installing       : authselect-1.2.6-2.el9.x86_64                        37/40
  Cleanup          : glibc-2.34-100.el9_4.2.x86_64                        38/40
  Cleanup          : glibc-minimal-langpack-2.34-100.el9_4.2.x86_64       39/40
  Cleanup          : glibc-common-2.34-100.el9_4.2.x86_64                 40/40
  Running scriptlet: authselect-libs-1.2.6-2.el9.x86_64                   40/40
  Running scriptlet: sssd-common-2.9.5-4.el9_5.4.x86_64                   40/40
  Running scriptlet: glibc-common-2.34-100.el9_4.2.x86_64                 40/40
  Verifying        : c-ares-1.19.1-2.el9_4.x86_64                          1/40
  Verifying        : adcli-0.9.2-1.el9.x86_64                              2/40
  Verifying        : samba-common-libs-4.20.2-2.el9_5.1.x86_64             3/40
  Verifying        : samba-common-4.20.2-2.el9_5.1.noarch                  4/40
  Verifying        : samba-client-libs-4.20.2-2.el9_5.1.x86_64             5/40
  Verifying        : libwbclient-4.20.2-2.el9_5.1.x86_64                   6/40
  Verifying        : libsmbclient-4.20.2-2.el9_5.1.x86_64                  7/40
  Verifying        : authselect-libs-1.2.6-2.el9.x86_64                    8/40
  Verifying        : authselect-1.2.6-2.el9.x86_64                         9/40
  Verifying        : dbus-tools-1:1.12.20-8.el9.x86_64                    10/40
  Verifying        : libdhash-0.5.0-53.el9.x86_64                         11/40
  Verifying        : libtdb-1.4.10-1.el9.x86_64                           12/40
  Verifying        : sssd-proxy-2.9.5-4.el9_5.4.x86_64                    13/40
  Verifying        : sssd-nfs-idmap-2.9.5-4.el9_5.4.x86_64                14/40
  Verifying        : sssd-ldap-2.9.5-4.el9_5.4.x86_64                     15/40
  Verifying        : sssd-krb5-common-2.9.5-4.el9_5.4.x86_64              16/40
  Verifying        : sssd-krb5-2.9.5-4.el9_5.4.x86_64                     17/40
  Verifying        : sssd-ipa-2.9.5-4.el9_5.4.x86_64                      18/40
  Verifying        : sssd-common-pac-2.9.5-4.el9_5.4.x86_64               19/40
  Verifying        : sssd-common-2.9.5-4.el9_5.4.x86_64                   20/40
  Verifying        : sssd-client-2.9.5-4.el9_5.4.x86_64                   21/40
  Verifying        : sssd-ad-2.9.5-4.el9_5.4.x86_64                       22/40
  Verifying        : sssd-2.9.5-4.el9_5.4.x86_64                          23/40
  Verifying        : libsss_nss_idmap-2.9.5-4.el9_5.4.x86_64              24/40
  Verifying        : libsss_idmap-2.9.5-4.el9_5.4.x86_64                  25/40
  Verifying        : libsss_certmap-2.9.5-4.el9_5.4.x86_64                26/40
  Verifying        : libipa_hbac-2.9.5-4.el9_5.4.x86_64                   27/40
  Verifying        : libtevent-0.16.1-1.el9.x86_64                        28/40
  Verifying        : libtalloc-2.4.2-1.el9.x86_64                         29/40
  Verifying        : libldb-2.9.1-2.el9.x86_64                            30/40
  Verifying        : cyrus-sasl-gssapi-2.1.27-21.el9.x86_64               31/40
  Verifying        : glibc-gconv-extra-2.34-125.el9_5.8.x86_64            32/40
  Verifying        : oddjob-mkhomedir-0.34.7-7.el9.x86_64                 33/40
  Verifying        : oddjob-0.34.7-7.el9.x86_64                           34/40
  Verifying        : glibc-minimal-langpack-2.34-125.el9_5.8.x86_64       35/40
  Verifying        : glibc-minimal-langpack-2.34-100.el9_4.2.x86_64       36/40
  Verifying        : glibc-common-2.34-125.el9_5.8.x86_64                 37/40
  Verifying        : glibc-common-2.34-100.el9_4.2.x86_64                 38/40
  Verifying        : glibc-2.34-125.el9_5.8.x86_64                        39/40
  Verifying        : glibc-2.34-100.el9_4.2.x86_64                        40/40

Upgraded:
  glibc-2.34-125.el9_5.8.x86_64
  glibc-common-2.34-125.el9_5.8.x86_64
  glibc-minimal-langpack-2.34-125.el9_5.8.x86_64
Installed:
  adcli-0.9.2-1.el9.x86_64
  authselect-1.2.6-2.el9.x86_64
  authselect-libs-1.2.6-2.el9.x86_64
  c-ares-1.19.1-2.el9_4.x86_64
  cyrus-sasl-gssapi-2.1.27-21.el9.x86_64
  dbus-tools-1:1.12.20-8.el9.x86_64
  glibc-gconv-extra-2.34-125.el9_5.8.x86_64
  libdhash-0.5.0-53.el9.x86_64
  libipa_hbac-2.9.5-4.el9_5.4.x86_64
  libldb-2.9.1-2.el9.x86_64
  libsmbclient-4.20.2-2.el9_5.1.x86_64
  libsss_certmap-2.9.5-4.el9_5.4.x86_64
  libsss_idmap-2.9.5-4.el9_5.4.x86_64
  libsss_nss_idmap-2.9.5-4.el9_5.4.x86_64
  libtalloc-2.4.2-1.el9.x86_64
  libtdb-1.4.10-1.el9.x86_64
  libtevent-0.16.1-1.el9.x86_64
  libwbclient-4.20.2-2.el9_5.1.x86_64
  oddjob-0.34.7-7.el9.x86_64
  oddjob-mkhomedir-0.34.7-7.el9.x86_64
  samba-client-libs-4.20.2-2.el9_5.1.x86_64
  samba-common-4.20.2-2.el9_5.1.noarch
  samba-common-libs-4.20.2-2.el9_5.1.x86_64
  sssd-2.9.5-4.el9_5.4.x86_64
  sssd-ad-2.9.5-4.el9_5.4.x86_64
  sssd-client-2.9.5-4.el9_5.4.x86_64
  sssd-common-2.9.5-4.el9_5.4.x86_64
  sssd-common-pac-2.9.5-4.el9_5.4.x86_64
  sssd-ipa-2.9.5-4.el9_5.4.x86_64
  sssd-krb5-2.9.5-4.el9_5.4.x86_64
  sssd-krb5-common-2.9.5-4.el9_5.4.x86_64
  sssd-ldap-2.9.5-4.el9_5.4.x86_64
  sssd-nfs-idmap-2.9.5-4.el9_5.4.x86_64
  sssd-proxy-2.9.5-4.el9_5.4.x86_64

Complete!
[root@hammer14 ~]#

```

```bash
root@hammer14 ~]# authselect select sssd with-mkhomedir --force
Backup stored at /var/lib/authselect/backups/2025-05-24-03-12-46.SzyKN4
Profile "sssd" was selected.
The following nsswitch maps are overwritten by the profile:
- passwd
- group
- netgroup
- automount
- services

Make sure that SSSD service is configured and enabled. See SSSD documentation for more information.

- with-mkhomedir is selected, make sure pam_oddjob_mkhomedir module
  is present and oddjobd service is enabled and active
  - systemctl enable --now oddjobd.service

[root@hammer14 ~]#

[root@hammer14 ~]# systemctl enable --now oddjobd.service
Created symlink /etc/systemd/system/multi-user.target.wants/oddjobd.service → /usr/lib/systemd/system/oddjobd.service.
[root@hammer14 ~]# systemctl status oddjobd.service
● oddjobd.service - privileged operations for unprivileged applications
     Loaded: loaded (/usr/lib/systemd/system/oddjobd.service; enabled; preset: >
     Active: active (running) since Fri 2025-05-23 20:14:11 MST; 22s ago
   Main PID: 12455 (oddjobd)
      Tasks: 1 (limit: 24363)
     Memory: 548.0K
        CPU: 9ms
     CGroup: /system.slice/oddjobd.service
             └─12455 /usr/sbin/oddjobd -n -p /run/oddjobd.pid -t 300

May 23 20:14:11 hammer14 systemd[1]: Started privileged operations for unprivil>
[root@hammer14 ~]#

```



### 2. Uncomment and fix the lines in /etc/openldap/ldap.conf

*vi /etc/openldap/ldap.conf*

```bash
[root@hammer14 ~]# grep -v ^# /etc/openldap/ldap.conf | uniq                    
BASE dc=prolug,dc=lan
URI ldap://ldap.prolug.lan/

TLS_CACERT /etc/pki/tls/ldapserver.crt
TLS_REQCERT never

SASL_NOCANON    on

[root@hammer14 ~]#

```

### 3. Edit the sssd.conf file

*vi /etc/sssd/sssd.conf*

```bash
[root@hammer14 ~]# vi /etc/sssd/sssd.conf
[root@hammer14 ~]# cat /etc/sssd/sssd.conf
[domain/default]
id_provider = ldap
autofs_provider = ldap
auth_provider = ldap
chpass_provider = ldap
ldap_uri = ldap://ldap.prolug.lan/
ldap_search_base = dc=prolug,dc=lan
#ldap_id_use_start_tls = True
#ldap_tls_cacertdir = /etc/openldap/certs
cache_credentials = True
#ldap_tls_reqcert = allow

[sssd]
services = nss, pam, autofs
domains = default

[nss]
homedir_substring = /home
```

```bash
[root@hammer14 ~]# chmod 0600 /etc/sssd/sssd.conf
[root@hammer14 ~]#  systemctl start sssd
[root@hammer14 ~]# systemctl status sssd
● sssd.service - System Security Services Daemon
     Loaded: loaded (/usr/lib/systemd/system/sssd.service; enabled; preset: ena>
     Active: active (running) since Fri 2025-05-23 20:19:47 MST; 7s ago
   Main PID: 12467 (sssd)
      Tasks: 5 (limit: 24363)
     Memory: 48.0M
        CPU: 315ms
     CGroup: /system.slice/sssd.service
             ├─12467 /usr/sbin/sssd -i --logger=files
             ├─12468 /usr/libexec/sssd/sssd_be --domain default --uid 0 --gid 0>
             ├─12469 /usr/libexec/sssd/sssd_nss --uid 0 --gid 0 --logger=files
             ├─12470 /usr/libexec/sssd/sssd_pam --uid 0 --gid 0 --logger=files
             └─12471 /usr/libexec/sssd/sssd_autofs --uid 0 --gid 0 --logger=fil>

May 23 20:19:46 hammer14 systemd[1]: Starting System Security Services Daemon...
May 23 20:19:47 hammer14 sssd[12467]: Starting up
May 23 20:19:47 hammer14 sssd_be[12468]: Starting up
May 23 20:19:47 hammer14 sssd_nss[12469]: Starting up
May 23 20:19:47 hammer14 sssd_autofs[12471]: Starting up
May 23 20:19:47 hammer14 sssd_pam[12470]: Starting up
May 23 20:19:47 hammer14 systemd[1]: Started System Security Services Daemon.
```


### 4. Validate that the user can be seen

```bash
[root@hammer14 ~]# id testuser
uid=15000(testuser) gid=15000(testuser) groups=15000(testuser)

```

### done























