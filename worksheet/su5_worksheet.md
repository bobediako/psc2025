## ProLUG Security Engineering
# Unit 5 Worksheet

*Instructions*
*Fill out this sheet as you progress through the lab and discussions. Hold your worksheets until the end to turn them in as a final submission packet.*

---

## Discussion Questions:

### Unit 5 Discussion Post 1: 
Review the rocky documentation on Software management in Linux. https://docs.rockylinux.org/books/admin_guide/13-softwares/


https://discord.com/channels/611027490848374811/1365793131362123787/1367713844855701534

Unit 5 Discussion Post 1: Review the rocky documentation on Software management in Linux. https://docs.rockylinux.org/books/admin_guide/13-softwares/
What do you already understand about the process?

```I already understood the concept of packages. I’ve had past experience using the rpm command.```

What new things did you learn or pick up?

```The yum and dnf commands.```

What are the DNF plugins?

```Plugins add extra features to dnf,  For example local allows dnf to specify a local repository for rpm packages.```

a.    What is the use of the versionlock plugin?

```Versionlock simplifies managing multiple versions of a package.  The repo being used may have many different versions of any given package,  versionlock allows an administrator to define the preferred and current version to use.  This helps to ensure all machines in the environment are using the same version.```

What is an EPEL? 

```EPEL is a curated set of extra packages for linux.  These extra packages are widely used open-source packages.  They are not considered necessary for standard enterprise environments and are not usually deployed to production serves.```

a.    Why do you need to consider this when using one?

```Many vendors will deny support of environments that have enabled epel packages.  This is because EPEL packages being open-source and apparently filled with tools that can allow users to change environments in unexpected ways.  Vendors prefer that environments are limited and tightly controlled.   Restricting the installation to a small set of known packages and tools is preferred for support purposes.```


### Unit 5 Discussion Post 2: Do a google search for "patching enterprise Linux" and try to wade through all of the noise.


https://discord.com/channels/611027490848374811/1365793272513171607/1367856588186062858

Unit 5 Discussion Post 2: Do a google search for “patching enterprise Linux” and try to wade through all of  the noise.

1.    What blogs (or AI) do you find that enumerates a list of steps or checklists to consider?

```
https://phanes.cloud/step-by-step-guide-to-updating-and-patching-your-linux-server/
https://tuxcare.com/blog/linux-patch-management/  (advertisement)
https://www.msp360.com/resources/blog/guide-to-linux-patching-management/
https://netdepot.com/blog/8-patch-management-best-practices-you-should-know-about/

```

2.    After looking at that, how does patching a fleet of systems in the enterprise differ from pushing “update now” on your local desktop?

```
Patching in the enterprise must be done after testing on a staging environment (at a minimum.) “Update now” is performed on desktops which are usually closed source and tightly controlled systems (with less likely problems).  Even though desktops also integrate many separate and distinct vendor products their risk is low because desktops, etc. can allow for downtime.
```

     a.    What seems to be the major considerations?

```
Enterprise systems usually have defined downtime schedules for patches and reboots.  If a desktop type client becomes bricked after an update they can usually be swapped out.  But enterprise systems must be successfully patched or successfully rolled back to their previous state within a change window.
```

    b.    What seems to be the major roadblocks?

```
Zero-day vulnerabilities also affect all types of systems.  But special care needs to taken when updating enterprise systems.  Performance or standard updates may need to be postponed and re-evaluated if an emergency patch is performed for a major vulnerability.

```

---

## Definitions/Terminology
- Patching:

    >OS patching is the process of applying updates to the systtem's software.  This is done to ensire the software remains free of vulnerabilities, has applied the most recent bug fixes and benefits from the newest features.  Performing patches on a regular and scheduled basis is a core function of system administration.

- Repos:

    > A software repository (repo) is a place (online server) for storing software in the form of packages (or bundled with tools like tar or zip).  These repos are expected to be free, safe tampering, accessible to all.  Many will include digital signatures so that the files obtained from the repos can be trusted as tamper-free/malware-free.

- Software:

    > (This should be one of the first three questions :-)  Software is a computer program.  Computer programs are software. Computer programs a set of files that perfom actions and execute functions on a computer.  Software is compiled from a high-level language down to the simplest form which is machine language in specific to the cpu processor of the specific computer. Some computer programs are interpreted at excution time (just in time compilation).

- EPEL:

    > Extra Packages for Enterprise Linux.  These are additional packages that are useful but usually not officially supported by vendors for use in production systems.

- BaseOS v. Appstream (in RHEL/Rocky)
- Other types you can find?:
    > BaseOS are the core set packages for RHEL.  AppStream are the extra and additional commonly used packages.


- httpd:

    > The Hyper Text Transfer Protocol ... it's daemon... functions a a webserver.

- GPG Key:

    > GNU Privacy guard -an implementation of a public key cryptography.
    > It uses two keys:
    > A public key for encryption-  this is public and shared with anypne who wants to communicate with another person (the person waiting for receipt of the information)
    > A private key is kept secret and it is used for decryption on the information after receipt.
    > To send a file of information securely, involves the following steps, in order.
    > The person (he) who wants to receive files first has to generate a public and private key pair.
    > He then has to give the public key to the sender.
    > The sender (she) will encrypt the files/information using the public key on her machine.
    > The sender can now safely transmit the information over the internet,etc.
    > The receiver can then use his private key to decrypt the files/information in privacy.

- DNF/YUM:

    > dnf and yum are package management tools using the rpm format of packages for RHEL based distros.



---


Notes During Lecture/Class:

Links:
- https://wiki.rockylinux.org/rocky/repo/
- https://www.sans.org/information-security-policy/
- https://www.sans.org/blog/the-ultimate-list-of-sans-cheat-sheets/
- https://public.cyber.mil/stigs/downloads/

Terms:

Useful tools:
- STIG Viewer 2.18
- SCC Tool (version varies by type of scan)
- OpenScap

Lab and Assignment
Unit5_Repos_and_Patching - To be completed outside of lecture time.


Digging Deeper
1. After completing the lab and worksheet, draw out how you would deploy a software repository into your system.
    a. How are you going to update it?
    b. What tools do you find that are useful in this space?

Reflection Questions
1. Why is it that repos are controlled by root/admin functions and not any user, developer, or manager?
2. What questions do you still have about this week?
3. How are you going to use what you've learned in your current role?

