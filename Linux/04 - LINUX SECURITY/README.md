# Linux SEC

## Introduction

The course is given by Ric Messier (*GSEC, GCIH, C|EH, CISSP, MS / DFS...*). We will harden our Linux OS.

## What Will Be Covered

1. Distibutions
2. Boot Process
    + 2.1 Boot loader
    + 2.2 Protecting the boot process
3. Services
    + 3.1 Managing services
    + 3.2 Hardening services
4. Logging
    + 4.1 Syslog
    + 4.2 Log management
    + 4.3 Remote Logging
5. Intrusion Detection
    + 5.1 Network-based
    + 5.2 Host-based
    + 5.3 Managing IDS
6. Users and Permissions
    + 5.1 Setting password polices
    + 5.2 AppArmor
    + 5.3 SELinux
    + 5.4 Using Pluggable Authentication Modules
7. Utilities
    + 7.1 Networking
    + 7.2 Security-related
    + 7.3 Vulnerability scanning
8. Linux Kernel
    + 8.1 Modules
    + 8.2 Configuring the kernel
    + 8.3 Building the kernel
9. Firewalls
    + 9.1 Iptables
    + 9.2 Firewalld
    + 9.3 UFW

## What Is Linux

Linux is the core Operating System (Kernel) : the thing which interfaces with the hardware and the softwares.

## Distributions

[distrowatch.com](https://distrowatch.com/)

## Scratch Versus Binary

The disadvantage to binary based (Redhat, Ubuntu...) distros is that they determine what all of the dependencies that are going to be installed.

A distribution like Arch-Linux for example, does give you the ability to build your own packages depending on the dependencies an the specific subsystems and packages you want to install.

### Conclusion :
- Scratch version : more flexibility control and power
- Binary version : more easier to begin with

## LINUX Security  01 06 Ubuntu Package Management

## LINUX Security  01 07 RedHat Package Management

## 02 01 The Boot Process

When you start a computer up, the first thing that happens is it runs a **power-on self-test** :
1. make sure that all of the internal hardware to the system is operating correctly
2. transfers control to the firmware that is going to look for a specific portion of the hard drive (*the Master Boot Record or the GUID partition table - associated with a DOS system*)

> Instead of using the **GR**and **U**nified **B**oot**L**oader or **GRUB**, you can use those boot loaders **LI**nux **LO**ader or **LILO**.

The point of the boot loader is to boot the OS from the hard disk. It does this by reading the first section of the hard drive. *Hmm `/dev/sda1` ?*

## 02 02 Physical Protections

Things you can do to protect your computer in case somedy can get physical access to it :
> just a boot disk allow to have root priviledge to your system
- Turn on BIOS passwords (*Supervisor password, User password, HDD password...*)
- Enable Hard drive encryption

## 02 03 The Boot Manager GRUB

GRUB is a piece of software that has a number of stages.
- `/etc/grub.d/00_header`
- `/boot/grub/grub.cfg`

## 02 04 Protecting The Boot Manager

In addition to using BIOS passwords, you can also set a password on the BootLoader.

Settings some password on GRUB  :
```bash
echo "set superusers='me'" >> /etc/grub.d/00_header
echo "password me azerty123" >> /etc/grub.d/00_header # clairtext for now
echo "export superusers" >> /etc/grub.d/00_header
sudo update-grub
```

## 02 05 xinetd

One way to have network services on a Linux System is to make use of the Internet super server.

`inetd` or `xinetd` is the Internet super server process. 

The reason it's called the super server is because its one service that actually manages :
- a number of applications that sit behind it.
- all of the listening and communication and passes those communications off to the service program.

> `cd /etc/xinetd.d/; ls`

## 02 06 Runlevels

**runlevels** are ways of grouping services together, you specify a particular way of booting so you get specifically the services that you are looking for.

- `/etc/rc0.d` or **run level 0** : stopping the OS / shutting the machine down
- `/etc/rc1.d` : single user mode
- `/etc/rc1.d` : start adding additional services

`S*` => startup script => links for servics in `../init.d/`
`K*` => kill script

The order that they run in is actually ascii influenced (by the name).

`/etc/inittab` : config file for the init system

[comment]: <> (as we go up in run levels)

## 02 07 Setting Default Runlevels

`systemd` vs `init` *(both starting the system up).*

`init` is older

`/etc/systemd`.
Rather than **run levels** 0, 1, 2 ..., we have targets instead :`*.target.wants`.

`systemctl` control the systemd system and service manager.

## 02 08 GRUB2

```bash
grub-mkpasswd-pbkdf2 # give a hash string for the password in /etc/grub.d/00_header
```

## 02 09 LILO

## LINUX Security 03 01 Service Management

**service** : a programm that is started up in n and immediately goes to the background and just kind of runs there, waiting for requests.
> ***Examples**: web / ftp server, dbus (manages inter-process communication)...*

In most of Linux ditributions, you can manage services by doing :
```bash
sudo /etc/init.d/$service stop
sudo /etc/init.d/$service status
sudo /etc/init.d/$service star`
```

## LINUX Security 03 02 Service Management With RHEL7

With **systemd** you can use `systemctl` to list all the services that are running in this particular system :

```bash
sudo systemctl restart $name.service
sudo systemctl stop $name.service
sudo systemctl status $name.service # much more detailed than init system
sudo systemctl is-enabled $name.service
sudo systemctl enable $name.service
sudo systemctl disable $name.service
```

## LINUX Security 03 03 TCP Wrappers

A **TCP Wrapper** is a program that is designed to wrap a service program. 

You might use this for `telnet` or `ftp`, or maybe a potentially fragile service that doesn't have its own ability to manage wether somebody is going to be allowed to get access to it or not.

```bash
man tcpd
man hosts_access
cat /etc/hosts.deny # black-list
cat /etc/hosts.allow # white-list
```

## LINUX Security 03 04 Listening Ports

A **port** is (*a local address that a program connects to*) associated with a particular transport protocol.

```bash
cat /etc/services # list services with port that are associated with them
sudo netstat --listening # list listening ports
```

## LINUX Security 03 05 Standard Postfix Configuration

**Postfix** is amail server that was designed to be far more secure than some of the other options that were out there.

```bash
/etc/postfix/main.cf " config file of postfix
```

## LINUX Security 03 06 Apache Configuration

```bash
/etc/apatche$N # $N version
```

## LINUX Security 03 07 Hardening Apache

```bash
/etc/apache$N/conf-enabled/security.conf
```

1. Comment the line with `ServerTokens OS` and add `ServerTokens Prod` *(for product only)* in order to minimise the informations you'll give to the attacker. 
2. Replace `ServerSignature On` by `ServerSignature Off`
> This is simply going to say Apache and it's not even going to give the version number.

3. `TraceEnable On` to `TraceEnable Off`

Now go to `/etc/apache$N/sites-enabled/000-default.conf`, you can add :
```
<Directory /var/www/html>
    Options -Indexes
    Directory index.html main.html index.htm index.php
```
**This will turn off the directory listing when there's no index file.**

In `/etc/apache$N/mods-enabled/`, you can see you modules.

## LINUX Security 03 08 Virtual Hosts In Apache

In `/etc/apache$N/sites-enabled/000-default.conf`, you can add a **VirtualHost** :
```
<VirtualHost $ip:$port>
    ServerAdmin webmaster@foo.com
    DocumentRoot /var/www/www.foo.com/docs
<VirtualHost>
```
> This is a minimalist example

## LINUX Security 03 09 DNSSec

**DNSSec** to protect your DNS Server.

File : `/etc/bind/named.conf.options` :
```
...
dnssec-enable yes;
dnssec-validation yes;
dnssec-lookaside auto;
...
```
```bash
cd /etc/bind/zones
dnssec-keygen -a NSEC3RSASHA1 -b 2048 -n ZONE mydomain.com
dnssec-keygen -f KSK -a NSEC3RSASHA1 -b 4096 -n ZONE mydomain.com
for k in `ls Kmydomain.com*.key`
> do
> echo "\$INCLUDE $k" >> db.mydomain.com
> done
cat db.mydomain.com
dnssec-signzone -3 $a_salt_of_16_digits -o mydomain.com -t db.mydomain.com
cd ..
vim named.conf.local
```

Add `.signed` to the `file "/etc/bind/zones/db.mydomain.com"` line.

```bash
dig @127.0.0.1 DNSKEY mydomain.com +multiline
```

## LINUX Security 03 10 MySQL

## LINUX Security 03 11 PostgreSQL

## LINUX Security  03 12 Tomcat

## LINUX Security  03 13 JBoss

## LINUX Security  03 14 mod security

**WAF:** web application security

We will the use of **modsecurity**.

## LINUX Security 03 15 SSLTLS And Apache .

```bash
openssl req -new -x509 -days 365 -sha1 -newkey rsa:2048 -nodes -keyout server.key -out server.crt -subj '/0=MyCompany/OU=MyDepartment/CN=www.mycompany.com' # create a certificate
cp server.key /etc/ssl/private
cp server.crt /etc/ssl/certs/
```

## LINUX Security  03 16 SPF And Greylisting In Postfix

