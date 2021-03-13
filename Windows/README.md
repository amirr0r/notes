# Windows

1. [A bit of history...](#history)
    + [Windows Desktop](#windows-desktop)
    + [Windows Server](#windows-server)
2. [Boot partition directories (`C:\`)](#boot-partition-directories-c)
3. [File systems](#file-systems)
    + [`NTFS` permissions](#ntfs-permissions)
- [Useful commands](#useful-commands)
- [Useful links](#useful-links)

## History

### Windows Desktop

> **MS-DOS**: **M**icrosoft **D**isk **O**perating **S**ystem is an OS for x86-based personal computers mostly developed by Microsoft (initial release 1981). 

- **1985**: **Microsoft** introduced the **Windows** operating system.

![Windows 1.0](https://upload.wikimedia.org/wikipedia/commons/d/df/Microsoft_Windows_1.0_screenshot.png)

- **1995**: **Windows 95** wwith built-in Internet support _(with IE browwser)_.

![Windows 95](https://i.guim.co.uk/img/static/sys-images/Guardian/Pix/pictures/2014/10/2/1412248985311/1128a943-f6de-4c90-a777-501bebb80be6-620x374.png?width=620&quality=85&auto=format&fit=max&s=72e3f02e034ee176bf4f92fe58294935)


Followed by other version of Windows: **XP** in 2001, **Vista** in 2007, **7** in 2009, **8** in 2012 and the current one **10** in 2015.

### Windows Server

**Windows Server** was first released in **1993** with the release of **Windows NT 3.1** Advanced Server. Over the years, many features were added to improve Windows Server such as Internet Information Services (IIS), various networking protocols, Administrative Wizards.

With the release of **Windows 2000**, Microsoft debuted **Active Directory**, Microsoft Management Console (**MMC**) and supported dynamic disk volumes.

**Windows Server 2003** came next with server roles, a built-in firewall, the Volume Shadow Copy Service, and more.

**Windows Server 2008** included failover clustering, Hyper-V virtualization software, Server Core, Event Viewer, and many other major enhancements.

Followed by several Server versions (2012, 2016 ...) and the latest one **Windows Server 2019** with support for `Kubernetes`, **Linux containers**, and even more advanced security features.

___

As new versions of Windows are introduced, older versions are deprecated and no longer receive Microsoft updates (unless a long-term support contract is purchased in some cases). Currently, only Server 2012 R2 and later are in support. 

However, Microsoft has released out-of-band patches for earlier versions of Windows in the past few years due to the discovery of the critical SMBv1 vulnerability (EternalBlue).

Organizations often find themselves running various older operating systems to support critical applications or due to operational or budgetary concerns. 

> Do not confuse versions and the various misconfigurations and vulnerabilities inherent to each.

___

## Boot partition directories (`C:\`)

Directory               | Description 
------------------------|--------------
`\`                     | Root directory
`C:\`                   | Root directory of C drive (can be another letter)
`C:\Perflogs`           | Performance logs
`C:\Program Files`      | Location of installed programs (only 64-bit programs for 64-bit systems, and only 16-bit plus 32-bit programs for 32-bit systems)
`C:\Program Files (x86)`| Location of 16-bit and 32-bit programs on 64-bit editions of Windows
`C:\ProgramData`        | (Hidden folder) Essential data for certain installed programs.
`C:\Users`              | Users home directories plus folders `Default` and `Public` 
`C:\Users\Default`      | (Hidden folder) Default user profile template for all created users
`C:\Users\Public`       | Sharing folder accessible to all users by default. _This folder is shared over the network by default but requires a valid network account to access._
`C:\Users\<username>\AppData`          | (Hidden folder) Data and settings per user application. 
`C:\Users\<username>\AppData\Local`    | Specific to the computer itself and is never synchronized across the network
`C:\Users\<username>\AppData\LocalLow` | Similar to the `Local` folder, but it has a lower data integrity level
`C:\Users\<username>\AppData\Roaming`  | Machine-independent data that should follow the user's profile,
`C:\Windows`         | Location of majority of the files required for the Windows operating system
`C:\Windows\System`, `C:\Windows\System32`, `C:\Windows\SysWOW64`   | Location of all DLLs required for the core features of Windows. _The operating system searches these folders any time a program asks to load a DLL without specifying an absolute path._
`C:\Windows\WinSxS`             | Windows Component Store contains a copy of all Windows components, updates, and service packs


___

## File systems

Nowadays, there are 3 main types of Windows file systems: 

1. **FAT32** (File Allocation Table) widely used across many types of storage devices (USB, SD cards, HDD)
2. **NTFS** (New Technology File System) which is the default Windows file system
3. **exFAT**

File system | Pros                                                                                                         | Cons
------------|--------------------------------------------------------------------------------------------------------------|---------------------------------
`FAT32`     | **compatibility** between OS and devices (computers, digital cameras, gaming consoles, smartphones, tablets) | <ul><li>File size must be **less than 4GB**</li><li>Need third-party tools for file encryption</li><li>Partitions are limited to a maximum capacity of 8TB</li><li>No built-in data protection or file compression features</li></ul>
`NTFS`      |  <ul><li>**reliability** (can restore the consistency of the file system in case of power loss for instance)</li><li>**journaling** built-in: file modifications (addition, modification, deletion) are logged<li>**security**: possibility to set granular permissions (files + folders)</li></li>very large-sized partitions support</ul> | Not supported natively by most devices (plus, older media devices like TVs and digital cameras do not offer support at all)

### `NTFS` permissions

- **Full Control**
- **Modify**: read, write, and <u>delete</u> files/folders.
- **List contents** (specific to folders): view and list (sub)folders + executing files.
- **Read and Execute**
- **Write** 
- **Read** 
- **Traverse Folder**: allows or denies the ability to move through folders to reach other files or folders _(even when listing and viewing permissions are not enable)_.

> NTFS set permissions inheritance by default for folders and files. 
> It can be manged from File Explorer GUI under the security tab.

### Integrity Control Access Control List ([`icacls`](https://ss64.com/nt/icacls.html))

`icacls` utility allows to manage (list out, add, remove) NTFS permissions on a specific directory.

#### Inheritance settings

- (`CI`): container inherit
- (`OI`): object inherit
- (`IO`): inherit only
- (`NP`): do not propagate inherit
- (`I`): permission inherited from parent container

#### Permissions

- `F`: full access
- `D`:  delete access
- `N`:  no access
- `M`:  modify access
- `RX`:  read and execute access
- `R`:  read-only access
- `W`:  write-only access


___

## Useful commands

> See [PowerShell equivalents for common Linux/bash commands](https://mathieubuisson.github.io/powershell-linux-bash/)

`dir`, `tree`, `more` ...

- RDP (Remote Desktop) connection from Linux:

```bash
xfreerdp /v:<targetIp> /u:Username /p:Password
```

- Get information about the operating system (such as OS version and build number) using `WMI classes`:

```powershell
Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumber
```

- Process listing:

```powershell
Get-WmiObject -Class win32_Process
```

- Service listing:

```powershell
Get-WmiObject -Class win32_Service
```

- BIOS Information:

```powershell
Get-WmiObject -Class Win32_Bios
```

- Start/Stop service on local/remote computers with `GetWmiObject`

- View the permissions set on a directory

```powershell
icacls <directory>
```

- View the permissions set on a directory

```powershell
icacls <directory>
```

- Grant a user full permissions to a directory

```powershell
icacls c:\users /grant username:f:
```

- Remove a users' permissions on a directory:

```powershell
icacls c:\users /remove username
```


___

## Resources

- [Command line reference](https://ss64.com/)
- [HTB academy - Windows Fundamentals](https://academy.hackthebox.eu/course/preview/windows-fundamentals)
- [Microsoft Windows version history](https://en.wikipedia.org/wiki/Microsoft_Windows_version_history)
- [From Windows 1 to Windows 10: 29 years of Windows evolution](https://www.theguardian.com/technology/2014/oct/02/from-windows-1-to-windows-10-29-years-of-windows-evolution)
- [Abatchy's tutorials - Windows Kernel Exploitation](https://www.abatchy.com/tag/Kernel%20Exploitation/)