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

Establish a filesystem for your block storage device

`mkfs -t ext4 <filesystem name>`

The `-t` flag takes the filesystem type as an argument. `ext4` is the typical filesystem type used on Ubuntu/Debian distros.

Mounting a volume to a directory location (i.e. mount point)

`mount <filesystem name> <path/to/directory>`

Note that this command will not persist between reboots, it's more useful as a quick sanity check to make sure it is configured the way you want. To persist between reboots you will want to edit the `/etc/fstab` file. A mount entry will look like this:

`<filesystem name> <path/to/directory> <filesystem type> defaults 0 2`

`defaults` is the mount options, `0` is dump frequency, and `2` is the pass number for fsck.
