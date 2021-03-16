# Networks

## Summary

[Introdution](#introduction)
1. [Networking equipment](#)
2. [Networking structures](#)
    + [Types](#)
    + [Topologies](#)
3. [Protocols](#protocols)
4. [Addressing](#addressing)
5. [Web](#web)
6. [Security](#security)

___

## Introduction

We can define a network as a group of computers capable of communicating with each other.

**Internet** can be considered as a network of ... networks ! 

> Indeed, it is a _interconnection_ of many subdivided networks.

> Not to be confused with the web _(or [World Wide Web](https://en.wikipedia.org/wiki/World_Wide_Web))_ which is just websites available over the Internet.

When we interact with Internet, we're constantly sending/receiving **packets**.

Each machine connected to Internet has an **IP address** (which is a kind of a geographical position).

Think of a **router** as a <u>post office</u> and Internet Service Providers (**ISP**) as the main post office. 

![Network overview](https://academy.hackthebox.eu/storage/modules/34/redesigned/net_overview.png)

> In the diagram above, regarding the _Company Network_, the five machine represent a five separate network. 

___

## Networking equipment


Equipment | Definition
----------|-------------
**Router**| |
**Switch**| |


___

## Networking structures

### Types

Network Type 	            | Definition
----------------------------|--------------
Local Area Network (**LAN**)| Internal Networks (Ex: Home or Office)
Wide Area Network (**WAN**) | a large number of LANs joined together (Internet/Intranet for instance)

> **Note**: Generally, in an internal network (LAN), machines are not directly exposed to the Internet. They only have **Private IP Addresses** while the router may have a <u>LAN Address</u> (Private IP) to communicate with them, and a <u>WAN Address</u> (**Public IP**) to communicate with Internet.

### Topologies


___

## Protocols

___

## Adressing

MAC

IP

### Subnetting

#### Mask

> The `/24` network allows computers to talk to each other as long as the first three octets of an IP Address are the same (ex: 192.168.1.xxx)

___

## Web

**HTTP**

Uniform Resource Locator (**URL**)

Domain Name(**DN**) translated into an IP address via **DNS**

___

## Security

Taking the time to map out and document each network's purpose

Firewalls

Intrusion Detection Systems like Suricata or Snort

DMZ (Demilitarized Zone)

**Spoofing**

**Snooping** in on any communication between these devices

eavesdrop

Man In The Middle (MITM)

**DoS** (Denial of Service)

OSPF (Open Shortest Path First) advertisements

> routers should have a trusted network
___

## +Concepts

QoS (Quality of Service: prioritize their traffic to prevent high latency more easily)
___

## Resources

- [**code.org**: How Computers Work?](https://www.youtube.com/playlist?list=PLzdnOPI1iJNcsRwJhvksEo1tJqjIqWbN-) 
- [**code.org**: How The Internet Works?](https://www.youtube.com/playlist?list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7)
- [**Openclassrooms**: Les réseaux de zéro](https://openclassrooms.com/fr/courses/1561696-les-reseaux-de-zero)
- [**HTB Academy**: Introduction to Networking](https://academy.hackthebox.eu/course/preview/introduction-to-networking)
- [COMMENT DÉTRUIRE INTERNET dans le MONDE ?](https://www.youtube.com/watch?v=6hNCEQpjKqE)