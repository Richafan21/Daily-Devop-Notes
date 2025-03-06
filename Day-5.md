# VI editor and Networking

---

## Vi editor

The VI editor is a console-based text editor used for editing files and writing code. To open a file in VI editor, simply use the command `vi` and the path to the file as the argument.  

### The Three Operation Modes

- **Command Mode**
- Editor goes to the **Command Mode** by default, which only understands commands.
- You can switch to **Insert Mode** using "i"
- You can switch to the **Last Line Mode** using colon ":".

- **Insert Mode**
- You can enter this mode by pressing "i" when in the **Command mode**.
- In this mode, you can edit the file such as add or remove text.
- Press "esc" to leave and go back to "Command Mode"

- **Last Line Mode**
- Enter this mode by using ":" during **Command Mode**
- In this mode, you can:
- Save file
- Discard Changes
- Exit vi editor
- "esc" to go back to **Command Mode**

### More on the Three Operation Modes

- **Command Mode**
- In this mode, you will see a cursor.
- You can use the direction keys(or k,j,h,l) to move around the cursor.
- Allows you to use the mouse to highlight, copy, paste, or move texts.
- To copy: Move the cursor to the intended line and use "y y" command.
- To paste: Move the cursor above the line you want to paste and press "p"
- To save the file: Use uppercase "Z Z"
- To delete a letter: Move the cursor to the intended location and type "x"
- To delete a line: Move the cursor to any location on the line and type "d d"
- To delete a specific amount of lines: Use "d `number_of_lines` d"
- To undo: Type "u"
- To redo: Press "ctrl r"
- To find a string: Use "/" followed by the pattern you want to search for. It will search the file from the current line downwards. Example: `/line`.
- To find a string: Use "?", which does the same thing as "/" except searches upwards.
- To find next and previous: Use "n" and "N". 

- **Insert Mode**
- After entering this mode from Command Mode using either "i", "a", "o", or "A".
- There should be "Insert" at the bottom, telling you that you're in this mode.
- Behaves similar to notepad, just use backspace and enter.

- **Last Line Mode**
- ":" To enter this mode.
- To save: Type in ":w"
- To exit: Type in ":q"
- To save and exit: Type in ":wq"
- To exit but not save(without Confirmation): Use ":q!"

### VIM

VIM stands for VI Improved  
- Open a file with vim the same way as vi but with `vim`.

---

## Networking

### DNS

When two computers are on the same network. The computer A can use the ping command to test connectivity of the computer B.  
 
Example:
```bash
[~]$ ping 192.168.1.11
Reply from 192.168.1.11: bytes=32 time=4ms TTL=117
Reply from 192.168.1.11: bytes=32 time=4ms TTL=117
```

As you can see from the example, it is hard to always remember and type out an IP address. Fortunately, we can assign an alias to help us.
```bash
[~]$ cat >> /etc/hosts
192.168.1.11    db
```
That successfully appends an entry in system A's `/etc/hosts` file mapping the IP address to the alias.  
After adding the entry, the ping command will use the mapping from the hosts file.  
The `/etc/hosts` file provides hostname-to-IP mappings for a system. The system does not verify whether the alias matches the actual hostname.

Example:

```sh
cat >> /etc/hosts <<EOF
192.168.1.11    db
192.168.1.11    www.google.com
EOF
```

Pinging either alias directs traffic to the same IP:

```sh
ping db
ping www.google.com
```

Any command—ping, ssh, curl—first checks `/etc/hosts` for mappings. This is known as **name resolution**.

### Transition to Centralized DNS

For small networks, managing host entries in `/etc/hosts` may work:

```sh
cat >> /etc/hosts <<EOF
192.168.1.10 web
192.168.1.11 db
192.168.1.12 nfs
EOF
```

However, as networks grow, updating every system manually becomes impractical. Instead, a **centralized DNS server** allows all hosts to query a single point for name resolution.

#### Configuring DNS Resolution

To use a DNS server (e.g., `192.168.1.100`), update `/etc/resolv.conf`:

```sh
cat > /etc/resolv.conf <<EOF
nameserver 192.168.1.100
EOF
```

If a hostname isn't found locally, the system queries the DNS server.

### Local vs. Centralized Resolution

Local `/etc/hosts` entries take precedence over DNS records, as configured in `/etc/nsswitch.conf`:

```sh
cat /etc/nsswitch.conf | grep hosts
hosts:          files dns
```

Example of adding a test hostname locally:

```sh
cat >> /etc/hosts <<EOF
192.168.1.115 test
EOF
```

Testing resolution:

```sh
ping test
```

If a hostname is not in `/etc/hosts` or DNS, resolution fails:

```sh
ping www.facebook.com
ping: www.facebook.com: Temporary failure in name resolution
```

To resolve external domains, internal DNS servers can forward queries to public nameservers like Google's `8.8.8.8`.

### Public DNS and Domain Hierarchy

DNS follows a hierarchical structure:

- **Root (`.`)** → **TLDs (`.com`, `.net`)** → **Subdomains (`mail.google.com`)**
- Internal domains follow a similar structure, e.g., `web.mycompany.com`, `mail.mycompany.com`.

#### Search Domains

To simplify hostname resolution, add search domains in `/etc/resolv.conf`:

```sh
cat >> /etc/resolv.conf <<EOF
nameserver 192.168.1.100
search mycompany.com
EOF
```

Now, `ping web` is interpreted as `ping web.mycompany.com`.

Multiple search domains can be specified:

```sh
cat >> /etc/resolv.conf <<EOF
nameserver 192.168.1.100
search mycompany.com prod.mycompany.com
EOF
```

The system appends each search domain sequentially until a match is found.

### DNS Record Types & Diagnostics

DNS servers store different record types:

- **A Record:** Maps hostname to IPv4 address.
- **AAAA Record:** Maps hostname to IPv6 address.
- **CNAME Record:** Maps an alias to another hostname (e.g., `eat` → `fooddelivery.com`).

To check DNS resolution without `/etc/hosts` interference:

```sh
nslookup www.google.com
dig www.google.com
```

Example `dig` output:

```sh
;; ANSWER SECTION:
www.google.com. 245 IN A 64.233.177.103
www.google.com. 245 IN A 64.233.177.105
...
;; SERVER: 8.8.8.8#53(8.8.8.8)
```

These tools help diagnose DNS issues and verify correct resolution.






