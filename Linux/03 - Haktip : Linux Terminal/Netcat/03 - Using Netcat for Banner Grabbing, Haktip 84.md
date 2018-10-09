# Netcat 101: Using Netcat for Banner Grabbing, Haktip 84

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