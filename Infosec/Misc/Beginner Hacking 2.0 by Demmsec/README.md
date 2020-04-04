## [Video 3 : NMAP Usage](https://www.youtube.com/watch?v=byq8x6jlhV4&list=PLbjcIaeZ3pVPOO9rGzkoU8dgspzPx8n0x&index=3)

```bash
nmap -iL <filename> 	# read & scan list of IP targets
nmap -oA <filename> $IP # save scan results into 3 files : <filename>.(xml | nmap | gnmap)
nmap -p <port-range> $IP
nmap -sU $IP 		# UDP scan
nmap -p- $IP		# scan all port
nmap -A $IP		# OS detection (-O), version scanning (-sV), script scanning (-sC) and traceroute (--traceroute)
```



