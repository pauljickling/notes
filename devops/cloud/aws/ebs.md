# EBS

EBS stands for Elastic Block Storage, and allows a cloud provider to mount virtualized disks to their EC2 instances, allowing for arbitrary storage scaling.

Instructions for mounting an EBS volume can be found [here](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-using-volumes.html).

## Quick References for Managing Block Devices

Note that these commands are best executed with root or sudo privilege.

There are two methods for locating the name of your attached block device.

Method 1:

`df -h`

You'll need the filesystem name in the first field, and you should be able to recognize the block device in the "Mounted on" field. If it still isn't obvious, method two should make it even more explcit.

Method 2:

`lsblk`

Establish the filesystem for your block storage device

`mkfs -t ext4 <device name>`

The `-t` flag takes the filesystem type as an argument. `ext4` is the typical filesystem type used on Ubuntu/Debian distros.

Mounting a volume to a directory location (i.e. mount point)

`mount <device name> <path/to/directory>`

Note that this command will not persist between reboots. To persist between reboots you will want to edit the `/etc/fstab` file. A mount entry will look like this:

`<device name> <path/to/directory> <filesystem type> defaults 0 2`

`defaults` is the mount options, `0` is dump frequency, and `2` is the pass number for fsck.

## fstab

fstab is divided into multiple columns that correspond to the following:

- Device: usually the given name or UUID of the mounted device (sda1/sda2/etc).
- Mount Point: designates the directory where the device is/will be mounted. 
- File System Type: nothing trick here, shows the type of filesystem in use. 
- Options: lists any active mount options. If using multiple options they must be separated by commas. 
- Backup Operation: (the first digit) this is a binary system where 1 = dump utility backup of a partition. 0 = no backup. This is an outdated backup method and should NOT be used. 
- File System Check Order: (second digit) Here we can see three possible outcomes.  0 means that fsck will not check the filesystem. Numbers higher than this represent the check order. The root filesystem should be set to 1 and other partitions set to 2. 
