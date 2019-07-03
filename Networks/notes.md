# PCAP

Notes from [PCAP Analysis](http://maki.bzh/articles/wiresharkhowtobasic/).

PCAP file contain data from 2nd to 7th layer in OSI model.

Usage: identify malicious traffic, track a threat, identifies rogue DHCP servers, monitors intrusions...

## Tools: 

- **Wireshark-qt (GUI) / tshark (command line)**: analysis and capture tool

```bash
tshark -r filename.pcap -Y display_filter -Tfields -e some_specific_filter
# Example: tshark -r OtterLeak.pcap -Y 'smb && data.data' -Tfields -e data.data
```

- **CapAnalysis**: statistical analysis such as counting the number of requests per IP, listing protocols used over time and so on.
- **Binwalk**: reverse engineering tool &rarr; extracting firmware images. Detect some file patten inside another.

```bash
binwalk -e filetransfer.pcapng
```

**Foremost** is similar to **Binwalk**.

- **Scripting languages (ex: Python _(pyshark, scapy...)_ / Bash)**

> The Linux distribution TSURUGI has been developed for forensic analysts in order to make a turnkey OS with all the essential pre-configured tool &rarr; https://tsurugi-linux.org/downloads.php

___

# [TCPDUMP](https://www.tcpdump.org/)

`tcpdump` works exactly like **Wireshark**, except that it is the <u>command line</u> for Linux machines.

> `windump` is a similar program for Windows users (except Windows 10 ?).

**TCPdump** will print out a description of all the contents of all the packets that is capturing preceded by a timestamp _(save the data into a file, capture a specific amount of packets...)_.

Some options:
- `-D`: list available network interfaces.
- `-i <interface | number>`: capture the traffic of a specific interface.
- `-n`: don't convert addresses to names.
- `-A`: print packet to ASCII.
- `-c`:Exit after receiving count packets.
- `<PROTOCOL>`: filter by protocol.
- `dst|src <ip>`: filter by destination|source.
- `-t` : delete all timestamps.
- `-tttt` : time + date.
- `-x`: print in hex (with link level headers type `-xx`).
- `-X`: print ASCII + hex (with link level headers type `-Xx`).

> **link level headers**: include OS layer 2 information like MAC addresses.

Genrally outputs look like:

```
<time(h,m,s,ms)> <protocol> (<port>) <packet information> 
```

To go further check [**TCPDUMP filters cheat sheet**](http://alumni.cs.ucr.edu/~marios/ethereal-tcpdump.pdf).
___
