# Networks

## Summary

[Introdution](#introduction)

1. [Equipments](#1-networking-equipment) (router, switch...)
2. [Structures](#2-networking-structures)
    + [Types](#21-types) (LAN/WAN, VPN/Proxy...)
    + [Topologies](#22-topologies) (Mesh, Tree, Star, Ring...)
3. [Protocols](#3-protocols) (TCP/UDP/IP...)
4. [Models](#4-models) (OSI, TCP/IP)
5. [Addressing](#5-addressing) (MAC, IPv4/IPv6 ...)
6. [Web](#6-web)
7. [Security](#7-security)
8. [+Concepts](#8-concepts)

[Resources](#resources)

___

## Introduction

We can define a network as <u>a group of computers capable of communicating with each other</u>.

**Internet** can be considered as a <u>network of networks</u>! 

> Indeed, it is a _interconnection_ of many subdivided networks.

> Not to be confused with the web _(or [World Wide Web](https://en.wikipedia.org/wiki/World_Wide_Web))_ which is just websites available over the Internet.

When we interact with Internet, we're constantly sending/receiving **packets**.

Each machine connected to Internet has an **IP address** (which is a kind of a geographical position).

Think of a **router** as a <u>post office</u> and Internet Service Providers (**ISP**) as the main post office. 

![Network overview](https://academy.hackthebox.eu/storage/modules/34/redesigned/net_overview.png)

> In the diagram above, regarding the _Company Network_, the five machine represent five separate networks. 

___

## 1. Networking equipment

To enable communication, we need mediums (ethernet/fiber/coax/wireless...) and equipments:

> Crossover cables are used to directly connect two computers while straight cables are used to connect a computer to another device such as a hub or a switch.

Equipment                                  | Image | Definition     |
-------------------------------------------|-------|----------------|
**Router**/Modem                           | ![](img/router.jpg) | allows communication between different networks (exemple: local network and Internet) |
**Switch**                                 | ![](img/switch.jpg) | allows to link several computers together. By relying on their physical addresses (MAC addresses), the switch transmits to each machine only what is addressed to it (unlike the **Hub**) |
Network interface controller/card (**NIC**)| ![](img/nic.jpg) | deals with everything related to network communication |

MAC are physical addresses while IP are logical addresses. These notions will be discussed in more detail later.

> Of course, there are also repeaters.

> _A router does not necessarily have antennas as in the image below._

**Wired connections**: Coaxial cabling, Glass fiber cabling, Twisted-pair cabling etc.

**Wireless connections**: Wi-Fi, Cellular (3G/4G/5G), Satellite etc.

___

## 2. Networking structures

### 2.1. Types

Network Type 	            | Definition
----------------------------|--------------
Local Area Network (`LAN`)  | Internal Networks (Ex: Home or Office)
Wide Area Network (`WAN`)   | a large number of LANs joined together (Internet/Intranet for instance)

**Note**: Generally, in an internal network (LAN), machines are not directly exposed to the Internet. They only have **Private IP Addresses** while the router may have a <u>LAN Address</u> (Private IP) to communicate with them, and a <u>WAN Address</u> (**Public IP**) to communicate with Internet. These notions will be discussed in more detail later.

> **WLAN** is the acronym of Wireless Local Area Network.

> _In books, we can come across with terms like Global Area Network (`GAN`), Metropolitan Area Network (`MAN`), Personal Area Network (`PAN`) and its wireless variant (`WPAN`). However, they are more academical terms than operational terms)_.

> A way to identify if the network is a WAN is to use a WAN Specific routing protocol such as `BGP` and if the IP Schema in use is not within **RFC 1918** (`10.0.0.0/8`, `176.16.10.0/10`, `192.168.0.0/16`).

#### VPN

- **VPN** (Virtual Private Networks) is just a way to access to another LAN (like you were plugged into it), although you are connected to a primary network.

There 3 main types of VPNs:

1. **Site-To-Site**: (most commonly used to join company networks) both the client and server are Network Devices and share entire network ranges..
2. **Remote access VPN**: client's computer create a virtual interface that behaves as if it is on a client's network (`tun0` on Linux for example).
3. **SSL VPN**: stream applications or entire desktop sessions within the web browser.

A VPN can be used to obfuscate/"hide" your location, because your IP address used for web requests will be located in the other LAN you are connected to.

#### Proxy

- A **proxy** is when a device or service sits in the middle of a connection and acts as a <u>mediator</u> (inspects traffic's content). A proxy does not necessary change your IP address!

> Without the ability to be a mediator, the device is technically not a proxy, just a **gateway**.

Key types of proxy services:

Type of proxy                     | Image          | Definition
--------------------|----------------|-----------
`Forward Proxy`     | ![](img/forward_proxy.png) | client makes a request to the proxy, and that proxy carries out the request. (Mainly used in companies networks)|
`Reverse Proxy`     | ![](img/reverse_proxy.png) | The reverse of a `Forward Proxy`, instead of being designed to filter outgoing requests, it filters incoming ones. (Examples: `CloudFlare`, `ModSecurity`) |

> **Note**: Malwares that want to bypass `forward proxy` need to be `proxy aware` or use a non-traditional `C2` (a way to receive tasking information) like `DNS`. On Windows, web browsers like Internet Explorer, Edge, or Chrome all obey the "System Proxy" settings by default. If the malware utilizes `WinSock` (Native Windows API), it will likely be `proxy aware` <u>without any additional code</u>. Firefox rely on `libcurl` instead (enables to use the same code on any OS). In that case, in order to be `proxy aware`, malwares would need to look for Firefox and pull the proxy settings.

>  Monitoring DNS can be done with [`Sysmon`](https://medium.com/falconforce/sysmon-11-dns-improvements-and-filedelete-events-7a74f17ca842)

Proxies act either **non-transparently** (communication partner) or **transparently** (client doesn't know about its existence).


### 2.2. Topologies

A **topology** corresponds to the way devices in a network are physically and/or logically connected to each other.

It determines the access methods to the transmission media.

Topology            | Image                         | Description
--------------------|-------------------------------|--------------
**Point-to-Point**  | ![](img/topo_p2p.png)         | Direct physical link exists between two hosts |
**Bus**             | ![](img/topo_bus.png)         | All hosts are connected via a transmission medium in the bus topology. Since the medium is shared with all the others, only one host can send, and all the others can only receive and evaluate the data and see whether it is intended for itself. (Exemple: coaxial cable) |
**Star**            | ![](img/topo_star.png)        | Each host is connected to the <u>central network component</u> via a separate link |
**Ring**            | ![](img/topo_ring.png)        | Each host is connected to the ring with two cables (one for the incoming traffic, one for the outgoing). _The logical ring topology works with a `token`._ |
**Mesh**            | ![](img/topo_mesh.png)        | Each host is connected to every other host in the network in a **fully meshed structure**. In the **partially meshed structure**, the endpoints are connected by only one connection. |
**Tree**            | ![](img/topo_tree.png)        | Extended star topology |
**Daisy Chain**     | ![](img/topo_daisy-chain.png) | (often found in automation technology like `CAN`) Multiple hosts are connected by placing a cable from one node to another. The signal is sent to and from a component via its previous nodes to the computer system |
**Hybrid**          | ![](img/topo_hybrid.png)      | Hybrids of two or more of the basic topologies mentioned above. _Internet has an hybrid topology,_|

> Do not confuse Point-to-Point and `P2P` (**Peer-to-Peer** architecture)

> Check [spanning tree](https://en.wikipedia.org/wiki/Spanning_Tree_Protocol)

___

## 3. Protocols

A **protocol** corresponds to a set of rules for communication between hosts.

One of the most important protocols used in Internet communication is simply call `IP` (**I**nternet **P**rotocol).

Each **packet** has the IP (internet address) of **where it came from and where it's going** !

`TCP` or Transmission Control Protocol, manages the sending and receiving of all you data as packets.

Unlike `UDP` (User Datagram Protocol) that doesn't support acknowledgment of receipt.

The Internet is entirely based on the TCP/IP protocol family.

A protocol that you are probably familiar with is `HTTP` (Hypertext Transfer Protocol) which is used for websites.

As you may guessed, the WEB isn't the only way to communicate though Internet, there are a lot of protocols for a lot of different uses such as `FTP` for File Transfer Protocol.

>  Some IoT (Internet of Things) specific protocols: `Insteon`, `Z-Wave`, and `ZigBee`.
___

## 4. Models

`OSI` and `TCP/IP` models both describe packets workflow. 

They are representation of the **layers** through which the data goes when it is transferred from host to another.

![](img/net_models.png)

> `OSI` stands for **O**pen **S**ystems **I**nterconnection. This model was published by published by the International Telecommunication Union (**ITU**) and the International Organization for Standardization (**ISO**).

When a Packet is transferred, it is processed layer by layer. Each layer is adding its **headers** and perform its assigned functions.

This is called [encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(networking)).

The receiver reverses the process and unpacks the data on each layer with the header information.

![](img/packet_transfer.png)

### OSI model

Layer           | Description
----------------|------------
`1.Physical`    | **Bitstreams** transmission takes place on **wired** or **wireless** transmission lines: electrical signals, optical signals, or electromagnetic waves. 
`2.Data Link`   | Enables reliable and error-free transmissions on the respective medium. Bitstreams are divided into blocks or **frames**.
`3.Network`     | Connections are established in circuit-switched networks, and data **packets** are forwarded in packet-switched networks.
`4.Transport`   | End-to-end control of the transferred data. It can detect and avoid _congestion_ situations. Packets become **segments** / **datagrams**.
`5.Session`     | Controls the logical connection between two systems and prevents, connection breakdowns or other problems.
`6.Presentation`| Transfers the system-dependent presentation of data into a form independent of the application.
`7.Application` | Controls the input and output of data and provides the application functions (among other things).

### TCP/IP model


Layer            | Description
---------------- | -------
`1.Link`         | Responsible for placing the TCP/IP packets on the network medium and receiving corresponding packets from the network medium. It is designed to work independently of the network access method, frame format, and medium.
`2.Internet`     | Responsible for host addressing, packaging, and routing functions.
`3.Transport`    | Provides (TCP) session and (UDP) datagram services for the Application Layer.
`4.Application`  | Allows applications to access the other layers' services and defines the protocols applications use to exchange data.

___

## 5. Addressing

### MAC

> `MAC` stands for Media Access Control.

- **MAC address**: unique identifier of the hardware.

It is composed of 48-bit/6 bytes represented in hexadecimal format. Example: `00:C4:B5:45:B2:43`

### IP

- **IP address**: variable identifier of a machine on a network. (IPv4 example: `192.168.1.241/24`; IPv6 example: `fe80::2f7c:73ff:4397:121f/64`)

Just like a home address has a country, a city, a street, and a house number, an IP address has many parts:

- Earlier numbers usually identify the country and regional network of the device.
- Then come the subnetwork.
- And finally the address of the specific host on this subnetwwork.

#### IPv4

The **IPv4** version which provides more than 4 billion unique adresses. It uses 32-bit/4 bytes, each is an number ranging from 0-255.

The IP address is divided into a `host part` and a `network part`. 

The process of dividing a network into small networks is called **subnetting**.

A **netmask** (which is as long as an IPv4 address) to perform this mathematical operation.

IPs are distinguished between **public** vs **private** classes (RFC 1918, `10.0.0.0/8`, `176.16.10.0/10`, `192.168.0.0/16`):

Most of the time, the all _first IPv4 address_ of the subnets is used for the default **gateway**, and the all _last IPv4 address_ of the subnets is used for **broadcast**.

> Broadcast in a network is a message that is transmitted simultaneously to all participants of a network and does not require any response.

> `+ 2` indicates the broadcast address and the gateway address.

Classless Inter-Domain Routing (`CIDR`) is a suffix that indicates how many bits from the beginning of the IPv4 address belong to the network.

- The `/24` network allows computers to talk to each other as long as the first three octets (8 + 8 + 8 = 24) of an IP Address are the same (ex: 192.168.1.xxx)

Class  | Network Address | First Address  | Last Address      | Subnetmask      | CIDR      | Subnets       | IPs
------ | --------------- | -------------- | ----------------- | --------------- | --------- | --------------| -------
**A**  | `1.0.0.0`       | `1.0.0.1`      | `127.255.255.255` | `255.0.0.0`     | `/8`      | **127**       | 16,777,214 + 2
**B**  | `128.0.0.0`     | `128.0.0.1`    | `191.255.255.255` | `255.255.0.0`   | `/16`     | **16,384**    | 65,534 + 2
**C**  | `192.0.0.0`     | `192.0.0.1`    | `223.255.255.255` | `255.255.255.0` | `/24`     | **2,097,152** | 254 + 2
**D**  | `224.0.0.0`     | `224.0.0.1`    | `239.255.255.255` | Multicast       | Multicast | Multicast     | Multicast
**E**  | `240.0.0.0`     | `240.0.0.1`    | `255.255.255.255` | reserved        | reserved  | reserved      | reserved

- [Subnet Calculator for IPV4](https://www.site24x7.com/tools/ipv4-subnetcalculator.html)

#### IPv6

**IPv6** has recently appeared to fill the void of available addresses. It uses 128-bit/16 bytes per address and provides over 340 undecillion unique addresses.

___

## 6. Web

**HTTP**

Uniform Resource Locator (**URL**)

Domain Name(**DN**) translated into an IP address via **DNS**

___

## 7. Security

Taking the time to map out and document each network's purpose

**Firewalls**

**Web Application Firewalls** (`WAF`) inspect web requests for malicious content and block the request if it is malicious. _(Read: [OWASP ModSecurity Core Rule Set](https://owasp.org/www-project-modsecurity-core-rule-set/) to get started)_

**Intrusion Detection Systems** (`IDS`) like `Suricata` or `Snort`

**DMZ** (Demilitarized Zone)

**Spoofing**

**Snooping** in on any communication between these devices

eavesdrop

**Man In The Middle** (MITM)

**DoS** (Denial of Service)

OSPF (Open Shortest Path First) advertisements

> routers should have a trusted network
___

## 8. +Concepts

QoS (Quality of Service: prioritize their traffic to prevent high latency more easily)
___

## Resources

- [**code.org**: How Computers Work?](https://www.youtube.com/playlist?list=PLzdnOPI1iJNcsRwJhvksEo1tJqjIqWbN-) 
- [**code.org**: How The Internet Works?](https://www.youtube.com/playlist?list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7)
- [**Openclassrooms**: Les réseaux de zéro](https://openclassrooms.com/fr/courses/1561696-les-reseaux-de-zero)
- [**HTB Academy**: Introduction to Networking](https://academy.hackthebox.eu/course/preview/introduction-to-networking)
- [COMMENT DÉTRUIRE INTERNET dans le MONDE ?](https://www.youtube.com/watch?v=6hNCEQpjKqE)