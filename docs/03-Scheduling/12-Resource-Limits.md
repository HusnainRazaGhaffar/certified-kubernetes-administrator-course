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
- In Kubernetes a pod can consume as mush CPU and memory resources as are available on the node. To restrict a pod from doing that, we can add limits under the resource section
  
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

## Combinition of Requests and Limits
- When requests are not set and only limits are set, Kubernetes will apply the requests same as limit values
- When requests and limits are set, Kubernetes will guarantee that pod will have the resources requested but will not allow to use more then the limits set. In this way even if free CPU/RAM are available but kubernetes will not allow the pod to use it
- When requests are set but not limits are set, that's the recommended approach where kubernetes will gurantee the pod to have requested resources but then pod can use available resources but because other pods will also have requests set, resource intensive pod won't eat up other's resources

## Default Requests and Limits
- To set default requests and limits for all containers of pods we can define limit ranges
- These limits are applied when a pod is created. So if you create or change these limits it won't affect the existing pods, it will only affect the newer pods created after the limits are applied or changed.
- You define a LimitRange with this manifest as follows,

```
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-resource-constraint
spec:
  limits:
  - default: # this section defines default limits
      cpu: 500m
    defaultRequest: # this section defines default requests
      cpu: 500m
    max: # max and min define the limit range
      cpu: "1"
    min:
      cpu: 100m
    type: Container
```

## Resource Quotas
- We can apply hard limits on the resources that can be used by all pods by applying resource quotas on a namespace
- Here is the reference https://kubernetes.io/docs/concepts/policy/resource-quotas/

#### K8s Reference Docs:
- https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
- https://kubernetes.io/docs/concepts/policy/limit-range/
  
  
