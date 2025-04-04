# Hands on With Kubernetes

##  What Is a Pod?

A **Pod** is the smallest deployable unit in Kubernetes.  
Think of it as a wrapper around one or more containers. Pods allow these containers to share:

- **Networking**: They communicate over `localhost`
- **Storage**: Shared volumes if needed
- **Resources**: Like CPU and memory limits

You don’t scale Kubernetes by adding more containers to a single Pod.  
Instead, you **create more Pods** that can be distributed across multiple nodes.

**Multi-container Pods** are useful when:
- You have a helper container
- You need logging or sidecar functionality
- Or you're building a processing pipeline



## Run a Single-Container Pod

```bash
kubectl run nginx --image=nginx
kubectl get pods
kubectl describe pod nginx
```

This runs an nginx container in a Pod. Let’s check the Pod's status.


## Create a YAML for Multi-Container Pod

**File: `multi-container-pod.yaml`**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
    - name: main-app
      image: nginx
    - name: sidecar
      image: busybox
      command: ['sh', '-c', 'while true; do echo Sidecar is running; sleep 5; done']
```

Apply it:
```bash
kubectl apply -f multi-container-pod.yaml
kubectl get pods
kubectl logs multi-container-pod -c sidecar
```

Both containers share the same Pod. The sidecar container keeps logging.


## Delete Pods

```bash
kubectl delete pod nginx
kubectl delete pod multi-container-pod
```

---

## Scaling with Deployments

```bash
kubectl create deployment my-app --image=nginx
kubectl get deployments
kubectl scale deployment my-app --replicas=3
kubectl get pods
```

 Here we create a deployment and scale it to 3 Pods.

## Delete Deployment

```bash
kubectl delete deployment my-app
```
