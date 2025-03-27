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
