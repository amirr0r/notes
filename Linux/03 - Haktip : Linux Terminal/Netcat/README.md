# Netcat

It first surfaced in 1995, and it is one of the most popular and very lightweight network security tools to date.

**Netcat** lets two computers transfer data with each other via TCP and UDP.

It runs as a client to initiate connections with others computers. It can operates as a server or a "*listener*".

Some common uses for net cat include :
- using it as a chat / messaging server
- file transfers
- port scanning
- banner grabbing (collect information about a :computer: such as the OS, services versions...)

It can be used in Linux, Mac and Windows as well.

# How to Use Netcat To Chat

1. Listener `nc ` :
    + `-l` : listen
    + `-p` : port
2. Client : `nc $ip $port`

# Transfer Files with Netcat

Sender (Windows) :
- `nc -v -w 30 -p 31337 -l < C:\Users\Me\Desktop\test.txt`

Reciever (Linux) :
- `nc -v -w 2 $ip $port > test.txt`

# Using Netcat for Banner Grabbing

```bash
$> nc 10.73.31.1 81
HTTP/1.1 200

...
<address>Apache/2.2.22 ..
...

$> nc www.google.com 80
GET / HTTP/1.1
...
```

- Services Versions :
```bash
$> nc 10.73.31.1 222
SSH-1.99-OpenSSH_5.8
$> nc 10.73.31.9 22
SSH-2.0-OpenSSH_6.1
$> # Have to upgrade the SSH version of the Server 1
```
# Port Scanning in Netcat, Haktip 85

`nc -v -w 1 $ip -z 1-1000` : scan port from 1 to 1000 (`-z` speed up)

# Remote Shells in Windows, HakTip 86

**remote shell:** execute shell commands as another user on another computer.

- Target :
```bash
nc -Lp 31337 -vv -e cmd.exe
```
>  persistant listening mode (always waiting for new entry)

- "Remoter":
```bash
nc $ip $port
```

# Part.2: Remote Shells From Windows into Linux

- Target :
```bash
sudo nc -lp 31337 -e /bin/bash
```
# Cryptcat: Netcat Using Two-Fish Encryption, HakTip 88

**netcat** usage is transmitted in plaintext.

**cryptcat** an another command line tool built on top of **netcat** using [Twofish encryption](https://en.wikipedia.org/wiki/Twofish).

> **remind:** $ip = the IP you've chosen, same thing for the $port

- Listener :
```bash
cryptcat -k $password -l -p $port
```

- Client :
```bash
cryptcat -k $password $ip $port
```

## To verify if its really encrypted

Open `Wireshark` :
- Type `tcp.port == $port` in the `filter bar`

# Making Processes Talk To Each Other, HakTip 89

- Linux :
```bash
tar -cf - Pictures | nc -l -p $port
cat web.jpeg | nc -l -p $port
```
- Windows :
```bash
nc $ip $port | tar -xf -
nc $ip > web.jpeg
```

# Direct Network Traffic with Netcat

