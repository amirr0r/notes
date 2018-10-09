# Netcat 101: Using Netcat to Transfer Files, Haktip 83

Sender (Windows) :
- `nc -v -w 30 -p 31337 -l < C:\Users\Me\Desktop\test.txt`

Reciever (Linux) :
- `nc -v -w 2 $ip $port > test.txt`
