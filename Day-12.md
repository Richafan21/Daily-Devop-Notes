# Service Management and Storage in Linux

---

## SYSTEMD Tools

### Managing Services with systemctl

We will use Docker as an example service  

**Common Commands**:  
- To start a service: `[~]$ systemctl start docker`
- To stop a service: `[~]$ systemctl stop docker`
- To restart a service (this stops and then starts the service again): `[~]$ systemctl restart docker`
- To reload a service without interrupting its normal functionality: `[~]$ systemctl reload docker`
- To enable a service so that it starts automatically at boot: `[~]$ systemctl enable docker`
- To disable a service (preventing it from starting automatically at boot): `[~]$ systemctl disable docker`
- To check the status of a service: `[~]$ systemctl status docker`

To check if the service is running as intended, run `systemctl status docker`.

### Reloading SystemD Configuration

After modifying a unit file, make sure to reload the system manager configuration to apply those changes:  
`[~]$ systemctl daemon-reload`

For editing unit files, you can use the `system edit` command. Example of fully replacing the configuration for the Project Mercury service:  
`[~]$ systemctl edit project-mercury.service --full`  
After saving the changes, they will be applied automatically without requireing daemon reload.  

### Working with System Targets  

Systemd targets define the state of the machine. You can view or change the default target with `systemctl`  

- To view the current default target: `[~]$ systemctl get-default`
- To change the default target (for example, to multi-user mode): `[~]$ systemctl set-default multi-user.target`
- Additionally, list all units (both loaded and attempted) using: `[~]$ systemctl list-units --all`

If you want to view only active units, run `systemctl list-units` without the `--all` flag.

### Querying Logs with journalctl  
The `journalctl` command is an essential troubleshooting tool for querying the systemd journal logs. By default, running journalctl displays all log entries from oldest to newest.  
Example: `[~]$ journalctl -u docker.service`
