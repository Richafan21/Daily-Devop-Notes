# Security

---

## Troubleshooting

### Diagnosing Connectivity Problems

Possible causes for a connection timeout includes:

- A disconnected local network interface.
- DNS resolution failure.
- Routing problems.
- Server-side issues, such as an inactive web service.


### Step 1: Verify Local Network Connectivity

First, confirm that the local network interface is active using the `ip link` command:

```bash
ip link
```
Example output:

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
   link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp1s0f1: <BROADCAST,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
   link/ether 08:97:98:6e:55:4d brd ff:ff:ff:ff:ff:ff
```

Since `enp1s0f1` is `UP`, the local connection is operational.


### Step 2: Check DNS Resolution

Ensure that the hostname resolves to the correct IP address using `nslookup`:

```bash
nslookup caleston-repo-01
```

Expected response:

```bash
Server:         192.168.1.100
Address:        192.168.1.100#53

Non-authoritative answer:
Name:   caleston-repo-01
Address: 192.168.2.5
```

This confirms that `caleston-repo-01` maps to `192.168.2.5`.


### Step 3: Test Remote Connectivity

Use `ping` to check if the server is reachable:

```bash
ping caleston-repo-01
```

If all packets are lost:

```bash
3 packets transmitted, 0 received, 100% packet loss, time 2034ms
```

This suggests a potential routing issue or server-side problem.


### Step 4: Trace the Network Route

Use `traceroute` to identify where connectivity breaks:

```bash
traceroute caleston-repo-01
```

Key findings:

- The first router (`192.168.1.1`) responded correctly.
- The second router (`192.168.2.1`) also responded correctly.
- The connection timed out beyond the second router, indicating a problem near the repository server.


### Step 5: Troubleshoot the Repository Server

Since the issue is likely on the server, Jackie collaborated with the support team for further diagnostics.

### Check if the HTTP Service is Running

Use `netstat` to confirm that the web server is listening on port `80`:

```bash
netstat -an | grep 80 | grep -i LISTEN
```

Expected output:

```bash
tcp6       0      0 :::80                   LISTEN
```

This verifies that the web service is active.

### Check Network Interfaces on the Server

```bash
ip link
```

Output showed that `enp1s0f1` was `DOWN`:

```bash
2: enp1s0f1: <BROADCAST,BROADCAST,MULTICAST,DOWN> mtu 1500 qdisc fq_codel state DOWN mode DEFAULT group default qlen 1000
```

### Bring the Interface Back Up

```bash
ip link set dev enp1s0f1 up
```

Verify the status again:

```bash
ip link
```

```bash
2: enp1s0f1: <BROADCAST,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
```

With the network interface now active, the issue was resolved.

---

## Summary of Key Commands

| Command | Purpose |
|---------|---------|
| `ip link` | Check network interface status |
| `nslookup hostname` | Verify DNS resolution |
| `ping hostname` | Test basic connectivity |
| `traceroute hostname` | Identify where connectivity fails |
| `netstat -an (pipe sign) grep 80` | Check if the web server is running |
| `ip link set dev enp1s0f1 up` | Reactivate a network interface |

---

## Linux Account

### Security in Linux

Linux systems use authentication mechanisms based on users and passwords to regulate system access. Several tools and frameworks help manage authentication:

- **PAM (Pluggable Authentication Modules)**: Defines how services and applications handle authentication.
- **Network Security Tools**: Utilities like `iptables` and `firewalld` control access to network services.
- **SSH (Secure Shell)**: Enables secure remote access, with SSH hardening preventing unauthorized connections.
- **SELinux**: Enforces strict security policies to isolate applications running on the same system.


## User and Group Accounts

### What is a Linux Account?

Each Linux user has an account that contains details such as:

- **Username**
- **Password**
- **User ID (UID)**
- **Home directory**
- **Default shell**

Account information is stored in `/etc/passwd`. Example:

```sh
$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bob:x:1000:1000:Bob Kingsley,,,:/home/bob:/bin/bash
```

Linux groups help organize users based on roles. Group details, including Group ID (GID), are stored in `/etc/group`:

```sh
$ cat /etc/group
developers:x:1003:bob,michael
```

To check a user’s account and group memberships:

```sh
$ id michael
uid=1001(michael) gid=1001(michael) groups=1001(michael),1003(developers)
```

#### Types of User Accounts

1. **Regular User Accounts** – For individual users requiring system access.
2. **Superuser Account** – The `root` account (UID 0) with unrestricted access.
3. **System Accounts** – Created for software and services, often with UIDs below 1000.
4. **Service Accounts** – Similar to system accounts but dedicated to specific services like `nginx`.

### Switching Users and Privilege Escalation

#### Using the `su` Command

The `su` (substitute user) command lets users switch accounts, including to `root`:

```sh
$ su -
Password:
# (Now running as root)
```

To run a command as another user without switching:

```sh
$ su -c "whoami"
Password:
root
```

> **Note**: `su` is useful, but `sudo` is recommended for better security.

### Using the `sudo` Command

`sudo` allows trusted users to run commands as root without directly logging in as root:

```sh
$ sudo apt-get install nginx
[sudo] password for michael:
```

The default configuration for `sudo` is stored in `/etc/sudoers`. Example:

```sh
$ cat /etc/sudoers
root    ALL=(ALL:ALL) ALL
%sudo   ALL=(ALL:ALL) ALL
bob     ALL=(ALL:ALL) ALL
sarah   localhost=/usr/bin/shutdown -r now
```

### Understanding the `sudoers` File

- Lines starting with `#` are comments.
- The first field specifies the user or group (`%group` for groups) granted privileges.
- The second field (typically `ALL`) defines applicable hosts.
- The third field (in parentheses) defines the users/groups the command can be executed as.
- The fourth field lists allowed commands (`ALL` for full access or specific commands for restrictions).

Properly configuring `sudoers` ensures only authorized users execute sensitive commands, minimizing security risks.

---

## Access Control Files

Some key Linux access control files essential for maintaining system security and managing user access can be found under the `/etc` directory:

- `/etc/passwd` – Stores user account details.
- `/etc/shadow` – Holds encrypted passwords and password expiry information.
- `/etc/group` – Manages group memberships.

### Important Notes
- Always use built-in commands to modify these files rather than editing them directly with a text editor.
- The `/etc` directory is world-readable by default, but only the root user can make modifications.

### The `/etc/passwd` File

The `/etc/passwd` file contains user account details. Its format is:

```
USERNAME:PASSWORD:UID:GID:GECOS:HOMEDIR:SHELL
```

Where:

- **USERNAME**: The user's login name.
- **PASSWORD**: An `x` placeholder, indicating that the encrypted password is stored in `/etc/shadow`.
- **UID**: The unique numeric user identifier.
- **GID**: The primary group numerical identifier.
- **GECOS**: An optional field for additional user information (e.g., full name).
- **HOMEDIR**: The path to the user's home directory.
- **SHELL**: The user's default shell (such as Bash).

Passwords are not stored here; instead, an `x` appears in the password field, indicating that encrypted passwords are located in `/etc/shadow`.

### Example:

```sh
$ grep -i ^bob /etc/passwd
bob:x:1001:1001::/home/bob:/bin/bash


