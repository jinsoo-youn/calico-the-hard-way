# Configure Calico IP Pools

## SSH to the control-plane

```bash
docker exec -it calico-control-plane bash
```

## Configure Calico IP Pools
```bash
calicoctl apply -f /tmp/calico/default-pool.yaml
```

```bash
calicoctl create -f default-pool.yaml
```

verify

```text
calicoctl get ippool
```

```text
NAME                  CIDR             SELECTOR
default-ipv4-ippool   192.168.0.0/18   all()
```

check the IP pool on CRD
```bash
kubectl get ippool
```

```text
NAME                  AGE
default-ipv4-ippool   54s
```
