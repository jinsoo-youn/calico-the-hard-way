# Install CNI Plugin 

## Provision Kubernetes user account for the plugin 

```text
docker exec -it calico-control-plane bash
cd /etc/kubernetes/pki
```

```bash
openssl req -newkey rsa:4096 \
  -keyout cni.key \
  -nodes \
  -out cni.csr \
  -subj "/CN=calico-cni"
```

```bash
openssl x509 -req -in cni.csr \
  -CA ca.crt \
  -CAkey ca.key \
  -CAcreateserial \
  -out cni.crt \
  -days 3653
```

results:

```text
ls -al | grep cni
-rw-r--r-- 1 root root 1342 Jun 16 04:06 cni.crt
-rw-r--r-- 1 root root 1586 Jun 16 04:05 cni.csr
-rw------- 1 root root 3268 Jun 16 04:05 cni.key
```

## Create Kubernetes user account and move to every worker node
```bash
cd ~
APISERVER=$(kubectl config view -o jsonpath='{.clusters[0].cluster.server}')
kubectl config set-cluster kubernetes \
    --certificate-authority=/etc/kubernetes/pki/ca.crt \
    --embed-certs=true \
    --server=$APISERVER \
    --kubeconfig=cni.kubeconfig

kubectl config set-credentials calico-cni \
    --client-certificate=/etc/kubernetes/pki/cni.crt \
    --client-key=/etc/kubernetes/pki/cni.key \
    --embed-certs=true \
    --kubeconfig=cni.kubeconfig

kubectl config set-context default \
    --cluster=kubernetes \
    --user=calico-cni \
    --kubeconfig=cni.kubeconfig

kubectl config use-context default --kubeconfig=cni.kubeconfig

cp cni.kubeconfig /tmp/calico/cni.kubeconfig
```

## Provision RBAC

```bash
kubectl apply -f /tmp/calico/rbac.yaml
```

## Install the plugin 

Do this on every node

```bash
# control-plane
docker exec -it calico-control-plane bash
# worker 1
docker exec -it calico-worker bash
# worker 2 
docker exec -it calico-worker2 bash
```

### Install the CNI plugin Binaries 
```bash
cp /tmp/calico/cni/amd64/* /opt/cni/bin
chmod +x /opt/cni/bin/*
```


```bash
mkdir -p /etc/cni/net.d
```

Copy the kubeconfig
```bash
cp /tmp/calico/cni.kubeconfig /etc/cni/net.d/calico-kubeconfig
chmod 600 /etc/cni/net.d/calico-kubeconfig
```

Copy the CNI configuration
```bash
cp /tmp/calico/10-calico.conflist /etc/cni/net.d/10-calico.conflist
sed -i "s@__KUBERNETES_NODE_NAME__@$(hostname)@g" /etc/cni/net.d/10-calico.conflist
```

## Check 

```bash
kubectl get nodes 
```

results:
```
NAME                   STATUS   ROLES           AGE   VERSION
calico-control-plane   Ready    control-plane   55m   v1.30.0
calico-worker          Ready    <none>          55m   v1.30.0
calico-worker2         Ready    <none>          55m   v1.30.0
```
