# Navigation in Linux

This class teaches students how to navigate through the file directories in Linux.

## Changing Directories
`CD`

## Listing Contents of Directories
`ls` or `ls -m`

## Searching
`sudo find -iname XXX` (You can use Wild Cards)
`-iname` arguement is to make the search case insensitive

## Making, Moving Copying, Renaming and Deleting Directories
`sudo mkdir folder`
`sudo rm folder` (sudo rm folder --r for directory and trees)
`sudo mv file/folder -- moves/ renames`
`sudo cp souce destination- copy` (add --r for directories)

## Mounting Hard Drives and Flash Drives
Basic Process = Create Directory -- List Attached Drives -- Link Directory to Drive
Make folder to be mounted : `sudo mkdir /mnt/folder` (Create folder in /mnt directory for easier administration)
To list drives attached to computer : `sudo fdisk --l`
To mount a drive : `sudo mount drive folder`

## Mounting the CDROM
Mount cdrom : `sudo mount /dev/cdrom  /folder`
To unmount drive : `sudo umount drive`
Use /mnt/subfolder for exclusions later
