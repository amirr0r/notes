# Web

## Common Wordlists

| **Command**   | **Description**   |
| --------------|-------------------|
| `/opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt` | Directory/Page Wordlist |
| `/opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt` | Extensions Wordlist |
| `/opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt` | Domain Wordlist |
| `/opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt` | Parameters Wordlist |

____

## File inclusion

### LFI 

- `....//....//....//....//....//....//....//....//....//....//....//....//....//etc/passwd`

#### LFI to RCE (Poisoning)

- `LFI` to `RCE` by poisoning `User-Agent` and  through Apache / Nginx Log Files.

- Poisoning Sessions files: if the `PHPSESSID` cookie is set to `el4ukv0kqbvoirg7nkp4dncpk3` then it's location on disk would be `/var/lib/php/sessions/sess_el4ukv0kqbvoirg7nkp4dncpk3`.

### RFI

#### Windows

Using `SMB` protocol can help to bypass restrictions applied by `allow_url_include` &rarr; Windows treats files on remote SMB servers as normal files (referenced directly with a UNC path)

> **Tips**: **Impacket**'s `smbserver.py` allows to spin up a SMB server (anonymous authentication by default) &rarr; `smbserver.py -smb2support share $(pwd)`

___


## Directory/Subdomain/Parameter fuzzing/brute-forcing

Using a wordlist and send requests to the target webserver to check if the page/subdomain with the name from our list exists on it.

> Relying on HTTP response code

- Tools: [ffuf](https://github.com/ffuf/ffuf), [wfuzz](https://github.com/xmendez/wfuzz), [gobuster](https://github.com/OJ/gobuster), [dirb](https://tools.kali.org/web-applications/dirb)

To fuzz sub-domains under websites that are not public, we need to perform `vhost Fuzzing`.

> a `vhost` is basically a sub-domain on the same server with the same IP

### `ffuf`

| **Command**   | **Description**   |
| --------------|-------------------|
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ` | Directory Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/indexFUZZ` | Extension Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/directory/FUZZ.php` | Page Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v` | Recursive Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u https://FUZZ.domain.name/` | Subdomain Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u http://domain.name:PORT/ -H 'Host: FUZZ.domain.name' -fs xxx` | VHost Fuzzing |
| `ffuf -w wordlist.txt:FUZZ -u http://admin.domain.name:PORT/index.php?FUZZ=key -fs xxx` | Parameter Fuzzing - GET |
| `ffuf -w wordlist.txt:FUZZ -u http://admin.domain.name:PORT/index.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx` | Parameter Fuzzing - POST |
| `ffuf -w wordlist.txt:FUZZ -u http://admin.domain.name:PORT/index.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx` | Value Fuzzing |  

___

## Resources

- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
- [SecLists](https://github.com/danielmiessler/SecLists)
- [OWASP Top 10 exercises](https://www.tryhackme.com/room/owasptop10)
- [DAMN VULNERABLE WEB APPLICATION (DVWA)](https://github.com/digininja/DVWA)
- [hacker101 videos by **hackerone** _(bug bounty platform)_](https://www.hacker101.com/videos)
- [**Zeecka** - Les Failles Applicatives Web](https://zeecka.fr/GARRIDO_Alex_VeilleTechno_2018.pdf)
- [**John Hammond** - Web Security (CTF WU)](https://www.youtube.com/watch?v=S2mQBXcW3P0&list=PL1H1sBF1VAKX9Mz0UVU2eR7EdGmtb5XJK)