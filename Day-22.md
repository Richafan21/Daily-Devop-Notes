# Basic Docker Commands

---

Understanding fundamental Docker commands is crucial for effectively managing containers and images. Today we will go over some essential commands:

## Running a Container

To create and start a container from a specified image:

```bash
docker run <image_name>
```

Example: Running an Nginx container:

```bash
docker run nginx
```

If the image isn't present locally, Docker pulls it from the Docker Hub. Pulling Example:
```bash
docker run nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
fc71811084d0: Already exists
d2e987ca2267: Pull complete
0b760b431b11: Pull complete
Digest: sha256:96fb261b66270b900ea5a2c17a26abbfabe95506e73c3a3c65869a6dbe83223a
Status: Downloaded newer image for nginx:latest
```

## Listing Containers
You can view running containers using the docker ps command. This command provides an overview, including container IDs, image names, statuses, and container names. For instance:
```bash
docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
796856ac413d        nginx               "nginx -g 'daemon of..." 7 seconds ago       Up 6 seconds        80/tcp              silly_sammet
To list all containers, including those that have stopped or exited, add the -a flag:

docker ps -a
CONTAINER ID        IMAGE               COMMAND                        CREATED             STATUS                        NAMES
796856ac413d        nginx               "nginx -g 'daemon of..."       7 seconds ago       Up 6 seconds                  silly_sammet
cff8ac918a2f        redis               "docker-entrypoint.s..."       6 seconds ago       Exited (0) 3 seconds ago
```

## Stopping and Removing Containers
To stop a running container:
```bash
docker stop <container_name_or_ID>
```

To remove a stopped container:
```bash
docker rm <container_name_or_ID>
```
Removed container will no longer appear in Docker Listings.




