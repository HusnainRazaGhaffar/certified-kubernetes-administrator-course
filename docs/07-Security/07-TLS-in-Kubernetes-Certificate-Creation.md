# TLS in kubernetes - Certificate Creation
  - Take me to [Video Tutorial](https://kodekloud.com/topic/tls-in-kubernetes-certificate-creation/)
  
In this section, we will take a look at TLS certificate creation in kubernetes

## Generate Certificates
- There are different tools available such as easyrsa, openssl or cfssl etc. or many others for generating certificates.

## Generating Certificate Authority (CA) Certificates

- Generate Keys
  ```
  $ openssl genrsa -out ca.key 2048
  ```
- Generate CSR
  ```
  $ openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
  ```
- Sign certificates (self-signed)
  ```
  $ openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
  ```
 
 ![ca1](../../images/ca1.PNG)
 
## Generating Client Certificates

#### Generating Admin User Certificates

- Generate Keys
  ```
  $ openssl genrsa -out admin.key 2048
  ```
- Generate CSR. It is NOT required to have exact name "kube-admin" it can be anything else as well
  ```
  $ openssl req -new -key admin.key -subj "/CN=kube-admin" -out admin.csr
  ```
- **NOTE:** If we create the CSR like above it will act like a normal user. To make the user a Kubernetes admin, we have to specify `system:masters` group name in the CSR like in this command
  ```
  $ openssl req -new -key admin.key -subj "/CN=kube-admin/O=system:masters" -out admin.csr
  ```
- Sign certificates (with CA cert/key)
  ```
  $ openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt
  ```
  
  ![ca2](../../images/ca2.PNG)
  
#### We follow the same procedure to generate client certificate for all other components that access the kube-apiserver.

#### Generating Kube-Scheduler Certificates

- Generate Keys
  ```
  $ openssl genrsa -out scheduler.key 2048
  ```
- Generate CSR. **NOTE:** It is required to have `system` prefix for kube-scheduler name
  ```
  $ openssl req -new -key scheduler.key -subj "/CN=system:kube-scheduler" -out scheduler.csr
  ```
- Sign certificates (with CA cert/key)
  ```
  $ openssl x509 -req -in scheduler.csr -CA ca.crt -CAkey ca.key -out scheduler.crt
  ```
  ![crt1](../../images/crt1.PNG)

#### Generating Kube-Controller-Manager Certificates

- Generate Keys
  ```
  $ openssl genrsa -out controller-manager.key 2048
  ```
- Generate CSR. **NOTE:** It is required to have `system` prefix for controller-manager name
  ```
  $ openssl req -new -key controller-manager.key -subj "/CN=system:kube-controller-manager" -out controller-manager.csr
  ```
- Sign certificates (with CA cert/key)
  ```
  $ openssl x509 -req -in controller-manager.csr -CA ca.crt -CAkey ca.key -out controller-manager.crt
  ```
  
  ![crt2](../../images/crt2.PNG)

#### Generating Kube-Proxy Certificates

- Generate Keys
  ```
  $ openssl genrsa -out kube-proxy.key 2048
  ```
- Generate CSR.
  ```
  $ openssl req -new -key kube-proxy.key -subj "/CN=kube-proxy" -out kube-proxy.csr
  ```
- Sign certificates (with CA cert/key)
  ```
  $ openssl x509 -req -in kube-proxy.csr -CA ca.crt -CAkey ca.key -out kube-proxy.crt
  ```

  ![crt3](../../images/crt3.PNG)
   
  ![crt4](../../images/crt4.PNG)
  
## Generating Server Certificates

## ETCD Server certificate
#### Generating ETCD Server Certificates

- Generate Keys
  ```
  $ openssl genrsa -out etcdserver.key 2048
  ```
- Generate CSR.
  ```
  $ openssl req -new -key etcdserver.key -subj "/CN=etcd-server" -out etcdserver.csr
  ```
- Sign certificates (with CA cert/key)
  ```
  $ openssl x509 -req -in etcdserver.csr -CA ca.crt -CAkey ca.key -out etcdserver.crt
  ```

- **Note:** If we are creating ETCD server in high availability, we will also need to generate etcd-peer certificiates for all the etcd peer servers
  ![etc1](../../images/etc1.PNG)
  
  ![etc2](../../images/etc2.PNG)
  
## Kube-apiserver certificate
#### Generating Certificates
- Generate Keys
  ```
  $ openssl genrsa -out apiserver.key 2048
  ```
- Generate CSR. **Note:** Although API Server is called kube-apiserver but it also needs to have it's following alternative names and server IP addresses added to it using openssl.cnf file
  - kubernetes
  - kubernetes.default
  - kubernetes.default.svc
  - kubernetes.default.svc.cluster.local

  ```
  $ openssl req -new -key apiserver.key -subj "/CN=kube-apiserver" -out apiserver.csr --config openssl.cnf
  ```
- Sign certificates (with CA cert/key)
  ```
  $ openssl x509 -req -in apiserver.csr -CA ca.crt -CAkey ca.key -out apiserver.crt
  ```

  ![api1](../../images/api1.PNG)
  
  ![api2](../../images/api2.PNG)
  
## Kubelet Nodes (Server Cert)
- Kubelet server is an https API server that runs on the each node, responsible for managing the node. This is the API that api-server talks to to monitor the as well as send information regarding what pod is scheduled on this node
- So each node will have a dedicated TLS server certificate with the node name in the SSL
- These certificates are then added to the kubelet server config

   ![kctl1](../../images/kctl1.PNG)
   
## Kubelet Nodes (Client Cert)
- Each node will have a dedicated TLS client certificate with the node name in the SSL as well
- These certificates are then added to the kubelet server config

- Generate Keys
  ```
  $ openssl genrsa -out kubelet-client.key 2048
  ```
- Generate CSR. **NOTE:** It is required to have `system:node` prefix for each node name and they must be added to `system:node` group
  ```
  $ openssl req -new -key kubelet-client.key -subj "/CN=system:node:node-01/O=system:nodes" -out kubelet-client.csr
  ```
- Sign certificates (with CA cert/key)
  ```
  $ openssl x509 -req -in kubelet-client.csr -CA ca.crt -CAkey ca.key -out kubelet-client.crt
  ```

   ![kctl2](../../images/kctl2.PNG)
   
   
   
  
  

  

  


  
  
  
  
 
