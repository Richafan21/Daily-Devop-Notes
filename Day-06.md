# Networking

---

## DNS

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

### Configuring DNS Resolution

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

### Search Domains

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

---

## Networking Basics

### Switches and Local Networks
A switch creates a local network by connecting multiple devices. Each device (host) requires a network interface, either physical or virtual, to connect. To view a host’s network interfaces, use:

```bash
ip link
```
Example output for `eth0`:

```bash
eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
```

### Assigning IP Addresses
If the network address is `192.168.1.0`, devices will receive IPs from this range. To assign IPs, first ensure the `eth0` interface is active:

```bash
ip addr add 192.168.1.10/24 dev eth0  # Device A
ip addr add 192.168.1.11/24 dev eth0  # Device B
```

With IPs assigned, devices can communicate via the switch. To test connectivity, use:

```bash
ping 192.168.1.11
```

### Connecting Multiple Networks with a Router
Now consider another network (`192.168.2.0`), where:
- Device C: `192.168.2.10`
- Device D: `192.168.2.11`

To enable communication between `192.168.1.0` and `192.168.2.0`, a **router** is required. The router has:
- `192.168.1.1` (first network)
- `192.168.2.1` (second network)

When device B (`192.168.1.11`) needs to reach C (`192.168.2.10`), it must forward packets via the router. This requires configuring a route:

```bash
ip route add 192.168.2.0/24 via 192.168.1.1
```
Similarly, device C needs a route to `192.168.1.0` via `192.168.2.1`:

```bash
ip route add 192.168.1.0/24 via 192.168.2.1
```

### Viewing and Configuring Routes
To check the routing table, use:

```bash
route
```
Example routing table:

```bash
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
192.168.1.0     192.168.2.1     255.255.255.0   UG    0      0        0 eth0
0.0.0.0         192.168.2.1     255.255.255.255 UG    0      0        0 eth0
```

A default gateway allows devices to route unknown traffic through the router:

```bash
ip route add default via 192.168.2.1
```

### Troubleshooting and Persistence
If routing issues occur, verify the routing table and update incorrect entries:

```bash
ip route add 192.168.1.0/24 via 192.168.2.2
```

**Note:** These changes are temporary and will be lost on reboot. To persist, update network configuration files (e.g., `/etc/network/interfaces`).

### Key Commands Summary

| Command | Purpose | Example |
|---------|---------|---------|
| `ip link` | List network interfaces | `ip link` |
| `ip addr` | Display IP addresses | `ip addr` |
| `ip addr add` | Assign an IP address | `ip addr add 192.168.1.10/24 dev eth0` |
| `route` | View routing table | `route` |
| `ip route add` | Add a routing entry | `ip route add default via 192.168.2.1` |
