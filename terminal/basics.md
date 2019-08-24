# Useful Terminal Commands

## Directory Traversal

`cd` change directory

`cd ..` go back one directory

`cd ../..` go back two directories, etc.

`cd -` return to last visited directory

`mkdir {directory name}` make directory

`rm -rf {directory name}` remove directory

**Careful not to `rm -rf /` which would delete the root directory!**

## Tab Management

`ctrl + shift + t` create new tab

`alt + {number}` switch to tab number

## File Manipulation

`xdg-open {file name}` open file

`cp {file source} {file destination}` copy file

`rm {file name}` remove file

`mv {file source} {file destination}` move file

**Note that if you use the same directory you can use the move command to rename a file.**

`convert {file name} {new file name}` convert file

`touch {file name}` create empty file

`vi {file name}` open vi editor

`nano {file name}` open nano editor

`sed -i.bak "{number}s|{phrase}|{phrase}|g" {file name}` searches the number line for instances of a phrase and replaces it with another phrase in the specified file, using a flag that makes this command friendly for GNU and BSD flavors of sed

`cat {file1} {file2}` concatenate files

**If you only provide one file as an argument this is a useful way to view a file as well.** 

`less {file name}` read file

`wc {file name}` produce word count of file

`du -h` get memory size of files or directories in bytes

`grep {phrase} {file name}` regex search of file

`grep -r {phrase} {directory name}` recursive grep search

`{data stream} > {destination}` redirecting data stream to a file

**Note that the destination file will be cleared. If you want to append data, use `>>`**

## Ownership Methods

`id` get id info

`sudo chown -R {owner name} {directory name}` change ownership of a directory

`chgrp` change a file's group ownership

`passwd {user}` change a user's password

`chmod` change file mode

`umask` set default permissions

`su {user}` substitute user

## Processes

`ps` snapshot of current processes

`top` display tasks

`jobs` list active jobs

`kill {pid}` kill a process

`killall {program or username}` kill all processes associated with a specified program or username

`sudo reboot` restart system

`sudo shutdown -h now` shutdown system now (instead of default minute delay)

## Networking

`ssh -i {path/to/key_file} {username@remote_host}` creates ssh connection to remote host using an identity specified by a private key

`scp {filename} {username@ipaddress:path/to/remote_dir} {path/to/local_dir}` remote file copy program. Use `-r` flag for directories

`sftp {username@ipaddress}` Sets up a secure FTP protocol

`dig {url}` DNS lookup utility

`host myip.opendns.com resolver1.opendns.com | grep "myip.opendns.com has" | awk '{print $4}'` DNS lookup for public ip addresses

`sudo service network-manager restart` reset wifi network lookup

**This one is helpful while traveling.**

## SSH Keygen

`ssh-keygen -t rsa -b 4096 -C  "{email address}"` Generates a SSH key associated with your email address

`ssh-add ~/.ssh/id_rsa` Adds SSH private key to the SSH-agent.

## Security

`md5 {file}` Creates a checksum for a file

`md5 -s {string}` Creates a checksum for a string

## Misc

`ctrl + a` go to beginning of line

`ctrl + e` go to end of line

`set -o vi` use vi style commands for terminal

`ctrl + alt + t` open new terminal

`exit` exit terminal

`man {command name}` read the manual for a command

`df -h` get amount of free space on each partition

`df -h --total` find out about total disk space useage

`mount` show mounted filesystems

`iostat` get statistics about disk read/writes

`echo hello world` stdout text

`echo $((365 * 24))` arithmetic

`bc <<< "(365 * 24)"` an alternative method for arithmetic