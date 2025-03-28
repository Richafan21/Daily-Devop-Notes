# Docker Networking

---

By default, docker creates three networks after installation: bridge, none, host. Launching a container without specifying a network will connect the bridge network by default.

To specify the network, use the `--network` parameter:
```bash
docker run ubuntu
docker run ubuntu --network none
docker run ubuntu --network host
```

## The Three Network Types:
- **Bridge Network**: A private, internal network created by Docker on the host. Containers connected to this network receive an internal IP address—typically in the 172.17.x.x range—and can communicate with each other using these addresses. To allow external access to a container, map its ports to ports on the Docker host.

- **Host Network**: This network uses the host’s network stack directly, eliminating network isolation between the container and the Docker host. For example, running a web server container on port 5000 will make the server immediately accessible on the host’s port 5000 without any additional port mapping. However, this also means that multiple containers cannot simultaneously use the same port on the host.

- **None Network**: Disconnects the container from any networking, ensuring complete isolation from external networks and other containers.

## Custom Network
By default, Docker creates an internal bridge network. To further isolate containers, you can create your own network using the bridge driver and a custom subnet with the following command:
```bash
docker network create \
  --driver bridge \
  --subnet 182.18.0.0/16 \
  custom-isolated-network
```
You can list all available Docker networks with: `docker network ls`

To inspect the network settings and IP addresses assigned to a container, use the following command with the container name or ID: `docker inspect {container_name}`

This command outputs detailed JSON information including network settings, internal IP addresses, MAC addresses, and the network types the container is connected to.

Containers on the same network can communicate using their container names instead of IP addresses, making it easier to remember.

---

# Docker Registry

We know that by default, docker uses the docker hub registry(docker.io) to pull images from.

## Private Registries

In order to run a container using an image from a private registry, you need to log in:
```bash
docker login private-registry.io
Username: registry-user
Password:
WARNING! Your password will be stored unencrypted in /home/vagrant/.docker/config.json.
Login Succeeded
```

After logging in, run the container by specifying the private registry in the image name:

`docker run private-registry.io/a`

## Setting up a Custom On-Premise Docker Registry

If you're running your applications on-premises and lack a pre-configured private registry, you can deploy your own Docker registry. The Docker registry itself is distributed as a Docker image named "registry" which exposes its API on port 5000.

To start your custom registry, run:

`docker run -d -p 5000:5000 --name registry registry:2`

Once your registry is running, tag your image to include the registry URL. For instance, if your image is named "my-image" and your registry is hosting on the same machine, execute:

`docker image tag my-image localhost:5000/my-image`
Push the tagged image to your local private registry:

`docker push localhost:5000/my-image`

This image can then be pulled from any host within your network using either "localhost" (if on the same machine) or the appropriate IP address or domain name:
```bash
docker pull localhost:5000/my-image
# OR
docker pull 192.168.56.100:5000/my-image
```

---
