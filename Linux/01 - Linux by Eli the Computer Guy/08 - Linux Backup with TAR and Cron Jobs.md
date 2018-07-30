# Linux Backup with TAR and Cron Jobs

This class teaches students how to backup directories using TAR, and demonstrates how to schedule tasks using Cron Jobs.
Topics Covered

## Backing Up Directories with TAR

Backup = `sudo tar --cvpzf backup.taz.gz --exclude/=directory` (recursive) PATH
- `--c` = create new file (overwrites old file)
- `--v` = verbose
- `--p` = preserve permissions
- `--z` = compress
- `--f` = filename (very important)
- `--exclude` = DIRCECTORY is Recursive
- Naming Files with time =  `filename-$(date +%F-%T)`

## Recovering Directories with TAR
Recover Files from a TAR File
- Recover = `sudo tar --xvpzf FILE --C /DIRECTORY`
- Capital `-C` = change to directory
- `-x` = extract

## Setting Up Cron Jobs for Scheduled Tasks
To Edit the Crontab File = `sudo cron --e` (first time it will ask you your default editor)

Format = minute (0-59), hour (0-23, 0 = midnight), day (1-31), month (1-12), weekday (0-6, 0 = Sunday), command

`* Wildcard for Every Minute/Day/Hour/Month?Day of Week`

Example to Backup Entire Server for 1am Every Morning = 
`0 1 * * * sudo tar -cvpzf /backup.tar.gz --exclude=/mnt /"`

