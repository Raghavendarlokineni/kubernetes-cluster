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
LAMU02WJ2MEHTD6:kind-cluster rlokinen$ 
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
