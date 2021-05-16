# SMB

**SMB** stands for Server Message Block Protocol.

It's mainly to share resources (files, printers, etc.) over a network.

Clients and servers are communicating via TCP/IP (more precisely via **NetBIOS** over TCP/IP).

> Since Windows 95, Microsoft provides a client and a server for the **SMB** protocol. 

It is supported on Unix Systems via `Samba` (open source).

`smbclient` is part of the default `Samba` suite:
- `smbclient //[IP]/[SHARE] -U <username%password> -P <port> `

## Useful links / Resources

- [RFC1001](https://datatracker.ietf.org/doc/html/rfc1001)
- [RFC1002](https://datatracker.ietf.org/doc/html/rfc1002)