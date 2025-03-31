# Kubernetes and YAML Introduction

## Running Applications: Docker vs. Kubernetes
When using Docker, you can run a single instance of an application using a straightforward command:

`docker run my-web-server`

Kubernetes, on the other hand, enables seamless scaling. For instance, the following command deploys a thousand instances simultaneously:

`kubectl run --replicas=1000 my-web-server`

Beyond simple scaling, Kubernetes supports dynamic adjustments—up to two thousand instances with another command. It also facilitates rolling upgrades, allowing you to update one instance at a time, and includes a rollback mechanism if issues arise. Moreover, Kubernetes enables you to experiment with new features by upgrading only a subset of instances using A/B testing methodologies.

Below is an example demonstrating Kubernetes' rolling update and scaling features:
```bash
docker run my-web-server
kubectl rolling-update my-web-server --rollback
kubectl rolling-update my-web-server --image=web-server:2
kubectl scale --replicas=2000 my-web-serve
```

## Kubernetes Architecture Overview

Kubernetes leverages container runtimes like **Docker, Rocket, or CRI-O** to execute applications within isolated containers. A **Kubernetes cluster** consists of multiple **nodes** managed by a **master node** that orchestrates containerised applications.

## Nodes

Nodes are **physical or virtual machines** running Kubernetes software. They act as **workers** where containers are deployed. A typical Kubernetes cluster includes **multiple nodes** to guarantee **high availability**:
- If **one node fails**, the remaining nodes continue to operate, ensuring **uninterrupted service**.

## Master Node

The **master node** runs the **control plane components** that manage and monitor the state of the cluster.  
It is responsible for:
- **Maintaining cluster health**  
- **Monitoring nodes**  
- **Reallocating workloads** if a node fails  

## Core Components of Kubernetes

When installing Kubernetes, the following components are set up:

### API Server  
- The **primary interface** for all administrative tasks  
- Allows **users, management devices, and CLI tools** to communicate with the Kubernetes cluster  

### etcd  
- A **distributed and reliable key-value store**  
- Stores **all cluster data** and ensures **consistency** across nodes  

### Scheduler  
- **Distributes** newly created containers across available nodes  

### Controllers  
- Detect **changes** in the cluster (e.g., node failures, container issues)  
- **Initiate corrective actions** by deploying new containers when needed  

### Container Runtime  
- The underlying software (e.g., **Docker**) that runs containers  

### kubelet  
- An **agent that runs on each node**  
- Ensures that **containers operate as expected**

## The kubectl Command-Line Tool

`kubectl` is the essential CLI tool for deploying and managing applications on a Kubernetes cluster. It is also used for retrieving critical cluster-related information, such as node status. Some basic commands include:
```bash
kubectl run hello-minikube
kubectl cluster-info
kubectl get nodes
```
These commands deploy an application, display cluster details, and list all nodes within the cluster. Additionally, you can deploy more complex applications. For example, to launch an application with 100 replicas, use:

`kubectl run my-web-app --image=my-web-app --replicas=100`

---

# YAML Basics  

## YAML in Action  

Here's a sample YAML file that describes **server settings**:  

```yaml
Servers:  
  - name: Server1  
    owner: John  
    created: 2012-12-23  
    status: active  
``` 

As you can see, YAML relies on **indentation** rather than brackets or tags to structure data.  


## Understanding Key-Value Pairs  

At the core of YAML are **key-value pairs**. A key is followed by a **colon (`:`)** and a **space**, with its associated value afterward.  

Example:  
```yaml
Fruit: Apple  
Vegetable: Carrot  
Liquid: Water  
Meat: Chicken  
```  

## Lists in YAML  

To define a **list (array)**, use a **dash (`-`)** before each item.  

Example:  

```yaml
Fruits:  
  - Orange  
  - Apple  
  - Banana  

Vegetables:  
  - Carrot  
  - Cauliflower  
  - Tomato  
```

---

## Organising Data with Dictionaries  

Dictionaries (also called **maps**) help group related data under a common key.  

Example:  

```yaml
NutritionFacts:  
  Banana:  
    Calories: 105  
    Fat: 0.4 g  
    Carbs: 27 g  
  Grapes:  
    Calories: 62  
    Fat: 0.3 g  
    Carbs: 16 g  
```

**NOTE: Indentation matters in YAML!** If you misalign elements, the file may become invalid.  


## YAML Supports Lists of Dictionaries  

If you need multiple entries with the same structure, you can use a **list of dictionaries**.  

Example:  

```
Fruits:  
  - Name: Banana  
    Calories: 105  
    Fat: 0.4 g  
    Carbs: 27 g  
  - Name: Grape  
    Calories: 62  
    Fat: 0.3 g  
    Carbs: 16 g  
```


## When to Use a Dictionary vs. a List  

Dictionaries work best for **storing attributes** of a single item, while **lists** help manage **multiple items**.  

For instance, if we define **a single car**, a dictionary is the best approach:  

```
Car:  
  Color: Blue  
  Model:  
    Name: Corvette  
    Year: 1995  
  Transmission: Manual  
  Price: $20,000  
``` 

But if we need to store **multiple cars**, a list of dictionaries is more effective:  

```yaml
Cars:  
  - Color: Blue  
    Model:  
      Name: Corvette  
      Year: 1995  
    Transmission: Manual  
    Price: $20,000  

  - Color: Red  
    Model:  
      Name: Corvette  
      Year: 1995  
    Transmission: Automatic  
    Price: $22,000  
``` 

## Key Differences Between Dictionaries and Lists  

| Feature       | Dictionary | List |
|--------------|------------|------|
| **Order Matters?** | No | Yes |
| **Key-Value Pairs?** | Yes | No |
| **Best for?** | Attributes of an item | Grouping similar items |

**Lists maintain order**, while dictionaries **store key-value mappings without a specific order**.  



## Comments in YAML  

YAML allows you to add comments using the `#` symbol. Comments are ignored when the file is processed.  



