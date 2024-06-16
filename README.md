# The the Hard Way 

## About Tutorial

본 튜토리얼은 `Calico the hard way` 와 유사한 방법을 사용하여 Calico를 설치하는 방법을 설명한다.
공식 문서에서 제공하는 방법은 실제 구성 시 잘 되지 않으며, typha 설치, bgp 등을 사용하기 때문에 본문에서는 CNI에 대해서 잘 이해하면서 실제 배포를 어떻게 하는지에
중점을 맞춘다.


## Cluster Details 

* Kubernetes as the database
* Calico CNI plugin
* Setup Calico with VXLAN overlay
* Calico IP address management (IPAM)

## Labs 

* [Lab 1](docs/1-Set-Up-Cluster-Without-CNI.md): Install Kubernetes
* [Lab 2](docs/2-The-Calico-Datastore.md): Setup the Calico Datastore - Kubernetes
* [Lab 3](docs/3-Configure-Calico-IP-POOLS.md): Setup Calico IP Pools
* [Lab 4](docs/4-Install-CNI-Plugin.md): Setup CNI Plugin
* [Lab 5](docs/5-Install-Calico-Node.md): Install Calico Node
* [Lab 6](docs/6-Test-Networking.md): Test Container Networking
* [Lab 7](docs/7-Cleanup.md): Delete the test cluster

