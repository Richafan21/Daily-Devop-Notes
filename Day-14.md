# Storage: DAS NAS and SAN

---

Three commonly used external storage technologies: 
- Direct-Attached Storage (DAS)
- Network-Attached Storage (NAS)
- Storage Area Network (SAN). 

While onboard storage or an attached external drive may work well for desktops and laptops, these solutions are insufficient for enterprise-grade servers  
such as those running production databases or high-demand web applications. In such cases, high-capacity external storage with high availability and exceptional performance is critical.


### Direct-Attached Storage (DAS)

Connects storage devices directly to a host system. The operating system recognizes these devices as block devices,  
allowing for excellent performance with minimal latency because there is no network or firewall overhead.

Well-suited for small businesses and environments where storage does not need to be shared among multiple servers.  
However, since it is directly attached, it means scalability is limited, making it less ideal for larger, enterprise environments that demand shared and flexible storage solutions.

### Network-Attached Storage (NAS)

Network-Attached Storage (NAS) is ideal for mid-to-large businesses that require shared file storage accessible over a network.  
NAS devices are not physically connected to the computing hosts, and data is transferred via a network connection.  
Even if the physical distance is minimal—such as devices within the same rack in a data center—the data still travels through network infrastructure.

NAS typically functions as an NFS server, exporting storage as directories or shares that can be accessed simultaneously by multiple hosts.  
Its design supports centralized shared storage with high availability and good performance, particularly with high-speed Ethernet connectivity.  
NAS is well-suited for web servers, application servers, and non-production database environments.  
It is important to note that installing an operating system directly on a NAS device is generally not recommended.

### Storage Area Network (SAN)

Storage Area Network (SAN) provides block storage designed for enterprises running mission-critical applications that demand high throughput and low latency.  
In a SAN system, storage is allocated to hosts as Logical Unit Numbers (LUNs), which represent sections of a shared storage pool presented as logical disks to the server.



When a host system detects a SAN device, it appears as a raw disk where partitions and file systems can be created, much like any other block device.  
SAN systems typically use Fibre Channel Protocol (FCP) for high-speed data transfer, though Ethernet-based solutions do exist.  
A Host Bus Adapter (HBA) installed in the server (usually via a PCI slot) connects to Fibre Channel switches, ensuring that SAN environments can deliver superior performance and reliability.


SAN is best for handling mission-critical applications and databases—such as Oracle, Microsoft SQL Server, and virtualized environments using platforms like 
VMware, KVM, or Microsoft Hyper-V—due to its high performance and reliability.

---

## NFS File System

Network File Systems, unlike block devices, stores data as idividual files rather than in blocks.  
NFS utilises a server-client model to share directories across the network.  
For instance, consider a software repository server that maintains a directory at `/software/repos`. By exporting this directory using NFS, employee laptops can access its contents as if the files were stored locally—even though they reside on the server.

On the NFS server, directory sharing is managed through the exports configuration file located at `/etc/exports`. This file specifies which clients are permitted to access the shared directories. For example, if the allowed client IP addresses are 10.61.35.201, 10.61.35.202, and 10.61.35.203, and the NFS server has an IP of 10.61.112.101, the configuration in `/etc/exports` might look like this:  
`/software/repos 10.61.35.201 10.61.35.202 10.61.35.203` 

This snippet demonstrates a basic export configuration. Instead of listing individual IP addresses, you could also specify network ranges or use a wildcard (*) if you want to allow access from any client. Note that in many environments a firewall sits between the server and its clients, requiring specific ports to be opened for NFS traffic.

After updating the /etc/exports file on the server, apply the changes and share the directories using the exportfs command. To export all the mounts defined in the exports file, use:  
`exportfs -a`  

Alternatively, to manually export a directory to a specific client, you can run:  
`exportfs -o 10.61.35.201:/software/repos`  

Once the directory is exported on the server, you can mount it on the client machine. To mount the shared directory (for example, to the local directory /mnt/software/repos), use the following command, ensuring that you specify the NFS server's IP (or hostname) followed by the exported directory separated by a colon:  
`mount 10.61.112.101:/software/repos /mnt/software/repos`  

With these steps, the NFS share should now be successfully mounted on the client system, enabling seamless file access from the server.

---

## LVM

Logical Volume Manager is a powerful tool lets you group multiple physical volumes(disks or partitions) into a single volume group(VG).  
From this group, you can allocate one or more logical volumes(LV). The examples in these notes will use three partitions, but keep in mind  
LVMs can work with any number of disks and unlimited partitions grouped under a single VG.  

LVMS can also dynamically resize logical volumes.  

**Prerequisites**: Make sure LVM2 is installed.  
`apt-get install lvm2`  

### Step 1: Create a Physical Volume
The initial step in configuring LVM is to identify available disks or partitions and create physical volumes (PVs) from them. A physical volume represents the disk or partition in LVM.

For example, to create a physical volume on the device path /dev/sdb, execute:  

`pvcreate /dev/sdb`

### Step 2: Create a Volume Group
Once the physical volume is established, create a volume group (VG) that will host your logical volumes. In this example, the VG is named after me, `richard_vg`, and includes `/dev/sdb`:

`vgcreate richard_vg /dev/sdb`

To display details about the physical volume, run:

`pvdisplay`

To get even moer information, run:  

`vgdisplay`

### Step 3: Create a Logical Volume
After establishing the volume group, create a logical volume (LV). In this example, we create a 1GB LV named vol1 within the richard_vg volume group:

`lvcreate -L 1G -n vol1 richard_vg`

### Step 4: Create and Mount a Filesystem
With your logical volume in place, the next step is to create a filesystem on it. In this example, we create an ext4 filesystem on `/dev/richard_vg/vol1`:

`mkfs.ext4 /dev/richard_vg/vol1`

After the filesystem is created, mount it to a directory (e.g., /mnt/vol1) to make it accessible:

`mount -t ext4 /dev/richard_vg/vol1 /mnt/vol1`

### Step 5: Resize the Logical Volume and Filesystem
Sometimes you may need to expand the logical volume while it remains mounted. Begin by verifying that there is sufficient free space in the volume group:

`vgs`

If there is enough free space, extend the logical volume by an additional 1GB:

`lvresize -L +1G /dev/richard_vg/vol1`

Keep in mind, that only the LV has been extended, we also have to extend the file system using:

`resize2fs /dev/richard_vg/vol1`

Verify the file system size with:

`df -hP /mnt/vol1`

### Access Paths for the Logical Volume

It’s important to note that the logical volume can be accessed through two different paths:

`/dev/richard_vg/vol1`
`/dev/mapper/richard_vg-vol1`

Both paths refer to the same logical volume, so you can use either interchangeably in your commands and configurations.

---

