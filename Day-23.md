# Docker Crash Course

[Nana's Docker Crash Course Video](https://www.youtube.com/watch?v=pg19Z8LL06w&t=174s)

---

## What is Docker?

Docker is a virtualisation software that greatly speeds up and eases the process of developing and deploying applications. 
This is done through packaging apllications into a **container**.
This container will contain all the neccesities for the application to run, such as libraries, dependencies, configuration, system tools, and runtime.
Essentially, a **container** is a standardised unit, that has everything the application needs to run within it.  

Having everything in a single Docker package makes it very portable and easy to share and distribute.

---

## Why is this a big deal? What Problems does Docker solve?

### Development Process before Containers:

Before containers, all developers would need to install and configure all services directly on their OS in their local machine.
This means that messaging, database, cahe, etc, would all need to be on your local environment. And every developer in the team would also need the same services
to configure and run on their own local environment. This means the installation process w ith differ based on the OS environment.
Since there are so many different steps, there is a high likelyhood something goes wrong during this process.
It will also take a lot of time, if your application requires 10 services, each developer would need to install 10 services individually.

### How does container solve this problem?

They have their own isolated environment and everything is packaged, so all the developer needs to do is start the service as a Docker Container using one simple Docker command.

This command will be the same no matter what OS you are on. So Docker standardises process of running any service on an local dev environment.

You can also run different versions of the same application without any conflicts.

### Deployment Process before Containers:

Before containers, development team would produce an application artifact(example: JAR FILE) alongside a set of instructions of how to install and configure the application package on the server.
Also a database service with a set of instructions on how to set it up on the server. These will be handed to the operations team, who then install and configure apps and its dependencies.

Installations and configurations done directly on the server's OS, which is very error prone as you could have dependency version conflicts or miscommunications between the two teams.  

### How does container solve this problem?

Docker artifact includes everything the app needs, such as configuration, app source code, and dependencies. So insteaf of textual, everything is packaged inside the Docker artifact.  
No configuration needed on the server apart from Docker runtime and less room for errors. Operation team just needs install docker runtime on the server and then run a Docker command to fetch and run the Docker artifacts.

---


## Docker vs Virtual Machine


### How an OS is made up

OS has 2 main layers: OS Applications Layer, and OS Kernel.

The OS Kernel communicates with the hardware components such as CPU, memory, storage, etc.  
Kernel is at the core of every operating system as it interacts between hardware and software components.

The Docker contains the OS Application Layer and services and apps isntalled on top that layer. Uses the hosts Kernel.

Virtual Machines on the other hand, has the applications layer and its own Kernel. Meaning when you download a Virtual machine image on the host, it boots up its own OS rather than using the hosts.

This means Docker images are much smaller and faster to start as it only needs to implemant one layer of the OS compared to Virtual Machines. However, you can only run Docker with Linux distros, whereas VM's are compatible with all OS.

Since most containers are Linux based, using Docker Desktop allows you to run Linux containers on Mac and Windows, as it uses a Hypervisor layer with a lightweight Linux distribution.  

---

## Docker Images vs Docker Containers

| Feature       | Docker Image 🖼️ | Docker Container 📦 |
|--------------|----------------|------------------|
| **Definition** | A **read-only template** that contains the application, dependencies, and configuration.(An application artifact) | A **running instance** of an image that operates as an isolated process. |
| **State** | Static and immutable. | Dynamic and can be started, stopped, or deleted. |
| **Storage** | Stored in a Docker registry (e.g., Docker Hub, private registry). | Runs in memory but uses a writable layer for changes. |
| **Creation** | Built using a `Dockerfile` (`docker build`). | Created from an image using `docker run`. |
| **Persistence** | Never changes once built. | Changes made inside a container do not persist after it stops (unless volumes are used). |
| **Example** | `ubuntu:latest`, `nginx:alpine` | A running instance of `ubuntu:latest` where you can interact via a shell. |

Analogy:
Docker Image → A recipe 📜 for baking a cake.

Docker Container → A cake 🎂 baked using that recipe.

You can make multiple cakes (containers) from the same recipe (image). If a cake is eaten (container is deleted), you can always bake a new one using the recipe.

---

## Docker Registries

How do we actually get these images to run containers from?  
Docker registries is a storage and distribution system for Docker images. Docker hosts one of the biggest Docker Registry called 'Docker Hub'.

### Image Versioning

Since technology is always changing, there will be updates to certain applications, which means there will be a new Docker image that will be created. Different Docker image versions are called image tags. Supported tags can be seen on Docker Hub.

There is a special tag called 'latest' tag, which gets the latest, most recently released image from docker hub. If you don't specify the version, it will pull the latest version of the application.

### How to pull an imge from Docker Hub

The best practice is to pull a specific version from Docker Hub.

Syntax: `docker pull {name}:{tag}` to pull an image from registry with specified version.

Docker hub(docker.io) is the default location for pulling images from.

After pulling, running `docker images` will show the image.

### Running an Image 

Execute `docker run {image_name}:{tag}`

To check if it is running properly: `docker ps`

You will notice that you cannot run this command on the same terminal as the logs are blocking the terminal. Ctrl + c will exit the container and the process will die. Running `docker ps` will show that it is not running anymore. To run the process while also not having it block the terminal, use the flag `-d` which stands for detach.

`docker run -d nginx:1.27.4`

However, if you still want to see logs, use: `docker logs {container_id}`

---

## Port Binding

How do we access this container?

### Container Port vs Host Port

Applications within containers run in an **isolated Docker network**, which allows us to run the same app running on the same port multiple times. This means we cannot access it on our local computer browser.

We need to first expose the container port to the host.(Port binding)

We can find the port under the PORTS section is `docker ps`. 

We can then tell Docker to bind that port to our local host via another port. This way we can access the container. This is done through an additional flag `-p`(publish).

First, stop the current application by running `docker stop {container_id}`

```bash
docker run -d -p 9000:80 nginx:1.27.4`
# docker run -d -p {new_port}:{port} {image_name}:{tag}
```
Only 1 service can run on a specific port on the host, so now no other service can run on port 9000.

It is standard practice to use the same port on your host as container is using.

---

## Start and Stop Containers

Whenever we execute `docker run` command, it creates a new container instead of re-using previous containers. Although if we run `docker ps` we do not see anything, that's why we need to use the `-a` flag: `docker ps -a` to show all containers including ones that aren't running.

To re-use a container: `docker start {container_id}`
Since it is hard to remember container id, we can assign containers names using the flag `--name`

`docker run --name web-app -d -p 80:80 nginx:1.27.4`

---

## Pricate Docker Registries






