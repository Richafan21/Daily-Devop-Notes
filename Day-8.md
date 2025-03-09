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

