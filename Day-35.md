## Volumes

**Why is it important?**
- If the database container or pod is restarted, the data would disappear which is inconvenient as you want the data to be persistent. Volumes can solve this problem, as it attached a physical storage on a hard drive to your pod.
- This could be on the local machine or remote storage outside the kubernetes cluster.
- This way if the database pod or container is restarted, all the data will still be there. Volumes are important as Kubernetes doesn't manage data persistance.
- It can also be used for shared storage, as if you have multiple containers in a Pod and need to access shared files, it can be a pain to set up a shared filesystem accross all the containers. Volumes act as a shared filesystem.

## Deployment

**What happens if the application Pod dies or requires a restart?**
- Without Deployment, there would be a downtime where users cannot access your application.
- Deployments allow you to create and run replicas of your application. This would be connected to the same service.
- Instead of creating a second pod manually, you can simply create deployments which can easily scale your application up or down.

## Statefulset

**What if database pod dies?**
- Deployments are used for stateless Apps, but for stateful Apps or Databases, we will use StateFul.
- This is more difficult that deployments, so its common to host Databses outside of Kubernetes cluster.

