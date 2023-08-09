# Node Affinity
  - Take me to the [Video Tutorial](https://kodekloud.com/topic/node-affinity-2/)
  
In this section, we will talk about "Node Affinity" feature in kubernetes.

#### The primary feature of Node Affinity is to ensure that the pods are hosted on particular nodes.
- With **`Node Selectors`** we cannot provide the advance expressions.
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   nodeSelector:
    size: Large
  ```
  ![ns-old](../../images/ns-old.PNG)
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: In
              values: 
              - Large
              - Medium
  ```
  ![na](../../images/na.PNG)
  
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: NotIn
              values: 
              - Small
  ```
  ![na1](../../images/na1.PNG)
  
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: Exists
  ```
  
  ![na2](../../images/na2.PNG)
  

## Node Affinity Types
- Available
  - requiredDuringSchedulingIgnoredDuringExecution - During Scheduling the pod Affinity match is required but after the pod starts it doesn’t matter
  - preferredDuringSchedulingIgnoredDuringExecution - During Scheduling the pod Affinity match is preferred if it doesn’t match it can be ignored but after the pod starts it doesn’t matter
- Planned
  - requiredDuringSchedulingRequiredDuringExecution - During Scheduling the pod Affinity match is required and after the pod starts it is also required
  - preferredDuringSchedulingRequiredDuringExecution - During Scheduling the pod Affinity match is preferred if it doesn’t match it can be ignored but after the pod starts it is required
  
  ![nat](../../images/nat.PNG)
  
## Node Affinity Types States

  ![nats](../../images/nats.PNG)
  
  ![nats1](../../images/nats1.PNG)
  
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes-using-node-affinity/
- https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/
