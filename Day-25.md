# Docker Engine & Storage

---

## Docker Engine
Docker Engine is the **core component** that enables containerisation on a system.  
It manages **containers, images, networks, and storage**.

### Core Components of Docker on Linux
Docker Engine consists of several key components:

- **Docker Daemon (`dockerd`)** – Runs in the background and manages containers.
- **Docker CLI (`docker`)** – Command-line tool to interact with the Docker daemon.
- **REST API** – Allows external applications to interact with Docker.


### How Containers Isolate Applications
Containers isolate applications using **namespaces** and **control groups (cgroups)**.

- **Namespaces** provide isolated environments for each container.
- **cgroups** limit the resources a container can use.


###  Understanding Process ID (PID) Namespaces
PID namespaces allow each container to have its own **process ID space**, preventing processes inside a container from seeing those in other containers or on the host.

Example:
```bash
docker run -it ubuntu bash  
ps aux  # Lists only processes inside the container  
```

### Managing Resources with cgroups
**Control groups (cgroups)** restrict the amount of **CPU, memory, disk I/O, and network** resources a container can use.

Example:

```bash
docker run -m 512m ubuntu  # Limit memory usage to **512MB** for a container
```

### To view cgroup limits inside a running container:

```bash
cat /sys/fs/cgroup/memory/memory.limit_in_bytes  
```

---

## Docker Storage  
Docker provides multiple **storage options** for managing data inside and outside containers.


### Docker's File Storage Architecture
Docker uses a **copy-on-write (CoW)** storage system where changes to files inside a container **do not** modify the original image but are stored in a separate layer.


### Docker's Layered Architecture
Each **Docker image** consists of multiple **read-only layers**, with a **writable container layer** on top.

Example: Inspecting image layers:

`docker history ubuntu`  


### Reusing Layers Across Images
Since **layers are shared** across images, Docker **saves disk space** and **speeds up builds**. For example, if two images use `ubuntu:latest`, they **share** the same base layer.


### Understanding Image Layers
Each **instruction** in a `Dockerfile` creates a **new image layer**.

Example:

```bash
FROM ubuntu  
RUN apt-get update  # Creates a new layer  
RUN apt-get install -y nginx  # Another layer  
```


### Managing Persistent Data with Volumes
Docker provides **volumes** for persisting data **outside the container filesystem**.

- **Volumes exist in** `/var/lib/docker/volumes/`
- **They survive container restarts**


### Creating and Using Volumes

- **Create a volume**: `docker volume create my_volume`  
- **Use it in a container**: `docker run -v my_volume:/data ubuntu`
- List volumes: `docker volume ls`


### Using the `--mount` Option  
Another way to attach storage is using `--mount`.

Example:
`docker run --mount source=my_volume,target=/data ubuntu`


### Docker Storage Drivers
Docker uses **storage drivers** to manage container file systems. Common ones include:

- **OverlayFS** (default for most Linux distributions)
- **AUFS** (older systems)
- **Device Mapper** (used in CentOS/RHEL)

- **Check the current storage driver**: `docker info | grep "Storage Driver"` 
- **Change storage driver in `/etc/docker/daemon.json`**:
```bash
{
  "storage-driver": "overlay2"
}
```
- **Restart Docker for changes to take effect**: `systemctl restart docker`

---

## Summary  
- **Docker Engine** manages containers using **namespaces** and **cgroups**.  
- **Containers** run in **isolated environments** with separate **PID namespaces**.  
- **Storage layers** enable efficient **image reuse**.  
- Use **volumes** for **persistent data** and **storage drivers** to optimise file system performance.

---
