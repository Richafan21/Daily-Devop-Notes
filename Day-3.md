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
- Managing dependencies to ensure a package is installed alongside all packages it requires to run proerply, to avoid dependency hell.

Types of Package Managers:  
- **DPKG**: Base package manager for Debian-based distributions.
- **APT**: Newer front-end for DPKG system such as Ubuntu.
- **APT-GET**: Traditional front-end for the DPKG system.
- **RPM**: Base package manager found in Red Hat-based distributions such as RHEL, CentOS, and Fedora.
- **YUM**: Front-end for the RPM system.
- **DNF**: More feature rich front-end for the RPM system.

---

## RPM and YUM

