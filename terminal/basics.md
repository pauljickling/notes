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

**Note that for the same path you can use the move command to rename a file.**

`convert {file name} {new file name}` convert file

`touch {file name}` create empty file

`vi {file name}` open vi editor

`nano {file name}` open nano editor

`sed -i.bak "{number}s|{phrase}|{phrase}|g" {file name}` searches the number line for instances of a phrase and replaces it with another phrase in the specified file, using a flag that makes this command friendly for GNU and BSD flavors of sed

`sort -t "," -k 2` BSD sort defines a delimiter with `-t` flag and `-k` flag defines the column to sort by

`cat {file1} {file2}` concatenate files

**If you only provide one file as an argument this is a useful way to view a file as well.** 

`less {file name}` read file

`wc {file name}` produce word count of file

`wc -l {file name} | awk '{ print $1 }'` number of lines of a file

`cat data.csv | awk "{ print NF }" FS=, | uniq` number of columns in csv file

`du -h` get memory size of files or directories in human readable format

`du -h --max-depth=1 {dir}` get memory size of files/directories of specified directory (as oppose to pwd)

`grep {phrase} {file name}` regex search of file (to use actual regular expressions place phrase in quotation marks i.e. `grep '[0-9]' file` to search for numbers in a file)

`grep -r {phrase} {directory name}` recursive grep search

`pgrep {phrase}` searches process names and outputs PIDs. Almost identical to the command `ps aux | grep {phrase} | awk '{ print $2 }'` but it has the advantage of not including the grep process as a match

`pkill {phrase}` like `pgrep` but kills the process(es)

`{data stream} > {destination}` redirecting data stream to a file

**Note that the destination file will be cleared. If you want to append data, use `>>`**

## Ownership Methods

`id` get id info

`chown -R {owner name} {directory name}` change ownership of a directory

`chgrp` change a file's group ownership

`passwd {user}` change a user's password

`chmod` change file mode

`umask` set default permissions

`su {user}` substitute user

## Processes

`ctrl + c` halt a process executed via the terminal

`ctrl + z` pause a process executed via the terminal

`bg` run paused process as a background job (alternatively you can append `&` at the end of a command to run it in the background)

`lsof` get a stream of open files

`lsof {file name}` list processes that are writing to a file

`jobs` list active jobs

`disown -h %{job number}` Disowns background job as a shell job which allows a process to continue running even if the controlling terminal session ends.

`nohup {command}` a simpler alternative command to running a background job and disowning. Typically it will feature `nohup {command} 2>&1`.

`ps` snapshot of current processes

`top` display tasks

`kill {pid}` kill a process

`killall {program or username}` kill all processes associated with a specified program or username

`reboot` restart system

`shutdown -h now` shutdown system now (instead of default minute delay)

`systemctl | grep daemon` get list of running daemons

## Networking

`ping {address}` pings address with small byte packet with a response time

`ifconfig` provides network info

`ifconfig | grep inet` filters the network info into an easy eay to lookup local ip address

`netstat` provides network statistics. Note that it is no longer included by default in recent Ubuntu/Debian distros. To install it you will need to run `apt install net-tools`

`ssh -i {path/to/key_file} {username@remote_host}` creates ssh connection to remote host using an identity specified by a private key

`scp {path/to/file} {username@ipaddress:path/to/remote_dir}` copies local file to remote directory. Use `-r` flag for directories.

`scp {username@ipaddress:path/to/remote_dir} {path/to/local_dir}` copies remote file to local directory  Use `-r` flag for directories.

`sftp {username@ipaddress}` Sets up a secure FTP protocol

`dig {url}` DNS lookup utility

`host {url}` DNS lookup for public ip addresses

`service network-manager restart` reset wifi network lookup. **This one is helpful while traveling.**

## SSH Keygen

`ssh-keygen -t rsa -b 4096 -C  "{email address}"` Generates a SSH key associated with your email address. Providing an email address at the end of your public key is a good practice because it allows an admin to easily identify who has access to a server in the `authorized_keys` file.

`ssh-add ~/.ssh/id_rsa` Adds SSH private key to the SSH-agent.

## Security

`sha256sum` Generates a hash function

`shasum -a 256` Use on Mac for the sha-256 formula (defaults to sha-1 which has broken collision resistance)

`pwgen -s {num}` Generate a random and secure password of `{num}` length

## Misc

`ctrl + w` delete word

`ctrl + u` clear line

`dmesg` review messages stored in the ring buffer. Useful for understanding processes that are failing without clear stderr messages, among other things.

`pbcopy` copies standard input to the pasteboard. For example, `cat foo.txt | pbcopy` would copy the contents of the foo.txt file to the pasteboard.

`pbpaste` pastes pasteboard content

`cat /etc/os-release` get information about the OS

`date +"%Z %z"` get time zone. Useful for remote servers where you aren't necessarily sure what region it is in.

`ctrl + a` go to beginning of line

`ctrl + e` go to end of line

`set -o vi` use vi style commands for terminal

`ctrl + alt + t` open new terminal

`exit` exit terminal

`man {command name}` read the manual for a command

`df -h` get amount of free space on each partition

`df -h --total` find out about total disk space useage

`free -m` View available and used RAM (note this is available on Ubuntu, but not sure about other versions of Linux)

`mount` show mounted filesystems

`iostat` get statistics about disk read/writes

`nproc` get number of CPUs available

`ulimit -n` get number of worker processes available

`echo hello world` stdout text

`echo $((365 * 24))` arithmetic

`bc <<< "(365 * 24)"` an alternative method for arithmetic
