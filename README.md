# kubernetes-cluster
kubernetes local cluster with KIND - https://kind.sigs.k8s.io/

## Create multi-cluster

```
$ ./kind create cluster --name=kubernetes-16 --image kindest/node:v1.16.1 --config multi-cluster.yaml
Creating cluster "kubernetes-16" ...
 ✓ Ensuring node image (kindest/node:v1.16.1) 🖼
 ✓ Preparing nodes 📦📦📦 
 ✓ Creating kubeadm config 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
 ✓ Joining worker nodes 🚜 
Cluster creation complete. You can now use the cluster with:

export KUBECONFIG="$(kind get kubeconfig-path --name="kubernetes-16")"
kubectl cluster-info
```

setting the environment 

```
LAMU02WJ2MEHTD6:kind-cluster rlokinen$ export KUBECONFIG="$(./kind get kubeconfig-path --name="kubernetes-16")"
```
Get the cluster-info

```
LAMU02WJ2MEHTD6:kind-cluster rlokinen$ kubectl cluster-info
Kubernetes master is running at https://127.0.0.1:52315
KubeDNS is running at https://127.0.0.1:52315/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
LAMU02WJ2MEHTD6:kind-cluster rlokinen$ kubectl get nodes
NAME                          STATUS   ROLES    AGE     VERSION
kubernetes-16-control-plane   Ready    master   6m52s   v1.16.1
kubernetes-16-worker          Ready    <none>   6m20s   v1.16.1
kubernetes-16-worker2         Ready    <none>   6m20s   v1.16.1
```

Install a load balancer in the Kubernetes cluster, for this setup we are using Nginx - - https://github.com/nginxinc/kubernetes-ingress/blob/master/docs/installation.md

git clone https://github.com/nginxinc/kubernetes-ingress.git

$ kubectl apply -f common/ns-and-sa.yaml
namespace/nginx-ingress created
serviceaccount/nginx-ingress created
LAMU02WJ2MEHTD6:deployments rlokinen$ 
LAMU02WJ2MEHTD6:deployments rlokinen$ kubectl apply -f common/default-server-secret.yaml
secret/default-server-secret created
LAMU02WJ2MEHTD6:deployments rlokinen$ 
LAMU02WJ2MEHTD6:deployments rlokinen$ kubectl apply -f common/nginx-config.yaml
configmap/nginx-config created
LAMU02WJ2MEHTD6:deployments rlokinen$ 
LAMU02WJ2MEHTD6:deployments rlokinen$ kubectl apply -f common/custom-resource-definitions.yaml
customresourcedefinition.apiextensions.k8s.io/virtualservers.k8s.nginx.org created
customresourcedefinition.apiextensions.k8s.io/virtualserverroutes.k8s.nginx.org created
LAMU02WJ2MEHTD6:deployments rlokinen$ 
$ kubectl apply -f rbac/rbac.yaml 
clusterrole.rbac.authorization.k8s.io/nginx-ingress created
clusterrolebinding.rbac.authorization.k8s.io/nginx-ingress created

LAMU02WJ2MEHTD6:deployments rlokinen$ kubectl apply -f deployment/nginx-ingress.yaml
deployment.apps/nginx-ingress created
LAMU02WJ2MEHTD6:deployments rlokinen$ 
LAMU02WJ2MEHTD6:deployments rlokinen$ kubectl get pods --namespace=nginx-ingress
NAME                             READY   STATUS              RESTARTS   AGE
nginx-ingress-7f4b784f79-s2jjl   0/1     ContainerCreating   0          20s
LAMU02WJ2MEHTD6:deployments rlokinen$ kubectl get pods --namespace=nginx-ingress
NAME                             READY   STATUS              RESTARTS   AGE
nginx-ingress-7f4b784f79-s2jjl   0/1     ContainerCreating   0          27s
LAMU02WJ2MEHTD6:deployments rlokinen$ kubectl create -f service/nodeport.yaml
service/nginx-ingress created
LAMU02WJ2MEHTD6:deployments rlokinen$ 
LAMU02WJ2MEHTD6:deployments rlokinen$ kubectl get pods --namespace=nginx-ingress
NAME                             READY   STATUS              RESTARTS   AGE
nginx-ingress-7f4b784f79-s2jjl   0/1     ContainerCreating   0          50s



