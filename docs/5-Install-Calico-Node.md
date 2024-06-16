# Install Calico Node 

vxlan 구성 파일에서 calico-cni 부분을 삭제하고 설치하게끔 yaml 파일을 수정했다. 

해당 파일로 설치를 진행한다. 

calico-node 에 대해 인증서를 별도로 만들 필요 없이, service account로 생성하면 된다. 

원본에서는 typha를 사용하지만, 이번 랩에서는 사용하지 않는다. 

## calico-node, calico-controller 설치 

```bash
kubectl apply -f ./config/calico/calico-vxlan.yaml
```

## Check 

```bash
kubectl get pods -n kube-system
```

results:

```text
NAME                                           READY   STATUS    RESTARTS   AGE
calico-kube-controllers-564985c589-k6skj       1/1     Running   0          9m33s
calico-node-5tcdb                              1/1     Running   0          9m33s
calico-node-cqw4t                              1/1     Running   0          9m33s
calico-node-j29bs                              1/1     Running   0          9m33s
coredns-7db6d8ff4d-6pt4j                       1/1     Running   0          18m
coredns-7db6d8ff4d-ss4td                       1/1     Running   0          84m
etcd-calico-control-plane                      1/1     Running   0          84m
kube-apiserver-calico-control-plane            1/1     Running   0          84m
kube-controller-manager-calico-control-plane   1/1     Running   0          84m
kube-proxy-nwnxf                               1/1     Running   0          84m
kube-proxy-wbvmc                               1/1     Running   0          84m
kube-proxy-xgj6k                               1/1     Running   0          84m
kube-scheduler-calico-control-plane            1/1     Running   0          84m
```

