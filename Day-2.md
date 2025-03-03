# Day 2: Linux Kernel and Package Management

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
