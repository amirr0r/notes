# Privesc

Privilege Escalation involves increasing permissions/rights on the target machine. 

This can be done by exploiting a vulnerability/misconfiguration on a system to gain unauthorized access to resources that are usually restricted from the users.

Being able to "prives" may allow you to:
- install malicious softwares, edit software configurations or more generally enable persistence via **backdoors** (such as in this [article](https://airman604.medium.com/9-ways-to-backdoor-a-linux-box-f5f83bae5a3c) or in this [example](https://0x90909090.blogspot.com/2016/06/creating-backdoor-in-pam-in-5-line-of.html))
- change passwords
- bypass access control or "pivot" (lateral movement)
- get `root.txt` flag :)


## SUID/GUID Files

A binary that can be run with the permissions of the owner/group of the file.

- `find / -perm -u=s -type f 2>/dev/null`

## Read/write sensitive files

- `/etc/passwd`
    + compute a password hash: `openssl passwd -1 -salt [salt] [password]`
    + add a new entry to `/etc/passwd`: `<USERNAME>:<HASH>:0:0:root:/root:/bin/bash`

### `sudo` misconfiguration + GTFOBins

- `sudo -l`
    + `NOPASSWD`

### crontab

` /etc/crontab`


## Useful links/tools

- [LinPeas](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS#linpeas---linux-privilege-escalation-awesome-script)
- [LinEnum](https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh)
- [GTFOBins](https://gtfobins.github.io/)
- [Linux Privilege Escalation Checklist](https://github.com/netbiosX/Checklists/blob/master/Linux-Privilege-Escalation.md)
- [PayloadsAllTheThings - Linux Privesc](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md#linux---privilege-escalation)
- [Total OSCP Guide - Linux Privesc](https://sushant747.gitbooks.io/total-oscp-guide/content/privilege_escalation_-_linux.html)
- [A guide to Linux Privilege Escalation](https://payatu.com/guide-linux-privilege-escalation)
- [Tib3rius - Linux Privilege Escalation for OSCP & Beyond!](https://www.udemy.com/course/linux-privilege-escalation/)
- [THM - Common Linux Privesc](https://tryhackme.com/room/commonlinuxprivesc)