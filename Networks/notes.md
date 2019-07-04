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

## Some options

- `-r <file.pcap>`: read a **pcap** file.
- `-D`: list available network interfaces.
- `-i <interface | number>`: capture the traffic of a specific interface.
- `-n`: don't resolve hostnames (another `-nn` will not resolve port).
- `-A`: print packet to ASCII.
- `-c`: Exit after receiving count packets.
- `-l`: force line buffered.
- `-w <filename>`: save into a file.
- `<PROTOCOL>`: filter by protocol.
- `dst|src <ip>`: filter by destination|source.
- `port <number>`: filter by port.
- `-s0`: set packet size limit to unlimited. Needed if you want to pull binaries / files from network traffic.
- `-t`: delete all timestamps.
- `-tttt`: time + date.
- `-x`: print in hex (with link level headers type `-xx`).
- `-X`: print ASCII + hex (with link level headers type `-Xx`).

> **link level headers**: include OS layer 2 information like MAC addresses.

Genrally outputs look like:

```
<time(h,m,s,ms)> <protocol> (<port>) <packet information> 
```

## Basic usage

1. To print all IPv4 HTTP packets to and from port 80, i.e. print only packets that contain data, not, for example, SYN and FIN packets and ACK-only packets.

```bash
tcpdump 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
```

2. Capture only packets that match GET:

```bash
tcpdump -s 0 -A -vv 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x47455420'
```

3. Alternatively we can select only on POST requests. Note that the POST data may not be included in the packet captured with this filter. It is likely that a POST request will be split across multiple TCP data packets:

```bash
tcpdump -s 0 -A -vv 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x504f5354'
```

4. Extract HTTP Request URL's:

```bash
tcpdump -s 0 -v -n -l | egrep -i "POST /|GET /|Host:"
```

5. Capture Cookies from Server and from Client:

```bash
tcpdump -nn -A -s0 -l | egrep -i 'Set-Cookie|Host:|Cookie:'
```

6. Capture Start and End Packets of every non-local host:

```bash
tcpdump 'tcp[tcpflags] & (tcp-syn|tcp-fin) != 0 and not src and dst net localnet'
```

7. Capture HTTP data packets:

```bash
tcpdump 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
```

8. Rotate Capture Files:

> When capturing large amounts of traffic or over a long period of time it can be helpful to automatically create new files of a fixed size. This is done using the parameters `-W`, `-G` and `-C`.

In this command the file **capture-(hour).pcap** will be created every (`-G`) **3600 seconds** (1 hour). The files will be overwritten the following day. So you should end up with **capture-{1-24}.pcap**, if the hour was **15** the new file is **(/tmp/capture-15.pcap**).

```bash
tcpdump  -w /tmp/capture-%H.pcap -G 3600 -C 200
```

9. Top Hosts by Packets:

```bash
tcpdump -nnn -t -c 200 | cut -f 1,2,3,4 -d '.' | sort | uniq -c | sort -nr | head -n 20
```

## Resources

To go further check :

- [**TCPDUMP filters cheat sheet**](http://alumni.cs.ucr.edu/~marios/ethereal-tcpdump.pdf).

- [**Tcpdump Examples**](https://hackertarget.com/tcpdump-examples/).

___

# TSHARK

Command line based _(great for automation)_.

## Capturing Packets & Promiscuous mode

> It is not recommended to run `tshark` or **Wireshark** as root because it may end up having some kind of parsing overflow vulnerability, and an attacker would be able to remotely compromised your system by exploiting it.

To change config type `dpkg-reconfigure wireshark-common`.

- `-D`: list available network interfaces.
- `-i <interface | number | any>`: capture the traffic of a specific interface.

> Default behavior of any interface is accept unicast _(destined for it)_, broadcast and chose multicast packets.

> **Promiscuous mode** &rarr; accept all received packets regardless of whether it is destined for you or not.

> **Monitor mode**: applies to 802.11 (Wi-Fi cards & interfaces) only

## Writing and exporting Packet Data

- `-T ek|fields|json|pdml|ps|psml|tabs|text`: set the format of the output when viewing decoded packet data.

> `ek` &rarr; Elastic search

In `/usr/share/wireshark/`, there is a `pdml2html.xsl` file... :smirk: :

`xsltproc /usr/share/wireshark/pdml2html.xsl <your_pdml_file>`

## Capture Filter ≠ Display Filter

**BPF** stands for (**B**erkeley **P**acket **F**ilter).

- `-f` : filters

### Display Filters

Appliyed to alreday <u>captured packets</u>.

- Allow to view a subsection of the captured packets based on a filter expression
- Helps in zeroing down on interesting packets
- Display and Capture filters have different syntax
- Display filter expression are more granular

## Extract fields

- `-e <field>`: add a field to the list of fields to display if `-T` is used. 
- `-Y '<display filter>'`: apply a specific filter. Example:

```bash
tshark -r file.pcap -Y ' wlan.fc.type_subtype == 0x0008 ' -Tfields -e wlan.ssid
```

- `-E <field print option>` : set an option controlling the printing of fields when -T fields is selected.

## Resources

- [tshark tutorial and filter examples](https://hackertarget.com/tshark-tutorial-and-filter-examples/)
- [Syntax](https://wireshark.org/docs/wsug_html_chunked/ChWorkBuildDisplayFilterSection.html)
- [Reference](https://wireshark.org/docs/dfref/)
- [pcap filter](https://wireshark.org/man-pages/pcap-filter.html)
___

# Wireshark

popular and powerful to capture and analyse network data.

```
    Data
      |
  (Frames)
      |
  [Packets]
```

## Resources

- [Wireshark Tutorial and Cheat Sheet](https://hackertarget.com/wireshark-tutorial-and-cheat-sheet/)
