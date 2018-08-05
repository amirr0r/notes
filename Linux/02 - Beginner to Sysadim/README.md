# [The Linux Basics Course: Beginner to Sysadmin, Step by Step](https://www.youtube.com/playlist?list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK)

- `` :

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


## [Video 26]()

- `tmux list-sessions`
- `tmux new-session -s [session name]`
- `tmux attach -t [session name]`

## [Video 27]()

**archiving** vs **compression**

- archiving : taking many files/folders in a directory and create one logical file above them.
- compression : make a file smaller *(with an algorithm which repeat a
pattern)*


## [Video 30]()

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

## [Video 32]()

Bash Scripting

- `$#` : number of arguments
- `$1` : first argument

## [Video 33]()

Bash condition

- `-n` : not null
- `-z` : zero length

You can use `(())` instead of `[]`.
> With `(())` there are `==, !=, <= ...`


## [Video 34]()

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

## Questions I have

- **What is the usefulness of a symbolic link ? Why use them ?**
