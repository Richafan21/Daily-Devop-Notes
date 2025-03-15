# Storage

---

## Disk Partitions

### Block Devices

Remember that a Block device a file type under the `/dev` directory and denotes a hardware component used for data storage.  
Solid-state disk(SSD) is an example of a block storage in Linux. It is called block storage because data is read or written in blocks or chunks of space.  
`lsblk` to see all the block devices in the system.
Alternatively, you can also use the command `ls -l /dev | grep "^b"`
```bash
$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0 119.2G 0 disk 
├─sda1   8:1    0   100M 0 part /boot/efi
├─sda2   8:2    0  72.5G 0 part /media/MM/Data
└─sda3   8:3    0  46.6G 0 part /


$ ls -l /dev/ | grep "^b"
brw-rw----  1 root disk  8,  0 Mar 19 17:43 sda
brw-rw----  1 root disk  8,  1 Mar 19 17:42 sda1
```

In the above lsblk output, the device sda represents the entire disk (approximately 119GB), while sda1 through sda3 indicate the individual partitions.

Each block device is assigned a major and minor number:

The major number (8 in this case) identifies the type of block device (commonly associated with SCSI devices, hence the "sd" prefix).
The minor numbers differentiate among individual physical or logical devices, such as the whole disk and its partitions.

### Disk Partitions

Taking a closer look at the output of `lsblk` from before, we can see that the SSD disk is divided into three partitions:  
1. `sda3` is used as the root partition.
2. `sda2` (72.5GB) is mounted at /media/MM/Data and is typically used for storing backup data.
3. `sda1` is mounted at /boot/efi to store boot loader files needed during system startup.

`fdisk` is another useful command as it not only lists the partition table, but also lets you create or delete partitions.   
Running `fdisk -l /dev/sda` provides detailed information such as partition type, disk size in bytes, and sector details.  
```bash
$ sudo fdisk -l /dev/sda
Disk /dev/sda: 119.2 GiB, 1280355676160 bytes, 250069680 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 7CABF26E-9723-4406-ZEA1-C2B9B6270A23


Device        Start       End   Sectors Size Type
/dev/sda1     2048    206847   204800 100M EFI System
/dev/sda2   239616  150194175 149954560 71.5G Linux filesystem
/dev/sda3   150194176 247955455 97761280 46.6G Linux filesystem
```

### Partitioning Schemes

There are three primary types of disk partitions:

- **Primary Partition**: Used for booting an operating system. Traditionally, disks could have a maximum of four primary partitions.
- **Extended Partition**: Acts as a container for logical partitions; it cannot be used directly.
- **Logical Partition**: Created within an extended partition to bypass the four primary partition limitation.

A partitioning scheme, or partition table, defines how these partitions are organized on a disk.  
The older MBR partitioning scheme permits only four primary partitions and limits disk sizes to 2TB (unless using an extended partition).  
In contrast, GPT (GUID Partition Table) supports an almost unlimited number of partitions—often limited only by the operating system, 
such as RHEL’s limit of 128 partitions per disk—and eliminates the 2TB disk size restriction. So Unless the operating system requires MBR,  
GPT is generally the preferred choice due to its enhanced capacity and flexibility.

### Creating a New Partition with GPT

`gdisk` can be used to create a new partition:  
```bash
$ lsblk
fd0      2:0    1  4K   0 disk
sr0     11:0    1 1024M 0 rom
sda      8:0   0 97.7G 0 disk
|-sda1   8:1   0 93.7G 0 part /
|-sda2   8:2   0   1K 0 part
|-sda5   8:5   0  3.9G 0 part
sdb      8:15  0 20G   0 disk


$ gdisk /dev/sdb
GPT fdisk (gdisk) version 1.0.1


Partition table scan:
MBR: protective
BSD: not present
APM: not present
GPT: present


Found valid GPT with protective MBR; using GPT.
Command (? for help):
```
Within the `gdisk` interface, you can enter `?` to view all available commands. To add a new partition, press `n` and follow the interactive prompts.  
After providing the details and confirming with the default hexcode(8300 for Linux filesystem), finalise the changes by writing the new partition table  
to disk. Press `w` at the prompt. After this, the new partition `/dev/sdb1` is now available. Verify the partition creation using `lsblk` or `fdisk -l`.

---

## File Systems

To make a disk usable in Linux, we need to create file systems. After partitioning divides a disk into segments of usable space,  
the Linux kernel still treats these partitions as raw disk areas. To read and write data, you must create a file system that defines  
the storage structure and then mount that file system to a directory.  For this section, we will focus on the extended file system family.  

### Comparing ext2, ext3, and ext4

Both ext2 and ext3 file systems support a maximum file size of 2 TB and a maximum volume size of 4 TB. While they efficiently store data,  
ext2 may experience long boot times after an unclean shutdown such as a power outage.  
In contrast, ext3 adds journaling features that enable a faster system startup following such events.  
Ext4 further enhances these capabilities and is one of the most popular general-purpose file systems today—it supports files up to 16 TB and volumes as large as 1 exabyte.  
Additionally, ext4 (as well as ext3) offers backward compatibility: a file system created with ext4 can be mounted as ext3 or ext2, and an ext3 file system can be mounted as ext2.

### Creating and Mounting an ext4 File System

To create an ext4 file system on the device /dev/sdb1:  
1. Use the mkfs.ext4 command to format the device.
2. Create a mount point directory.
3. Mount the file system.
```bash
[~]$ mkfs.ext4 /dev/sdb1
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done


[~]$ mkdir /mnt/ext4
[~]$ mount /dev/sdb1 /mnt/ext4
```

### Verifying the Mounted File System

You can confirm that the file system is mounted correctly by using the mount command combined with grep:

``` [~]$ mount | grep /dev/sdb1```  
```/dev/sdb1 on /mnt/ext4 type ext4 (rw,relatime,data=ordered)```

### Configuring Automatic Mounting at Boot

To automatically mount the file system during system boot, add an entry to the /etc/fstab file. This enables persistent mounting and ensures the file system is available after a reboot. For example:
```bash
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
/dev/sda1  /  ext4  defaults,relatime,errors=panic  0  1
```

