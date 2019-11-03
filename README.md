
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

**setting the environment** 

```
LAMU02WJ2MEHTD6:kind-cluster rlokinen$ export KUBECONFIG="$(./kind get kubeconfig-path --name="kubernetes-16")"
```
**Get the cluster-info**

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

**Clone the Nginx-ingress repo:**

```
git clone https://github.com/nginxinc/kubernetes-ingress.git
```

**Create namespace and service account for Ingress controller:**

`
$ kubectl apply -f common/ns-and-sa.yaml
namespace/nginx-ingress created
serviceaccount/nginx-ingress created
`

**Create secret with TLS certificate and a key for default server in NGINX:**

`
$ kubectl apply -f common/default-server-secret.yaml
secret/default-server-secret created
`

**Create a config map**

`
LAMU02WJ2MEHTD6:deployments rlokinen$ kubectl apply -f common/nginx-config.yaml
configmap/nginx-config created
`

**Configure RABC**

`
$ kubectl apply -f rbac/rbac.yaml 
clusterrole.rbac.authorization.k8s.io/nginx-ingress created
clusterrolebinding.rbac.authorization.k8s.io/nginx-ingress created
`

**Create deployment**

`
$ kubectl apply -f deployment/nginx-ingress.yaml
deployment.apps/nginx-ingress created
`

**Check the pod status**

`
kubectl get pods --namespace=nginx-ingress
NAME                             READY   STATUS    RESTARTS   AGE
nginx-ingress-7f4b784f79-s2jjl   1/1     Running   8          82m
`

**Create a service with node port to access the Ingress controller outside the cluster**

`
$ kubectl create -f service/nodeport.yaml
service/nginx-ingress created
`

### Access the monitoring dashboard of Nginx

By default, `stub_status` is available on port 8080. Use `kubectl port-forward` command to forward connections to port 8080 on your local machine to port 8080 of an NGINX ingress controller.

```
kubectl port-forward nginx-ingress-7f4b784f79-zxrlm 8080:8080 --namespace=nginx-ingress
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080
Handling connection for 8080
```

