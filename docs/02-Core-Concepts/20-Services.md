# Kubernetes Services
  - Take me to [Video Tutorial](https://kodekloud.com/topic/services-3/)
  
In this section we will take a look at **`services`** in kubernetes

- Kubernetes services enable communication between various components within and outside of the application
- Kubernetes services helps us connect application together with other applications or users
- For example services made group of frontend pods available to the end user and services made backend pods available to the frontend pods and also helps connectivity of backend pods to the external data-source or database
- There are 3 types of services
    - Node Port - Forwards traffic from a port on the K8s node to the pod
    - Cluster IP - Virtual IP of the K8s network, usually used to communicate between group of pods
    - Load Balancer - Creates an external load balancer (of supported cloud provider) that send the traffic to pods

### Node Port Service

- Pods gets an IP address of a private network that is different from the host Kubernetes nodes. Services (node port) listens on the ports of the node and directs the traffic to the port of the pod that is on another network inside the Kubernetes host
- Node Port service ports can be between 30,000 to 32,767 range
- Following is a example Node Port service definition
    
    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: myapp-service
    spec:
      type: NodePort
      ports:
        - targetPort: 80
          port: 80
          nodePort: 30008
      selector:
        app: myapp
        type: frontend
    ```
    
- Here port is mandatory, if we don’t provide `targetPort`, value of `port` will be used for that. If we don’t provide `nodePort` any port between 30,000 to 32,767 will be allocated.
- `selector` should match the pod definition to which we want to forward the traffic of the service
- We can have multiple port mappings in a service definition

### Cluster IP Service

- Cluster IP service creates a virtual IP that sends the traffic to group of pods.
- Usually it is used for communication between different group of pods for reliable communication as pods are created and destroyed at any time
- Cluster IP type is the default service type, so if we don’t specify the type, by-default Cluster IP service will be created
- Following is a example Cluster IP service definition
    
    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: backend-service
    spec:
      type: ClusterIP
      ports:
        - targetPort: 80
          port: 80
      selector:
        app: myapp
        type: back-end
    ```
    
### Load Balancer Service

- In supported cloud platforms, it will create an external load balancer that will forward the traffic to the group of pods inside the Kubernetes cluster
- Following is a example Load Balancer service definition
    
    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: myapp-service
    spec:
      type: LoadBalancer
      ports:
        - targetPort: 80
          port: 80
          nodePort: 30008
      selector:
        app: myapp
        type: frontend
    ```
    

## Services
- Kubernetes Services enables communication between various components within and outside of the application.

  ![srv1](../../images/srv1.PNG)
  
#### Let's look at some other aspects of networking

## External Communication

- How do we as an **`external user`** access the **`web page`**?

  - From the node (Able to reach the application as expected)
  
    ![srv2](../../images/srv2.PNG)
    
  - From outside world (This should be our expectation, without something in the middle it will not reach the application)
  
    ![srv3](../../images/srv3.PNG)
   
    
 ## Service Types
 
 #### There are 3 types of service types in kubernetes
 
   ![srv-types](../../images/srv-types.PNG)
 
 1. NodePort
    - Where the service makes an internal port accessible on a port on the NODE.
      ```
      apiVersion: v1
      kind: Service
      metadata:
       name: myapp-service
      spec:
       types: NodePort
       ports:
       - targetPort: 80
         port: 80
         nodePort: 30008
      ```
     ![srvnp](../../images/srvnp.PNG)
      
      #### To connect the service to the pod
      ```
      apiVersion: v1
      kind: Service
      metadata:
       name: myapp-service
      spec:
       type: NodePort
       ports:
       - targetPort: 80
         port: 80
         nodePort: 30008
       selector:
         app: myapp
         type: front-end
       ```

    ![srvnp1](../../images/srvnp1.PNG)
      
      #### To create the service
      ```
      $ kubectl create -f service-definition.yaml
      ```
      
      #### To list the services
      ```
      $ kubectl get services
      ```
      
      #### To access the application from CLI instead of web browser
      ```
      $ curl http://192.168.1.2:30008
      ```
      
      ![srvnp2](../../images/srvnp2.PNG)

      #### A service with multiple pods
      
      ![srvnp3](../../images/srvnp3.PNG)
      
      #### When Pods are distributed across multiple nodes
     
      ![srvnp4](../../images/srvnp4.PNG)
     
            
 1. ClusterIP
    - In this case the service creates a **`Virtual IP`** inside the cluster to enable communication between different services such as a set of frontend servers to a set of backend servers.
    
 1. LoadBalancer
    - Where the service provisions a **`loadbalancer`** for our application in supported cloud providers.
    
K8s Reference Docs:
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/

