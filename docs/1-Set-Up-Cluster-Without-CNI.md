# Set Up Cluster Without CNI

테스트용 클러스터는 `kind` 로 이용해서 구성한다. 

## Install Kubernetes

```bash
# Create a cluster
kind create cluster --name calico --config ./config/kind-config.yaml
```

result:

```text
Creating cluster "calico" ...
 ✓ Ensuring node image (kindest/node:v1.30.0) 🖼
 ✓ Preparing nodes 📦 📦 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing StorageClass 💾
 ✓ Joining worker nodes 🚜
Set kubectl context to "kind-calico"
You can now use your cluster with:

kubectl cluster-info --context kind-calico

Have a nice day! 👋
```

## Chekout the cluster

```bash
kubectl config use-context kind-calico
kubectl get nodes
```

result:

```text
NAME                   STATUS     ROLES           AGE   VERSION
calico-control-plane   NotReady   control-plane   43s   v1.30.0
calico-worker          NotReady   <none>          22s   v1.30.0
calico-worker2         NotReady   <none>          22s   v1.30.0
```

## SSH to the nodes

```bash
docker ps
```

```bash
## SSH to the control-plane
docker exec -it calico-control-plane bash

## SSH to the worker nodes
docker exec -it calico-worker bash
docker exec -it calico-worker2 bash
```


