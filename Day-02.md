# Day 2: Linux Kernel, File types, and File System Hierarchy

## Kernel Space and User Space

- **Kernel Space**:
  - Reserved for the kernel's execution and its critical services.
  - Processes operating in this space have complete access to the system's hardware resources.
  - **Analogy**: Like a "staff-only" section in a library.

- **User Space**:
  - Dedicated to processes running outside the kernel, with limited access to hardware resources.
  - **Analogy**: Like the main area of a library with books, DVDs, and resources—except in this case, it’s programming languages, utilities, and graphical tools (collectively called the **userland**).
  - Programs in the User Space primarily work with data by performing operations on memory or disk. They send their requests to the kernel via **system calls**.

- **System Calls**:
  - Provide a secure interface for user programs to request services from the kernel.
  - Examples: `close`, `getpid`, `readdir`, `strlen`, `closedir`.
  - Ensure system stability, order, and security.

---

## How Linux Works With Hardware

- **Hardware Detection**:
  - When a USB disk is attached, the corresponding device driver in the Linux kernel detects the state change and generates a **uEvent**.
  - This event is forwarded to the user-space device manager daemon, **udev**, which dynamically creates a device node under the `/dev` filesystem for the new drive.
  - Once this process completes, the new disk appears in the `/dev` filesystem.

- **`dmesg` Command**:
  - Displays messages from the kernel's ring buffer, revealing how hardware was detected and configured.
  - Example usage:
    ```bash
    $ dmesg | less  # View messages page by page
    $ dmesg | grep USB  # Search for USB-related messages
    ```

- **`udevadm` Utility**:
  - A management tool for **udev**.
  - `udevadm info`: Queries the udev database for device information.
    ```bash
    $ udevadm info --query=path --name=/dev/sda5
    ```
  - `udevadm monitor`: Monitors kernel uEvents in real time. Useful for tracking details of newly attached or removed devices (e.g., unplugging a USB mouse).

---

## Hardware Information Commands

- **`lspci`**:
  - Lists information on all PCI (Peripheral Component Interconnect) devices configured in the system (e.g., Ethernet cards, video cards).
  - PCI represents devices directly attached to the motherboard.

- **`lsblk`**:
  - Lists information on block devices.
  - Key terms:
    - **Disk**: Refers to the whole physical disk.
    - **Partition**: Reusable disk space carved out from the disk.
    - **Major Number**: Identifies the type of device driver associated with the device.
    - **Minor Number**: Differentiates between devices that share the same major number.
  - Common major numbers:
    - `1`: RAM
    - `3`: Hard disk or CD-ROM
    - `6`: Parallel printers
    - `8`: SCSI disk

- **`lscpu`**:
  - Displays information on the CPU.
  - Key terms:
    - **32-bit vs. 64-bit**:
      - 32-bit CPUs can address up to 4GB of memory (2^32 values).
      - 64-bit CPUs can address up to 18EB of memory (2^64 values).
      - You cannot run a 64-bit OS on a 32-bit CPU, but the reverse is possible (though not recommended).
    - **Sockets x Cores x Threads = CPUs**: Total number of logical processors.

- **`lsmem`**:
  - Lists available memory in the system.
  - Use the `--summary` option to print a summary.
  - Alternative: Use the `free` command to see total and used memory.
    ```bash
    $ free -m  # Display memory in MB
    $ free -g  # Display memory in GB
    ```

- **`lshw`**:
  - Extracts detailed information on the entire hardware configuration of the machine.
  - Example:
    ```bash
    $ sudo lshw  # Requires root privileges
    ```

---

## Important Notes

- **`sudo` Command**:
  - Adding `sudo` before a command allows you to run it as the superuser (root), granting elevated privileges.
  - Not every user can run every command in Linux; `sudo` ensures proper access control.
 
---

## Linux Boot Sequence

There are two ways to initiate a Linux boot process:
1. Start a Linux device in a halted or stopped state.
2. Reboot or reset a running system.

### 4 Steps of the Linux Boot Process

1. **BIOS POST**:
   - **Initial stage**: POST stands for **Power On Self Test**.
   - The BIOS runs a POST test to ensure that the hardware components attached to the device are functioning correctly.
   - If the POST fails, the boot process stops, and an error message or beep code is displayed.

2. **Boot Loader**:
   - This stage begins only after a successful POST.
   - The BIOS loads and executes the boot code from the designated boot device. For Linux, this is typically located in the first sector of the hard disk, within the `/boot` filesystem.
   - The bootloader (e.g., GRUB2) displays a menu of boot options, such as:
     - Microsoft Windows
     - Ubuntu 24.04
   - After the user selects an option, the bootloader:
     - Loads the Linux kernel into memory.
     - Passes required parameters to the kernel.
     - Transfers control to the kernel.
   - **GRUB2** is the most popular and standard bootloader for Linux systems.

3. **Kernel Initialisation**:
   - After the Linux kernel is loaded, it is decompressed (if compressed) and loaded into memory.
   - The kernel begins execution and performs several critical tasks:
     - **Hardware initialisation**: Detects and initialises hardware devices.
     - **Memory management**: Sets up virtual memory and allocates memory for the system.
   - Once the kernel is operational, it looks for the **INIT process** to run. This process is essential for setting up the environment before user-space initialisation.

4. **Service Initialisation (INIT Process)**:
   - Most modern Linux distributions use **systemd** as the INIT process.
   - **systemd** is responsible for transitioning the system into a fully operational state.
   - Key features of systemd:
     - **Parallelisation**: Starts services in parallel, significantly reducing boot time.
     - **Dependency management**: Ensures services start in the correct order based on dependencies.
   - To check the INIT system used, run:
     ```bash
     $ ls -l /sbin/init
     ```
   - Once systemd ensures that all required services are running properly, the boot sequence is complete, and the system is ready for user interaction.

---

## Run levels

Linux can run in multiple modes such as graphical mode. These modes are set by the run level.  
- `runlevel` command should return what run level you're running.
- Run level 5: Boots into Graphical Interface(Systemd Targets:graphical.target)
- Run level 3: Boots into a Command Line Interface(Systemd Targets:multiuser.target)

### Viewing and changing Systemd Target
- `systemctl get-default` gets the default system target  
- `systemctl set-default multi-user.target` to change the default target

### File Types in Linux

"Everything is a file in Linux" is a fundamental concept. There are three main types of files:

#### 1. **Regular Files**
   - These include images, scripts, configuration files, and data files.
   - **Examples**:
     - JPG files
     - Shell scripts
     - Code files

#### 2. **Directories**
   - Directories are special files that act as containers for other files and directories.
   - **Examples**:
     - `/home/richard`
     - `/root`
     - `/home/richard/code-directory`

#### 3. **Special Files**
   - **Character Files**: Devices under the `/dev` filesystem, such as a mouse or keyboard.
   - **Block Files**: Block devices located under `/dev`, such as hard disks and RAM.
   - **Links**:
     - **Hard Links**: Associate two or more file names that share the same block of data on the physical disk. Deleting one link will delete the data.
     - **Symbolic (Sym) Links**: Act as pointers to another file. Deleting the link won't delete the file.
   - **Socket Files**: Enable communication between two processes.
   - **Named Pipes**: Connect one process as an input to another. They are unidirectional (from the first process to the second).

---

### Identifying File Types

- Use the `file` command with an argument to determine the file type:
  ```bash
  $ file myfile.txt
- `ls` with the option `-ld` and an argument will show the file type with the initial character in the output.
- d: Directory
- -: Regular File
- c: Character device
- l: Link(symbolic or hard link)
- s: Socket
- p: Named Pipe
- b: Block Device

---

## Filesystem Hierarchy

### Key Directories and Purposes
1. `/home`
   - This directory holds the home directories for all users except the root user as the root user's home directory is located at `/root`.

2. `/opt`
  - Third party programs are typically installed here.

3. `/mnt` and `/tmp`
   - `/mnt` is used for temporarily mounting filesystems, whereas `/tmp` is reserved for storing temporary data.
  
4. `/media`
   - Reserved for all external media such as USB drives.
  
5. `/dev`
   - Contains special block and character device files. These files represent hardware devices such as external hard disks, mice, and keyboard.

6. `/bin` and `/etc`
   - Essential programs like `cp`, `mv`, `mkdir` reside in `/bin`. Meanwhile `/etc` is vital because it stores the majority of Linux config files.

7. `/lib` and `/lib64`
   - These directories store shared libraries required by programs during runtime.
  
8. `/usr`
   - Modern Linux systems use `/usr` to store userland applications and their data. This directory contains software such as Firefox, Thunderbird mail, and vi editor.
  
9. `/var`
    - Logs and cached data are stored in this directory for when issues arise and can help troubleshooting.
  
The `df` command or disk filesystem command is useful for viewing details about each mounted filesystem. Example: `df -hP`
