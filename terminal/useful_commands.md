# Useful Terminal Commands

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

**Remove File**

`rm {file name}`

**Remove Directory**

`rm -rf {directory name}`

Careful not to `rm -rf /` which would delete the root directory!

**Open File**

`xdg-open {file name}`

**Copy File**

`cp {file source} {file destination}`

**Move File**

`mv {file source} {file destination}`

Note that if you use the same directory you can use the move command to rename a file.

**Convert File**

`convert {file name} {new file name}`

**Create Empty File**

`touch {file name}`

**Read the Manual for a Command**

`man {command name}`

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

**Get Amount of Free Space on Each Partition**

`df -h`

**Show Mounted Filesystems**

`mount`

**Get Statistics About Disk Read/Writes**

`iostat`

**Redirecting Data Stream to a File**

`{data stream} > {destination}`

Note that the destination file will be cleared. If you want to append data, use `>>`

**Reset Wifi Network Lookup**

`sudo service network-manager restart`

This one is helpful while traveling.

**Change Ownership of a Directory**

`sudo chown -R {owner name} {directory name}`

**Search Text of a File**

`grep {phrase} {file name}`

**Recursively Search a Folder**

`grep -r {phrase} {directory name}`
