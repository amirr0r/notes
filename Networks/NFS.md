# NFS

**NFS** stands for Network File System.

The NFS protocol allows to share files over the network between computers running different operating systems.

Clients can request to mount a directory from a remote host on a local directory. Obviously the server will check if they have the right permissions (i.e read and write of files).

NFS uses file handle to represent files and directories on the server.

It relies on the **RPC** protocol.

`nfs-common` is a tool to interact wit any NFS share.

We can mount a remote share on our local machine by doing so:

```bash
sudo mount -t nfs IP:share /mnt/whatever/ -nolock
```

## Root-Squashing

**Root Squashing** prevents having root access to the NFS volume when connecting to the NFS share. 
Users become `nfsnobody` when connected.
Root Squashing is enabled by default on NFS shares.

If this is not enabled, users can create **SUID** files.

## Resources / Useful links

- <https://docs.oracle.com/cd/E19683-01/816-4882/6mb2ipq7l/index.html>
- <https://wiki.archlinux.org/title/NFS>