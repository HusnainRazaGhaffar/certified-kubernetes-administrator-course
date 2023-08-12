# Configuring Kubernetes Schedulers
  - Take me to [video Tutorial](https://kodekloud.com/topic/configuring-kubernetes-scheduler/)
  
In this section, we will take a look at configuring kubernetes schedulers specially scheduler profiles
- A scheduling Profile allows you to configure the different stages of scheduling in the kube-scheduler. Each stage is exposed in an extension point. Plugins provide scheduling behaviors by implementing one or more of these extension points.
- You can configure a single instance of kube-scheduler to run multiple profiles
- Further details can be read here https://kubernetes.io/docs/reference/scheduling/config/#profiles

## References
- https://kubernetes.io/docs/reference/scheduling/config/#profiles
- https://github.com/kubernetes/community/blob/master/contributors/devel/sig-scheduling/scheduler.md
- https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/
- https://jvns.ca/blog/2017/07/27/how-does-the-kubernetes-scheduler-work/
- https://stackoverflow.com/questions/28857993/how-does-kubernetes-scheduler-work

