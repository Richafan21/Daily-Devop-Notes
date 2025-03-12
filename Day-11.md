# Service Management

---

## Creating a SYSTEMD Service

Here are the requirements for a specific service:  
- A shell script named `project-mercury.sh` located in `/usr/bin` must execute as a background service.
- The service should be enabled to start automatically during boot.
- The script launches a Python Django application that depends on a PostgreSQL database; therefore, PostgreSQL must be running prior to the application's start.
- The service must run under a pre-created account called `project_mercury` rather than the default root user.
- In case of application failure, the service should restart automatically, waiting 10 seconds between attempts.
- The service must not restart automatically if it is manually stopped by an administrator.
- All service events—including start, stop, and errors—must be logged for later analysis.
- The service should adhere to the `multi-user.target` since Bob’s system does not require a graphical environment.

We can construct the service unit file in gradual steps:  

### 1. Creating the Basic Service Unit
Start by defining a basic service unit file named `project-mercury.service` under the `/etc/systemd/system/` directory.  
Initially, the unit file only needs to execute the command in the background because `project-mercury.sh` is a Bash script.  

Minimal service unit definition:  
```ini
[Service]
ExecStart=/usr/bin/project-mercury.sh
```

To start the service in the background:  
```bash
sudo systemctl start project-mercury.service
```

You can verify whether the service is active with:  
```bash
sudo systemctl status project-mercury.service
```

Finally, to stop the service:  
```bash
sudo systemctl stop project-mercury.service
```

---

### 2. Enabling the Service on Boot
To automate the start of the service during boot, add an `[Install]` section to the service unit file.
The `WantedBy` directive ties the service to the `multi-user.target`, which is appropriate for server environments.

Updated service unit file:
```ini
[Service]
ExecStart=/usr/bin/project-mercury.sh

[Install]
WantedBy=multi-user.target
```

---

### 3. Running the Service Under a Specific User
By default, the service runs as root. To have the service operate under the `project_mercury` account, include the `User` directive within the `[Service]` section:  
```ini
[Service]
ExecStart=/usr/bin/project-mercury.sh
User=project_mercury

[Install]
WantedBy=multi-user.target
```

---

### 4. Configuring Automatic Restarts
Configure the service to restart automatically if it fails. Use the `Restart` directive set to `on-failure` and specify a 10-second delay between restart attempts using `RestartSec`:
```ini
[Service]
ExecStart=/usr/bin/project-mercury.sh
User=project_mercury
Restart=on-failure
RestartSec=10
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

---

### 5. Defining a Dependency on the PostgreSQL Service
Since the Python Django application depends on the PostgreSQL database, ensure that `project-mercury.service` starts only after PostgreSQL is active.
To achieve this, add a `[Unit]` section that includes a description, a documentation URL, and the `After` and `Requires` directives to express the dependency.
```ini
[Unit]
Description=Python Django for Project Mercury
Documentation=http://wiki.caleston-dev.ca/reported
Requires=postgresql.service
After=postgresql.service

[Service]
ExecStart=/usr/bin/project-mercury.sh
User=project_mercury
Restart=on-failure
RestartSec=10
RemainAfterExit=yes
StandardOutput=append:/var/log/project-mercury.log
StandardError=append:/var/log/project-mercury.log

[Install]
WantedBy=multi-user.target
```

---

### 6. Applying the Changes
After updating the service unit file, reload the systemd daemon to apply the changes:
```bash
sudo systemctl daemon-reload
```

Enable the service to start on boot:
```bash
sudo systemctl enable project-mercury.service
```

Start the service:
```bash
sudo systemctl start project-mercury.service
```

Check service status:
```bash
sudo systemctl status project-mercury.service
```

View logs:
```bash
journalctl -u project-mercury.service --no-pager
```

Check the custom log file:
```bash
cat /var/log/project-mercury.log
```

---

### **Final Notes**
- Ensure the script has execute permissions:
  ```bash
  chmod +x /usr/bin/project-mercury.sh
  ```
- The service now correctly:
  - Runs under `project_mercury`.
  - Restarts automatically on failure (with a 10s delay).
  - Logs outputs/errors to `/var/log/project-mercury.log`.
  - Starts only after PostgreSQL is **fully available**.
  - Does **not** restart if manually stopped.
  - Loads automatically on system boot.
