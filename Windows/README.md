# Windows

1. [A bit of history...](#history)
    + [Windows Desktop](#windows-desktop)
    + [Windows Server](#windows-server)
2. [Boot partition directories (`C:\`)](#boot-partition-directories-c)
3. [File systems](#file-systems)
    + [`NTFS` permissions](#ntfs-permissions)
    + [Integrity Control Access Control List (`icacls`)](#integrity-control-access-control-list-icaclshttpsss64comnticaclshtml)
4. [Services](#services)
5. [Sessions](#sessions)
6. [Windows Management Instrumentation (WMI)](#windows-management-instrumentation-wmi)
7. [Security](#security)
    + [SID: Security Identifier](#sid-security-identifier)
    + [Security Accounts Manager (SAM) and Access Control Entries (ACE)](#security-accounts-manager-sam-and-access-control-entries-ace)
    + [UAC: User Account Control](#uac-user-account-control)
    + [AppLocker](#applocker)
    + [Defender (Antivirus)](#defender-antivirus)
- [Useful commands](#useful-commands)
- [Resources](#resources)

___

## A bit of history...

The concept of a graphical user interface (GUI) was introduced in the late 1970s by the Xerox Palo Alto research laboratory. It was added to Apple and Microsoft operating systems to address usability concerns for everyday users that would likely have difficulty navigating the command line. 

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

> Nowadays, systems administrators commonly use GUI-based systems for administering Active Directory, configuring IIS, or interacting with databases.
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

- **Full Control**: allows the user/users/group/groups to set the ownership of the folder, set permission for others, modify, read, write, and execute files.
- **Modify**: read, write, and <u>delete</u> files/folders.
- **List contents** (specific to folders): view and list (sub)folders + executing files.
- **Read and Execute**
- **Write** (specific to folders and automatically set when "Modify" right is checked)
- **Read** 
- **Traverse Folder**: allows or denies the ability to move through folders to reach other files or folders _(even when listing and viewing permissions are not enable)_.

> NTFS set permissions inheritance by default for folders and files.

> It can be manged from File Explorer GUI under the security tab or with the tools `icacls`

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
- `D`: delete access
- `N`: no access
- `M`: modify access
- `RX`: read and execute access
- `R`: read-only access
- `W`: write-only access
- `AD`: append data (add subdirectories)
- `WD`: write data and add files

___

## Services

> (usually) only be created, modified, and deleted by users with administrative privileges
> Misconfigurations around service permissions are a common privilege escalation vector

[Lis of Windows Services](https://en.wikipedia.org/wiki/List_of_Microsoft_Windows_components#Services)

3 categories of services: 

1. **Local** Services
2. **Network** Services
3. **System** Services

> [Critical System Services](https://docs.microsoft.com/en-us/windows/win32/rstmgr/critical-system-services) cannot be stopped and restarted without a system restart

Service Control Manager (**SCM**)

`services.msc` (MMC add-in) provides a GUI interface for interacting with and managing services.

`lsass.exe` (Local Security Authority Subsystem Service) is the process that is responsible for enforcing the security policy.

[`\\live.sysinternals.com\tools`](https://docs.microsoft.com/en-us/sysinternals/): set of portable Windows applications that can be used to administer Windows systems (for the most part without requiring installation) => **Process Explorer** (show which handles and DLL processes are loaded when a program runs), **Task Manager**, and **Process Monitor** 
___

## Sessions

- **Interactive**: initiated by logging directly into the system, requesting secondary logon session using `runas` or via RDP.
- **Non-interactive**: do not require login credentials, used by Windows to automatically start services and applications without requiring user interaction.
    + `NT AUTHORITY\SYSTEM` (Local System Account): most powerful account in Windows systems.
    + `NT AUTHORITY\LocalService` (Local Service Account): similar privileges to a local user account. Limited functionalities and can start some services.
    + `NT AUTHORITY\NetworkService` (Network Service Account):   similar to a standard domain user account. Can establish authenticated sessions for certain network services.

___

## Windows Management Instrumentation (WMI)

**WMI** is a subsystem of PowerShell that provides system administrators with powerful tools for system monitoring. 

> It comes pre-installed since Windows 2000

> Provides a wide variety of uses for both red and blue team. 

`wmic` from cmd.

Some of the common uses:
- set up system logging
- status information for both local and remote systems
- security settings configuration (on remote machines/applications)
- configure user and group permissions
- configure system properties
- executing code
- schedule processes

**WMI** can be leveraged offensively for both enumeration and lateral movement.

___

## Security

> Due to the many built-in applications, features, and layers of settings, Windows systems can be easily misconfigured, thus opening them up to attack even if they are fully patched.

### SID: Security Identifier

SIDs are added to the user's access token to identify all actions that the user is authorized to take.

```
(S)-(revision level)-(identifier-authority)-(subauthority1)-(subauthority2)-(etc)-(relative-id)
            |                   |                  |              |                     |
            ⌄                   |                  |              |                     |
    has always been 1.          |                  ⌄              |                     |
                                | describe user or group's relation with the authority (in what order this authority created the user's account.)
                                ⌄                                 |                     |
                authority (computer/network) that created the SID |                     |               
                                                                  |                     ⌄
                                                                  |  whether this user is a normal user, a guest, an administrator, etc.
                                                                  ⌄
                                              which computer (or domain) created the number
```

### Security Accounts Manager (SAM) and Access Control Entries (ACE)

**SAM** grants rights to a network to execute specific processes.

Access Control Entries (**ACE**) define which user/process/group have which rights.

These ACEs are stored in in Access Control Lists (**ACL**).

### UAC: User Account Control

> <u>Example of UAC:</u> Admin Approval Mode ask for administrator rights in order to allow a software to be installed.

[How User Account Control works](https://docs.microsoft.com/en-us/windows/security/identity-protection/user-account-control/how-user-account-control-works)

> See also: [Windows Registry](https://en.wikipedia.org/wiki/Windows_Registry) and [Run and RunOnce registry keys](https://docs.microsoft.com/en-us/windows/win32/setupapi/run-and-runonce-registry-keys).

### AppLocker

**AppLocker** is Microsoft's application whitelisting solution.

### Group Policy

`gpedit.msc`

### Defender (Antivirus)

> First released as a downloadable anti-spyware tool for Windows XP and Server 2003 then started coming prepackaged as part of the operating system with Windows Vista/Server 2008.

Features:
- **real-time detection** against known threats
- **sample submission** for suspicious file analysis
- **tamper protection**: prevents security settings from being changed through the Registry, PowerShell cmdlets, or group policy)

___

## Useful commands

`dir`, `tree`, `more`, `help <command>` ...

> `<command> /?` another wway to access certains commands help menus

> See [PowerShell equivalents for common Linux/bash commands](https://mathieubuisson.github.io/powershell-linux-bash/)

- **RDP** (Remote Desktop) connection from Linux (port 3389):

```bash
xfreerdp /v:<targetIp> /u:Username /p:Password
```

### Cmdlets

**Cmdlets** are in the form of `Verb-Noun`.

- Open the online help for a cmdlet or function in our web browser:

```powershell
Get-Help <cmdlet-name> -Online 
```

- Download and install help files locally

```powershell
Update-Help
```

- Show the contents of directories:

```powershell
# current directory and all subdirectories
Get-ChildItem -Recurse

# specify directory path
Get-ChildItem -Path C:\Users\Administrator\Documents
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

- List running services: 

```powershell
Get-Service | ? {$_.Status -eq "Running"} |fl # | select -First 2
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

- See the execution policy:

```powershell
Get-ExecutionPolicy -List
```

- Show applications running under the current user while logged in to a system:

```powershell
reg query HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
```

- Check which protection settings are enabled:

```powershell
Get-MpComputerStatus
```

- Instal [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/):

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```


### Aliases

```powershell
get-alias # list all aliases
New-Alias -Name "Show-Files" Get-ChildItem # creating a new alias
```

___

## Resources

- [Command line reference](https://ss64.com/)
- [HTB academy - Windows Fundamentals](https://academy.hackthebox.eu/course/preview/windows-fundamentals)
- [Microsoft Windows version history](https://en.wikipedia.org/wiki/Microsoft_Windows_version_history)
- [From Windows 1 to Windows 10: 29 years of Windows evolution](https://www.theguardian.com/technology/2014/oct/02/from-windows-1-to-windows-10-29-years-of-windows-evolution)
- [Abatchy's tutorials - Windows Kernel Exploitation](https://www.abatchy.com/tag/Kernel%20Exploitation/)
- [Microsoft: Windows Command Reference](https://download.microsoft.com/download/5/8/9/58911986-D4AD-4695-BF63-F734CD4DF8F2/ws-commands.pdf)