# Kubernetes Access Management

## What is Kubernetes?
- An open source container orchestration tool devevloped by Google
- Helps manage containerised applications in different deployment environments

### What problems does it solve?
- There's been an increase usage of container technologies due to microservices. Managing so many containers manually can be very difficult or even impossible.
- Orchestration tools such as Kubernetes promise **high availability**, **scalability**, and **disaster recovery**.

## Kubernetes Architecture 
![image](https://github.com/user-attachments/assets/76675822-ed12-437e-8791-11845f20d9c1)

The Kubernetes cluster is made up of at least one master node, and worker nodes connected to it. Each node has a kubelet process running on it, which makes it possible for the nodes to communicate with each other.

Master node:
- API Server: Entrypoint to K8s cluster.
- Controller Manager: Keeps track of whats happening in the cluster.
- Scheduler: Ensures pods placement
- etcd: Kubernetes backing store

A Node is a virtual or physical machine
A Pod is the smallest unit in Kubernetes, it is an abstraction over a container. Usualy 1 Application per Pod.

Each Pod gets its own IP address.

Pods can die very easily, so a new IP address will be made on re-creation. This is where service comes in.

## Service
Service or SVC is a Permanent IP address(end point) that can be attached to each port. The life cycle of Pod and Service are not connected, so even if the Pod dies, the IP address of the service won't change.

Since you don't want your database to be accessible to public, you would create an internal service, but the IP address would look like 127.89....
Usually your url should look like https://my-app.com, this is where Ingres comes in.

## Ingress
Request will go to Ingress first and it will do the forwarding, and then route to your internal service.

![image](https://github.com/user-attachments/assets/63980978-3a90-4986-b66e-73499973505c)

## Config Map
Externalises configuration parameters from application code
`kubectl create configmap`

## Secrets
Contains passwords and other important information that can be leaked.
`kubectl create secret`
