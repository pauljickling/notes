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

`head {file name}` view the head of a file. By default it shows the first 10 results, but you can use the `-n` flag to specify the number of lines.

`tail {file name}` view the tail of a file. Like `head`, you can use the `-n` flag to specify the number of lines, but you can also use the `-r` flag to reverse the order so the last line appears first, then the second to last line, etc.

`tail -f {file name}` for files that are continuously being written to (e.g. logs) you can use the `-f` flag to view the file as a stream.

`wc {file name}` produce word count of file

`wc -l {file name} | awk '{ print $1 }'` number of lines of a file

`cat data.csv | awk "{ print NF }" FS=, | uniq` number of columns in csv file

`du -h` get memory size of files or directories in human readable format

`du -h --max-depth=1 {dir}` get memory size of files/directories of specified directory (as oppose to pwd)

`du -h --max-depth=1 {dir} | sort -hr` get memory size of files/directories of specified directory and sort output by file size (alternatively remove the `r` flag if you'd like the order to go from smallest to largest)

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

`stat -c "%a" {file/directory}` outputs the octal permissions for file/directory

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

`ab -n 1000 -c 10 {url}` apache benchmarker. `-n` flag is for the number of requests to poll, and `-c` is for the number of concurrent requests. Note that for the URL you must end with a `/` or `ab` will be an error.

`ip link add {device type} {device name}` creates an ip address for a new device type

`ip link list` view existing network conditions in current network namespace environment

`ip route list` provide a list of ip routes

`ip link set {device name} netns {network namespace name}` attaches a network device to a network namespace

`ip link set {device name} up` activates a network device

`ip link addr add {ip address} dev {device name}` assigns an ip address to a device within the network namespace environment

`nsenter --net=/run/netns/{network namespace name} bash` create a bash environment for your network namespace

## SSH Keygen

`ssh-keygen -t rsa -b 4096 -C  "{email address}"` Generates a SSH key associated with your email address. Providing an email address at the end of your public key is a good practice because it allows an admin to easily identify who has access to a server in the `authorized_keys` file.

`ssh-keygen -t ed25519` Generates an ssh key with the ED-25519 cryptography algorithm, which is preferred over RSA keys as best practice. Note that with these there is no need to specify the bit length using the `-b` flag.

`ssh-add ~/.ssh/id_rsa` Adds SSH private key to the SSH-agent.

## Security

`sha256sum` Generates a hash function

`shasum -a 256` Use on Mac for the sha-256 formula (defaults to sha-1 which has broken collision resistance)

`pwgen -s {num}` Generate a random and secure password of `{num}` length

## Misc

`stty -a` get a list of current settings for your terminal's stdout, including a list of available ctrl codes for your terminal

`ctrl + w` delete word

`ctrl + u` clear line

`!!` execute previous command. Useful for example, if you lack privileges, and need to escalate with sudo you can simply type `sudo !!`

`dmesg` review messages stored in the ring buffer. Useful for understanding processes that are failing without clear stderr messages, among other things.

`pbcopy` on macOS copies standard input to the pasteboard. For example, `cat foo.txt | pbcopy` would copy the contents of the foo.txt file to the pasteboard.

`pbpaste` on macOS pastes pasteboard content

`xclip` copies standard input to the pasteboard.

`xclip -o` outputs pasteboard content

`cat /etc/os-release` get information about the OS

`dpkg --print-architecture` get information about chip architecture

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
