# Web

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

## Resources

- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
- [OWASP Top 10 exercises](https://www.tryhackme.com/room/owasptop10)
- [DAMN VULNERABLE WEB APPLICATION (DVWA)](https://github.com/digininja/DVWA)
- [hacker101 videos by **hackerone** _(bug bounty platform)_](https://www.hacker101.com/videos)
- [**Zeecka** - Les Failles Applicatives Web](https://zeecka.fr/GARRIDO_Alex_VeilleTechno_2018.pdf)
- [**John Hammond** - Web Security (CTF WU)](https://www.youtube.com/watch?v=S2mQBXcW3P0&list=PL1H1sBF1VAKX9Mz0UVU2eR7EdGmtb5XJK)