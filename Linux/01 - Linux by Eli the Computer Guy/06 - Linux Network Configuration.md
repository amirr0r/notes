# Linux Network Configuration

## ifconfig Command
To Show Current IP Addresses and Network Configuration = sudo ifconfig

## DHCP Releae and Renew
To Release and Renew IP Address = sudo dhclient

## Restarting the Network Service
- To Restart the Networking Service = `sudo /etc/init.d/networking restart`
- Always Restart the Networking Service After Changing Network Configurations
Network Config file

## Editing the interfaces File to Setup a Static IP Address
- To Edit the Network Adapter Configurations = `sudo vim /etc/network/interfaces`
auto eth0 = Auto Negotiate Speed for Ethernet Card 0
- iface eth0 inet static /dhcp = Ethernet Card 0 Either Static or DHCP Address.  - If DHCP Don't Go Further.
```
address 192.168.1.100 = Static IP Address
netmask 255.255.255.0 = Subnet Mssk
network 192.168.1.0 = Network (Generally Your IP Address Siply with a 0 in the Last Octet))
broadcast 192.168.1.255 = Broadcast Address (The Last Address in Your 
Subnet. 
```
Generally Your IP Address with a 255 in the Last Octet.)
gateway 192.168.1.1 = Default Gateway.  Generally Your ISP Modem or Router

## Editing the resolv.conf File for DNS
To Edit the DNS Resolution File = `sudo vim /etc/resolv.conf`

## Changing the Serves Hostname
- To See Current Hostname = `sudo /bin/hostname`
- To Change Hostname = `sudo /bin/hostname newhostname`

## Ping for Troubleshooting
- To Determine if You Can See an IP Address = `ping IP Address`
- To Determine DNS is Working = `ping domainname`

## UFW Firewall Configuration
- To Chaneg Default Handling of Ports When UFW is Enabled = `sudo ufw default allow/deny`
- To Turn UFW on or Off = `sudo ufw enable/disable`
- To Open or Close Ports for Everyone = `sudo ufw allow/deny port#`
- To Delete a UFW Rule = `sudo ufw delete allow/deny port#`
- To Allow Access to All Ports from a Specific IP Address = `sudo ufw allow from IP Address`
- To Allow Access to a Specific Port from a Specific IP Address = `sudo ufw` allow from IP Address to any port port#


Drivers Should Work Out of the Box...
Wireless Networking is Its own Topic
