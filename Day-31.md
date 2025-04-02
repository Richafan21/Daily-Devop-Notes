# Using Minikube

In order to run kubernetes on my local machine, I installed minikube, below are some useful commands.

## Minikube Basic Commands

### Start Minikube

If Minikube is not running, start it:

`minikube start`

Check if Minikube is running:

`minikube status`

### Check Kubernetes Cluster Information

View nodes in the cluster:

`kubectl get nodes`

Check running pods:

`kubectl get pods -A`

Get all running services:

`kubectl get svc`

Get details about a pod:

`kubectl describe pod `

### Deploy an Application

Create an Nginx pod:

`kubectl run my-nginx --image=nginx`

Expose it as a service:

`kubectl expose pod my-nginx --type=NodePort --port=80`

Get the service URL:

`minikube service my-nginx --url`

### Manage Deployments

Create a deployment:

`kubectl create deployment my-app --image=nginx`

Scale the deployment to 3 replicas:

`kubectl scale deployment my-app --replicas=3`

Delete the deployment:

`kubectl delete deployment my-app` 

### Stop and Delete Minikube

Stop Minikube (but keep the cluster):

`minikube stop`

Delete Minikube (reset everything):

`minikube delete`


<img width="500" alt="image" src="https://github.com/user-attachments/assets/97bbb090-2ffc-470a-9600-d1072a61cf0e" />
