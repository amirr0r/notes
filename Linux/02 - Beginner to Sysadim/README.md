# [The Linux Basics Course: Beginner to Sysadmin, Step by Step](https://www.youtube.com/playlist?list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK)

## [Video 5](https://www.youtube.com/watch?v=ZeZVkA1zjWg&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=5)

- `Ctrl + e` : (end of line)
- `Ctrl + a`  : (beginning of line)

> Check if it's `Cmd` or `Ctrl` on MAC

- `poweroff` = `shutdown -h now`

## [Video 6](https://www.youtube.com/watch?v=EbOEqycEFeM&index=6&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK)

- `w` : who is logged on / what they are doing
- `netstat` : displays various network related information such network connections, routing tables, interfaces...
    + `-tupln` : **t**cp / **u**dp ports + **p**rogramms that's attached to that port +  **l**istening ports + **n**umerical addresses

## [Video 13](https://www.youtube.com/watch?v=UN1QB5BIvps&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=13)

In **/etc/shadow**, replace the encrypted password by a `!` and it will lock the account / command.

## [Video 19](https://www.youtube.com/watch?v=svh8sSuz5BI&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=19)

`man hier` : description of the filesystem hierarchy

## [Video 23](https://www.youtube.com/watch?v=kXWZz8RcW4o&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=23)

Read this first:
http://catb.org/~esr/faqs/smart-quest...

Stack Exchange Sites:
- http://serverfault.com/
- http://unix.stackexchange.com/
- http://stackoverflow.com/

RFCs
- http://www.ietf.org/rfc.html
- http://www.faqs.org/rfcs/

Manpages
- https://www.kernel.org/doc/man-pages/
- http://linux.die.net/man/


## [Video 26](https://www.youtube.com/watch?v=norO25P7xHg&t=0s&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=27)

- `tmux list-sessions`
- `tmux new-session -s [session name]`
- `tmux attach -t [session name]`

## [Video 27](https://www.youtube.com/watch?v=tSRlNwaUgPQ&t=0s&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=27)

**archiving** vs **compression**

- archiving : taking many files/folders in a directory and create one logical file above them.
- compression : make a file smaller *(with an algorithm which repeat a
pattern)*


## [Video 30](https://www.youtube.com/watch?v=MYWvVgIL_Ys&t=0s&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=30)

```bash
$> myvariable='hey'
$> echo $myvariable
hey
$> echo "myvariable=$myvariable" # Single quotes doesn't work
myvariable=hey
$> echo "myvariable=$myvariableyO"
myvariable=
$> echo "myvariable=${myvariable}O"
myvariable=heyO
$> echo "there is `ls | wc -l` file(s) in this folder" # Single quotes doesn't work
there is 4 file(s) in this folder
$>
```

## [Video 32](https://www.youtube.com/watch?v=Vbu8rfVaABw&t=0s&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=32)

Bash Scripting

- `$#` : number of arguments
- `$1` : first argument

## [Video 33](https://www.youtube.com/watch?v=VMZBFjYgjR4&t=0s&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=33)

Bash condition

- `-n` : not null
- `-z` : zero length

You can use `(())` instead of `[]`.
> With `(())` there are `==, !=, <= ...`


## [Video 34](https://www.youtube.com/watch?v=9EfN5clA710&t=0s&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=34)

- `$@` : all arguments

Define function - Method 1 :
```bash
function_name() {

}
```

Define function - Method 2 :
```bash
function function_name() {

}
```

## [Video 51](https://www.youtube.com/watch?v=9VkswePNh80&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=51)

ISP only sees your VPN Server vs ISP sees all sites you visit.

When you connect to a VPN and tunnel all your traffic through it before hitting that same web server it's like using a courier to deliver a message instead. 

The recipients never sees you, they only meet the courier that's delivering  your message. 

In Networking this is called **proxy**, because you're *proxying* through the courier in the same way you're *proxying* through a VPN.

When you use a VPN every server you connect to on the Internet sees your traffic as coming from the IP of the VPN server you're using, not the IP that you actually have sitting at home (*the one that your ISP give you*).

### Limitations

Despite occasional marketing claims to the contrary, **a VPN can't totally prtocet your privacy and it certainly does make you anonymous online.**

Tracking technos like flash cookies, browser fingerprinting, various taffic correlation techniques (*transparent DNS proxies for instance*) still allow third-party advertisers to spy on some of your traffic and foil your attempts at anonymity.

But it doesn't meen that VPN's are useless !

### Tips

1. You must tunnel DNS through the VPN. 
2. Check the history / country of the company => strong privacys laws.
3. Good VPN will cost money.

He's using Mullvad.

## [Video 53](https://www.youtube.com/watch?v=V8EUdia_kOE)

Shell Tricks

- `sudo !!` : run the previous command with sudo
- `Ctrl + k` : Cut/Kill to the end of the line
- `Ctrl + u` : Cut/Kill to the beginning of the line
- `Ctrl + w` : Cut/Kill a word backwards
- `Ctrl + u + Ctrl + y` : Cut/KIll & Paste
- `less +F` : load the file from the end + `Ctrl + c` or scroll to navigate or just `less` and then `Shift + F`
- `Ctrl + x + e` : open a buffer in your $EDITOR
- `Alt + .` : give the arguments from the previous command

## [Video 54](https://www.youtube.com/watch?v=tweyWNr6X18&list=PLtK75qxsQaMIlFCcFZpTBLnaCJ0I0uiaY&index=10&t=0s)

- `script` : record and replay shell sessions

## [SSH Basics](https://www.youtube.com/playlist?list=PLtK75qxsQaMII75AbcuIruao1k2qdxwjg)

Configure your own SSH server . Install `openssh-server` (Ubuntu). The config file is located at `/etc/ssh/sshd_config`.

## [LXC](https://www.youtube.com/playlist?list=PLtK75qxsQaMLwF_uCB_CK8wIE17D-afuJ)



- `` :
## Questions I have

- **What is the usefulness of a symbolic link ? Why use them ?**
