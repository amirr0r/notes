# Linux Terminal 101

## [0 - Getting Started](https://www.youtube.com/watch?v=b5NmtmNwMgU&t=0s&index=2&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `date`
- `cal` : calendar
- `free` : show the current free memory amount1
- `cd ~username` : go to `/home/username`

## [4 - type, which, and apropos](https://www.youtube.com/watch?v=CQvkF4LHY58&t=0s&index=6&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)
 
There is 8 section for man pages.
- `whatis` = `man -f`
- `info` : give more informations about a specific command.

## [7 - Redirecting Standard Terminal Errors in Linux](https://www.youtube.com/watch?v=faTXIBdZt-0&t=0s&index=9&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `stdin` < 0
- `stdout` < 1
- `stderr` < 2
- `&` : both `stdout` + `stderr`

## [10 - How to use echo](https://www.youtube.com/watch?v=FYJVlTQYvNY&t=0s&index=12&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `echo ~` : output `/home/username`
- `echo $((2 + 2))` :  output 4

## [11 - Using Expansions Commands in the Linux Terminal Part 2](https://www.youtube.com/watch?v=cQHua6LPLyc&t=0s&index=13&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `echo $(ls)` :  output the result of `ls`
- `find $(ls /usr/bin* | grep zip)` or for example : 
```bash
ls -l `which cp` 
```
## [13 - Your Favorite Tips and Tricks](https://www.youtube.com/watch?v=klP_Ik9NN7o&t=0s&index=15&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- to unalias --> put `\` before the command

## [14 - My Top Best Resources](https://www.youtube.com/watch?v=HT9I6nlsXzM&t=0s&index=16&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- [commandlinefu.com](https://www.commandlinefu.com/commands/browse)
- [unixref.pdf](https://files.fosswire.com/2007/08/fwunixref.pdf)

## [15 - Typing Less with Keyboard Shortcuts](https://www.youtube.com/watch?v=b2l-Zz_m-W0&t=0s&index=17&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)
> **Remind** : try on another terminal

## [16 - How to Use History](https://www.youtube.com/watch?v=zNelCx81TJA&t=0s&index=18&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `!1` : run the first command of `history`

## [18 - How to Change Your Identity](https://www.youtube.com/watch?v=lQ4ycCA7FOQ&t=0s&index=20&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)
- `id`
- `sudo -l` : show you how you can currently do

## [20 - Controlling Processes](https://www.youtube.com/watch?v=XUhGdORXL54&t=0s&index=22&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)
- `pstree` : display a tree of processes
- `vmstat` : report virtual memory statistics
- `tload` : graphic representation of system load average

## [22 - Viewer Tips Part 2!](https://www.youtube.com/watch?v=moOmrFc6J50&t=0s&index=24&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `uname -r` : Kernel version
- `cat /etc/*release` : Distro version

- [Learn Linux The Hard Way](https://archive.is/xDb8o)

# Linux Terminal 201

## [25 - Customize The Shell Prompt](https://www.youtube.com/watch?v=_kSCpNqKJbM&t=0s&index=27&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)
> Nice to know ^_^

## [29 - Networking Commands You Should Know!](https://www.youtube.com/watch?v=F1geJWP4Yvs&t=0s&index=31&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `netstat -ie` :  display your current devices networks status and setting, similar to `ifconfig -a`

## [30 - Networking Commands You Should Know Pt 2!](https://www.youtube.com/watch?v=RrmWU_Hg9e4&t=0s&index=32&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `ifconfig wlp58s0 promisc`
- `ifconfig wlp58s0 -promisc`
- `route`

## [31 - ifconfig vs ip](https://www.youtube.com/watch?v=XiERevpBTAk&t=0s&index=33&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

`ip` is considered to be much  more powerful than `ifconfig` 
- [ip command CHAT SHEET (redhat)](https://access.redhat.com/sites/default/files/attachments/rh_ip_command_cheatsheet_1214_jcs_print.pdf)

## [32 - How To Use Man Pages](https://www.youtube.com/watch?v=BWLSqZZfKc4&t=0s&index=34&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)
- `f` : move forward
- `b` : move backward
- `man -u <command>` : update

## [36 - Grep and Metacharacters](https://www.youtube.com/watch?v=xXo1L28Jc6A&t=0s&index=38&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `grep -E` = `egrep`

## [38 - What is POSIX in Unix?](https://www.youtube.com/watch?v=U0GbJtnfqSM&t=0s&index=40&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

**P**ortable **O**perating **S**ystem **I**nterface or **POSIX** is a family of standards specified by the IEEE for maintaining compatibility between OS.

- [regular-expressions.info](https://www.regular-expressions.info/)

## [40 - Using the Find Command Pt 2](https://www.youtube.com/watch?v=WuZYqiais54&t=0s&index=42&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `-a` : and
- `-o` : or
- `!` : not
- `\( \)` : parenthèses
- `-exec` : execute a command

## [41 - Monitoring System Resources Pt 1](https://www.youtube.com/watch?v=xcR_FjAy1HI&t=0s&index=43&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `ps auxf` : tree view
- `lsof` : list open files

## [42 - Monitoring System Resources Pt 2](https://www.youtube.com/watch?v=fwMTD9ghC3c&t=0s&index=44&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `dpkg -l linux-image*`

## [45 - How to Use Cut, Paste, and Join](https://www.youtube.com/watch?v=k014CkDmB2A&t=0s&index=47&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `paste` : merge lines of files
- `join` : join lines of two files on a common field

## [46 - How to Use Comm, Diff, and Patch](https://www.youtube.com/watch?v=yIU92YySlW0&t=0s&index=48&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `comm` - compare two sorted files line by line
- `patch` - apply a diff file to an original

## [47 - How to Use tr, sed, and aspell](https://www.youtube.com/watch?v=F7Brrn-L1Zg&t=0s&index=49&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `aspell` : Interactive spell checker.

## [48 - How to Use nl, fold, and fmt](https://www.youtube.com/watch?v=BksWPx-C8rg&t=0s&index=50&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `nl` : print line number
- `fold` : Wraps each line in an input file to fit a specified width and prints it to the standard output.
- `fmt` : format command

## [49 - How to Use pr and printf](https://www.youtube.com/watch?v=aLypSBf1TBY&t=0s&index=51&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)

- `pr` : paginate text
- `printf` : print format

## [50 - Document Formatting with Groff](https://www.youtube.com/watch?v=NW5ZWN2b4zw&t=0s&index=52&list=PLW5y1tjAOzI2ZYTlMdGzCV8AJuoqW5lKB)
- `groff` : Typesetting program that reads plain text mixed with formatting commands and produces formatted output. It is the GNU replacement for the troff and nroff Unix commands for text formatting.
> Nice to know ^_^