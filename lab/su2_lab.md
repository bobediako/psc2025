## Unit 2 Lab - Securing the Network Connection


### What information about the security posture of the system can you see here?
>  The standardbasic security posture is in effect.  The advanced and enhanced secuity options, like fips & selinux, are not enabled.


### Can you verify SELINUX status?

`
Security Enhanced Linux (SELinux) implements Mandatory Access Control (MAC). Every process and system resource has a special security label called an SELinux context. A SELinux context, sometimes referred to as an SELinux label, is an identifier which abstracts away the system-level details and focuses on the security properties of the entity.
`

```bash
[root@hammer14 evaluation]# grep -v ^# /etc/selinux/config
SELINUX=permissive
SELINUXTYPE=targeted
[root@hammer14 evaluation]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   permissive
Mode from config file:          permissive
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Memory protection checking:     actual (secure)
Max kernel policy version:      33

```



### Can you verify FIPS status?


`Federal Information Processing Standards (FIPS) are developed by the National Institute of Standards and Technology (NIST) to define how federal agencies and their private sector partners handle and encrypt sensitive information. These standards act as a blueprint for secure technology and cybersecurity practices, covering everything from cryptographic tools to data categorization.
Here’s why FIPS matter:

Consistency: All federal agencies follow the same security rules, ensuring interoperability.
Trust: FIPS ensures sensitive data is protected using validated, government-approved methods.
Relevance: Contractors working with federal agencies must often comply with FIPS, making it essential for businesses in the public sector.
`

```bash
[root@hammer14 evaluation]# sysctl -a | grep cryp
crypto.fips_enabled = 0
crypto.fips_name = Rocky Linux 9 - Kernel Cryptographic API
crypto.fips_version = 5.14.0-427.18.1.el9_4.x86_64
[root@hammer14 evaluation]# fips-mode-setup --check
Installation of FIPS modules is not completed.
FIPS mode is disabled.
```


---

### Connect to a hammer server
### Filter by ipv4 and see how many STIGs you have.
> 15

## Examine STIG V-257957
### What is the problem?
> Enabling tcp syncookies means the system can better resist SYN flood attacks.
> SYN cookies allow the server to avoid dropping connections when the SYN queue fills up.

### What is the fix?
>  No fix necessary
```bash
[root@hammer14 evaluation]# sysctl -a | grep ^net.ipv4.tcp_s
net.ipv4.tcp_sack = 1
net.ipv4.tcp_shrink_window = 0
net.ipv4.tcp_slow_start_after_idle = 1
net.ipv4.tcp_stdurg = 0
net.ipv4.tcp_syn_retries = 6
net.ipv4.tcp_synack_retries = 5
net.ipv4.tcp_syncookies = 1
```


### What type of control is being implemented?
>  Technical preventative


### Is it set properly on your system?
> Yes


### Can you remediate this finding?
```bash
[root@hammer14 evaluation]# ls -ltr /etc/sysctl.d/
total 8
lrwxrwxrwx. 1 root root  14 Apr  7  2024 99-sysctl.conf -> ../sysctl.conf
-rw-r--r--. 1 root root 106 Aug 17  2024 99-ipv4fix.conf
-rw-r--r--. 1 root root 910 Apr  5 17:38 98-remediate.conf
```

```bash
[root@hammer14 evaluation]# sysctl --system
* Applying /usr/lib/sysctl.d/10-default-yama-scope.conf ...
* Applying /usr/lib/sysctl.d/50-coredump.conf ...
* Applying /usr/lib/sysctl.d/50-default.conf ...
* Applying /usr/lib/sysctl.d/50-libkcapi-optmem_max.conf ...
* Applying /usr/lib/sysctl.d/50-pid-max.conf ...
* Applying /usr/lib/sysctl.d/50-redhat.conf ...
* Applying /etc/sysctl.d/98-remediate.conf ...
* Applying /etc/sysctl.d/99-ipv4fix.conf ...
* Applying /etc/sysctl.d/99-sysctl.conf ...
* Applying /etc/sysctl.conf ...
kernel.yama.ptrace_scope = 0
kernel.core_pattern = |/usr/lib/systemd/systemd-coredump %P %u %g %s %t %c %h
kernel.core_pipe_limit = 16
fs.suid_dumpable = 2
kernel.sysrq = 16
kernel.core_uses_pid = 1
net.ipv4.conf.default.rp_filter = 2
net.ipv4.conf.eth0.rp_filter = 2
net.ipv4.conf.lo.rp_filter = 2
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.conf.eth0.accept_source_route = 0
net.ipv4.conf.lo.accept_source_route = 0
net.ipv4.conf.default.promote_secondaries = 1
net.ipv4.conf.eth0.promote_secondaries = 1
net.ipv4.conf.lo.promote_secondaries = 1
net.ipv4.ping_group_range = 0 2147483647
net.core.default_qdisc = fq_codel
fs.protected_hardlinks = 1
fs.protected_symlinks = 1
fs.protected_regular = 1
fs.protected_fifos = 1
net.core.optmem_max = 81920
kernel.pid_max = 4194304
kernel.kptr_restrict = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.eth0.rp_filter = 1
net.ipv4.conf.lo.rp_filter = 1
kernel.kexec_load_disabled = 1
fs.protected_symlinks = 1
fs.protected_hardlinks = 1
kernel.dmesg_restrict = 1
kernel.perf_event_paranoid = 2
kernel.randomize_va_space = 2
net.ipv6.conf.default.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv6.conf.all.accept_source_route = 0
net.ipv6.conf.default.accept_source_route = 0
net.ipv6.conf.all.forwarding = 0
net.ipv6.conf.all.accept_ra = 0
net.ipv6.conf.default.accept_ra = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
kernel.unprivileged_bpf_disabled = 1
kernel.yama.ptrace_scope = 1
kernel.kptr_restrict = 1
user.max_user_namespaces = 0
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.conf.all.accept_redirects = 0
net.core.bpf_jit_harden = 2
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.all.forwarding = 0
net.ipv4.conf.default.send_redirects = 0
```

## Check and remediate V-257958 STIG
### What is the problem?
> RHEL 9 must ignore ICMP redirect messages
> aka ‘syn flood attack’

### What is the fix?
> Set a kernel parameter


### What type of control is being implemented?
> tech prev


### Is it set properly on your system?
> Yes
```bash
[root@hammer14 evaluation]# sysctl -a | grep ^net.ipv4.conf.default.accept_redirects
net.ipv4.conf.default.accept_redirects = 0
```

### How would you go about remediating this on your system?
> Add the value to /etc/sysctl.conf


## Check and remediate V-257960 and V-257961 STIGs
### What is the problem? How are they related?



### What is the fix?
> Set the kernel parameter
`
The presence of "martian" packets (which have impossible addresses) as well as spoofed packets, source-routed packets, and redirects could be a sign of nefarious network activity
`

### What type of control is being implemented?
> tech Prev


### Is it set properly on your system?
> No

```bash
[root@hammer14 evaluation]# sysctl -a | grep martians
net.ipv4.conf.all.log_martians = 0
net.ipv4.conf.default.log_martians = 0
net.ipv4.conf.eth0.log_martians = 0
net.ipv4.conf.lo.log_martians = 0
```

## Filter by firewall
### How many STIGS do you see?
> 4

### What do these STIGS appear to be trying to do? 
> 3 are trying to harden using the firewall
> 1 just mentions the word firewall but refers to the boot process

### What types of controls are they?
 t p

## Expose a network port through your firewall
```bash
[root@hammer14 evaluation]# ls /usr/lib/firewalld/services | grep -i node
kube-nodeport-services.xml
prometheus-node-exporter.xml
[root@hammer14 evaluation]# firewall-cmd --list-services
cockpit dhcpv6-client ssh
[root@hammer14 evaluation]# cat /usr/lib/firewalld/services/prometheus-node-exporter.xml
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>prometheus-node-exporter</short>
  <description>The node-exporter agent for Prometheus monitoring system.</description>
  <port protocol="tcp" port="9100"/>
</service>
[root@hammer14 evaluation]# firewall-cmd --permanent --add-service=prometheus-node-exporter
success
[root@hammer14 evaluation]# firewall-cmd --reload
success
[root@hammer14 evaluation]# firewall-cmd --list-services
cockpit dhcpv6-client prometheus-node-exporter ssh
```
## Examine the default values for STIGS

[root@hammer14 defaults]# grep -n 257784  main.yml
23:# R-257784 RHEL-09-211045
24:rhel9STIG_stigrule_257784_Manage: True
25:rhel9STIG_stigrule_257784__etc_systemd_system_conf_Value: 'none'

## Create an Ansible playbook from OpenSCAP
```bash
[root@hammer14 defaults]# cd /root
[root@hammer14 ~]# mkdir openscap
[root@hammer14 ~]# cd openscap
[root@hammer14 openscap]# oscap xccdf generate fix --profile ospp --fix-type ansible /usr/share/xml/scap/ssg/content/ssg-rhel9-ds.xml > draft-disa-remediate.yml

[root@hammer14 openscap]#
[root@hammer14 openscap]# wc draft-disa-remediate.yml
 10786  33460 376028 draft-disa-remediate.yml
[root@hammer14 openscap]# oscap xccdf generate fix --profile ospp --fix-type bash /usr/share/xml/scap/ssg/content/ssg-rhel9-ds.xml > draft-disa-remediate.sh
[root@hammer14 openscap]# wc draft-disa-remediate.sh
  4762  23532 238130 draft-disa-remediate.sh
```



