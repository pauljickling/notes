# Useful Terminal Commands

## Directory Traversal

**Change Directory**

`cd`

To go back a directory:

`cd..`

To go back two directories:

`cd../..`

etc.

To change to the directory you were last in:

`cd -`

**Make Directory**

`mkdir {directory name}`

**Remove Directory**

`rm -rf {directory name}`

Careful not to `rm -rf /` which would delete the root directory!

## File Manipulation

**Open File**

`xdg-open {file name}`

**Copy File**

`cp {file source} {file destination}`

**Remove File**

`rm {file name}`

**Move File**

`mv {file source} {file destination}`

Note that if you use the same directory you can use the move command to rename a file.

**Convert File**

`convert {file name} {new file name}`

**Create Empty File**

`touch {file name}`

**Open Terminal Text Editor**

`vi {file name}`

**Concatinate Files**

`cat {file1} {file2}`

If you only provide one file as an argument this is a useful way to view a file as well.

**Read Files**

`less {file name}`

**Produce Word Count of File**

`wc {file name}`

**Get Memory Size of Files/Directories in Bytes**

`du -h`

**Search Text of a File**

`grep {phrase} {file name}`

**Recursively Search a Folder**

`grep -r {phrase} {directory name}`

**Redirecting Data Stream to a File**

`{data stream} > {destination}`

Note that the destination file will be cleared. If you want to append data, use `>>`

## Ownership Methods

**Get Id Info**

`id`

**Change Ownership of a Directory**

`sudo chown -R {owner name} {directory name}`

**Change a File's Group Ownership**

`chgrp`

**Change a User's Password**

`passwd {user}`

**Change File Mode**

`chmod`

**Set Default Permissions**

`umask`

**Substitute User**

`su {user}`

## Processes

**Snapshot of Current Processes**

`ps`

**Display Tasks**

`top`

**List Active Jobs**

`jobs`

**Kill a Process**

`kill {pid}`

**Kill all Processes Associated with a Specified Program or Username**

`killall {program or username}`

**Restart System**

`sudo reboot`

**Shutdown System At This Moment in Time**

`sudo shutdown -h now`

## Misc

**Read the Manual for a Command**

`man {command name}`

**Get Amount of Free Space on Each Partition**

`df -h`

**Show Mounted Filesystems**

`mount`

**Get Statistics About Disk Read/Writes**

`iostat`

**Reset Wifi Network Lookup**

`sudo service network-manager restart`

This one is helpful while traveling.

**Print Text**

`echo hello world`

**Arithmetic**

`echo $((365 * 24))`
