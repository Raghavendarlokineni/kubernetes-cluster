# kubernetes-cluster
kubernetes cluster with GCP

kubernetes cluster info
```
$ kubectl get nodes
NAME                                                STATUS   ROLES    AGE     VERSION
gke-standard-cluster-1-default-pool-b5e24cb4-j23k   Ready    <none>   4m43s   v1.13.10-gke.0
gke-standard-cluster-1-default-pool-b5e24cb4-v5ff   Ready    <none>   4m43s   v1.13.10-gke.0
gke-standard-cluster-1-default-pool-b5e24cb4-xnfg   Ready    <none>   4m41s   v1.13.10-gke.0
```

create a namespace

```
$ kubectl create namespace training-1
namespace/training-1 created
```
get all the namespaces in the cluster

```
$ kubectl get namespaces
NAME          STATUS   AGE
default       Active   18m
kube-public   Active   18m
kube-system   Active   18m
training-1    Active   8m53s
```

get the pods, by default `default` namespace pods are displayed.
```
$ kubectl get pods
No resources found.
```
create a pod using file
```
kubectl create -f basic.yaml
pod/basicpod created
$ kubectl get pods
NAME       READY   STATUS              RESTARTS   AGE
basicpod   0/2     ContainerCreating   0          4s
$ kubectl get pods
NAME       READY   STATUS    RESTARTS   AGE
basicpod   2/2     Running   0          14s
```

to get pods from all the namespaces
```
$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                                           READY   STATUS    RESTARTS   AGE
default       nginx                                                          1/1     Running   0          76s
default       nginx-training-7f45db97b6-5slk7                                1/1     Running   0          4m49s
kube-system   event-exporter-v0.2.4-5f88c66fb7-xzwpz                         2/2     Running   0          3d15h
kube-system   fluentd-gcp-scaler-59b7b75cd7-vq5cg                            1/1     Running   0          3d15h
kube-system   fluentd-gcp-v3.2.0-cp2mm                                       2/2     Running   0          3d15h
kube-system   fluentd-gcp-v3.2.0-sddsp                                       2/2     Running   0          3d15h
kube-system   fluentd-gcp-v3.2.0-t4ht7                                       2/2     Running   0          3d15h
kube-system   heapster-v1.6.1-66fd5c9d75-k9kvg                               3/3     Running   0          3d15h
kube-system   kube-dns-79868f54c5-8mstm                                      4/4     Running   0          3d15h
kube-system   kube-dns-79868f54c5-wlbzm                                      4/4     Running   0          3d15h
kube-system   kube-dns-autoscaler-bb58c6784-jft24                            1/1     Running   0          3d15h
kube-system   kube-proxy-gke-standard-cluster-1-default-pool-b5e24cb4-j23k   1/1     Running   0          3d15h
kube-system   kube-proxy-gke-standard-cluster-1-default-pool-b5e24cb4-v5ff   1/1     Running   0          3d15h
kube-system   kube-proxy-gke-standard-cluster-1-default-pool-b5e24cb4-xnfg   1/1     Running   0          3d15h
kube-system   l7-default-backend-fd59995cd-7d4xd                             1/1     Running   0          3d15h
kube-system   metrics-server-v0.3.1-57c75779f-btx2j                          2/2     Running   0          3d15h
kube-system   prometheus-to-sd-2plmt                                         1/1     Running   0          3d15h
kube-system   prometheus-to-sd-525c9                                         1/1     Running   0          3d15h
kube-system   prometheus-to-sd-v2pzp                                         1/1     Running   0          3d15h
training-1    basicpod                                                       2/2     Running   0          3d14h

```
to display pods running on different node

```
$ kubectl get pods --output=wide -n default
NAME                                READY   STATUS    RESTARTS   AGE   IP         NODE                                                NOMINATED NODE   READINESS GATES
nginx-cli-75f844cf5f-tqsdj          1/1     Running   0          47m   10.8.2.5   gke-standard-cluster-1-default-pool-b5e24cb4-xnfg   <none>           <none>
nginx-deployment-59b6fd499d-j6v4d   1/1     Running   0          23m   10.8.2.6   gke-standard-cluster-1-default-pool-b5e24cb4-xnfg   <none>           <none>
nginx-training-7f45db97b6-5slk7     1/1     Running   0          61m   10.8.1.3   gke-standard-cluster-1-default-pool-b5e24cb4-v5ff   <none>           <none>
```


delete pod if exists and create pod using a command
```
$ kubectl run --generator=run-pod/v1 nginx --image=raghavendar/docker-training:latest
Error from server (AlreadyExists): pods "nginx" already exists

$ kubectl delete pod nginx
pod "nginx" deleted

$ kubectl run --generator=run-pod/v1 nginx --image=raghavendar/docker-training:latest
pod/nginx created
```

delete a deployment
```

$ kubectl delete deployment nginx-deployment
deployment.extensions "nginx-deployment" deleted
```
create deployment using a command, get the deployments
```
$ kubectl create deployment nginx-deployment --image=raghavendar/docker-training:latest
deployment.apps/nginx-deployment created

$ kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-cli          1/1     1            1           24m
nginx-deployment   1/1     1            1           9s
nginx-training     1/1     1            1           38m
```

$ kubectl expose deployment  nginx-deployment --port=80 --target-port=80 --type=NodePort --name=nginx-deployment-nodeport
service/nginx-deployment-nodeport exposed
lokineni_raghavendar@cloudshell:~ (vernal-reality-242816)$ kubectl get service
NAME                        TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
kubernetes                  ClusterIP   10.12.0.1     <none>        443/TCP        3d16h
nginx-deployment            ClusterIP   10.12.1.145   <none>        80/TCP         11m
nginx-deployment-nodeport   NodePort    10.12.2.213   <none>        80:31002/TCP   16s

lokineni_raghavendar@cloudshell:~ (vernal-reality-242816)$ kubectl describe service nginx-deployment-nodeport
Name:                     nginx-deployment-nodeport
Namespace:                default
Labels:                   app=nginx-deployment
Annotations:              <none>
Selector:                 app=nginx-deployment
Type:                     NodePort
IP:                       10.12.2.213
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  31002/TCP
Endpoints:                10.8.2.6:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
lokineni_raghavendar@cloudshell:~ (vernal-reality-242816)$ gcloud compute instances list
NAME                                               ZONE           MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP     STATUS
gke-standard-cluster-1-default-pool-b5e24cb4-j23k  us-central1-a  g1-small                   10.128.0.14  35.223.131.125  RUNNING
gke-standard-cluster-1-default-pool-b5e24cb4-v5ff  us-central1-a  g1-small                   10.128.0.15  35.239.188.73   RUNNING
gke-standard-cluster-1-default-pool-b5e24cb4-xnfg  us-central1-a  g1-small                   10.128.0.16  34.69.48.4      RUNNING


```

