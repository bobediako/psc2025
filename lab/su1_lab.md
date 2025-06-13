# Unit 1 Lab - Build Standards and Compliance



## Check and remediate v-253666 STIG.
### What is the problem?
> The setting max_user_connections is unset and therefore a security risk
```sql
MariaDB [(none)]> select host,user,max_connections,max_user_connections from mysql.user;
+-----------+-------------+-----------------+----------------------+
| Host      | User        | max_connections | max_user_connections |
+-----------+-------------+-----------------+----------------------+
| localhost | mariadb.sys |               0 |                    0 |
| localhost | root        |               0 |                    0 |
| localhost | mysql       |               0 |                    0 |
+-----------+-------------+-----------------+----------------------+
```
### What is the fix?
> To set it to a specific number for each relevant user

```sql

MariaDB [(none)]> grant usage on *.* to root@localhost with max_user_connections 256;
Query OK, 0 rows affected (0.001 sec)
MariaDB [(none)]> grant usage on *.* to mysql@localhost with max_user_connection
s 8;
Query OK, 0 rows affected (0.001 sec)


MariaDB [(none)]> select host,user,max_user_connections, max_connections from mysql.user;
+-----------+-------------+----------------------+-----------------+
| Host      | User        | max_user_connections | max_connections |
+-----------+-------------+----------------------+-----------------+
| localhost | mariadb.sys |                    0 |               0 |
| localhost | root        |                  256 |               0 |
| localhost | mysql       |                    8 |               0 |
+-----------+-------------+----------------------+-----------------+
3 rows in set (0.002 sec)

```
### What type of control is being implemented?

> This is a technical preventative control

### Is it set properly on your system?

> No it was not set by default
> The above is the evidence of the change



---
## Check and remediate v-253677 STIG

### What is the problem?
> The CVE/STIG requires the db server to shutdown if the log space runs out.

### What is the fix?
> The fix is to implement proper disk monitoring to ensure that disk space does not run out on the server
> The STIG states that if procedures exist... prove they work...

### What type of control is being implemented?
> Technical prevenative

### Is it set properly on your system?
> No, not on the ProLUG lab
> in reality we would have procedures in place such as monitoring by grafa or any equivalent tool to ensure log rotation,compression, offboarding and deletion can be performed to ensure space is available instead of simply shutting down the server as the STIG suggested.


---

## Check and remediate v-253678 STIG

### What is the problem?
> This CVE also asks that steps be performed to avoid disk space 'running out.'

### What is the fix?
> The fix is to have proper disk space monitoring and procedures to reduce space (or dynamically increase space).

### What type of control is being implemented?


### Is it set properly on your system?
> No the config file needs to be updated
> Note the STIG mentions the file mariadb-enterprise.cnf 
> But the lab is using the basic community server version of msql/mariadb
> so the actual file name is different
```bash
[root@hammer14 ~]# rpm -ql mariadb-server | grep cnf
/etc/my.cnf.d/enable_encryption.preset
/etc/my.cnf.d/mariadb-server.cnf
/etc/my.cnf.d/spider.cnf
/usr/share/mariadb/wsrep.cnf
```
> Below is the section of the file to be updated
> server_audit_output_type = 'syslog' 
> 
```
[root@hammer14 ~]# cat -n /etc/my.cnf.d/mariadb-server.cnf | head -22
     1  #
     2  # These groups are read by MariaDB server.
     3  # Use it for options that only the server (but not clients) should see
     4  #
     5  # See the examples of server my.cnf files in /usr/share/mysql/
     6  #
     7
     8  # this is read by the standalone daemon and embedded servers
     9  [server]
    10
    11  # this is only for the mysqld standalone daemon
    12  # Settings user and group are ignored when systemd is used.
    13  # If you need to run mysqld under a different user or group,
    14  # customize your systemd unit file for mysqld/mariadb according to the
    15  # instructions in http://fedoraproject.org/wiki/Systemd
    16  [mysqld]
    17  datadir=/var/lib/mysql
    18  socket=/var/lib/mysql/mysql.sock
    19  log-error=/var/log/mariadb/mariadb.log
    20  pid-file=/run/mariadb/mariadb.pid
    21
```

---

## Check and remediate v-253734 STIG

### What is the problem?
> THE CVE/STIG requires that a specific setting be performed
> bind-address = 127.0.0.1 #just an example
> 


### What is the fix?
> The fix is to uncomment the setting / set it in the proper file 

```bash
[root@hammer14 ~]# grep bind !$/*
grep bind /etc/my.cnf.d/*
/etc/my.cnf.d/mariadb-server.cnf:#bind-address=0.0.0.0
```

### What type of control is being implemented?
> Tech Preventative


### Is it set properly on your system?
> I remediated this with the below

```bash
[root@hammer14 ~]# grep bind /etc/my.cnf.d/*                                    /etc/my.cnf.d/mariadb-server.cnf:#bind-address=0.0.0.0
[root@hammer14 ~]# perl -pi -e "s/^#bind-address=0.0.0.0/bind-address=0.0.0.0/" /etc/my.cnf.d/mariadb-server.cnf
[root@hammer14 ~]# diff -c /etc/my.cnf.d/mariadb-server.cnf /etc/my.cnf.d/.mariadb-server.cnf-bak1
*** /etc/my.cnf.d/mariadb-server.cnf    2025-05-22 20:21:02.667000000 -0700
--- /etc/my.cnf.d/.mariadb-server.cnf-bak1      2025-05-08 21:34:09.000000000 -0700
***************
*** 34,40 ****
  #
  # Allow server to accept connections on all interfaces.
  #
! bind-address=0.0.0.0
  #
  # Optional setting
  #wsrep_slave_threads=1
--- 34,40 ----
  #
  # Allow server to accept connections on all interfaces.
  #
! #bind-address=0.0.0.0
  #
  # Optional setting
  #wsrep_slave_threads=1
```
