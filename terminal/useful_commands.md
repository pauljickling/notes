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

**Convert File**

`convert {file name} {new file name}`

**Get Memory Size of Files/Directories in Bytes**

`du -h`

**Get Amount of Free Space on Each Partition**

`df -h`

**Show Mounted Filesystems**

`mount`

**Get Statistics About Disk Read/Writes**

`iostat`

**Reset Wifi Network Lookup**

`sudo service network-manager restart`

This one is helpful while traveling.

**Change Ownership of a Directory**

`sudo chown -R {owner name} {directory name}`

**Search Text of a File**

`grep {phrase} {file name}`

**Recursively Search a Folder**

`grep -r {phrase} {directory name}`

## Useful Database Commands

### PostGreSQL

**Startup PostGreSQL**

`sudo su -postgres`

**Create Database**

`createdb {database name}`

**Enter PSQL Shell**

`psql`

**Exit PSQL Shell**

`\q`

### MongoDB

**Start, Restart, or Stop MongoDB**

`sudo service mongod start`

`sudo service mongod restart`

`sudo service mongod stop`

**Enter Mongo Shell**

`mongo`
