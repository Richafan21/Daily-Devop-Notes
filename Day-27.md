# Container Orchestration

`docker run` will run a single instance of an application on a single docker host, but what if the users increase? Will you keep running `docker run` and always keep watch on how many users there are and continuosly run `docker run`?

The solution to this issue is Container Orchestration. It manages and scales containerised applications across multiple hosts.

## What is Container Orchestration?

Container Orchestration is a solution which consists of a set of tools and scripts that can automate the deployment. scaling, and management of containerised applications. An effective orchestration solution spans multiple Docker hosts, ensuring that if one host fails, the application remains available for other hosts.

Using Docker Swam, you can scale your Node.js application by running:
`docker service create --replicas=100 nodejs`

This command will deploy a service with 100 replicas, making the scaling process much easier.

## The Benefits
- **Automatic Scaling**: Dynamically adjust the number of container instances based on load.
- **High Availability**: Distribute containers across multiple hosts, ensuring continued service even if a host fails.
- **Advanced Networking**: Allow seamless communication between containers across hosts and imolement load balancing for incoming requests.
- **Centralised Storage Management**: Provide persistent data sharing alongside centralised configuration and security management.

## Popular Container Orchestration Tools

| Platform       | Key Features                 | Use Case                                   |
|--------------|--------------------------------|--------------------------------------------|
| Docker Swarm  | Simplified setup & management | Quick deployments with moderate scaling  |
| **Kubernetes**    | Extensive customization & multi-cloud support | Complex, large-scale production environments |
| Apache Mesos  | Advanced features with higher configuration complexity | Highly specialized or custom orchestrations |


# Docker Swarm

## What is it?

Docker swarm is a powerful container orchestration tool which allows you to combine multiple Docker hosts into a single cluster. Within this cluster:
- The **manager node** orchestrates the distribution of services (application instances) across different hosts.
- **Worker nodes** execute tasks assigned by the manager.
- Ensures **high availability** and **load balancing**.

To set up Docker Swarm, you need multiple Docker-installed hosts:
- One host is designated as the **manager**(master or swarm manager).
- Other hosts are designated as **workers**(worker nodes).

## Initializing the Swarm

To initialize Docker Swarm on the **manager node**, use:

```bash
docker swarm init --advertise-addr <manager-ip>
```

Example output:
```bash
root@osboxes:/root/simple-webapp-docker # docker swarm init --advertise-addr 192.168.1.12
Swarm initialized: current node (0j76dum2r56p1xfn4u1pls2c) is now a manager.

To add a worker to this swarm, run the following command:
docker swarm join --token SWMTKN-1-35va8b3fi5krpdkefqxgtmulw3z828daucri7y526ne0sgu-2ee3g7q1g9d5j632a1yvmc4t 8.1.12:2377

To add a manager to this swarm, run:
docker swarm join-token manager
```

## Deploying Services using Docker Swarm

Usually, you may use `docker run` to deploy an application instance, but doing this individually for each worker will be impractical for larger clusters. 

Docker Swarm addresses these challenges by orchestrating containers through Docker services. Docker services let you run one or more instances (replicas) of an application across your cluster nodes.

For example, to deploy multiple instances (replicas) of your web server application across worker nodes, execute the following command on the manager node:
```bash
docker service create --replicas=3 -p 8080:80 --network frontend my-web-server
```
