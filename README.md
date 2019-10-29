# kubernetes-cluster
kubernetes cluster with GCP

```
$ kubectl get nodes
NAME                                                STATUS   ROLES    AGE     VERSION
gke-standard-cluster-1-default-pool-b5e24cb4-j23k   Ready    <none>   4m43s   v1.13.10-gke.0
gke-standard-cluster-1-default-pool-b5e24cb4-v5ff   Ready    <none>   4m43s   v1.13.10-gke.0
gke-standard-cluster-1-default-pool-b5e24cb4-xnfg   Ready    <none>   4m41s   v1.13.10-gke.0


$ kubectl create namespace training-1
namespace/training-1 created

$ kubectl get namespaces
NAME          STATUS   AGE
default       Active   18m
kube-public   Active   18m
kube-system   Active   18m
training-1    Active   8m53s

$ kubectl get pods
No resources found.

kubectl create -f basic.yaml
pod/basicpod created
$ kubectl get pods
NAME       READY   STATUS              RESTARTS   AGE
basicpod   0/2     ContainerCreating   0          4s
$ kubectl get pods
NAME       READY   STATUS    RESTARTS   AGE
basicpod   2/2     Running   0          14s

```

