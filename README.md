# Daily-Devop-Notes

*Keeping track of what I learn each day on my way to become a DevOps Engineer. Commands will be represented by a $ sign before it.*

## Day 1: Linux Basics

Linux shell is a program that allows text-based interactions between the program and the user.  
The home directory (`/home/richard`) is like a dedicated locker assigned to you. Other users cannot access anything within your own home directory. By default, it is represented by `~`.

### Command and Arguments

- `echo`: A **command** that acts like a print statement. It does not do anything unless you add an argument (e.g., `$echo hello`).
- `uptime`: A **command** that does not require an argument. It tells us how long the system has been running since the last reboot.

Linux also has **options** (or **flags**), which are added after a command to alter its behavior. Options are usually represented by a single letter preceded by a hyphen (e.g., `-n`).  
It can be challenging to memorize all the options, so it’s better to refer to the command’s help options.  

There are two types of commands: **Internal** and **External**. Built-in commands are internal, while external commands are binary programs or scripts, usually located in distinct files in the system. These can come preinstalled or be installed by the user. You can use `type` followed by another command as the argument to determine whether that command is internal or external.  
Example: `$type cd` (output: `cd is a shell builtin`).

### Common Internal Type Commands
- `cd`: Change directory. Add the directory you want to move to as the argument.  
  Example: `$cd /home/richard`.
- `mkdir`: Make directory.  
  Example: `$mkdir new_folder`.
- `pwd`: Print working directory.  
  Example: `$pwd` (output: `/home/richard`).
