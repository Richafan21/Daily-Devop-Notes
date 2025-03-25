# Docker Crash Course

[Nana's Docker Crash Course Video](https://www.youtube.com/watch?v=pg19Z8LL06w&t=174s)

---

## What is Docker?

Docker is a virtualization software that greatly speeds up and eases the process of developing and deploying applications. 
This is done through packaging applications into a **container**.
This container will contain all the necessities for the application to run, such as libraries, dependencies, configuration, system tools, and runtime.
Essentially, a **container** is a standardized unit that has everything the application needs to run within it.  

Having everything in a single Docker package makes it very portable and easy to share and distribute.

---

## Why is this a big deal? What Problems does Docker solve?

### Development Process before Containers:

Before containers, all developers would need to install and configure all services directly on their OS in their local machine.
This means that messaging, database, cache, etc., would all need to be on your local environment. And every developer in the team would also need the same services
to configure and run on their own local environment. This means the installation process will differ based on the OS environment.
Since there are so many different steps, there is a high likelihood something goes wrong during this process.
It will also take a lot of time; if your application requires 10 services, each developer would need to install 10 services individually.

### How does a container solve this problem?

They have their own isolated environment and everything is packaged, so all the developer needs to do is start the service as a Docker Container using one simple Docker command.

This command will be the same no matter what OS you are on. So Docker standardizes the process of running any service on a local dev environment.

You can also run different versions of the same application without any conflicts.

### Deployment Process before Containers:

Before containers, the development team would produce an application artifact (example: JAR FILE) alongside a set of instructions on how to install and configure the application package on the server.
Also, a database service with a set of instructions on how to set it up on the server. These will be handed to the operations team, who then install and configure apps and their dependencies.

Installations and configurations are done directly on the server's OS, which is very error-prone as you could have dependency version conflicts or miscommunications between the two teams.  

### How does a container solve this problem?

A Docker artifact includes everything the app needs, such as configuration, app source code, and dependencies. So instead of textual instructions, everything is packaged inside the Docker artifact.  
No configuration is needed on the server apart from the Docker runtime and less room for errors. The operation team just needs to install the Docker runtime on the server and then run a Docker command to fetch and run the Docker artifacts.

---

## Docker vs Virtual Machine

### How an OS is made up

OS has 2 main layers: **OS Applications Layer** and **OS Kernel**.

The OS Kernel communicates with the hardware components such as CPU, memory, storage, etc.  
The Kernel is at the core of every operating system as it interacts between hardware and software components.

The **Docker container** includes the OS Application Layer and services and apps installed on top of that layer. It uses the host's Kernel.

**Virtual Machines**, on the other hand, have the applications layer and their own Kernel. Meaning when you download a Virtual Machine image on the host, it boots up its own OS rather than using the host's.

This means Docker images are much smaller and faster to start as they only need to implement one layer of the OS compared to Virtual Machines. However, you can only run Docker with Linux distros, whereas VMs are compatible with all OS.

Since most containers are Linux-based, using Docker Desktop allows you to run Linux containers on Mac and Windows, as it uses a Hypervisor layer with a lightweight Linux distribution.  

---

## Docker Images vs Docker Containers

| Feature       | Docker Image 🖼️ | Docker Container 📦 |
|--------------|----------------|------------------|
| **Definition** | A **read-only template** that contains the application, dependencies, and configuration. (An application artifact) | A **running instance** of an image that operates as an isolated process. |
| **State** | Static and immutable. | Dynamic and can be started, stopped, or deleted. |
| **Storage** | Stored in a Docker registry (e.g., Docker Hub, private registry). | Runs in memory but uses a writable layer for changes. |
| **Creation** | Built using a `Dockerfile` (`docker build`). | Created from an image using `docker run`. |
| **Persistence** | Never changes once built. | Changes made inside a container do not persist after it stops (unless volumes are used). |
| **Example** | `ubuntu:latest`, `nginx:alpine` | A running instance of `ubuntu:latest` where you can interact via a shell. |

**Analogy:**
Docker Image → A recipe 📜 for baking a cake.

Docker Container → A cake 🎂 baked using that recipe.

You can make multiple cakes (containers) from the same recipe (image). If a cake is eaten (container is deleted), you can always bake a new one using the recipe.

---

## Docker Registries

How do we actually get these images to run containers from?  
Docker registries are a storage and distribution system for Docker images. Docker hosts one of the biggest Docker Registries called **Docker Hub**.

### Image Versioning

Since technology is always changing, there will be updates to certain applications, which means there will be a new Docker image that will be created. Different Docker image versions are called **image tags**. Supported tags can be seen on Docker Hub.

There is a special tag called `'latest'`, which gets the latest, most recently released image from Docker Hub. If you don't specify the version, it will pull the latest version of the application.

### How to pull an image from Docker Hub

The best practice is to pull a specific version from Docker Hub.

Syntax:  
```bash
docker pull {name}:{tag}
```
This pulls an image from the registry with the specified version.

### Running an Image 

Execute:  
```bash
docker run {image_name}:{tag}
```
To check if it is running properly:  
```bash
docker ps
```
To run the process in detached mode:  
```bash
docker run -d nginx:1.27.4
```
View logs:  
```bash
docker logs {container_id}
```

---

## Port Binding

### Container Port vs Host Port

Applications within containers run in an **isolated Docker network**, so they are not accessible from the local machine by default.

To expose a container’s port to the host, we use **port binding** with the `-p` flag.

```bash
docker run -d -p 9000:80 nginx:1.27.4
# docker run -d -p {new_port}:{container_port} {image_name}:{tag}
```

---

## Start and Stop Containers

To list all containers, including stopped ones:  
```bash
docker ps -a
```
To restart an existing container:  
```bash
docker start {container_id}
```
To name a container:  
```bash
docker run --name web-app -d -p 80:80 nginx:1.27.4
```

---

## Dockerfile - Creating Own Images
