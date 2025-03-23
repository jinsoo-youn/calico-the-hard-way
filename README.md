# Calico the Hard Way

## 튜토리얼 소개

이 문서는 `Calico the hard way` 방식과 유사하게, Calico를 수동으로 설치하고 구성하는 과정을 다룹니다.  
공식 문서에서 제공하는 설치 방법은 실제 환경에서 제대로 동작하지 않거나, `typha`, `BGP`와 같은 고급 기능들을 포함하고 있어 학습에 방해가 될 수 있습니다.  
따라서 이 문서에서는 Calico의 CNI 구성 방식에 대한 이해를 바탕으로, **핵심적인 구성 요소만 사용하여 실제 배포 과정을 단계별로 설명**하는 데 중점을 둡니다.

이 튜토리얼은 Kubernetes 클러스터에 Calico를 수동으로 설치하고 구성하고자 하는 사람들을 위한 실습 중심의 가이드입니다.

## 클러스터 구성 정보

- 데이터 저장소로 Kubernetes 사용 (Kubernetes as the Calico datastore)
- CNI 플러그인으로 Calico 사용
- VXLAN 오버레이 네트워크 구성
- Calico의 IP 주소 관리(IPAM) 사용

## 실습 목록

1. [Lab 1](docs/1-Set-Up-Cluster-Without-CNI.md): CNI 없이 Kubernetes 클러스터 설치  
2. [Lab 2](docs/2-The-Calico-Datastore.md): Calico 데이터 저장소 구성 (Kubernetes 연동)  
3. [Lab 3](docs/3-Configure-Calico-IP-POOLS.md): Calico IP 풀 설정  
4. [Lab 4](docs/4-Install-CNI-Plugin.md): CNI 플러그인 설치  
5. [Lab 5](docs/5-Install-Calico-Node.md): Calico Node 구성  
6. [Lab 6](docs/6-Test-Networking.md): 컨테이너 네트워크 연결 테스트  
7. [Lab 7](docs/7-Cleanup.md): 테스트 클러스터 정리  
