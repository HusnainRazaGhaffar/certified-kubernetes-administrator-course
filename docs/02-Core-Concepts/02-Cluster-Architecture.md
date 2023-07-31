# Cluster Architecture

  - Take me to [Video Tutorial](https://kodekloud.com/topic/cluster-architecture/)

In this section , we will take a look at the kubernetes Architecture at high level.
- 10,000 Feet Look at the Kubernetes Architecture

## Cluster Architecture

- Master node of the Kubernetes manages the cluster using components known as control plane components
    - ETCD Cluster - Is a highly available database that stores all the information in key-value format about where and when a resource is deployed in Kubernetes cluster
    - `kube-scheduler` - Schedules the pods on the worker nodes based on the pods resource requirements and nodes available resources
    - Controllers
        - Node Controller - Responsible for on-boarding of new nodes to the cluster and handling situations when nodes become unavailable
        - Replication Controller - Responsible for make sure that desired number of pods are running all the time in replication group
        - Controller Manager - NA
    - `kube-apiserver`- Its the primary management component of Kubernetes. It is responsible for orchestrating all operations within the cluster. It exposes the Kubernetes API that is used by external users to perform management operations on the cluster as well as the various controllers to monitor the state of the cluster and make necessary changes as required. Also helps worker node to communicate with the Kubernetes master server
    - Container Runtime Engine - Docker or ContainerD is used as container runtime on worker nodes as well as master nodes so that control plane components can be run as container on master node
- On Worker node
    - `kubelet` - Is a agent that runs on each node in a cluster. It listens for instructions from the `kube-apiserver` and deploys or destroys containers on nodes as required. The `kube-apiserver` periodically fetches status reports from the `kubelet` to monitor the status of nodes and containers on them.
    - Container Runtime Engine - Docker or ContainerD is used as container runtime on worker nodes as well as master nodes so that control plane components can be run as container on master node
    - `kube-proxy` - Communication between pods on a node is enabled by `kube-proxy` service. It ensures that necessary rules are in place on the worker nodes that allows the containers running on them to reach each other.

  ![Kubernetes Architecture](../../images/k8s-arch.PNG)
  
  ![Kubernetes Architecture 1](../../images/k8s-arch1.PNG)

K8s Reference Docs:
- https://kubernetes.io/docs/concepts/architecture/
