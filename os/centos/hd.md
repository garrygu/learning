
## Finding the New Hard Drive
Typically, the disk drives in a system are assigned device names beginning hd or sd followed by a letter to indicate the device number.   
`ls /dev/sd*`


## Creating Linux Partitions
`fdisk /dev/hdb`


## Creating a File System on a CentOS Disk Partition
`/sbin/mkfs.ext3 -L /userdata /dev/hdb`


## Mounting a File System
```
mkdir /data
mount /dev/sdb1 /data
```

## Configuring CentOS to Automatically Mount a File System
In order to set up the system so that the new file system is automatically mounted at boot time an entry needs to be added to the `/etc/fstab` file.
http://pclosmag.com/html/issues/200709/page07.html
