# Users, Groups and Permissions in Linux

This class teaches students how to manage users, groups and permissions in a Linux enviornment.
Topics Covered

## Adding and Deleting Users
- Add User = `sudo adduser username`
- Change User password = `sudo passwd username`
- Delete User = `sudo userdel username`

## Editing the passwd File Which Contains User Configurations
Edit Users Configuration File = `sudo vim /etc/passwd` (shows usernames, names of users, home directories)

## Changing User Passwords
`sudo passwd username`

## Adding and Deleting Groups
- `Sudo groupadd groupname`
- `Sudo groupdel groupname`

## Adding and Deleting Users from Groups

## Editing the group Configuration File
`Sudo vim /etc/group` (shows groups and users)

## Understanding Permission Numbering System
Numbers = owner/group/everyone else
- 4 = read
- 2 = write
- 1 = execute

To Chanege Permissions of a File or Folder = `sudo chmod 777 file/folder` (-R for recursive)

## Changing User and Group Ownership for Files and Folders
To Change User Ownership = sudo chown -R username file/folder
To Change Group Ownership =sudo chgrp --R groupname file/folder
-R for Recursive for Folders"
