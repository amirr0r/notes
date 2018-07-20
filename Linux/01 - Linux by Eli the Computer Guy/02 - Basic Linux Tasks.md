# Basic Linux Tasks

## Sudo (Super User Do)
Sudo is used before a command.  It acts like \"run as administrator\" in Windows

# Man Pages
Man pages are reference pages for commands and programs
Example of usage: man ping
q quits man pages

# Installing software using tasksel
The tasksel command installs numerous programs at one time to create a server (LAMP or such)
Example: sudo tasksel

# Installing software using apt-get
Apt-get Installs, Removes or Updates Software
Install: sudo apt-get install xxx
Uninstall: sudo apt-get remove xxx
Update: Sudo apt-get upgrade

# Restarting services
Restart Services
Example: sudo  /etc/init.d/xxx (restart start stop)

# the top command
Top is like \"Task Manager\" in Windows
Example: sudo top
K to kill a process
H for help

# Basic navigation
To change to another directory use the cd command
Example: cd /etc/var/www (changes to web root folder)
Linux is literal
To Logout just use the exit command
