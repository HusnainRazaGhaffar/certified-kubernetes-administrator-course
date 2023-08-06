# Certification Tips - Imperative Commands with kubectl
  - Take me to the [Certification tips page](https://kodekloud.com/topic/certification-tips-imperative-commands-with-kubectl/)
## Imperative vs Declarative
- Imperative approach is where we specify what to do and how to do it. Basically step by step instructions to complete a task
- Declarative approach is where we just specify that we need this done. Like in Terraform we only specify that we need an EC2 server created with these specs
- In Kubernetes, imperative way of creating resources is by using CLI command where we mention what to do with the resources like,
    - `kubectl run nginx --image=nginx`
    - `kubectl create -f nginx.yaml`
- In Kubernetes, declarative way of creating resources is by using `kubectl apply` command where Kubernetes decides what to do if resource is creating for the first time or if it is created already
- When we use `kubectl apply` it saves the last-applied-configuration as an annotation in the live object’s configuration so that next time we use apply command it will compare the new configuration to last-applied-configuration and decide what needs to be done. `kubectl create` or `kubectl replace` command doesn’t save last-applied-configuration
