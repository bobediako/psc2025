## ProLUG Security Engineering

# Unit 4 Worksheet
*Instructions*
*Fill out this sheet as you progress through the lab and discussions. Hold your worksheets until the end to turn them in as a final submission packet.*

---

## Discussion Questions:

Unit 4 Discussion Post 1: 
Review some of the blogs here:
https://discord.com/channels/611027490848374811/1363211160467407002/1365753668200038411

Security - Unit 4 Discussion Post 1: Review some of the blogs here: https://aws.amazon.com/search/?searchQuery=air+gapped#facet_type=blogs&page=1 https://aws.amazon.com/blogs/security/tag/bastion-host/ or that you find on your own about air-gapped systems.

What seems to be the theme of air-gapped systems?

> The theme for air-gapped systems is that any connection to the internet is the greatest threat and that air-gapped systems provide the solution to this problem.
The theme for bastions is that it is a single point of entry thus a single point of infiltration/exfiltration.


What seems to be their purpose?

> An air-gapped systems’ purpose is completely isolate the system or network to protect the data and information on the network.  
The main purpose is to ensure the security of the company or organizations computers.  
It is the method used to ensure only insiders can access the systems.
A bastion systems’ purpose is to act as a gateway into an internal network to the external internet.  
The bastion systems’ purpose is to withstand attacks and provide a safe gateway into internal company resources.  
It is usually placed in a DMZ, where anyone can access from the outside, such as a way to transfer files with vendors.


If you use google, or an AI, what are some of the common themes that come up when asked about air-gapped or bastion systems?
- isolation
- cannot be hacked
- disconnected
- perimeter
- fortified
- DMZ
- secure access


Unit 4 Discussion Post 2: Do a Google or AI search of topics around jailing a user or
processes in Linux.
https://discord.com/channels/611027490848374811/1363211294013915399/1365758289081798786
Security - Unit 4 Discussion Post 2: Do a Google or AI search of topics around jailing a user or processes in Linux. 

**Can you enumerate the methods of jailing users?**
- use /etc/passwd (or nis, nss, ldap, ad) and the shell entry to limit users to a jailed script (instead of a shell) as shown in the ProLUG lecture and lab.  This also uses chroot to limit the user to a subset of files.
- there is apparently a set of tools called jailkit with commands using the prefix ‘jk_’  Like jk_lsh, jk_uchroot, jk_jailuser
- putting users into virtual machines is another method of effectively jailing them

**Can you think of when you’ve been jailed as a Linux user? If not, can you think of the useful ways to use a jail?**
```
I am jailed as a user when I use the ProLUG lab and the killercoda environments.  
In the past I was also jailed when I entered a DMZ host to upload or download files from a vendor. 
```

---

## Definitions/Terminology

Air-gapped:

    > This system or network is completely isolated from external networks for the goal of achieving highest level of secuirty.  In theory users will have have to physically infiltrate the location where an air-gapped system is present.

Bastion:

    > A system that is strategically positioned at the perimter of a network.  It connents the untrusted internet to the trusted intranet, etc.  It is the way to access internal systems securely from outside locations.  Bastions are often virtual machines , using time-limited access, via ssh or rdp (windows).

Jailed process:

    > This is a pre-cloud virtualization concept that allows for individual sessions on on a computer system to be completely seprate from others.  Thejailed processes cannot interact with other processes on the same machine.

Isolation:

    > Isolation in security context has the goal to safeguard systems and data from threats. If a system is isolated then an attack cannot easily spread to other systems.  SOme examples of techniques are virtualization, sandboxing and containerization.

Ingress:

    > iData entering a system or network.

Egress:

    > Data leaving a system or network.

Exfiltration:

    > This term is used to describe unauthorized extraction of data.  It is not used for normal day to day movement of data.  It usually refers to a removal or theft of data.

Cgroups:

    > This is a linux kernel feature that is short for control groups.  It a;;pws resources or prcesses to be organized in a hierachically ordered groups.

Namespaces:
- Mount, PID, IPC, UTS
Namespaces allow the OS to provide a container process with an isolated view of system resources.



---

Notes During Lecture/Class:
Links:
- https://www.sans.org/information-security-policy/
- https://www.sans.org/blog/the-ultimate-list-of-sans-cheat-sheets/
-
Terms:
Useful tools:
- STIG Viewer 2.18
- SCC Tool (version varies by type of scan)
- OpenScap


Digging Deeper

1. While this isn't, strictly speaking, an automation course there is some value in
looking at automation of the bastion deployments. Check out this ansible code:
https://github.com/het-
tanis/stream_setup/blob/master/roles/bastion_deploy/tasks/main.yml
    a. Does the setup make sense to you with our deployment?
    b. What can improve and make this better?

2. Find a blog or github where someone else deploys a bastion. Compare it to our
process.

3. Knowing what you now know about bastions, jails, and air-gapped systems. Reflect
on the first 3 weeks, all the STIGs you've reviewed and touched. Do any of them
seem moot, or less necessary if applied in an air-gapped environment?
    a. Does your answer change if you read about Zero Trust and know how much of
    a hot topic that is in the security world now?
        i. Why or why not?

4. Think of a Linux system where you would like to deploy a bastion (If you cannot think
of one, use ProLUG Lab). Draw out how you think the system works in
excalidraw.com.

Reflection Questions
1. Does it matter if the user knows that they are jailed? Why or why not?
2. What questions do you still have about this week?
3. How are you going to use what you've learned in your current role?
