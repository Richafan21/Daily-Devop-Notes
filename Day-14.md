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
A Host Bus Adapter (HBA) installed in the server (usually via a PCI slot) connects to Fibre Channel switches, ensuring that SAN environments can deliver  
superior performance and reliability.


SAN is best for handling mission-critical applications and databases—such as Oracle, Microsoft SQL Server, and virtualized environments using platforms like 
VMware, KVM, or Microsoft Hyper-V—due to its high performance and reliability.

---

