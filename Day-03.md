# Day 3: Package Management

## Introduction to Package Management

Examples of package managers include:  
- Apt and DPKG (Ubuntu and Debian)
- RPM ( Red Hat and CentOS)

One of the ways to categorise a Linux distribution is by the package manager it uses.  
- .DEB: Ubuntu, Debian, Linux Mint
- .RPM: RHEL, CentOS, Fedora

A package is a compressed archive that contains all the files that are required by a particular software to run.  
For example, when installing the image editing software GIMP (GNU Image Manipulation Program) on Ubuntu,  
the GIMP.DEB package contains all the necessary binaries, supporting files, and metadata for proper installation and execution. 

The diversity of Linux distributions can lead to compatibility issues as each have their own challenges, packages, and dependencies.  
This is where a package manager comes to save the day. It is a software in a Linux system that provides a consistent and automated process of installing, upgrading, configuring, and removing packages from the OS.  

Some essential functions of a package manager include:  
- Ensuring Package integrity and authenticity by verifying their digital certificates.
- Simplifying the entire package management process.
- Grouping packages by function to reduce user confusion.
- Managing dependencies to ensure a package is installed alongside all packages it requires to run properly, to avoid dependency hell.

Types of Package Managers:  
- **DPKG**: Base package manager for Debian-based distributions.
- **APT**: Newer front-end for DPKG system such as Ubuntu.
- **APT-GET**: Traditional front-end for the DPKG system.
- **RPM**: Base package manager found in Red Hat-based distributions such as RHEL, CentOS, and Fedora.
- **YUM**: Front-end for the RPM system.
- **DNF**: More feature rich front-end for the RPM system.

---

## RPM and YUM

This section covers **RPM** and **YUM**, the two primary package managers for RPM-based Linux distributions. These tools handle software installation, updates, removal, and dependency management.

### RPM (Red Hat Package Manager)

RPM is the core package manager for Red Hat Enterprise Linux(RHEL), CentOS, Fedora, and similar distributions. It manages `.rpm` packages and supports essential operations:

### Basic RPM Commands

- **Install a package:**
  ```bash
  rpm -ivh package.rpm
  ```
  - `-i`: Install the package.
  - `-v`: Verbose mode.
  - `-h`: Show progress.

- **Uninstall a package:**
  ```bash
  rpm -e package
  ```

- **Upgrade an existing package:**
  ```bash
  rpm -Uvh package.rpm
  ```
  - `-U`: Upgrade (replaces older versions).

- **Query installed packages:**
  ```bash
  rpm -q package
  ```

- **Verify installed package integrity:**
  ```bash
  rpm -V package
  ```
  - Ensures files haven't been modified from their original version.

### Limitations of RPM

RPM does **not** automatically resolve dependencies, which can lead to "dependency hell." This is where **YUM** comes in.

---

### YUM (Yellowdog Updater Modified)

YUM simplifies package management by handling dependencies automatically. It retrieves software from repositories, ensuring smoother installation and updates.

### Key Features of YUM

- **Automatic dependency resolution**
- **Access to remote repositories**
- **Batch updates & installations**
- **Easier package searching**

### YUM Configuration

YUM repositories are defined in `.repo` files located in:
```bash
/etc/yum.repos.d/
```
Repositories can be:
- **Official (default system repositories)**
- **Third-party (e.g., EPEL, Remi, NGINX)**
- **Local (self-hosted RPM packages)**

### Common YUM Commands

| Command | Purpose |
|---------|---------|
| `yum repolist` | List available repositories |
| `yum search package` | Find a package |
| `yum install package` | Install a package |
| `yum remove package` | Uninstall a package |
| `yum update` | Update all packages |
| `yum update package` | Update a specific package |
| `yum provides /path/to/file` | Find which package owns a file |

### Example: Listing Repositories
```bash
$ yum repolist
Repo ID                    Repo Name                          Status
base/7/x86_64              CentOS-7 - Base                   10,000+
epel/x86_64                Extra Packages for Enterprise     13,000+
extras/7/x86_64            CentOS-7 - Extras                   300+
```

### Example: Installing a Package
```bash
$ yum install httpd
```

### Example: Updating All Packages
```bash
$ yum update
Transaction Summary
=================================================
Install    ( 4 Dependent packages )
Upgrade    ( 78 Packages )
Total download size: 64M
Is this ok [y/N]: y
```

### Best Practices

- **Always review** the transaction summary before confirming installations/updates.
- **Enable security updates** to keep the system secure.
- **Use `yum clean all`** periodically to free up disk space by clearing metadata and cache.
- **Prefer YUM over RPM** for most package management tasks to avoid dependency issues.

By combining **RPM** for direct package control and **YUM** for automated dependency handling, Linux package management becomes efficient and scalable. 

## DPKG and APT

This guide covers **DPKG** and **APT**, two essential package management tools for Debian-based distributions like Ubuntu, Linux Mint, and PureOS. **DPKG** is a low-level package manager for handling `.deb` files, while **APT** is a higher-level tool that automates dependency management and simplifies software installation.

### DPKG (Debian Package Manager)

DPKG is similar to **RPM** on Red Hat-based systems. It provides direct control over `.deb` packages but **does not** handle dependencies automatically. If required dependencies are missing, installations may fail.

### Basic DPKG Commands

- **Install a package:**
  ```bash
  dpkg -i package.deb
  ```
- **Remove a package:**
  ```bash
  dpkg -r package
  ```
- **List installed packages:**
  ```bash
  dpkg -l package
  ```
- **Check package status:**
  ```bash
  dpkg -s package
  ```

### DPKG Dependency Issues

Since DPKG does not resolve dependencies, installing a package may result in errors:
```bash
$ dpkg -i gimp.deb
...
dpkg: dependency problems prevent configuration of gimp:
 gimp depends on libgimp2.0 (>= 2.10.8); however:
 Version of libgimp2.0 on system is 2.8.22-1.
dpkg: error processing package gimp (--install):
 dependency problems - leaving unconfigured
```
In such cases, dependencies must be installed manually or resolved using **APT**.

---

### APT (Advanced Package Tool)

APT is a high-level package manager that simplifies software installation by **automatically resolving dependencies**. It functions similarly to **YUM** on Red Hat-based systems, leveraging DPKG while managing repositories and dependencies.

### Key Features of APT
- **Automatic dependency resolution**
- **Simplifies package installation and removal**
- **Manages software repositories**
- **Batch updates and system upgrades**

### Common APT Commands

| Command | Purpose |
|---------|---------|
| `apt update` | Refresh package lists from repositories |
| `apt upgrade` | Upgrade all installed packages |
| `apt install package` | Install a package with dependencies |
| `apt remove package` | Remove an installed package |
| `apt search package` | Search for a package in the repository |

### Repository Management
APT uses repositories defined in:
```bash
/etc/apt/sources.list
```
These can point to:
- **Official repositories** (default system sources)
- **Third-party repositories** (e.g., PPAs for Ubuntu)
- **Local sources** (CD-ROMs or internal mirrors)

To edit sources manually:
```bash
sudo nano /etc/apt/sources.list
```
Or use the command:
```bash
apt edit-sources
```

### Example: Installing GIMP with APT
Instead of using `dpkg -i gimp.deb`, APT simplifies the process:
```bash
sudo apt install gimp
```
APT ensures all necessary dependencies are installed automatically.

### Best Practices
- **Always run `apt update`** before installing software to ensure you have the latest package lists.
- **Use `apt upgrade` regularly** to keep your system secure and up to date.
- **Prefer `apt install` over `dpkg -i`** to avoid dependency issues.
- **Use `apt autoremove`** to clean up unused dependencies.

By combining **DPKG** for direct package control and **APT** for dependency management, Debian-based systems achieve a powerful and efficient software management system.



