# Resource Limits
  - Take me to [Video Tutorials](https://kodekloud.com/topic/resource-limits/)
  
In this section we will take a look at Resource Limits

#### Let us take a look at 3 node kubernetes cluster.
- Each node has a set of CPU, Memory and Disk resources available.
- If there is no sufficient resources available on any of the nodes, kubernetes holds the scheduling the pod. You will see the pod in pending state. If you look at the events, you will see the reason as insufficient CPU.
  
  ![rl](../../images/rl.PNG)
  
## Resource Requirements
- By default, K8s assume that a pod or container within a pod requires **`0.5`** CPU and **`256Mi`** of memory. This is known as the **`Resource Request` for a container**, the minimum amount of resources required by pod.
  
  ![rr](../../images/rr.PNG)
  
- If your application within the pod requires more than the default resources, you need to set them in the pod definition file.
- 1 CPU is equal to 1 vCPU in AWS, 1 Azure Core, 1 GCP Core or 1 Hyperthread in physical machine. We can specify CPU in millis as well like `100m`
- Memory should be specific in Mebibyte or Gibibyte, but it can also be specified in Megabyte and Gigabyte as well
  - 1 G (Gigabyte) = 1,000,000,000 bytes
  - 1 M (Megabyte) = 1,000,000 bytes
  - 1 K (Kilobyte) = 1,000 bytes
  - 1 Gi (Gibibyte) = 1,073,741,824 bytes
  - 1 Mi (Mebibyte) = 1048,576 bytes
  - 1 Mi (Mebibyte) = 1048,576 bytes


  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: simple-webapp-color
    labels:
      name: simple-webapp-color
  spec:
   containers:
   - name: simple-webapp-color
     image: simple-webapp-color
     ports:
      - containerPort:  8080
     resources:
       requests:
        memory: "1Gi"
        cpu: "1"
  ```
  ![rr-pod](../../images/rr-pod.PNG) 
   
## Resources - Limits
- In Kubernetes a pod can consume as mush CPU and memory resources as are available on the node but default resource limits are 1 vCPU and 512 Mebibytes of RAM. To restrict a pod from doing that, we can add limits under the resource section
  
  ![rsl](../../images/rsl.PNG)

- Note: Remember Requests and Limits for resources are set per container in the pod.
- You can set the resource limits in the pod definition file.
  
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: simple-webapp-color
    labels:
      name: simple-webapp-color
  spec:
   containers:
   - name: simple-webapp-color
     image: simple-webapp-color
     ports:
      - containerPort:  8080
     resources:
       requests:
        memory: "1Gi"
        cpu: "1"
       limits:
         memory: "2Gi"
         cpu: "2"
  ```
  ![rsl1](../../images/rsl1.PNG)
 
  
## Exceed Limits
- When a pod tries to consume more CPU then what’s mentioned in its limits, Kubernetes tries to throttle its CPU. On the other hand when pod tries to consume more memory then whats allowed in its limits, Kubernetes will let that happen but will evict the pod if it happens again and again.

   ![el](../../images/el.PNG)
   
  
#### K8s Reference Docs:
- https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
  
  
