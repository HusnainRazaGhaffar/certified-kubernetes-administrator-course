# Cluster Roles
  - Take me to [Video Tutorial](https://kodekloud.com/topic/cluster-roles/)
  
In this section, we will take a look at cluster roles

## Roles
- Roles and Rolebindings are namespaced meaning they are created within namespaces.
  
  ![roles](../../images/roles.PNG)
  
## Namespaces
- Can you group or isolate nodes within  a namespace?
  - No, those are cluster wide or cluster scoped resources. They cannot be associated to any particular namespace.
  
  ![namespace](../../images/namespace.PNG)
  
- So the resources are categorized as either namespaced or cluster scoped.
  
- To see namespaced resources
  ```
  $ kubectl api-resources --namespaced=true
  ```
- To see non-namespaced resources
  ```
  $ $ kubectl api-resources --namespaced=false
  ```
  
  ![namespace1](../../images/namespace1.PNG)
  
## Cluster Roles and Cluster Role Bindings
- Cluster Roles are roles except they are for a cluster scoped resources. Kind as **`CLusterRole`** 
  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    name: cluster-administrator
  rules:
  - apiGroups: [""] # "" indicates the core API group
    resources: ["nodes"]
    verbs: ["get", "list", "delete", "create"]
  ```
  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
    name: cluster-admin-role-binding
  subjects:
  - kind: User
    name: cluster-admin
    apiGroup: rbac.authorization.k8s.io
  roleRef:
    kind: ClusterRole
    name: cluster-administrator
    apiGroup: rbac.authorization.k8s.io
  ```
  ```
  $ kubectl create -f cluster-admin-role.yaml
  $ kubectl create -f cluster-admin-role-binding.yaml
  ```
  
 ![cr1](../../images/cr1.PNG)
  
- You can create a cluster role for namespace resources as well. When you do that user will have access to these resources across all namespaces.

# Service Accounts
- User roles are consumed by humain users but service accounts are used by different services that wants to interact with Kubernetes
- When we create a service account, it's Authentication Bearer Token in Kubernetes is also created (with the same name)
- Till Kubernetes v1.22 By default one service account is created for each namespace we create and that service account's secret is bydefault attached to all pods of that namespace. But this service account has very limited access of Kubernetes APIs
- Starting Kubernetes v1.24 the default service account is mounted as projected volume and it also uses Kubernetes service account api. Also in v1.24 when a service account is created, no token is created with that by-default so a token has to be created on your own.
- Also in v1.24 service account token will have a default expiry time of 1 hour if you don't specify the expiry. But if you want to create service account's token wihtout any expiry, you can do that by using specific type and annotiation `kubernetes.io/service/-account-token` like shown below
```
apiVersion: v1
kind: Secret
type: kubernetes.io/service/-account-token
metadata:
  name: mysecretname
  annotations:
    kubernetes.io/service/-account-token: mysecretname
```

- Command to decode the service account JWT Authentication Bearer Token is as follows. Or it can be decoded from http://jwt.io
```
jq -R 'split(".") | select(length > 0) | .[0],.[1] | @base64d | fromjson' <<< long-token-string-ajhHOLhKHJKhkJGHKJGJH
```

#### K8s Reference Docs
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/#command-line-utilities
- https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/#bound-service-account-token-volume
  
