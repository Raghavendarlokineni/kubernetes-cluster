# kubernetes-cluster
kubernetes cluster with GCP

### **kubernetes cluster info**
```
$ kubectl get nodes
NAME                                                STATUS   ROLES    AGE     VERSION
gke-standard-cluster-1-default-pool-b5e24cb4-j23k   Ready    <none>   4m43s   v1.13.10-gke.0
gke-standard-cluster-1-default-pool-b5e24cb4-v5ff   Ready    <none>   4m43s   v1.13.10-gke.0
gke-standard-cluster-1-default-pool-b5e24cb4-xnfg   Ready    <none>   4m41s   v1.13.10-gke.0
```

**create a namespace**

```
$ kubectl create namespace training-1
namespace/training-1 created
```

**get all resources supported by kubernetes**
```
$ kubectl api-resources
NAME                              SHORTNAMES   APIGROUP                       NAMESPACED   KIND
bindings                                                                      true         Binding
componentstatuses                 cs                                          false        ComponentStatus
configmaps                        cm                                          true         ConfigMap
endpoints                         ep                                          true         Endpoints
events                            ev                                          true         Event
limitranges                       limits                                      true         LimitRange
namespaces                        ns                                          false        Namespace
nodes                             no                                          false        Node
persistentvolumeclaims            pvc                                         true         PersistentVolumeClaim
persistentvolumes                 pv                                          false        PersistentVolume
pods                              po                                          true         Pod
podtemplates                                                                  true         PodTemplate
replicationcontrollers            rc                                          true         ReplicationController
resourcequotas                    quota                                       true         ResourceQuota
secrets                                                                       true         Secret
serviceaccounts                   sa                                          true         ServiceAccount
services                          svc                                         true         Service
mutatingwebhookconfigurations                  admissionregistration.k8s.io   false        MutatingWebhookConfiguration
validatingwebhookconfigurations                admissionregistration.k8s.io   false        ValidatingWebhookConfiguration
customresourcedefinitions         crd,crds     apiextensions.k8s.io           false        CustomResourceDefinition
apiservices                                    apiregistration.k8s.io         false        APIService
controllerrevisions                            apps                           true         ControllerRevision
daemonsets                        ds           apps                           true         DaemonSet
deployments                       deploy       apps                           true         Deployment
replicasets                       rs           apps                           true         ReplicaSet
statefulsets                      sts          apps                           true         StatefulSet
tokenreviews                                   authentication.k8s.io          false        TokenReview
localsubjectaccessreviews                      authorization.k8s.io           true         LocalSubjectAccessReview
selfsubjectaccessreviews                       authorization.k8s.io           false        SelfSubjectAccessReview
subjectaccessreviews                           authorization.k8s.io           false        SubjectAccessReview
horizontalpodautoscalers          hpa          autoscaling                    true         HorizontalPodAutoscaler
cronjobs                          cj           batch                          true         CronJob
jobs                                           batch                          true         Job
certificatesigningrequests        csr          certificates.k8s.io            false        CertificateSigningRequest
backendconfigs                                 cloud.google.com               true         BackendConfig
leases                                         coordination.k8s.io            true         Lease
daemonsets                        ds           extensions                     true         DaemonSet
deployments                       deploy       extensions                     true         Deployment
ingresses                         ing          extensions                     true         Ingress
networkpolicies                   netpol       extensions                     true         NetworkPolicy
podsecuritypolicies               psp          extensions                     false        PodSecurityPolicy
replicasets                       rs           extensions                     true         ReplicaSet
nodes                                          metrics.k8s.io                 false        NodeMetrics
pods                                           metrics.k8s.io                 true         PodMetrics
managedcertificates               mcrt         networking.gke.io              true         ManagedCertificate
networkpolicies                   netpol       networking.k8s.io              true         NetworkPolicy
poddisruptionbudgets              pdb          policy                         true         PodDisruptionBudget
podsecuritypolicies               psp          policy                         false        PodSecurityPolicy
clusterrolebindings                            rbac.authorization.k8s.io      false        ClusterRoleBinding
clusterroles                                   rbac.authorization.k8s.io      false        ClusterRole
rolebindings                                   rbac.authorization.k8s.io      true         RoleBinding
roles                                          rbac.authorization.k8s.io      true         Role
scalingpolicies                                scalingpolicy.kope.io          true         ScalingPolicy
priorityclasses                   pc           scheduling.k8s.io              false        PriorityClass
storageclasses                    sc           storage.k8s.io                 false        StorageClass
volumeattachments                              storage.k8s.io                 false        VolumeAttachment
```

**get all the namespaces in the cluster**

```
$ kubectl get namespaces
NAME          STATUS   AGE
default       Active   18m
kube-public   Active   18m
kube-system   Active   18m
training-1    Active   8m53s
```

**get the pods, by default `default` namespace pods are displayed.**
```
$ kubectl get pods
No resources found.
```
**create a pod using file**
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

**to get pods from all the namespaces**
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
**to display pods running on different node**

```
$ kubectl get pods --output=wide -n default
NAME                                READY   STATUS    RESTARTS   AGE   IP         NODE                                                NOMINATED NODE   READINESS GATES
nginx-cli-75f844cf5f-tqsdj          1/1     Running   0          47m   10.8.2.5   gke-standard-cluster-1-default-pool-b5e24cb4-xnfg   <none>           <none>
nginx-deployment-59b6fd499d-j6v4d   1/1     Running   0          23m   10.8.2.6   gke-standard-cluster-1-default-pool-b5e24cb4-xnfg   <none>           <none>
nginx-training-7f45db97b6-5slk7     1/1     Running   0          61m   10.8.1.3   gke-standard-cluster-1-default-pool-b5e24cb4-v5ff   <none>           <none>
```


**delete pod if exists and create pod using a command**
```
$ kubectl run --generator=run-pod/v1 nginx --image=raghavendar/docker-training:latest
Error from server (AlreadyExists): pods "nginx" already exists

$ kubectl delete pod nginx
pod "nginx" deleted

$ kubectl run --generator=run-pod/v1 nginx --image=raghavendar/docker-training:latest
pod/nginx created
```

**delete a deployment**
```

$ kubectl delete deployment nginx-deployment
deployment.extensions "nginx-deployment" deleted
```
**create deployment using a command, get the deployments**
```
$ kubectl create deployment nginx-deployment --image=raghavendar/docker-training:latest
deployment.apps/nginx-deployment created

$ kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-cli          1/1     1            1           24m
nginx-deployment   1/1     1            1           9s
nginx-training     1/1     1            1           38m
```

**creating a template from an available resource**

```
kubectl get deployment demo-deployment -o yaml --export > deployment.yaml
```

```
$ kubectl expose deployment  nginx-deployment --port=80 --target-port=80 --type=NodePort --name=nginx-deployment-nodeport
service/nginx-deployment-nodeport exposed

$ kubectl get service
NAME                        TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
kubernetes                  ClusterIP   10.12.0.1     <none>        443/TCP        3d16h
nginx-deployment            ClusterIP   10.12.1.145   <none>        80/TCP         11m
nginx-deployment-nodeport   NodePort    10.12.2.213   <none>        80:31002/TCP   16s

$ kubectl describe service nginx-deployment-nodeport
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

```

#### **access service using LoadBalancer IP**
```

$ kubectl get service -n tst
NAME                 TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)        AGE
demo-nginx-load      LoadBalancer   10.12.7.27    35.238.11.93   80:32415/TCP   5h31m
demo-nginx-node      NodePort       10.12.6.120   <none>         80:32011/TCP   5h33m
demo-nginx-service   ClusterIP      10.12.3.142   <none>         80/TCP         5h35m


$ curl http://35.238.11.93:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

#### creating peristent volumes

```
$ kubectl apply -f persistent-volume.yaml -n tst                                                                             
persistentvolume/pv-volume created

$ kubectl get pv -n tsts
NAME        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
pv-volume   10Gi       RWO            Retain           Available           manual                  8s

```

#### creating peristent volume claims

```
$ kubectl apply -f persistent-volume-claim.yaml -n tst
persistentvolumeclaim/pv-claim created


$ kubectl get pvc -n tst
NAME       STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pv-claim   Bound    pvc-1decd296-012f-11ea-a594-42010a80015e   3Gi        RWO            standard       7s
```

#### need for PVC
```
)$ kubectl get pods -n tst
NAME                                READY   STATUS    RESTARTS   AGE
demo-nginx-7fdd948cc-msmkn          1/1     Running   0          20h
nginx-deployment-59b6fd499d-d8cjr   1/1     Running   0          20h


$ kubectl exec -it demo-nginx-7fdd948cc-msmkn /bin/bash -n tst
root@demo-nginx-7fdd948cc-msmkn:/#
root@demo-nginx-7fdd948cc-msmkn:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@demo-nginx-7fdd948cc-msmkn:/# cd tmp
root@demo-nginx-7fdd948cc-msmkn:/tmp# ls
root@demo-nginx-7fdd948cc-msmkn:/tmp# touch hello
root@demo-nginx-7fdd948cc-msmkn:/tmp# ls
hello
root@demo-nginx-7fdd948cc-msmkn:/tmp# exit
exit


$ kubectl delete pod demo-nginx-7fdd948cc-msmkn -n tst
pod "demo-nginx-7fdd948cc-msmkn" deleted


$ kubectl get pods -n tst
NAME                                READY   STATUS    RESTARTS   AGE
demo-nginx-7fdd948cc-5sp9b          1/1     Running   0          40s
nginx-deployment-59b6fd499d-d8cjr   1/1     Running   0          20h


$ kubectl exec -it demo-nginx-7fdd948cc-5sp9b /bin/bash -n tst
root@demo-nginx-7fdd948cc-5sp9b:/# cd tmp
root@demo-nginx-7fdd948cc-5sp9b:/tmp# ls
root@demo-nginx-7fdd948cc-5sp9b:/tmp#
```

#### persisting data

```
$ kubectl apply -f demo-deployment-pvc.yaml -n tst
deployment.extensions/demo-nginx-pvc created

$ kubectl get pods -n tst
NAME                                READY   STATUS    RESTARTS   AGE
demo-nginx-7fdd948cc-5sp9b          1/1     Running   0          6m27s
demo-nginx-pvc-5bcd8cc96c-vdz8k     1/1     Running   0          16s
nginx-deployment-59b6fd499d-d8cjr   1/1     Running   0          20h

$ kubectl exec -it demo-nginx-pvc-5bcd8cc96c-vdz8k /bin/bash -n tst
root@demo-nginx-pvc-5bcd8cc96c-vdz8k:/# cd /tmp
root@demo-nginx-pvc-5bcd8cc96c-vdz8k:/tmp# ls
lost+found
root@demo-nginx-pvc-5bcd8cc96c-vdz8k:/tmp# touch hello
root@demo-nginx-pvc-5bcd8cc96c-vdz8k:/tmp# ls
hello  lost+found
root@demo-nginx-pvc-5bcd8cc96c-vdz8k:/tmp# ls -ltr
total 16
drwx------ 2 root root 16384 Nov  7 07:35 lost+found
-rw-r--r-- 1 root root     0 Nov  7 07:37 hello
root@demo-nginx-pvc-5bcd8cc96c-vdz8k:/tmp# exit
exit

$ kubectl delete pod demo-nginx-pvc-5bcd8cc96c-vdz8k -n tst
pod "demo-nginx-pvc-5bcd8cc96c-vdz8k" deleted

$ kubectl get pods -n tst
NAME                                READY   STATUS              RESTARTS   AGE
demo-nginx-7fdd948cc-5sp9b          1/1     Running             0          8m28s
demo-nginx-pvc-5bcd8cc96c-mqdkl     0/1     ContainerCreating   0          11s
nginx-deployment-59b6fd499d-d8cjr   1/1     Running             0          20h
$ kubectl get pods -n tst
NAME                                READY   STATUS    RESTARTS   AGE
demo-nginx-7fdd948cc-5sp9b          1/1     Running   0          8m54s
demo-nginx-pvc-5bcd8cc96c-mqdkl     1/1     Running   0          37s
nginx-deployment-59b6fd499d-d8cjr   1/1     Running   0          20h

$ kubectl exec -it demo-nginx-pvc-5bcd8cc96c-mqdkl /bin/bash -n tst
root@demo-nginx-pvc-5bcd8cc96c-mqdkl:/# cd /tmp
root@demo-nginx-pvc-5bcd8cc96c-mqdkl:/tmp# ls
hello  lost+found
root@demo-nginx-pvc-5bcd8cc96c-mqdkl:/tmp#
```

#### creating secrets
```
$ kubectl get secrets -n tst
NAME                  TYPE                                  DATA   AGE
default-token-l4k2t   kubernetes.io/service-account-token   3      21h


$ kubectl create -f secret.yaml -n tst
secret/test-secret created

$ kubectl get secrets -n tst
NAME                  TYPE                                  DATA   AGE
default-token-l4k2t   kubernetes.io/service-account-token   3      21h
test-secret           Opaque                                1      4s
```

#### scaling replicas from command line

```

$kubectl autoscale deployment php-apache --min=2 --max=10 --cpu-percent=10

$ kubectl get pods
NAME                                READY   STATUS              RESTARTS   AGE
demo-deployment-75996f6ccf-259nq    1/1     Running             0          8d
demo-nginx-7fdd948cc-n5nrj          1/1     Running             0          4d3h
nginx                               1/1     Running             0          22h
nginx-cli-75f844cf5f-tqsdj          1/1     Running             0          8d
nginx-deployment-59b6fd499d-j6v4d   1/1     Running             0          8d
nginx-training-7f45db97b6-5slk7     1/1     Running             0          8d
php-apache-6b565b5d5-42ps7          0/1     Pending             0          40m
php-apache-6b565b5d5-5kvx9          0/1     Pending             0          40m
php-apache-6b565b5d5-7qprm          1/1     Running             0          40m
php-apache-6b565b5d5-ggpw5          1/1     Running             0          43m
php-apache-6b565b5d5-l8hmv          0/1     Pending             0          40m
php-apache-6b565b5d5-zvs7r          0/1     ContainerCreating   0          39m

$ kubectl scale --replicas=1 deployment php-apache
deployment.extensions/php-apache scaled

$ kubectl get pods
NAME                                READY   STATUS        RESTARTS   AGE
demo-deployment-75996f6ccf-259nq    1/1     Running       0          8d
demo-nginx-7fdd948cc-n5nrj          1/1     Running       0          4d3h
nginx                               1/1     Running       0          22h
nginx-cli-75f844cf5f-tqsdj          1/1     Running       0          8d
nginx-deployment-59b6fd499d-j6v4d   1/1     Running       0          8d
nginx-training-7f45db97b6-5slk7     1/1     Running       0          8d
php-apache-6b565b5d5-7qprm          0/1     Terminating   0          42m
php-apache-6b565b5d5-ggpw5          1/1     Running       0          44m
php-apache-6b565b5d5-zvs7r          0/1     Terminating   0          40m

$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
demo-deployment-75996f6ccf-259nq    1/1     Running   0          8d
demo-nginx-7fdd948cc-n5nrj          1/1     Running   0          4d3h
nginx                               1/1     Running   0          22h
nginx-cli-75f844cf5f-tqsdj          1/1     Running   0          8d
nginx-deployment-59b6fd499d-j6v4d   1/1     Running   0          8d
nginx-training-7f45db97b6-5slk7     1/1     Running   0          8d
php-apache-6b565b5d5-ggpw5          1/1     Running   0          44m
```

