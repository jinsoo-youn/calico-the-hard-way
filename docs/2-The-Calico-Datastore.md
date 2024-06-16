# The Calico Datastore

Calico 에서는 두가지 데이터 저장소를 지원한다. 
* Kubernetes - Kubernetes API 를 통해서 저장 (default)
* etcd - etcd 에 직접 접근해서 저장

이번 랩에서는 Kubernetes를 기본 저장소로 설정하는 방법을 살펴본다.

## 사전 작업 -필요한 바이너리 다운로드 

```bash
wget -O ./calico-v3.28.0.tgz https://github.com/projectcalico/cni-plugin/releases/download/v3.28.0/calico-amd64
tar -xvf calico-v3.28.0.tgz
```


## Using Kubernetes as the datastore 

Calico 에서 Kubernetes API를 기본 저장소로 사용하기 위해선 별도의 Custom Resources Definition (CRD) 를 설치해야 한다.

설치 Calico 버전 
v3.28.x (latest)
링크의 manifest 파일을 이용해서 설치한다.
https://github.com/projectcalico/calico/tree/release-v3.28/manifests

```text
# This manifest includes the following component versions:
#   calico/ctl:v3.28.0
```

### ssh to the control-plane
```bash
docker exec -it calico-control-plane bash
```

```bash

kubectl apply -f ./config/crds.yaml
```


## Install Calicoctl on the control-plane 


```bash

chmod +x calicoctl
sudo mv calicoctl /usr/local/bin/

export DATASTORE_TYPE=kubernetes
```

## Test 

```bash 
calicoctl get nodes 
```

results:
```text
NAME
calico-control-plane
calico-worker
calico-worker2
```

```
calicoctl get ippools
```

results:
```text
NAME   CIDR   SELECTOR
```