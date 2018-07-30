# SSH and FTP for Remote Administration

SSH -- Secure Shell
FTP -- File Transfer Protocol

## Install SSH
Install SSH on Server = `sudo apt-get install ssh`

## Connecting to Server Using a Terminal Emulator
SSH Requires Port 22
PuTTy -- SSH, Telnet and Rlogin Client

## Installing vsftpd for FTP
Install FTP Server = `sudo apt-get install vsftpd`

## Connecting to vsftpd using a FTP Client
Use a Terminal Emulator to Connect to the Server (PuTTy)
Edit vsftpd Configuration Files = `sudo vim /etc/vsftpd.conf`
Uncoment `#local_enable=YES` to Allow Local Users to Login
Uncomment `#write_enable=YES` to Allow File Uploads

To Restart vsftpd Service = `sudo service vsftpd restart`
Use an FTP Client to Connect to FTP Server (FileZilla)
