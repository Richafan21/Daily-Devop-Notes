# Daily-Devop-Notes

*Keeping track of what I learn each day on my way to become a DevOps Engineer. Commands will be represented by a $ sign before it.*  

## Table of Contents
- [Day 1: Linux Basics and Core Concepts](#day-1-linux-basics-and-core-concepts)

## Day 1: Linux Basics and Core Concepts

Linux shell is a program that allows text-based interactions between the program and the user.  
The home directory (`/home/richard`) is like a dedicated locker assigned to you. Other users cannot access anything within your own home directory. By default, it is represented by `~`.

### Command and Arguments

- `echo`: A **command** that acts like a print statement. It does not do anything unless you add an argument (e.g., `$echo hello`).
- `uptime`: A **command** that does not require an argument. It tells us how long the system has been running since the last reboot.

Linux also has **options** (or **flags**), which are added after a command to alter its behavior. Options are usually represented by a single letter preceded by a hyphen (e.g., `-n`).  
It can be challenging to memorize all the options, so it’s better to refer to the command’s help options.  

There are two types of commands: **Internal** and **External**. Built-in commands are internal, while external commands are binary programs or scripts, usually located in distinct files in the system. These can come preinstalled or be installed by the user. You can use `type` followed by another command as the argument to determine whether that command is internal or external.  
Example: `$type cd` (output: `cd is a shell builtin`).

### Basic Linux Commands
- `cd`: Change directory. Add the directory you want to move to as the argument.  
  Example: `$cd /home/richard/Food`.  
  This can only navigate down from the current working directory. To move up, use `cd ..`. To return to the home directory, use `cd` without any arguments.
- `mkdir`: Make directory. Add the name of the directory as the argument, and it will become a directory under the working directory.  
  Example: `$mkdir Food`.  
  To create a directory inside another directory that doesn’t exist yet, use the `-p` option:  
  Example: `$mkdir -p Food/Burger`.
- `pwd`: Print working directory.  
  Example: `$pwd` (output: `/home/richard`).
- `ls`: List storage. Lists everything inside a directory. No argument is needed to list the current directory.  
  Example: `$ls /home/richard/Food`.  
  Options:
  - `-l`: Long list format (shows details like ownership and modification time).
  - `-a`: Lists all files, including hidden ones.
  - `-lt`: Lists files in order of creation.
  - `-ltr`: Lists files in reverse order of creation.
- `pushd` and `popd`: Save and navigate directories.  
  - `pushd`: Saves the current directory and moves to a new one.  
    Example: `$pushd /home/richard/Food` (saves `/home/richard` and moves to `/home/richard/Food`).
  - `popd`: Returns to the saved directory.  
    Example: `$popd` (returns to `/home/richard`).
- `mv`: Move or rename files/directories. Requires two arguments: the source and the destination.  
  Example: `$mv /home/richard/Food/Fanta /home/richard/Drink`.  
  To rename: `$mv Food/Burga Food/Burger`.
- `cp`: Copy files/directories. Requires two arguments: the source and the destination.  
  Example: `$cp Food/Burger/Ingredients.txt Food/Sandwich`.  
  Use `-r` (recursive) to copy directories.
- `rm`: Remove files/directories. Add the file or directory as the argument.  
  Example: `$rm Food/Burger/Ingredients.txt`.  
  Use `-r` to remove directories.
- `cat`: Read file contents. Add the file name as the argument.  
  Example: `$cat Food/Burger/Ingredients.txt`.  
  To edit: `$cat > Food/Burger/Ingredients.txt` (input: `Lettuce`), then press `Ctrl + D` to save.
- `touch`: Create an empty file. Add the file name as the argument.  
  Example: `$touch /home/richard/Food/Burger/Recipe.txt`.
- `more`: View file contents page by page. Add the file name as the argument.  
  Navigation:
  - **Spacebar**: Scroll one screenful.
  - **Enter**: Scroll one line.
  - **b**: Scroll backward.
  - **/**: Search text.
  - **q**: Quit.
- `less`: View file contents without loading the entire file. Add the file name as the argument.  
  Navigation:
  - **Up/Down Arrow**: Scroll one line.
  - **/**: Search text.
  - **q**: Quit.

### Help Commands
- `whatis`: Provides a one-line description of a command.  
  Example: `$whatis ls`.
- `man`: Displays the manual page for a command.  
  Example: `$man ls`.
- `--help`: Displays a brief description and usage options for a command.  
  Example: `$ls --help`.
- `apropos`: Searches the man pages for a keyword.  
  Example: `$apropos list`.

### Bash Shell
Also known as Bourne Again Shell. Use `echo $SHELL` to check what shell is being used. Use `chsh` to change the default shell. You will be prompted for your password and then the new shell.  
Other types of shells: Bourne Shell(sh), C Shell(csh or tcsh), Korn Shell(ksh), Z Shell(zsh)  
Some features of the Bash shell include:
- **Auto-completion**: Press `Tab` to auto-complete commands or file paths.
- **Aliases**: Set shortcuts for commands.  
  Example: `alias dt=date`.  
  To make this persist we can do `echo 'alias dt=date' >> /home/richard/.profile`  
- Use `history` to check previously used commands and aliases.
- **Environment Variables**: Use `env` to see all environment variables.  
  Example: `export NEW_VAR="value"`.
- **Path Variable**: Use `echo $PATH` to check the PATH variable.  
  Use `which` to find the location of a program.  
  Example: `which ls`.  
  Add a new directory to PATH:  
  Example: `export PATH=$PATH:/opt/obs/bin`.
- **Custom Bash Prompt**: Customize the Bash prompt using `PS1`.  
  Example: `PS1="[\d \t \u@\h:\w ] $ "`  
  Output: `Mon Mar 3 11:12:44 richard@ubuntu:~ ] $`.  
  Note: These changes may not be persistent across sessions, so we should use the command line `>> ~/.profile` to the end of it.  
  Example: ` echo 'PS1="[\d \t \u@\h:\w ] $ "' >> /home/richard/.profile`

### Linux Kernel

The Linux Kernel is the core component of the Linux operating system. It acts as the bridge between the hardware and the software, managing resources and ensuring that applications can run efficiently and securely. To better understand its role, let’s use an analogy:

- **Library (OS)**: The entire system, including hardware and software.
- **Books/Resources (Hardware Resources)**: CPU, memory, disk, network, etc.
- **Students (Processes)**: Applications or programs running on the system.
- **Librarian (Kernel)**: Manages access to resources, ensures fairness, and enforces rules.

The Linux Kernel is responsible for four main tasks:

1. **Memory Management**:
   - The kernel keeps track of memory usage, ensuring that each process has access to the memory it needs without interfering with other processes.
   - It handles **virtual memory**, which allows the system to use disk space as an extension of RAM when physical memory is full (a process known as **swapping**).
   - It also enforces memory protection, preventing processes from accessing memory allocated to other processes.

2. **Process Management**:
   - The kernel decides which processes get access to the CPU, when, and for how long. This is known as **scheduling**.
   - It handles process creation, termination, and communication (e.g., inter-process communication or IPC).
   - The kernel ensures that processes run efficiently and fairly, even when multiple applications are competing for resources.

3. **Device Drivers**:
   - Device drivers act as translators between hardware devices (e.g., printers, keyboards, network cards) and the operating system.
   - The kernel loads and manages these drivers, making hardware accessible to applications.
   - Linux supports a wide range of hardware devices thanks to its modular design, allowing drivers to be loaded and unloaded dynamically as needed.

4. **System Calls and Security**:
   - System calls are the interface between user-space applications and the kernel. They allow programs to request services from the kernel, such as reading a file or creating a process.
   - The kernel enforces security policies, such as file permissions, user privileges, and access control, to ensure the system remains secure.

#### Kernel Space vs. User Space
- **Kernel Space**:
  - A privileged area where the kernel and device drivers operate.
  - Direct access to hardware and system resources.
  - Code running here has full control over the system.
- **User Space**:
  - Where applications and user processes run.
  - Restricted access to hardware and system resources.
  - Applications must request access via system calls.

#### Monolithic and Modular Design
- The Linux Kernel is **monolithic**, meaning all core components (memory management, process management, device drivers, etc.) run in a single address space.
- However, it is also **modular**, allowing additional functionality to be loaded as kernel modules at runtime. This makes the kernel flexible and efficient, as only the necessary components are loaded into memory.

#### Checking Kernel Information
- Use the `uname` command to check kernel details:
  ```bash
  $ uname -r  # Displays the kernel release version
  $ uname -a  # Displays all system information (kernel name, version, hostname, etc.)

