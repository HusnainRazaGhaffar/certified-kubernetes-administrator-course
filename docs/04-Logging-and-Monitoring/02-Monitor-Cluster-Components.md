# Monitor Cluster Components
  - Take me to [Video Tutuorials](https://kodekloud.com/topic/monitor-cluster-components/)
  
In this section, we will take a look at monitoring kubernetes cluster

#### How do you monitor resource consumption in kubernetes? or more importantly, what would you like to monitor?
- Out of the box Kubernetes doesn't offer any option to collect pods/nodes metrics. We need to use any of the following options
  - Metrics Server
  - Prometheus
  - Elastic Stack
  - DataDog
  - dynatrace

  ![mon](../../images/mon.PNG)
 
## Heapster vs Metrics Server
- Heapster is now deprecated and a slimmed down version was formed known as the **`metrics server`**.
- We can have one metrics server per Kubernetes cluster

  ![hpms](../../images/hpms.PNG)
  
## Metrics Server
- Metrics server doesn't store any metrics on the disk, it stores metrics in-memory and that's why you can see historical performance data
- For historical performance data, we must use the advanced monitoring tools like datadog etc

  ![ms1](../../images/ms1.PNG)

#### How are the metrics generated for the PODs on these nodes?
- Kubelet collects nodes metrics and kubelet component cAdvisor collects the metrics of pods and make those metrics available through the kubelet API for metrics server

  ![ca](../../images/ca.PNG)
  
## Metrics Server - Getting Started
- To install metrics server on a minikube cluster execute `minikube addons enable metrics-server`
- For other installations, clone the menifest files from the github and install the metrics server in Kubernetes

  ![msg](../../images/msg.PNG)
  
- Clone the metric server from github repo
  ```
  $ git clone https://github.com/kubernetes-incubator/metrics-server.git
  ```
- Deploy the metric server
  ```
  $ kubectl create -f metric-server/deploy/1.8+/
  ```
  
- View the cluster performance
  ```
  $ kubectl top node
  ```
- View performance metrics of pod
  ```
  $ kubectl top pod
  ```
  
  ![view](../../images/view.PNG)
  
  
