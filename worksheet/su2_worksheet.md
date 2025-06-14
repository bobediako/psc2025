## ProLUG Security Engineering 
# Unit 2 Worksheet


*Instructions*
*Fill out this sheet as you progress through the lab and discussions. Hold your worksheets until the end to turn them in as a final submission packet.*

---

## Discussion Questions:

### Unit 2 Discussion Post 1: 

https://discord.com/channels/611027490848374811/1358101042159419542/1360398634352513035

Security - Unit 2 Discussion Post 1: 
There are 401 stigs for RHEL 9. If you filter in your stig viewer for sysctl there are 33 (mostly network focused), ssh - 39, and network - 58. Now there are some overlaps between those, but review them and answer these questions
As systems engineers why are we focused on protecting the network portion of our server builds?
``` 
Our network is how we interact with the server.  
Gone of the days of servers in locked rooms.  
The network is our primary point of failure for unauthorized access.  
```

Why is it important to understand all the possible ingress points to our servers that exist?
``` 
The ingress points are how we make valid connections to the server. 
These are how the various outside services we rely upon connect to a machine.
Hackers or unauthorized users will sometimes enter systems through network vulnerabilities. 
Network exploits, like two from 2017 called NotPetya and WannaCry, are probably the biggest threat.
```

Why is it so important to understand the behaviors of processes that are connecting on those ingress points?
```
During an attack or simply during a system wide failure, having a solid previous understanding of the system, and how it normally works is essential to troubleshooting.  
Spending our free time and our quiet time, making sure that we know the ins and outs of our servers, makes things much easier during a crisis.
```



### Unit 2 Discussion Post 2: 

https://discord.com/channels/611027490848374811/1358101115173863636/1361837113556996116

### Security - Unit 2 Discussion Post 2: 
Read this: https://ciq.com/blog/demystifying-and-troubleshooting-name-resolution-in-rocky-linux/ or similar blogs on DNS and host file configurations.

What is the significance of the nsswitch.conf file?
In this question we are focused on the “host” line of Nsswitch,conf 
The order of this entry matters – “files” is always first 
files points to /etc/hosts


Next is usually “dns”
The Domain Name System DNS is a hierarchy or like branches on a tree
We use the /etc/resolv.conf to define our starting point in this hierarchy
We need to enter into the name of at least one nameserver to access this hierarchy

We also want to make our lives easier when accessing hosts on our intranet so we can define a “search” domain for our local servers

p.s. I found that there are some other cool options for this file such as “option attempts” & “option timeout” that are useful when listing multiple nameservers for redundancy.


What are security problems associated with DNS and common exploits? 
(May have to look into some more blogs or posts for this)
Common exploits include:
DNS Spoofing(cache poisoning)
This involves inserting false info into the DNS cache, so that DNS queries return incorrect responses, that will lead users to likely malicious sites.


DNS Amplification Attacks
This is a DDoS attack where the attacker floods a target with DNS response traffic.  This overwhelms the DNS nameserver so that users do not receive a timely response.

DNS Tunneling
This is a technique used to bypass network firewalls.  This exploit is used to exfiltrate data from a compromised system.


---

## Definitions/Terminology

sysctl:

    > This command allows sysadmins to configure/change various kernel parameters while a system is up and running.  
    > #/sbin/sysctl -a
    > the above command will list allof the current systems kernel parameters.

nsswitch.conf:

    > Name Services as a concept allowed for the storage of variois information in distrubuted locations.  This information could be located and retrieved from a different system which was the centralized source for the information.  The nsswitch.conf allows us to switch between the various sources of information.
    > For example the below /etc/nswwitch.conf entry says that the first source is the local files, the 2nd source is the dns service, the 3rd source is a system call to the kernel.
    > hosts:      files dns myhostname

DNS:
    > Domain Name Service.  This is a hierarchical and decentralized naming system that translates IP addresses into easy to read domain names. The DNS system works like branches on a tree.  There are 13 root servers that serve as the trunk of the tree.  But we almost never contact those servers-  everyone using the internet work with authoritative name servers and second level domains.

Openscap:

    > openscap is an open source vulnerability scanning project and set of tools.  It can search for vulnerabilities to resolve STIG or CIS benchmarks.

CIS Benchmarks:

    > The Center for Internet Security is a non profit sponsored by companies and the US government. The benchmarks are a set of recognized best pratices for securing IT systems.

ss/netstat:

    > This is an excerpt from the man page- better than my explanation in this case...
    > "Netstat  prints  information about the .." networking subsytem. On Linux .." this  program  is mostly obsolete.  Replacement for netstat is ss.  Re‐ placement for netstat -r is ip route.  Replacement for netstat -i is ip -s link.  Replacement for netstat -g is ip maddr. "

    > I have to retrain myself to use these new commands instead of netstat.

tcpdump:

    > This command captures and displays all of the incming packets on a network interface.

ngrep:

    > ngrep is similar to tcpdump but adds regex searching of the output.  Essentially nrep is tcpdump plus grep.

---

Notes During Lecture/Class:

Links:
* https://www.sans.org/information-security-policy/
* https://www.sans.org/blog/the-ultimate-list-of-sans-cheat-sheets/
* https://docs.rockylinux.org/gemstones/core/view_kernel_conf/
* https://ciq.com/blog/demystifying-and-troubleshooting-name-resolution-in- rocky-linux/
* https://www.activeresponse.org/wp-content/uploads/2013/07/diamond.pdf


Terms:


Useful tools:
* STIG Viewer 2.18
* SCC Tool (version varies by type of scan)
* OpenScap


Lab and Assignment
Unit2_Network_Standards_and_Compliance - To be completed outside of lecture
time.

Digging Deeper
1. See if you can find any DNS exploits that have been used and written up in the diamond model of intrusion analysis format. If you can, what are the primary actors and actions that made up the attack?


Reflection Questions
1. What questions do you still have about this week?


2. How are you going to use what you've learned in your current role?
