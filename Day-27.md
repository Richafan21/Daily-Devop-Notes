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
