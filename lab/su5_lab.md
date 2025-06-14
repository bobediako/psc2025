# Unit 5 Lab - Repos and Patching

## Apache STIGs Review
### Look at the 4 STIGs for "tls"
### What file is fixed for all of them to be remediated?
   /etc/httpd/conf.d/httpd.conf (for all 4)
   /etc/httpd/conf.d/ssl.conf (for 2 of 4)

### Install httpd on your Hammer server
```bash
[root@hammer14 ~]# systemctl stop wwclient
[root@hammer14 ~]# dnf info httpd
Last metadata expiration check: 0:12:00 ago on Sat May 24 12:08:27 2025.
Available Packages
Name         : httpd
Version      : 2.4.62
Release      : 1.el9_5.2
Architecture : x86_64
Size         : 45 k
Source       : httpd-2.4.62-1.el9_5.2.src.rpm
Repository   : appstream
Summary      : Apache HTTP Server
URL          : https://httpd.apache.org/
License      : ASL 2.0
Description  : The Apache HTTP Server is a powerful, efficient, and extensible
             : web server.

[root@hammer14 ~]# dnf install -y httpd
Last metadata expiration check: 0:12:24 ago on Sat May 24 12:08:27 2025.
Dependencies resolved.
================================================================================
 Package                Arch        Version                Repository      Size
================================================================================
Installing:
 httpd                  x86_64      2.4.62-1.el9_5.2       appstream       45 k
Installing dependencies:
 apr                    x86_64      1.7.0-12.el9_3         appstream      122 k
 apr-util               x86_64      1.6.1-23.el9           appstream       94 k
 apr-util-bdb           x86_64      1.6.1-23.el9           appstream       12 k
 httpd-core             x86_64      2.4.62-1.el9_5.2       appstream      1.4 M
 httpd-filesystem       noarch      2.4.62-1.el9_5.2       appstream       12 k
 httpd-tools            x86_64      2.4.62-1.el9_5.2       appstream       79 k
 mailcap                noarch      2.1.49-5.el9           baseos          32 k
 rocky-logos-httpd      noarch      90.15-2.el9            appstream       24 k
Installing weak dependencies:
 apr-util-openssl       x86_64      1.6.1-23.el9           appstream       14 k
 mod_http2              x86_64      2.0.26-2.el9_4.1       appstream      163 k
 mod_lua                x86_64      2.4.62-1.el9_5.2       appstream       59 k

Transaction Summary
================================================================================
Install  12 Packages

Total download size: 2.0 M
Installed size: 6.1 M
Downloading Packages:
(1/12): mailcap-2.1.49-5.el9.noarch.rpm         116 kB/s |  32 kB     00:00
(2/12): rocky-logos-httpd-90.15-2.el9.noarch.rp  80 kB/s |  24 kB     00:00
(3/12): apr-util-openssl-1.6.1-23.el9.x86_64.rp  48 kB/s |  14 kB     00:00
(4/12): apr-util-bdb-1.6.1-23.el9.x86_64.rpm     75 kB/s |  12 kB     00:00
(5/12): apr-util-1.6.1-23.el9.x86_64.rpm        549 kB/s |  94 kB     00:00
(6/12): mod_http2-2.0.26-2.el9_4.1.x86_64.rpm   600 kB/s | 163 kB     00:00
(7/12): httpd-tools-2.4.62-1.el9_5.2.x86_64.rpm 794 kB/s |  79 kB     00:00
(8/12): apr-1.7.0-12.el9_3.x86_64.rpm           431 kB/s | 122 kB     00:00
(9/12): httpd-filesystem-2.4.62-1.el9_5.2.noarc 136 kB/s |  12 kB     00:00
(10/12): httpd-2.4.62-1.el9_5.2.x86_64.rpm      492 kB/s |  45 kB     00:00
(11/12): mod_lua-2.4.62-1.el9_5.2.x86_64.rpm     74 kB/s |  59 kB     00:00
(12/12): httpd-core-2.4.62-1.el9_5.2.x86_64.rpm 2.4 MB/s | 1.4 MB     00:00
--------------------------------------------------------------------------------
Total                                           1.2 MB/s | 2.0 MB     00:01
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1
  Installing       : apr-1.7.0-12.el9_3.x86_64                             1/12
  Installing       : apr-util-bdb-1.6.1-23.el9.x86_64                      2/12
  Installing       : apr-util-1.6.1-23.el9.x86_64                          3/12
  Installing       : apr-util-openssl-1.6.1-23.el9.x86_64                  4/12
  Installing       : httpd-tools-2.4.62-1.el9_5.2.x86_64                   5/12
  Running scriptlet: httpd-filesystem-2.4.62-1.el9_5.2.noarch              6/12
  Installing       : httpd-filesystem-2.4.62-1.el9_5.2.noarch              6/12
  Installing       : rocky-logos-httpd-90.15-2.el9.noarch                  7/12
  Installing       : mailcap-2.1.49-5.el9.noarch                           8/12
  Installing       : httpd-core-2.4.62-1.el9_5.2.x86_64                    9/12
  Installing       : mod_lua-2.4.62-1.el9_5.2.x86_64                      10/12
  Installing       : mod_http2-2.0.26-2.el9_4.1.x86_64                    11/12
  Installing       : httpd-2.4.62-1.el9_5.2.x86_64                        12/12
  Running scriptlet: httpd-2.4.62-1.el9_5.2.x86_64                        12/12
  Verifying        : mailcap-2.1.49-5.el9.noarch                           1/12
  Verifying        : rocky-logos-httpd-90.15-2.el9.noarch                  2/12
  Verifying        : apr-util-openssl-1.6.1-23.el9.x86_64                  3/12
  Verifying        : apr-util-bdb-1.6.1-23.el9.x86_64                      4/12
  Verifying        : apr-util-1.6.1-23.el9.x86_64                          5/12
  Verifying        : mod_http2-2.0.26-2.el9_4.1.x86_64                     6/12
  Verifying        : apr-1.7.0-12.el9_3.x86_64                             7/12
  Verifying        : mod_lua-2.4.62-1.el9_5.2.x86_64                       8/12
  Verifying        : httpd-tools-2.4.62-1.el9_5.2.x86_64                   9/12
  Verifying        : httpd-filesystem-2.4.62-1.el9_5.2.noarch             10/12
  Verifying        : httpd-core-2.4.62-1.el9_5.2.x86_64                   11/12
  Verifying        : httpd-2.4.62-1.el9_5.2.x86_64                        12/12

Installed:
  apr-1.7.0-12.el9_3.x86_64                apr-util-1.6.1-23.el9.x86_64
  apr-util-bdb-1.6.1-23.el9.x86_64         apr-util-openssl-1.6.1-23.el9.x86_64
  httpd-2.4.62-1.el9_5.2.x86_64            httpd-core-2.4.62-1.el9_5.2.x86_64
  httpd-filesystem-2.4.62-1.el9_5.2.noarch httpd-tools-2.4.62-1.el9_5.2.x86_64
  mailcap-2.1.49-5.el9.noarch              mod_http2-2.0.26-2.el9_4.1.x86_64
  mod_lua-2.4.62-1.el9_5.2.x86_64          rocky-logos-httpd-90.15-2.el9.noarch

Complete!
[root@hammer14 ~]# systemctl start httpd
[root@hammer14 ~]# systemctl status httpd
● httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; preset: d>
     Active: active (running) since Sat 2025-05-24 12:21:10 MST; 16s ago
       Docs: man:httpd.service(8)
   Main PID: 10198 (httpd)
     Status: "Total requests: 0; Idle/Busy workers 100/0;Requests/sec: 0; Bytes>
      Tasks: 177 (limit: 24363)
     Memory: 29.6M
        CPU: 212ms
     CGroup: /system.slice/httpd.service
             ├─10198 /usr/sbin/httpd -DFOREGROUND
             ├─10199 /usr/sbin/httpd -DFOREGROUND
             ├─10200 /usr/sbin/httpd -DFOREGROUND
             ├─10201 /usr/sbin/httpd -DFOREGROUND
             └─10202 /usr/sbin/httpd -DFOREGROUND

May 24 12:21:10 hammer14 systemd[1]: Starting The Apache HTTP Server...
May 24 12:21:10 hammer14 httpd[10198]: AH00558: httpd: Could not reliably deter>
May 24 12:21:10 hammer14 httpd[10198]: Server configured, listening on: port 80
May 24 12:21:10 hammer14 systemd[1]: Started The Apache HTTP Server.
[root@hammer14 ~]# ss -ntulp | head -1
Netid State  Recv-Q Send-Q Local Address:Port Peer Address:PortProcess                                                                                          
[root@hammer14 ~]# ss -ntulp | egrep "(^N|http)"
Netid State  Recv-Q Send-Q Local Address:Port Peer Address:PortProcess                                                                                          
tcp   LISTEN 0      511                *:80              *:*    users:(("httpd",pid=10202,fd=4),("httpd",pid=10201,fd=4),("httpd",pid=10200,fd=4),("httpd",pid=10198,fd=4))
[root@hammer14 ~]#

```
## Check STIG V-214234
### What is the problem?
> This is a request for additional logging.

### What is the fix?
> Request that alerting be configured via an SIEM sucj as ELK or splunk


### What type of control is being implemented?
> Operational Detective

### Is it set properly on your system?
> No not yest.. future labs will enable grafana (SIEM) and possibly allow for monitoring of a web server


## Check STIG V-214248

### What is the problem?
> The apachae webserver is sometimes set up as a privelegede accunt with access to priveledged files

### What is the fix?
> To setup the the apache webserver with a limited user account with limited priveledges.

### What type of control is being implemented?
> Tech Preventative

### Is it set properly on your system?
> Yes by default
```bash
[root@hammer14 ~]# ps -ef | grep http
root       10198       1  0 12:21 ?        00:00:02 /usr/sbin/httpd -DFOREGROUND
apache     10199   10198  0 12:21 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache     10200   10198  0 12:21 ?        00:00:07 /usr/sbin/httpd -DFOREGROUND
apache     10201   10198  0 12:21 ?        00:00:07 /usr/sbin/httpd -DFOREGROUND
apache     10202   10198  0 12:21 ?        00:00:09 /usr/sbin/httpd -DFOREGROUND
root       10936    1947  0 18:41 pts/0    00:00:00 grep --color=auto http
```
### How do you think SELINUX will help implement this control in an enforcing state? Or will it not affect it?
> I think the use of SELINUX will definetly help implement this crontrol.  The process will only be able to affect a limited amount of files on the running system.

## Building repos
### Start out by removing all your active repos
```bash
[root@hammer14 ~]# cd /etc/yum.repos.d
[root@hammer14 yum.repos.d]# mkdir old_archive
[root@hammer14 yum.repos.d]# mv *.repo old_archive
[root@hammer14 yum.repos.d]# dnf repolist
No repositories available
[root@hammer14 yum.repos.d]#
```

### Mount the local repository and make a local repo
```bash
[root@hammer14 yum.repos.d]# mount -o loop /lab_work/repos_and_patching/Rocky-9.5-x86_64-dvd.iso /mnt
[root@hammer14 yum.repos.d]# df -h
Filesystem                Size  Used Avail Use% Mounted on
devtmpfs                  4.0M     0  4.0M   0% /dev
tmpfs                     1.9G     0  1.9G   0% /dev/shm
tmpfs                     1.9G  8.5M  1.9G   1% /run
tmpfs                     8.0G  1.7G  6.4G  21% /
tmpfs                     1.9G     0  1.9G   0% /run/shm
192.168.200.25:/home      194G   54G  141G  28% /home
192.168.200.25:/opt       194G   54G  141G  28% /opt
192.168.200.25:/labs      194G   54G  141G  28% /labs
192.168.200.25:/lab_work  194G   54G  141G  28% /lab_work
tmpfs                     388M     0  388M   0% /run/user/0
/dev/loop0                 11G   11G     0 100% /mnt
[root@hammer14 yum.repos.d]# ls -l /mnt
total 13
drwxr-xr-x. 1 10004 10005 2048 Nov 15  2024 AppStream
drwxr-xr-x. 1 10004 10005 2048 Nov 15  2024 BaseOS
drwxrwxr-x. 1 root  root  2048 Nov 15  2024 EFI
-rw-r--r--. 1 root  root  2204 Oct 31  2024 LICENSE
drwxrwxr-x. 1 root  root  2048 Nov 15  2024 images
drwxrwxr-x. 1 root  root  2048 Nov 15  2024 isolinux
-rw-r--r--. 1 root  root   102 Nov 15  2024 media.repo
[root@hammer14 yum.repos.d]# touch /etc/yum.repos.d/rocky9.repo
[root@hammer14 yum.repos.d]# vi /etc/yum.repos.d/rocky9.repo
[root@hammer14 yum.repos.d]# ls -ltr !$
ls -ltr /etc/yum.repos.d/rocky9.repo
-rw-r--r--. 1 root root 351 May 24 18:48 /etc/yum.repos.d/rocky9.repo
[root@hammer14 yum.repos.d]# cat /etc/yum.repos.d/rocky9.repo
[BaseOS]
name=BaseOS Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
baseurl=file:///mnt/BaseOS/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[AppStream]
name=AppStream Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
baseurl=file:///mnt/AppStream/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[root@hammer14 yum.repos.d]#
[root@hammer14 yum.repos.d]# chmod 644 /etc/yum.repos.d/rocky9.repo
[root@hammer14 yum.repos.d]# dnf clean all
58 files removed

```

### Test the local repository.
```bash
[root@hammer14 yum.repos.d]# dnf repolist
repo id                      repo name
AppStream                    AppStream Packages Rocky Linux 9
BaseOS                       BaseOS Packages Rocky Linux 9
[root@hammer14 yum.repos.d]# dnf --disablerepo="*" --enablerepo="AppStream" list available | head
Last metadata expiration check: 0:00:41 ago on Sat May 24 18:55:01 2025.
Available Packages
389-ds-base.x86_64                                   2.5.2-2.el9_5                       AppStream
389-ds-base-libs.x86_64                              2.5.2-2.el9_5                       AppStream
389-ds-base-snmp.x86_64                              2.5.2-2.el9_5                       AppStream
Box2D.i686                                           2.4.1-7.el9                         AppStream
Box2D.x86_64                                         2.4.1-7.el9                         AppStream
CUnit.i686                                           2.1.3-25.el9                        AppStream
CUnit.x86_64                                         2.1.3-25.el9                        AppStream
HdrHistogram_c.i686                                  0.11.0-6.el9                        AppStream
```

### Approximately how many are available?
```bash
[root@hammer14 yum.repos.d]# dnf --disablerepo="*" --enablerepo="AppStream" list available | wc
   5591   16781  553403
[root@hammer14 yum.repos.d]#
```
### Now use BaseOS.Approximately how many are available?

```bash
[root@hammer14 yum.repos.d]# dnf --disablerepo="*" --enablerepo="BaseOS" list available | wc
   1002    3014   88092
```

### How many packages does it want to install?
```bash
Transaction Summary
================================================================================
Install  173 Packages

Total size: 147 M
Installed size: 582 M
Is this ok [y/N]: n
Operation aborted.
[root@hammer14 yum.repos.d]#
```

### How can you tell they're from different repos?
> I could not tell from the output alone

## Share out the local repository for your internal systems (tested on just this one system)
> 
```bash
[root@hammer14 yum.repos.d]# rpm -qa | grep -i httpd
httpd-tools-2.4.62-1.el9_5.2.x86_64
httpd-filesystem-2.4.62-1.el9_5.2.noarch
rocky-logos-httpd-90.15-2.el9.noarch
httpd-core-2.4.62-1.el9_5.2.x86_64
httpd-2.4.62-1.el9_5.2.x86_64
[root@hammer14 yum.repos.d]# systemctl status httpd
● httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; preset: d>
     Active: active (running) since Sat 2025-05-24 12:21:10 MST; 6h ago
       Docs: man:httpd.service(8)
   Main PID: 10198 (httpd)
     Status: "Total requests: 0; Idle/Busy workers 100/0;Requests/sec: 0; Bytes>
      Tasks: 177 (limit: 24363)
     Memory: 29.6M
        CPU: 29.219s
     CGroup: /system.slice/httpd.service
             ├─10198 /usr/sbin/httpd -DFOREGROUND
             ├─10199 /usr/sbin/httpd -DFOREGROUND
             ├─10200 /usr/sbin/httpd -DFOREGROUND
             ├─10201 /usr/sbin/httpd -DFOREGROUND
             └─10202 /usr/sbin/httpd -DFOREGROUND

May 24 12:21:10 hammer14 systemd[1]: Starting The Apache HTTP Server...
May 24 12:21:10 hammer14 httpd[10198]: AH00558: httpd: Could not reliably deter>
May 24 12:21:10 hammer14 httpd[10198]: Server configured, listening on: port 80
May 24 12:21:10 hammer14 systemd[1]: Started The Apache HTTP Server.
[root@hammer14 yum.repos.d]# ss -ntulp | grep 80
tcp   LISTEN 0      511                *:80              *:*    users:(("httpd",pid=10202,fd=4),("httpd",pid=10201,fd=4),("httpd",pid=10200,fd=4),("httpd",pid=10198,fd=4))
[root@hammer14 yum.repos.d]# lsof -i :80
COMMAND   PID   USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
httpd   10198   root    4u  IPv6  38862      0t0  TCP *:http (LISTEN)
httpd   10200 apache    4u  IPv6  38862      0t0  TCP *:http (LISTEN)
httpd   10201 apache    4u  IPv6  38862      0t0  TCP *:http (LISTEN)
httpd   10202 apache    4u  IPv6  38862      0t0  TCP *:http (LISTEN)
[root@hammer14 yum.repos.d]# cd /etc/httpd/conf.d
[root@hammer14 conf.d]# vi repos.conf
[root@hammer14 conf.d]# cat repos.conf
<Directory "/mnt">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
Alias /repo /mnt
<Location /repo>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Location>
[root@hammer14 conf.d]# systemctl restart httpd
[root@hammer14 conf.d]# vi /etc/yum.repos.d/rocky9.repo
[root@hammer14 conf.d]# cp -p /etc/yum.repos.d/rocky9.repo /etc/yum.repos.d/rocky9.repo.old
[root@hammer14 conf.d]# cat !$
cat /etc/yum.repos.d/rocky9.repo.old
[BaseOS]
name=BaseOS Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
baseurl=file:///mnt/BaseOS/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[AppStream]
name=AppStream Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
baseurl=file:///mnt/AppStream/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
[root@hammer14 conf.d]# perl -pi -e "s/baseurl=file:\/\/\/mnt/baseurl=http:\/\/hammer14\/repo/" /etc/yum.repos.d/rocky9.repo
[root@hammer14 conf.d]# diff /etc/yum.repos.d/rocky9.repo.old  /etc/yum.repos.d/rocky9.repo
6c6
< baseurl=file:///mnt/BaseOS/
---
> baseurl=http://hammer14/repo/BaseOS/
14c14
< baseurl=file:///mnt/AppStream/
---
> baseurl=http://hammer14/repo/AppStream/
[root@hammer14 conf.d]# dnf clean all
17 files removed
[root@hammer14 conf.d]# dnf repolist
repo id                      repo name
AppStream                    AppStream Packages Rocky Linux 9
BaseOS                       BaseOS Packages Rocky Linux 9
[root@hammer14 conf.d]# dnf --disablerepo="*" --enablerepo="BaseOS AppStream" install gimp
BaseOS Packages Rocky Linux 9                    49 MB/s | 2.3 MB     00:00
AppStream Packages Rocky Linux 9                117 MB/s | 8.3 MB     00:00
Last metadata expiration check: 0:00:01 ago on Sat May 24 19:15:32 2025.
Dependencies resolved.
================================================================================
 Package                          Arch   Version                Repo       Size
================================================================================
Installing:
 gimp                             x86_64 2:2.99.8-4.el9_3       AppStream  19 M
Installing dependencies:
 LibRaw                           x86_64 0.21.1-1.el9           AppStream 408 k
 SDL2                             x86_64 2.26.0-1.el9           AppStream 683 k
 adobe-mappings-cmap              noarch 20171205-12.el9        AppStream 1.9 M
...
mypaint-brushes                  noarch 1.3.1-6.el9            AppStream 1.3 M
 tracker-miners                   x86_64 3.1.2-4.el9_3          AppStream 888 k

Transaction Summary
================================================================================
Install  173 Packages

Total download size: 147 M
Installed size: 582 M
Is this ok [y/N]: n
Operation aborted.
[root@hammer14 conf.d]#

```
## Enterprise patching
### Complete the killercoda lab found here: https://killercoda.com/het-tanis/course/Ansible-Labs/102-Enterprise-Ansible-Patching
```bash
controlplane:~$ echo "Installing scenario..."
Installing scenario...
controlplane:~$ while [ ! -f /tmp/finished ]; do sleep 1; done
git clone https://github.com/het-tanis/HPC_Deploy.git
controlplane:~$ echo DONE
DONE
controlplane:~$ git clone https://github.com/het-tanis/HPC_Deploy.git
Cloning into 'HPC_Deploy'...
remote: Enumerating objects: 152, done.
remote: Counting objects: 100% (152/152), done.
remote: Compressing objects: 100% (89/89), done.
remote: Total 152 (delta 46), reused 119 (delta 19), pack-reused 0 (from 0)
Receiving objects: 100% (152/152), 15.32 KiB | 2.55 MiB/s, done.
Resolving deltas: 100% (46/46), done.
controlplane:~$ cd HPC_Deploy
controlplane:~/HPC_Deploy$ ansible-playbook -v -i /root/HPC_Deploy/hosts 03_package_update_or_install.yaml --extra-vars "action=install"
Using /root/HPC_Deploy/ansible.cfg as config file
[WARNING]: Found variable using reserved name: action

PLAY [all] ********************************************************************************

TASK [Gathering Facts] ********************************************************************
ok: [controlplane]
ok: [node01]

TASK [packages_install : Debug env variables just to see them] ****************************
ok: [controlplane] => 
  app: web
ok: [node01] => 
  app: db

TASK [packages_install : Install apache2 on the web server] *******************************
skipping: [node01] => changed=false 
  false_condition: '"web" in app'
  skip_reason: Conditional result was False
changed: [controlplane] => changed=true 
  cache_update_time: 1748137166
  cache_updated: false
  stderr: ''
  stderr_lines: <omitted>
  stdout: |-
    Reading package lists...
    Building dependency tree...
    Reading state information...
    The following additional packages will be installed:
      apache2-bin apache2-data apache2-utils libapache2-mod-php8.3 libapr1t64
      libaprutil1-dbd-sqlite3 libaprutil1-ldap libaprutil1t64 liblua5.4-0 php8.3
    Suggested packages:
      apache2-doc apache2-suexec-pristine | apache2-suexec-custom php-pear
    The following NEW packages will be installed:
      apache2 apache2-bin apache2-data apache2-utils libapache2-mod-php8.3
      libapr1t64 libaprutil1-dbd-sqlite3 libaprutil1-ldap libaprutil1t64
      liblua5.4-0 php php8.3
    0 upgraded, 12 newly installed, 0 to remove and 138 not upgraded.
    Need to get 3929 kB of archives.
    After this operation, 13.8 MB of additional disk space will be used.
    Get:1 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libapr1t64 amd64 1.7.2-3.1ubuntu0.1 [108 kB]
    Get:2 http://archive.ubuntu.com/ubuntu noble/main amd64 libaprutil1t64 amd64 1.6.3-1.1ubuntu7 [91.9 kB]
    Get:3 http://archive.ubuntu.com/ubuntu noble/main amd64 libaprutil1-dbd-sqlite3 amd64 1.6.3-1.1ubuntu7 [11.2 kB]
    Get:4 http://archive.ubuntu.com/ubuntu noble/main amd64 libaprutil1-ldap amd64 1.6.3-1.1ubuntu7 [9116 B]
    Get:5 http://archive.ubuntu.com/ubuntu noble/main amd64 liblua5.4-0 amd64 5.4.6-3build2 [166 kB]
    Get:6 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 apache2-bin amd64 2.4.58-1ubuntu8.6 [1330 kB]
    Get:7 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 apache2-data all 2.4.58-1ubuntu8.6 [163 kB]
    Get:8 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 apache2-utils amd64 2.4.58-1ubuntu8.6 [97.2 kB]
    Get:9 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 apache2 amd64 2.4.58-1ubuntu8.6 [90.2 kB]
    Get:10 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libapache2-mod-php8.3 amd64 8.3.6-0ubuntu0.24.04.4 [1850 kB]
    Get:11 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 php8.3 all 8.3.6-0ubuntu0.24.04.4 [9172 B]
    Get:12 http://archive.ubuntu.com/ubuntu noble/main amd64 php all 2:8.3+93ubuntu2 [4076 B]
    Fetched 3929 kB in 1s (3914 kB/s)
    Selecting previously unselected package libapr1t64:amd64.
    (Reading database ... (Reading database ... 5%(Reading database ... 10%(Reading database ... 15%(Reading database ... 20%(Reading database ... 25%(Reading database ... 30%(Reading database ... 35%(Reading database ... 40%(Reading database ... 45%(Reading database ... 50%(Reading database ... 55%(Reading database ... 60%(Reading database ... 65%(Reading database ... 70%(Reading database ... 75%(Reading database ... 80%(Reading database ... 85%(Reading database ... 90%(Reading database ... 95%(Reading database ... 100%(Reading database ... 162894 files and directories currently installed.)
    Preparing to unpack .../00-libapr1t64_1.7.2-3.1ubuntu0.1_amd64.deb ...
    Unpacking libapr1t64:amd64 (1.7.2-3.1ubuntu0.1) ...
    Selecting previously unselected package libaprutil1t64:amd64.
    Preparing to unpack .../01-libaprutil1t64_1.6.3-1.1ubuntu7_amd64.deb ...
    Unpacking libaprutil1t64:amd64 (1.6.3-1.1ubuntu7) ...
    Selecting previously unselected package libaprutil1-dbd-sqlite3:amd64.
    Preparing to unpack .../02-libaprutil1-dbd-sqlite3_1.6.3-1.1ubuntu7_amd64.deb ...
    Unpacking libaprutil1-dbd-sqlite3:amd64 (1.6.3-1.1ubuntu7) ...
    Selecting previously unselected package libaprutil1-ldap:amd64.
    Preparing to unpack .../03-libaprutil1-ldap_1.6.3-1.1ubuntu7_amd64.deb ...
    Unpacking libaprutil1-ldap:amd64 (1.6.3-1.1ubuntu7) ...
    Selecting previously unselected package liblua5.4-0:amd64.
    Preparing to unpack .../04-liblua5.4-0_5.4.6-3build2_amd64.deb ...
    Unpacking liblua5.4-0:amd64 (5.4.6-3build2) ...
    Selecting previously unselected package apache2-bin.
    Preparing to unpack .../05-apache2-bin_2.4.58-1ubuntu8.6_amd64.deb ...
    Unpacking apache2-bin (2.4.58-1ubuntu8.6) ...
    Selecting previously unselected package apache2-data.
    Preparing to unpack .../06-apache2-data_2.4.58-1ubuntu8.6_all.deb ...
    Unpacking apache2-data (2.4.58-1ubuntu8.6) ...
    Selecting previously unselected package apache2-utils.
    Preparing to unpack .../07-apache2-utils_2.4.58-1ubuntu8.6_amd64.deb ...
    Unpacking apache2-utils (2.4.58-1ubuntu8.6) ...
    Selecting previously unselected package apache2.
    Preparing to unpack .../08-apache2_2.4.58-1ubuntu8.6_amd64.deb ...
    Unpacking apache2 (2.4.58-1ubuntu8.6) ...
    Selecting previously unselected package libapache2-mod-php8.3.
    Preparing to unpack .../09-libapache2-mod-php8.3_8.3.6-0ubuntu0.24.04.4_amd64.deb ...
    Unpacking libapache2-mod-php8.3 (8.3.6-0ubuntu0.24.04.4) ...
    Selecting previously unselected package php8.3.
    Preparing to unpack .../10-php8.3_8.3.6-0ubuntu0.24.04.4_all.deb ...
    Unpacking php8.3 (8.3.6-0ubuntu0.24.04.4) ...
    Selecting previously unselected package php.
    Preparing to unpack .../11-php_2%3a8.3+93ubuntu2_all.deb ...
    Unpacking php (2:8.3+93ubuntu2) ...
    Setting up libapr1t64:amd64 (1.7.2-3.1ubuntu0.1) ...
    Setting up liblua5.4-0:amd64 (5.4.6-3build2) ...
    Setting up apache2-data (2.4.58-1ubuntu8.6) ...
    Setting up libaprutil1t64:amd64 (1.6.3-1.1ubuntu7) ...
    Setting up libaprutil1-ldap:amd64 (1.6.3-1.1ubuntu7) ...
    Setting up libaprutil1-dbd-sqlite3:amd64 (1.6.3-1.1ubuntu7) ...
    Setting up apache2-utils (2.4.58-1ubuntu8.6) ...
    Setting up apache2-bin (2.4.58-1ubuntu8.6) ...
    Setting up libapache2-mod-php8.3 (8.3.6-0ubuntu0.24.04.4) ...
    Package apache2 is not configured yet. Will defer actions by package libapache2-mod-php8.3.
  
    Creating config file /etc/php/8.3/apache2/php.ini with new version
    No module matches
    Setting up apache2 (2.4.58-1ubuntu8.6) ...
    Enabling module mpm_event.
    Enabling module authz_core.
    Enabling module authz_host.
    Enabling module authn_core.
    Enabling module auth_basic.
    Enabling module access_compat.
    Enabling module authn_file.
    Enabling module authz_user.
    Enabling module alias.
    Enabling module dir.
    Enabling module autoindex.
    Enabling module env.
    Enabling module mime.
    Enabling module negotiation.
    Enabling module setenvif.
    Enabling module filter.
    Enabling module deflate.
    Enabling module status.
    Enabling module reqtimeout.
    Enabling conf charset.
    Enabling conf localized-error-pages.
    Enabling conf other-vhosts-access-log.
    Enabling conf security.
    Enabling conf serve-cgi-bin.
    Enabling site 000-default.
    info: Switch to mpm prefork for package libapache2-mod-php8.3
    Module mpm_event disabled.
    Enabling module mpm_prefork.
    info: Executing deferred 'a2enmod php8.3' for package libapache2-mod-php8.3
    Enabling module php8.3.
    Created symlink /etc/systemd/system/multi-user.target.wants/apache2.service → /usr/lib/systemd/system/apache2.service.
    Created symlink /etc/systemd/system/multi-user.target.wants/apache-htcacheclean.service → /usr/lib/systemd/system/apache-htcacheclean.service.
    Setting up php8.3 (8.3.6-0ubuntu0.24.04.4) ...
    Setting up php (2:8.3+93ubuntu2) ...
    Processing triggers for ufw (0.36.2-6) ...
    Processing triggers for man-db (2.12.0-4build2) ...
    Processing triggers for libc-bin (2.39-0ubuntu8.3) ...
    Processing triggers for libapache2-mod-php8.3 (8.3.6-0ubuntu0.24.04.4) ...
  
    Running kernel seems to be up-to-date.
  
    No services need to be restarted.
  
    No containers need to be restarted.
  
    No user sessions are running outdated binaries.
  
    No VM guests are running outdated hypervisor (qemu) binaries on this host.
  stdout_lines: <omitted>

TASK [packages_install : Install mariadb on the web server] *******************************
skipping: [controlplane] => changed=false 
  false_condition: '"db" in app'
  skip_reason: Conditional result was False
changed: [node01] => changed=true 
  cache_update_time: 1748137186
  cache_updated: false
  stderr: ''
  stderr_lines: <omitted>
  stdout: |-
    Reading package lists...
    Building dependency tree...
    Reading state information...
    The following package was automatically installed and is no longer required:
      squashfs-tools
    Use 'apt autoremove' to remove it.
    The following additional packages will be installed:
      galera-4 libcgi-fast-perl libcgi-pm-perl libclone-perl
      libconfig-inifiles-perl libdbd-mysql-perl libdbi-perl libencode-locale-perl
      libfcgi-bin libfcgi-perl libfcgi0t64 libhtml-parser-perl libhtml-tagset-perl
      libhtml-template-perl libhttp-date-perl libhttp-message-perl libio-html-perl
      liblwp-mediatypes-perl libmariadb3 libmysqlclient21 libsnappy1v5
      libtimedate-perl liburi-perl liburing2 mariadb-client-core mariadb-common
      mariadb-plugin-provider-bzip2 mariadb-plugin-provider-lz4
      mariadb-plugin-provider-lzma mariadb-plugin-provider-lzo
      mariadb-plugin-provider-snappy mariadb-server-core mysql-common pv socat
    Suggested packages:
      libmldbm-perl libnet-daemon-perl libsql-statement-perl libdata-dump-perl
      libipc-sharedcache-perl libio-compress-brotli-perl libbusiness-isbn-perl
      libregexp-ipv6-perl libwww-perl mailx mariadb-test doc-base
    The following NEW packages will be installed:
      galera-4 libcgi-fast-perl libcgi-pm-perl libclone-perl
      libconfig-inifiles-perl libdbd-mysql-perl libdbi-perl libencode-locale-perl
      libfcgi-bin libfcgi-perl libfcgi0t64 libhtml-parser-perl libhtml-tagset-perl
      libhtml-template-perl libhttp-date-perl libhttp-message-perl libio-html-perl
      liblwp-mediatypes-perl libmariadb3 libmysqlclient21 libsnappy1v5
      libtimedate-perl liburi-perl liburing2 mariadb-client mariadb-client-core
      mariadb-common mariadb-plugin-provider-bzip2 mariadb-plugin-provider-lz4
      mariadb-plugin-provider-lzma mariadb-plugin-provider-lzo
      mariadb-plugin-provider-snappy mariadb-server mariadb-server-core
      mysql-common pv socat
    0 upgraded, 37 newly installed, 0 to remove and 154 not upgraded.
    Need to get 19.7 MB of archives.
    After this operation, 201 MB of additional disk space will be used.
    Get:1 http://archive.ubuntu.com/ubuntu noble/universe amd64 galera-4 amd64 26.4.16-2build4 [736 kB]
    Get:2 http://archive.ubuntu.com/ubuntu noble/main amd64 mysql-common all 5.8+1.1.0build1 [6746 B]
    Get:3 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 mariadb-common all 1:10.11.11-0ubuntu0.24.04.2 [28.0 kB]
    Get:4 http://archive.ubuntu.com/ubuntu noble/main amd64 libdbi-perl amd64 1.643-4build3 [721 kB]
    Get:5 http://archive.ubuntu.com/ubuntu noble/main amd64 libconfig-inifiles-perl all 3.000003-2 [39.4 kB]
    Get:6 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 libmariadb3 amd64 1:10.11.11-0ubuntu0.24.04.2 [196 kB]
    Get:7 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 mariadb-client-core amd64 1:10.11.11-0ubuntu0.24.04.2 [1038 kB]
    Get:8 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 mariadb-client amd64 1:10.11.11-0ubuntu0.24.04.2 [2490 kB]
    Get:9 http://archive.ubuntu.com/ubuntu noble/main amd64 liburing2 amd64 2.5-1build1 [21.1 kB]
    Get:10 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 mariadb-server-core amd64 1:10.11.11-0ubuntu0.24.04.2 [8387 kB]
    Get:11 http://archive.ubuntu.com/ubuntu noble/main amd64 socat amd64 1.8.0.0-4build3 [374 kB]
    Get:12 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 mariadb-server amd64 1:10.11.11-0ubuntu0.24.04.2 [3470 kB]
    Get:13 http://archive.ubuntu.com/ubuntu noble/main amd64 libhtml-tagset-perl all 3.20-6 [11.3 kB]
    Get:14 http://archive.ubuntu.com/ubuntu noble/main amd64 liburi-perl all 5.27-1 [88.0 kB]
    Get:15 http://archive.ubuntu.com/ubuntu noble/main amd64 libhtml-parser-perl amd64 3.81-1build3 [85.8 kB]
    Get:16 http://archive.ubuntu.com/ubuntu noble/main amd64 libcgi-pm-perl all 4.63-1 [185 kB]
    Get:17 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libfcgi0t64 amd64 2.4.2-2.1ubuntu0.24.04.1 [27.0 kB]
    Get:18 http://archive.ubuntu.com/ubuntu noble/main amd64 libfcgi-perl amd64 0.82+ds-3build2 [21.7 kB]
    Get:19 http://archive.ubuntu.com/ubuntu noble/main amd64 libcgi-fast-perl all 1:2.17-1 [10.3 kB]
    Get:20 http://archive.ubuntu.com/ubuntu noble/main amd64 libclone-perl amd64 0.46-1build3 [10.7 kB]
    Get:21 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libmysqlclient21 amd64 8.0.42-0ubuntu0.24.04.1 [1254 kB]
    Get:22 http://archive.ubuntu.com/ubuntu noble/universe amd64 libdbd-mysql-perl amd64 4.052-1ubuntu3 [85.5 kB]
    Get:23 http://archive.ubuntu.com/ubuntu noble/main amd64 libencode-locale-perl all 1.05-3 [11.6 kB]
    Get:24 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libfcgi-bin amd64 2.4.2-2.1ubuntu0.24.04.1 [11.2 kB]
    Get:25 http://archive.ubuntu.com/ubuntu noble/main amd64 libhtml-template-perl all 2.97-2 [60.2 kB]
    Get:26 http://archive.ubuntu.com/ubuntu noble/main amd64 libtimedate-perl all 2.3300-2 [34.0 kB]
    Get:27 http://archive.ubuntu.com/ubuntu noble/main amd64 libhttp-date-perl all 6.06-1 [10.2 kB]
    Get:28 http://archive.ubuntu.com/ubuntu noble/main amd64 libio-html-perl all 1.004-3 [15.9 kB]
    Get:29 http://archive.ubuntu.com/ubuntu noble/main amd64 liblwp-mediatypes-perl all 6.04-2 [20.1 kB]
    Get:30 http://archive.ubuntu.com/ubuntu noble/main amd64 libhttp-message-perl all 6.45-1ubuntu1 [78.2 kB]
    Get:31 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 mariadb-plugin-provider-bzip2 amd64 1:10.11.11-0ubuntu0.24.04.2 [13.9 kB]
    Get:32 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 mariadb-plugin-provider-lz4 amd64 1:10.11.11-0ubuntu0.24.04.2 [13.8 kB]
    Get:33 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 mariadb-plugin-provider-lzma amd64 1:10.11.11-0ubuntu0.24.04.2 [13.8 kB]
    Get:34 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 mariadb-plugin-provider-lzo amd64 1:10.11.11-0ubuntu0.24.04.2 [13.8 kB]
    Get:35 http://archive.ubuntu.com/ubuntu noble/main amd64 libsnappy1v5 amd64 1.1.10-1build1 [28.6 kB]
    Get:36 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 mariadb-plugin-provider-snappy amd64 1:10.11.11-0ubuntu0.24.04.2 [13.8 kB]
    Get:37 http://archive.ubuntu.com/ubuntu noble/main amd64 pv amd64 1.8.5-2build1 [73.9 kB]
    Preconfiguring packages ...
    Fetched 19.7 MB in 2s (8322 kB/s)
    Selecting previously unselected package galera-4.
    (Reading database ... (Reading database ... 5%(Reading database ... 10%(Reading database ... 15%(Reading database ... 20%(Reading database ... 25%(Reading database ... 30%(Reading database ... 35%(Reading database ... 40%(Reading database ... 45%(Reading database ... 50%(Reading database ... 55%(Reading database ... 60%(Reading database ... 65%(Reading database ... 70%(Reading database ... 75%(Reading database ... 80%(Reading database ... 85%(Reading database ... 90%(Reading database ... 95%(Reading database ... 100%(Reading database ... 82217 files and directories currently installed.)
    Preparing to unpack .../00-galera-4_26.4.16-2build4_amd64.deb ...
    info: The home dir /nonexistent you specified can't be accessed: No such file or directory
  
    info: Selecting UID from range 100 to 999 ...
  
    info: Adding system user `_galera' (UID 109) ...
    info: Adding new user `_galera' (UID 109) with group `nogroup' ...
    info: Not creating `/nonexistent'.
    Unpacking galera-4 (26.4.16-2build4) ...
    Selecting previously unselected package mysql-common.
    Preparing to unpack .../01-mysql-common_5.8+1.1.0build1_all.deb ...
    Unpacking mysql-common (5.8+1.1.0build1) ...
    Selecting previously unselected package mariadb-common.
    Preparing to unpack .../02-mariadb-common_1%3a10.11.11-0ubuntu0.24.04.2_all.deb ...
    Unpacking mariadb-common (1:10.11.11-0ubuntu0.24.04.2) ...
    Selecting previously unselected package libdbi-perl:amd64.
    Preparing to unpack .../03-libdbi-perl_1.643-4build3_amd64.deb ...
    Unpacking libdbi-perl:amd64 (1.643-4build3) ...
    Selecting previously unselected package libconfig-inifiles-perl.
    Preparing to unpack .../04-libconfig-inifiles-perl_3.000003-2_all.deb ...
    Unpacking libconfig-inifiles-perl (3.000003-2) ...
    Selecting previously unselected package libmariadb3:amd64.
    Preparing to unpack .../05-libmariadb3_1%3a10.11.11-0ubuntu0.24.04.2_amd64.deb ...
    Unpacking libmariadb3:amd64 (1:10.11.11-0ubuntu0.24.04.2) ...
    Selecting previously unselected package mariadb-client-core.
    Preparing to unpack .../06-mariadb-client-core_1%3a10.11.11-0ubuntu0.24.04.2_amd64.deb ...
    Unpacking mariadb-client-core (1:10.11.11-0ubuntu0.24.04.2) ...
    Selecting previously unselected package mariadb-client.
    Preparing to unpack .../07-mariadb-client_1%3a10.11.11-0ubuntu0.24.04.2_amd64.deb ...
    Unpacking mariadb-client (1:10.11.11-0ubuntu0.24.04.2) ...
    Selecting previously unselected package liburing2:amd64.
    Preparing to unpack .../08-liburing2_2.5-1build1_amd64.deb ...
    Unpacking liburing2:amd64 (2.5-1build1) ...
    Selecting previously unselected package mariadb-server-core.
    Preparing to unpack .../09-mariadb-server-core_1%3a10.11.11-0ubuntu0.24.04.2_amd64.deb ...
    Unpacking mariadb-server-core (1:10.11.11-0ubuntu0.24.04.2) ...
    Selecting previously unselected package socat.
    Preparing to unpack .../10-socat_1.8.0.0-4build3_amd64.deb ...
    Unpacking socat (1.8.0.0-4build3) ...
    Setting up mysql-common (5.8+1.1.0build1) ...
    update-alternatives: using /etc/mysql/my.cnf.fallback to provide /etc/mysql/my.cnf (my.cnf) in auto mode
    Setting up mariadb-common (1:10.11.11-0ubuntu0.24.04.2) ...
    update-alternatives: using /etc/mysql/mariadb.cnf to provide /etc/mysql/my.cnf (my.cnf) in auto mode
    Selecting previously unselected package mariadb-server.
    (Reading database ... (Reading database ... 5%(Reading database ... 10%(Reading database ... 15%(Reading database ... 20%(Reading database ... 25%(Reading database ... 30%(Reading database ... 35%(Reading database ... 40%(Reading database ... 45%(Reading database ... 50%(Reading database ... 55%(Reading database ... 60%(Reading database ... 65%(Reading database ... 70%(Reading database ... 75%(Reading database ... 80%(Reading database ... 85%(Reading database ... 90%(Reading database ... 95%(Reading database ... 100%(Reading database ... 82691 files and directories currently installed.)
    Preparing to unpack .../00-mariadb-server_1%3a10.11.11-0ubuntu0.24.04.2_amd64.deb ...
    Unpacking mariadb-server (1:10.11.11-0ubuntu0.24.04.2) ...
    Selecting previously unselected package libhtml-tagset-perl.
    Preparing to unpack .../01-libhtml-tagset-perl_3.20-6_all.deb ...
    Unpacking libhtml-tagset-perl (3.20-6) ...
    Selecting previously unselected package liburi-perl.
    Preparing to unpack .../02-liburi-perl_5.27-1_all.deb ...
    Unpacking liburi-perl (5.27-1) ...
    Selecting previously unselected package libhtml-parser-perl:amd64.
    Preparing to unpack .../03-libhtml-parser-perl_3.81-1build3_amd64.deb ...
    Unpacking libhtml-parser-perl:amd64 (3.81-1build3) ...
    Selecting previously unselected package libcgi-pm-perl.
    Preparing to unpack .../04-libcgi-pm-perl_4.63-1_all.deb ...
    Unpacking libcgi-pm-perl (4.63-1) ...
    Selecting previously unselected package libfcgi0t64:amd64.
    Preparing to unpack .../05-libfcgi0t64_2.4.2-2.1ubuntu0.24.04.1_amd64.deb ...
    Unpacking libfcgi0t64:amd64 (2.4.2-2.1ubuntu0.24.04.1) ...
    Selecting previously unselected package libfcgi-perl.
    Preparing to unpack .../06-libfcgi-perl_0.82+ds-3build2_amd64.deb ...
    Unpacking libfcgi-perl (0.82+ds-3build2) ...
    Selecting previously unselected package libcgi-fast-perl.
    Preparing to unpack .../07-libcgi-fast-perl_1%3a2.17-1_all.deb ...
    Unpacking libcgi-fast-perl (1:2.17-1) ...
    Selecting previously unselected package libclone-perl:amd64.
    Preparing to unpack .../08-libclone-perl_0.46-1build3_amd64.deb ...
    Unpacking libclone-perl:amd64 (0.46-1build3) ...
    Selecting previously unselected package libmysqlclient21:amd64.
    Preparing to unpack .../09-libmysqlclient21_8.0.42-0ubuntu0.24.04.1_amd64.deb ...
    Unpacking libmysqlclient21:amd64 (8.0.42-0ubuntu0.24.04.1) ...
    Selecting previously unselected package libdbd-mysql-perl:amd64.
    Preparing to unpack .../10-libdbd-mysql-perl_4.052-1ubuntu3_amd64.deb ...
    Unpacking libdbd-mysql-perl:amd64 (4.052-1ubuntu3) ...
    Selecting previously unselected package libencode-locale-perl.
    Preparing to unpack .../11-libencode-locale-perl_1.05-3_all.deb ...
    Unpacking libencode-locale-perl (1.05-3) ...
    Selecting previously unselected package libfcgi-bin.
    Preparing to unpack .../12-libfcgi-bin_2.4.2-2.1ubuntu0.24.04.1_amd64.deb ...
    Unpacking libfcgi-bin (2.4.2-2.1ubuntu0.24.04.1) ...
    Selecting previously unselected package libhtml-template-perl.
    Preparing to unpack .../13-libhtml-template-perl_2.97-2_all.deb ...
    Unpacking libhtml-template-perl (2.97-2) ...
    Selecting previously unselected package libtimedate-perl.
    Preparing to unpack .../14-libtimedate-perl_2.3300-2_all.deb ...
    Unpacking libtimedate-perl (2.3300-2) ...
    Selecting previously unselected package libhttp-date-perl.
    Preparing to unpack .../15-libhttp-date-perl_6.06-1_all.deb ...
    Unpacking libhttp-date-perl (6.06-1) ...
    Selecting previously unselected package libio-html-perl.
    Preparing to unpack .../16-libio-html-perl_1.004-3_all.deb ...
    Unpacking libio-html-perl (1.004-3) ...
    Selecting previously unselected package liblwp-mediatypes-perl.
    Preparing to unpack .../17-liblwp-mediatypes-perl_6.04-2_all.deb ...
    Unpacking liblwp-mediatypes-perl (6.04-2) ...
    Selecting previously unselected package libhttp-message-perl.
    Preparing to unpack .../18-libhttp-message-perl_6.45-1ubuntu1_all.deb ...
    Unpacking libhttp-message-perl (6.45-1ubuntu1) ...
    Selecting previously unselected package mariadb-plugin-provider-bzip2.
    Preparing to unpack .../19-mariadb-plugin-provider-bzip2_1%3a10.11.11-0ubuntu0.24.04.2_amd64.deb ...
    Unpacking mariadb-plugin-provider-bzip2 (1:10.11.11-0ubuntu0.24.04.2) ...
    Selecting previously unselected package mariadb-plugin-provider-lz4.
    Preparing to unpack .../20-mariadb-plugin-provider-lz4_1%3a10.11.11-0ubuntu0.24.04.2_amd64.deb ...
    Unpacking mariadb-plugin-provider-lz4 (1:10.11.11-0ubuntu0.24.04.2) ...
    Selecting previously unselected package mariadb-plugin-provider-lzma.
    Preparing to unpack .../21-mariadb-plugin-provider-lzma_1%3a10.11.11-0ubuntu0.24.04.2_amd64.deb ...
    Unpacking mariadb-plugin-provider-lzma (1:10.11.11-0ubuntu0.24.04.2) ...
    Selecting previously unselected package mariadb-plugin-provider-lzo.
    Preparing to unpack .../22-mariadb-plugin-provider-lzo_1%3a10.11.11-0ubuntu0.24.04.2_amd64.deb ...
    Unpacking mariadb-plugin-provider-lzo (1:10.11.11-0ubuntu0.24.04.2) ...
    Selecting previously unselected package libsnappy1v5:amd64.
    Preparing to unpack .../23-libsnappy1v5_1.1.10-1build1_amd64.deb ...
    Unpacking libsnappy1v5:amd64 (1.1.10-1build1) ...
    Selecting previously unselected package mariadb-plugin-provider-snappy.
    Preparing to unpack .../24-mariadb-plugin-provider-snappy_1%3a10.11.11-0ubuntu0.24.04.2_amd64.deb ...
    Unpacking mariadb-plugin-provider-snappy (1:10.11.11-0ubuntu0.24.04.2) ...
    Selecting previously unselected package pv.
    Preparing to unpack .../25-pv_1.8.5-2build1_amd64.deb ...
    Unpacking pv (1.8.5-2build1) ...
    Setting up libconfig-inifiles-perl (3.000003-2) ...
    Setting up galera-4 (26.4.16-2build4) ...
    Setting up libmysqlclient21:amd64 (8.0.42-0ubuntu0.24.04.1) ...
    Setting up libclone-perl:amd64 (0.46-1build3) ...
    Setting up libfcgi0t64:amd64 (2.4.2-2.1ubuntu0.24.04.1) ...
    Setting up libhtml-tagset-perl (3.20-6) ...
    Setting up liblwp-mediatypes-perl (6.04-2) ...
    Setting up libfcgi-bin (2.4.2-2.1ubuntu0.24.04.1) ...
    Setting up libencode-locale-perl (1.05-3) ...
    Setting up libsnappy1v5:amd64 (1.1.10-1build1) ...
    Setting up socat (1.8.0.0-4build3) ...
    Setting up libio-html-perl (1.004-3) ...
    Setting up libmariadb3:amd64 (1:10.11.11-0ubuntu0.24.04.2) ...
    Setting up libtimedate-perl (2.3300-2) ...
    Setting up pv (1.8.5-2build1) ...
    Setting up libfcgi-perl (0.82+ds-3build2) ...
    Setting up liburing2:amd64 (2.5-1build1) ...
    Setting up liburi-perl (5.27-1) ...
    Setting up libdbi-perl:amd64 (1.643-4build3) ...
    Setting up libhttp-date-perl (6.06-1) ...
    Setting up mariadb-client-core (1:10.11.11-0ubuntu0.24.04.2) ...
    Setting up libdbd-mysql-perl:amd64 (4.052-1ubuntu3) ...
    Setting up libhtml-parser-perl:amd64 (3.81-1build3) ...
    Setting up mariadb-server-core (1:10.11.11-0ubuntu0.24.04.2) ...
    Setting up libhttp-message-perl (6.45-1ubuntu1) ...
    Setting up mariadb-client (1:10.11.11-0ubuntu0.24.04.2) ...
    Setting up libcgi-pm-perl (4.63-1) ...
    Setting up libhtml-template-perl (2.97-2) ...
    Setting up mariadb-server (1:10.11.11-0ubuntu0.24.04.2) ...
    Created symlink /etc/systemd/system/multi-user.target.wants/mariadb.service → /usr/lib/systemd/system/mariadb.service.
    Setting up mariadb-plugin-provider-bzip2 (1:10.11.11-0ubuntu0.24.04.2) ...
    Setting up mariadb-plugin-provider-lzma (1:10.11.11-0ubuntu0.24.04.2) ...
    Setting up mariadb-plugin-provider-lzo (1:10.11.11-0ubuntu0.24.04.2) ...
    Setting up mariadb-plugin-provider-lz4 (1:10.11.11-0ubuntu0.24.04.2) ...
    Setting up libcgi-fast-perl (1:2.17-1) ...
    Setting up mariadb-plugin-provider-snappy (1:10.11.11-0ubuntu0.24.04.2) ...
    Processing triggers for man-db (2.12.0-4build2) ...
    Processing triggers for libc-bin (2.39-0ubuntu8.3) ...
    Processing triggers for mariadb-server (1:10.11.11-0ubuntu0.24.04.2) ...
  
    Running kernel seems to be up-to-date.
  
    No services need to be restarted.
  
    No containers need to be restarted.
  
    No user sessions are running outdated binaries.
  
    No VM guests are running outdated hypervisor (qemu) binaries on this host.
  stdout_lines: <omitted>

TASK [fact_push : Create the facts directory in case it isn't there] **********************
changed: [controlplane] => changed=true 
  gid: 0
  group: root
  mode: '0755'
  owner: root
  path: /etc/ansible/facts.d
  size: 4096
  state: directory
  uid: 0
changed: [node01] => changed=true 
  gid: 0
  group: root
  mode: '0755'
  owner: root
  path: /etc/ansible/facts.d
  size: 4096
  state: directory
  uid: 0

TASK [fact_push : Push over the db_patching.fact] *****************************************
skipping: [controlplane] => changed=false 
  false_condition: '"db" in app'
  skip_reason: Conditional result was False
changed: [node01] => changed=true 
  checksum: 925e5a53291810ced7689f4b0ffa6661c3f269b0
  dest: /etc/ansible/facts.d/patching.fact
  gid: 0
  group: root
  md5sum: 5726b801f8102d19cf2401c8e776a831
  mode: '0644'
  owner: root
  size: 132
  src: /root/.ansible/tmp/ansible-tmp-1748139966.1450477-28297-72207233794001/source
  state: file
  uid: 0

TASK [fact_push : Push over the web_patching.fact] ****************************************
skipping: [node01] => changed=false 
  false_condition: '"web" in app'
  skip_reason: Conditional result was False
changed: [controlplane] => changed=true 
  checksum: 5eb4d4883c9c64cdc5b2325da4760281d64618c4
  dest: /etc/ansible/facts.d/patching.fact
  gid: 0
  group: root
  md5sum: b73ef6b9d9db89eaa0adc73aedcab2ee
  mode: '0644'
  owner: root
  size: 131
  src: /root/.ansible/tmp/ansible-tmp-1748139967.2123652-28314-209963595066718/source
  state: file
  uid: 0

TASK [packages_update : Upgrade all packages to the latest version] ***********************
skipping: [controlplane] => changed=false 
  false_condition: action == "update"
  skip_reason: Conditional result was False
skipping: [node01] => changed=false 
  false_condition: action == "update"
  skip_reason: Conditional result was False

PLAY RECAP ********************************************************************************
controlplane               : ok=5    changed=3    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0   
node01                     : ok=5    changed=3    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0   

controlplane:~/HPC_Deploy$ ls -ltr /etc/ansible/facts.d/patching.fact
-rw-r--r-- 1 root root 131 May 25 02:26 /etc/ansible/facts.d/patching.fact
controlplane:~/HPC_Deploy$ cat !$
cat /etc/ansible/facts.d/patching.fact
{
  "patch_group": "group1",
  "rebooting":   "false",
  "appserver":   "app",
  "database":    "false",
  "webserver":   "true"
}
controlplane:~/HPC_Deploy$ ssh node01
Last login: Sun May 25 02:26:06 2025 from 10.244.3.22
node01:~$ cat /etc/ansible/facts.d/patching.fact
{
  "patch_group": "group2",
  "rebooting":   "false",
  "appserver":   "true",
  "database":    "true",
  "webserver":   "false"
}
node01:~$ 
logout
Connection to node01 closed.
controlplane:~/HPC_Deploy$
```
### *step 1 of lab*
```bash
controlplane:~/HPC_Deploy$ cat 04_enterprise_patching.yaml
- hosts: all
  gather_facts: true
  vars:
  tasks:

  roles:
  - precheck
  - fact_push
  - patch
  - { role: reboot, when: ansible_local.patching.rebooting == "true" }
  - server_validation
  - app_validation
  - { role: report, when: report | default(false) == "true" }
  controlplane:~/HPC_Deploy$ 
```

```bash
controlplane:~/HPC_Deploy$ cat 04_enterprise_patching.yaml
- hosts: all
  gather_facts: true
  vars:
  tasks:

  roles:
  - precheck
  - fact_push
  - patch
  - { role: reboot, when: ansible_local.patching.rebooting == "true" }
  - server_validation
  - app_validation
  - { role: report, when: report | default(false) == "true" }
  controlplane:~/HPC_Deploy$
```
