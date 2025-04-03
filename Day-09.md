# IPTABLES

---

## Introduction

Another helpful tool for network security is configuring IP Table rules on Linux servers.  
Securing remote access, such as SSH and SCP file transfers, requires that the SSH service is active on the remote server, allowing you to connect from your client.

Ensure the following prerequisites are met:
- Valid authentication mechanisms (username/password or SSH key-based).
- Port 22 open between your client and the remote server.

Note: In complex environments featuring multiple clients and servers interconnected by various routers ands witches, implementing robust network security is essential.  
You can secure the network using external firewall appliances or apply filtering rules directly on each server with tools like iptables and firewalld on Linux.

### How to configure local iptables rules on a Linux server

We will use the following example going foward:

| Device | IP Address |
|--------|------------|
| Client Laptop| 172.16.238.187|
|Application server|	172.16.238.10|
|Database server |	172.16.238.11|

Without a firewall, all servers can communicate freely. We will enhance security by enforcing these rules:

- Allow SSH access from your laptop to the application server.
- Permit HTTP access (port 80) to the application server only from the client laptop, blocking other servers (e.g., devdb01).
- Allow the application server to connect to the database server on port 5432 for database operations.
- Grant the application server HTTP access to the software repository server (kailston-repo-01).
- Block outgoing internet access from the application server.
- Restrict the database server to accept connections on port 5432 solely from the application server.

### Establishing SSH Connectivity

On Red Hat and CentOS, iptables is installed by default. On Ubuntu, installation may be required:  
`[richard@devapp01 ~ ] $ sudo apt install iptables`

After installing, list the default iptables rules with:  
```bash
[richard@devapp01 ~]$ sudo iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination


Chain FORWARD (policy ACCEPT)
target     prot opt source               destination


Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

### Understanding IPTABLES Chains

Iptables uses three main chains:
- Input Chain: Manages incoming traffic. For instance, adding a rule here allows SSH connections from your client laptop.
- Output Chain: Controls traffic originating from the server, including outbound connections like databse queries.
- Forward Chain: Typically used by network routers to forward traffic between devices. Standard Linux servers rarely use this chain.

With no custom rules in place, it is defaulted to accept all incoming and outgoing traffic.

A chain is essentially a list of rules. Each rule checks network packets and decides whether to accept or drop them based on defined conditions.  
These could be souce IP, destination IP, port number, or protocol. If a packet does not match any rule, it continues to the next rule.  
This continues until it either matches one or reachers the chain's end.

Example:  
A packet from client 01 meets the first rule in the INPUT chain and is accepted.  
A packet from client 02 does not match the first rule and is evaluated by subsequent rules.  
A packet from client 05 might not match any allow rules and is eventually dropped by a default drop rule

---

## Iptables Securing the Environment

### Configuring SSH Access

Start by adding an incoming rule on the development server that allows SSH connections solely from the designated client. Run:

`iptables -A INPUT -p tcp -s 172.16.238.187 --dport 22 -j ACCEPT`

This is how the command works:  
- `-A INPUT`: Appends a rule to the INPUT chain.
- `-p tcp`: Specifies the TCP protocol.
- `-s 172.16.238.187`: Restricts the rule to connections coming from the client laptop.
- `--dport 22`: Indicates that the rule applies to the SSH port (22).
- `-j ACCEPT` Accepts the connection when all conditions are met.  

After adding the rule, verify it by listing the iptables rules with:  
`iptables -L`

### Blocking Unauthorised SSH Attempts

By default, if another client tries to access SSH without a specific allow rule, the connection would follow the default policy, which typically accepts all connections.  
Since our requirement is to restrict the SSH access to the specific client, add a rule that drops SSH traffic from all other sources:

`iptables -A INPUT -p tcp --dport 22 -j DROP`

Lising the iptables rules now will display an output like this:  
```bash
[richard@devapp01 ~]$ iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     tcp  --  172.16.238.187      anywhere             tcp dpt:ssh
DROP       tcp  --  anywhere             anywhere             tcp dpt:ssh


Chain FORWARD (policy ACCEPT)
target     prot opt source               destination


Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

### Configuring Outbound Traffic

On the development application server, additional configurations are needed to manage outbound connections:  
- Allow connections to the DB server on port 5432.
- Permit connections to the software repository server on port 80.
- Drop general HTTP (port 80) and HTTPS (port 443) traffic to the Internet.
- Explicitly allow HTTP access on port 80 from the client laptop.
- Add the following OUTPUT rules:
```bash
[richard@devapp01 ~]$ iptables -A OUTPUT -p tcp -d 172.16.238.11 --dport 5432 -j ACCEPT
[richard@devapp01 ~]$ iptables -A OUTPUT -p tcp -d 172.16.238.15 --dport 80 -j ACCEPT
[richard@devapp01 ~]$ iptables -A OUTPUT -p tcp --dport 443 -j DROP
[richard@devapp01 ~]$ iptables -A OUTPUT -p tcp --dport 80 -j DROP
```
Once these rules are applied, listing the iptables configuration will reveal three input rules and four output rules.  
This ensures that only the specified outbound communications are permitted.

## Allowing Specific HTTPS Traffic

If you need to access a particular site using HTTPS from the devapp-01 server (e.g., to 172.16.238.100), the general DROP rule for port 443 would normally block this connection. To allow HTTPS access to this specific destination, insert an ACCEPT rule at the top of the OUTPUT chain:

`[richard@devapp01 ~]$ iptables -I OUTPUT -p tcp -d 172.16.238.100 --dport 443 -j ACCEPT`  
The -I option inserts the rule at the top of the chain, making sure it takes precedence over the subsequent DROP rules.

### Deleting a Rule

The `-D` option can let you delete a rule based on position:  
`[richard@devapp01 ~]$ iptables -D OUTPUT 3`  
The above command will delete the rule at position 3 of the output chain.





