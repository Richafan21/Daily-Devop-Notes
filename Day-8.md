# File Permissions

---

## User Management

### Creating a New User

System administrators can utilise the `useradd` command to create a new local user in the system.

Example:
```bash
useradd richard
```
This command creates richard with a system-generated UID and GID. Home directory is set to `/home/richard` and login shell to `/bin/sh` by default.

To set a password, use `passwd` command with the user as the argument.

Example:
```bash
passwd richard
```
Both these commands must be used with root privileges.

### Options with 'useradd' Command

| Option | Purpose | 
|---------|---------|
| `-u` | Specify a custom UID | 
| `-g` | Specify the primary group via a custom GID | 
| `-d` | Define a custom home directory path | 
| `-s` | Set default login shell |
| `-c` | Add a commment | 
| `-e` | Set account experiation date| 
| `-G` | Add the user to additional (secondary) groups |

Example:
```bash
useradd -u 2004 -g 2004 -d /home/richa -s /bin/bash
```

Use the `id` command with user as argument to check the user's settings.

### User Deletion and Group Management

To delete a user account you can use the `userdel` command with the username as argument.

To add a new group, use `groupadd` command with the group name as the argument. Additionally, option -g lets you specify the GID.

To delete a group, use the `groupdel` command along with the group name as argument.

---

## File Permissions and Ownership

When listing files using `ls -l`, the first character in the output indicates the file type.

Example:
```bash
ls -l /home/richard
drwxrwxr 2 richard richard 4096 Mar 2 20:07 Africa
```
In the output above, the initial letter (`d`) signifies that `/home/richard` is a directory. Other types of identifiers include:
- `-` for regular file
- `c` for character device
- `b` for block device
- `l` for sumbolic link
- `s` for socket
- `p` for named pipe

The characters following the file type (e.g. `rwx`) represents permissions grouped into three categories:

1. Owner (u) permissions: the first three characters after the file type.
2. Group (g) permissions: The next three characters.
3. Others (o) permissions: The last three characters

For each permissions set:
- `r` (read): Permits reading the file (octal value of 4)
- `w` (write): Allows modigying the file (octal value of 2)
- `x` (execute): Gives permissions to run the file as a program (octal value of 1)

This means that `rwx` = 7(4+2+1), `r-x` = 5(4+0+1)

### Changing File Permissions with `chmod`

There are two modes to modify file permissions using `chmod`.

**Symbolic mode**

In this mode, you specify the target(user, group, or others) using the respective option letter (u, g, or o). You then adjust permissions using `+` or `-` to add or remove permissions.

Exmaples:
```bash
chmod u+rwx test-file
chmod ugo+r test-file
chmod o-rwx test-file
# These options can be combined for example to give full permissions to the owner.
# Add read and execute permissions to group and remove all permissions for others:
chmod u+rwx, g+r-x, o-rwx
```

**Numeric Mode**

In numeric mode, you provide a three-digit octal number:
- The first digit sets permissions for the owner.
- The second sets permissions for the group.
- The third sets permissions for others.

Examples:
```bash
chmod 777 test-file # Grants read, write, and execute for everyone.
chmod 555 test-file # Grants read ad execute permissions to everyone.
chmod 660 test-file # Grants read and write permissions to owner and group, but none to others.
```

### Changing ownershup with chown and chgrp.

To change both the owner and group of a file, use `chown`.
`chown (owner):(group) (file_name)`  
If you need to change the owner:
`chown (user) (file_name)`  
To change just the group, use `chgrp`:
`chgrp (group_name) (file_name)`

## SSH

SSH stands for secure shell, and it is a widely used protocol for logging in remotely and executing commands on another computer.  
You can use ssh via `ssh <hostname OR IP address>`. You can soecify the username using the `user@` syntax or the `-l` flag. `ssh <user>@<hostname OR IP address>`

Example for conecting to a server with hostname "devapp01"
```bash
ssh <hostname_OR_IP_Address>
ssh <user>@<hostname_OR_IP_Address>
ssh -l <user> <hostname_OR_IP_Address>
[~]$ ssh devapp01
richard@devapp01s password:
Last login: Tue Apr  7 20:08:58 2020 from 192.168.1.109
[richard@devapp01 ~]$
```

### Passwordless SSH with Key Pairs

You can avoid entering your password every time by configuring a passwordless authentication using SSH key pairs. A key pair consists of:
- **Private key**: Kept secure on your client device.
- **Public key**: Shared and installed on your remote server.

When the public key is added to the remote server's `~/.ssh/authorized_keys` file, the system will grant access to any client possessing the corresponding private key.

### Generating the key pair.  

On your client machine, generate a key pair using the following command. You can optionally secure your key with a passphrase, through that will prompt you to enter it everytime the key is utilized.

```bash
[richard@fan-lp10 ~]$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/richard/.ssh/id_rsa):
/home/richard/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/richard/.ssh/id_rsa.
Your public key has been saved in /home/richard/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:PCRTdbxzzffzmi8uunjn5V/1LZCG0BvhVJYXBr9gYsE richard@fan-lp10
The keys randomart image is:
+---[RSA 2048]----+
|          o oo   |
|         +E=oo   |
|        o O *..o. |
|         =o .o..  |
|        S O + .+  |
|         . . *    |
|          oo+     |
|           oo.+.  |
|          ..o+o   |
+----[SHA256]-----+
```
During this process, the public key is stored in `/home/richard/.ssh/id_rsa.pub`. While the private key remains safe in `/home/richard/.ssh/id_rsa`.

### Copying the Public Key to the Remote server

To enable passwordless SSH login, copy your public ket to the remote server using the `ssh-copy-id` and an argument such as `richard@fan` command. 

## SCP

SCP stands for secure copy. This is a powerful tool for transferring files and directoriessecurely over SSH. Acts like `cp` in Linux.

### Copying a file using SCP.

Let's say we are trying to transfer a file named 'richard-code.tar.gz from our laptops to the same directory on a remote server(devapp01), you can do it by:  

[richard@fann-lp10 ~]$ scp /home/richard/fan-code.tar.gz devapp01:/home/richard
richard@creators's password:
richard-code.tar.gz 

If you want to transfer entire directories, include the `-r` option to copy recursively. To maintain original file ownership and permissions, ahh the `-p` flag.
