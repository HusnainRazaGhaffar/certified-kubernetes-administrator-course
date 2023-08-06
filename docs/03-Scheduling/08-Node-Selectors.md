# Node Selectors
  - Take me to [Video Tutorial](https://kodekloud.com/topic/node-selectors/)

In this section, we will take a look at Node Selectors in Kubernetes
- If we want a pod to be deployed to a specific node we can add a property `NodeSelector` in pod’s manifest while creating it so that scheduler will then deploy the pod on the node that matches the key value pair of what’s mentioned in NodeSelector
- For node selector to work, it is necessary that nodes must already have those labels
- To add label to a node

  Syntax
  ```
  $ kubectl label nodes <node-name> <label-key>=<label-value>
  ```
  Example
  ```
  $ kubectl label nodes node-1 size=Large
  ```
 
![ln](../../images/ln.PNG)
  
- To apply node selector to the pod, when creating a pod using yaml manifest file, add `NodeSelector` in pod’s definition
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
  ```
  $ kubectl create -f pod-definition.yml
  ```
  
![nsel](../../images/nsel.PNG)
  
## Node Selector - Limitations
- We used a single label and selector to achieve our goal here. But what if our requirement is much more complex.
  
![nsl](../../images/nsl.PNG)
 
- For this we have **`Node Affinity and Anti Affinity`**
  
#### K8s Reference Docs
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector





