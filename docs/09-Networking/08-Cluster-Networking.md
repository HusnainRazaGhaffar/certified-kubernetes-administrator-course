# Pre-requisite Cluster Networking

  - Take me to [Lecture](https://kodekloud.com/topic/cluster-networking/)

In this section, we will take a look at **Pre-requisite of the Cluster Networking**

- Set the unique hostname.
- Get the IP addr of the system (master and worker node).
- Check the Ports.

## IP and Hostname

- To view the hostname

```
$ hostname 
```

- To view the IP addr of the system

```
$ ip a
```


## Set the hostname

```
$ hostnamectl set-hostname <host-name>

$ exec bash
```

## View the Listening Ports of the system

```
$ netstat -nltp
```

## Listening Ports of Kubernetes Master (Control Plane)
- kube-apiserver listens on 6443
- kubelet (if installed on master) listens on 10250
- Kube-scheduler listens on 10251
- kube-controller-manager listens on 10252
- etcd listens on port 2379
- If etcd is in high availability then port 2380 must also open on all etcd servers

## Listening Ports of Kubernetes Worker Nodes
- kubelet listens on 10250
- Worker node exposes different services on ports 30,000 to 32,767


#### References Docs

- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#check-required-ports
- https://kubernetes.io/docs/concepts/cluster-administration/networking/
