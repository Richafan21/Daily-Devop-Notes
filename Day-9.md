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
