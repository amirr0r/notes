# Questions

## 0. Drivers ?

a computer program that operates or controls a particular type of device that is attached to a computer (printer driver..)

## 1. Difference between  `sudo` & `su` ?

- `su` allows you to do anything the given user can do *(which is often more than you need)*. 
- `sudo` *(is safer)* allows you just enough authority to run the commands you need, nothing more.
> `su` substitute shell which is not the case of `sudo`

## 2. Kerberos ?

[Kerberos](http://web.mit.edu/kerberos/) is a network authentication protocol. It is designed to provide strong authentication for client/server applications by using secret-key cryptography.

> So, when users authenticate to network services using Kerberos, unauthorized users attempting to gather passwords by monitoring network traffic are effectively thwarted.

## 3. Proxy ?

get around limitations imposed by a network, and hide your internal network from the Internet.
> gateway → redirect requests, *only* hide your IP address. When using a proxy server, you will appear as though you are browsing from the mediating server’s location - **but only when browsing the internet.**

## 4. Firewall ?

Monitors and controls incoming and outgoing network traffic and define which types of communications are allowed.

## 5. VPN ?

A Virtual Private Network is a network that is constructed to allow connecting to a private network from a machine not physically present on the network.

Like proxies, it make your traffic appear as if it comes from a remote IP address. 

However, connection is formed over an **encrypted tunnel** to the private network.

## 6. (SSH) Tunnel ?

Method of transporting arbitrary networking data over an encrypted SSH connection.
> *port forwarding* TCP  → SSh

> not specific to SSH - a competent programmer can write a tool to tunnel ports in a few hours. Any device on the internal network can do it - it just needs to be able to communicate with some (any) service on the Internet. The tool could be made to work over **SSL/TLS**, could **emulate HTTP**, or could **operate over UDP** and use **packets** that look **like DNS requests and responses**. SSH just makes it easier for non-programmers. [Source](https://www.ssh.com/ssh/tunneling/)

## 7. Containers ? What ? Why ? How ?
## 8. Audit (daemon / framework) ?

Try to detect and collect all malicious activity (denial of service attacks, port scans...) by generating log entries and record as much information as possible.
> passive ? *The option report_URL is not yet implemented, but the author's intention was that it should be able to e-mail or maybe even execute scripts. [...] HIDS like AIDE are a great way to detect changes to your system, but it never hurts to have another line of defense. [Source](https://wiki.gentoo.org/wiki/Security_Handbook/Intrusion_detection)*

## 9. Fail2Ban ?

Scans the log files for too many failed login attempts and blocks the IP address which is showing malicious signs.


## 11. TCP Wrappers ?

TCP wrappers is a way of controlling access to services normally run by inetd, but it can also be used by xinetd and other services.

> The last version of tcp_wrappers was released 20 years ago (although IPv6 support was added later). At that time it was very powerful tool to "block all traffic", but these days we can do the same thing using firewalls/iptables/nftables for all traffic on network level or use similar filtering at the application level. One of the motivating factors for this change was the removal of TCP wrappers support from systemd and OpenSSH in 2014, 

## 12. Logs ?

### Definition 

A **log** is a automatically produced and time-stamped documentation file that records :
- events that occur in the OS or other software runs.
- messages between different users of a communication softwares.

### How do they work in Linux ?

They're usually stored in `/var/log/`. See [Linux log files](https://www.cyberciti.biz/faq/linux-log-files-location-and-how-do-i-view-logs-files/)

> **Warning:** Attackers will most likely try to erase their tracks by editing or deleting log files. You can make it harder for them by logging to one or more remote logging servers on other machines.
> **Loggers:** Sysklogd, Metalog, Syslog-ng..
