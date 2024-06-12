---
title: "K8s Orchestration & Networking"
excerpt: "K8s Orchestration & Networking"

wirter: Myeongwoo Yoon
categories:
  - Software Engineering
tags:
  - Kubernetes

toc: true
toc_sticky: true
use_math: true 

date: 2024-06-04
last_modified_at: 2024-06-11
---

Kubernetes Orchestration
======

K8s Network
------
<p align="center"><img src="/assets/img/Software-Engineering/8장-4-K8s-orchestraion&networking/1-1-K8s-Network.png" width="600"></p>

* K8s Cluster에 생성되는 모든 Pod들에 ephemeral "private" IP주소를 부여한다.
  - pod마다 pause continer에 eth0/IP 자동 생성
  - eth0: 운영 체제에서 사용되는 네트워크 인터페이스의 이름
* 각각의 pod는 안에 자신만의 virtual network를 갖게 되며 pause 컨테이너는 pod를 노드의 네트워크에 연결하는 gateway이다.
  - pause container는 pod 내 컨테이너에게 부모처럼 리눅스 namespace를 제공해준다.
  - 하나의 pod 속의 모든 컨테이너들은 pause 컨테이너의 eht0 부여된 IP 주소를 갖게 되어 localhost:컨테이너의 port 번호로 컨테이너 상호간 통신한다.
* K8s Cluster 속 전체 pod 사이의 통신 경로, 즉 pod 네트워크의 IP table은 클러스터를 구현할 때 제공한 **CNI addon(Calio)**이 생성하여 pod간 virtual 통신 경로가 구축된다.
* 노드에서 pod가 생성되면 pod 안의 네트워크 인터페이스 eth0와 노드가 통신하도록 노드는 veth0 인터페이스를 생성한다.
  - Veth(Virtual Ethernet Pair): 두 개의 가상 네트워크 인터페이스로 구성된 쌍이다. 이 인터페이스들은 마치 네트워크 케이블로 연결된 것처럼 동작한다.
* 노드들과 control plane으로 구성된 K8s 클러스터 속 전체 모든 Pod들은 하나의 subnet 속에서 생성된다.
  - pod는 마치 네트워크 상의 VM처럼 동작한다.
  - subnet: 하나의 네트워크가 분할되어 나눠진 작은 네트워크이다.
* 따라서 **각 pod는 다른 pod의 private IP주소로 패킷을 보낼 수 있다.**
* 하나의 노드 속의 pod들 사이의 통신은 그 어떤 하드웨어도 거치지 않고 소프트웨어 드라이버만 호출하여 통신하는 virtual 네트워크에서의 통신이다.
* 노드가 하드웨어이면 eth0는 하드웨어 이더넷 인터페이스, 노드가 VM이면 virtual 이더넷(소프트웨어 드라이버), 노드 간 network는 스위치 또는 케이블이다.

Why Pods in K8s?
------
* 인터넷 상에서 서비스를 제공하는 프로세스는 고유한 port번호가 부여되므로, 하나의 호스트에 동일한 서비스를 제공하는 컨테이너가 두 개 이상 공존할 수 없다.
  - 당연히 서로 다른 호스트에 같은 포트 번호를 갖는 컨테이너들은 전혀 문제가 되지 않는다.
* 그런데 K8s의 존재 이유 중 하나가 서비스 수요가 많아지면 같은 서비스를 제공하는 컨테이너를 scale up, 즉 더 생성한다는 것인데, 같은 컨테이너를 서로 다른 호스트에서 실행시키는 것은 문제가 없으나 호스트 안에 어떻게 동일한 컨테이너를 여럿(replicas)을 실행할 수 있을 까?
  - K8s는 이 문제를 Pod를 구현하여 해결하였다. pod는 같은 호스트에서 실행되든 다른 호스트에서 실행되든 고유한 IP 주소를 부여 받으므로 pod속 컨테이너 사이에서만 동일한 port 번호를 사용하지 않으면 포트 번호 충돌 문제가 없다. 이는 마치 pod는 서비스 제공 입장에서 virtual 호스트 역할을 수행하는 셈이다.
* Docker swarm 또는 전통적인 분산 환경에서의 동일한 종류의 서비스 제공은 서로 다른 호스트에 같은 서비스를 제공하여, 외부로부터의 서비스 요구를 여러 호스트에 나누어 할당하는 reverse proxy를 이용한다.

k8s workflow
------
&ensp;**k8s workflow**는 클러스터 내에서 애플리케이션을 배포, 관리 및 확장하는 데 필요한 단계를 설명한다.<br/>
<p align="center"><img src="/assets/img/Software-Engineering/8장-4-K8s-orchestraion&networking/1-2-K8s-workflow.png" width="600"></p>

* kubectl: 사용자가 Kubernetes API 서버와 상호 작용하기 위해 사용하는 커멘드 라인 도구이다. 명령을 통해 클러스터 내의 리소스를 생성, 수정, 삭제할 수 있다.
* Kubernetes Master
  - API Server: 클러스터의 중심이며, 모든 RESTful Kubernetes API 호출을 처리한다.
  - Cont-mangr(Controller Manager): 클러스터의 상태를 원하는 상태로 유지하지 위해 컨트롤러들을 실행한다.
  - Scheduler: 새로운 pod를 적절한 노드에 배치한다.
* Kubernetes Worker Nodes
  - 각 노드는 실제로 애플리케이션이 실행되는 곳이다.
  - kubelet: 각 노드에서 실행되며, pod 및 컨테이너의 상태를 관리하고 API 서버와 통신한다.
  - 위 그림에는 2개의 클러스터가 있고, 각 클러스터에는 여러 개의 워커 노드가 있다.
* Deployment
  - **deployment** 리소스는 애플리케이션의 원할한 배포와 업데이트를 담당한다.
  - **selector**: 특정 레이블을 가진 pod를 선택한다.
  - **template**: 배포할 pod의 템플릿을 정의한다. 여기서는 'app:fr' 레이블을 가진 pod 3개를 배포한다.
* pod
  - Kubernetes에서 배포 가능한 최소 단위로, 하나 이상의 컨테이너를 포함한다.
  - 위 그림에서는 'app:fr'과 'app:bk' 레이블을 가진 pod가 있다.
* Service
  - **service** 리소스는 pod 집합에 대한 네트워크 접근을 제공하며, 로드 밸런싱을 담당한다.
  - **selector**: 특정 레이블을 가진 pod를 선택한다. 여기서는 'app:fr' 레이블을 가진 pod를 선택한다.
  - **type: LoadBalancer**: 외부 네트워크에 접근할 수 있는 로드 밸런서를 생성한다.

&ensp;각 워커 노드는 private IP와 public IP를, 각 pod는 private IP를, 각 클러스터(서비스)는 private IP를 가진다. 외부에 노출된 서비스는 public IP가 필요하다.<br/>
&ensp;위 그림의 예를 보면 다음과 같다.
* 사용자가 kubectl을 통해 deployment와 service를 정의하고, Kubernetes API 서버에 요청을 보낸다.
* API 서버는 컨트롤러 매니저와 스케줄러를 통해 pod와 서비스를 생성한다.
* 스케줄러는 pod를 적절한 워커 노드에 배치한다.
* kubelet은 워커 노드에서 pod와 컨테이너를 실행하고, 상태를 모니터링한다.
* 서비스는 로드 밸런서를 통해 외부 트래픽을 적절한 pod로 라우팅한다.

Services & Ingress in K8s
------
* Service와 Ingress는 둘 다 외부에 일련의 pod들을 표출하는 데 사용된다.
  - Service: 여러 개의 pod에 접근할 수 있는 IP 하나를 제공한다(stable IP). pod가 클러스터 안 어디에 있든 고정 주소를 이용해 접근이 가능하다. 서비스에는 크게 4가지 종류가 있다.
    + Cluster IP: 기본 서비스 타입이며 클러스터 내부에서만 사용할 수 있다. 클러스터 내부 노드나 pod에서는 클러스터 IP를 이용해서 서비스에 연결된 pod에 접근하고 클러스터 외부에서는 이용이 불가능하다.
    + NodePort: 서비스 하나에 모든 노드의 지정된 포트를 할당한다. node1:8080, node2:8080처럼 노드에 상관없이 서비스에 지정된 포트 번호만 사용하여 pod에 접근할 수 있다. 노드의 포트를 사용하므로 클러스터 내부뿐만 아니라 외부에서도 접근할 수 있다. pod가 node1에서만 실행중이더라고 node2:8080으로 접근했을 때, node1에 실행된 파드로 연결한다. 클러스터 외부에서 크러스터 내부 파드로 접근할 때 사용할 수 있는 가장 간단한 방법이다.
    + LoadBalancer: 클라우드 서비스를 사용할 때 사용가능한 옵션이다. 클라우드에서 제공하는 로드밸런서와 파드를 연결한 후 해당 로드밸런서의 IP를 이용하여 클러스터 외부에서 pod에 접근할 수 있도록 한다.
    + ExternalName: 클러스터 내부에서 외부로 접근할 때 주로 이용한다.
  - Ingress: 클라우드 제공자가 제공하는 로드 밸런서를 사용하여 외부 트래픽을 자동으로 분산한다. 클러스터 외부에서 안에 있는 파드에 접근할 때 사용하는 방법이다.
* pod들은 ephemeral private IP 배정된다. 따라서 pod의 private IP를 사용해서는 외부에서 접근할 수 없는 것은 물론이고 pod네트워크 내부에서도 생성, 소멸하는 IP주소를 알 수 없으면 접근이 불가능하다.
* Service는 pod들에게 **stable IP**와 **DNS 이름**을 제공하여 해당 pod들을 접근할 수 있도록 하며 pod들 사이의 load balance와 service discovery 기능을 수행하여 다수 replica pod들에 외부 client가 접근할 수 있는 단일 endpoint를 제공한다.
* NodePort type의 service는 외부에 서비스를 제공하는 노드의 public IP를 공개하므로 개발자들이 편리하게 사용하는 용도 외에는 prodiction 서비스로는 제공할 수 없어서 Ingress가 제공된다.
* Ingress는 K8s service들을 K8s 클러스터 밖에서 접근할 수 있도록 하며 어떤 inbound traffic이 어떤 service에 접근할 수 있는지에 대한 일련의 rule을 생성하여 service에 대한 접근을 configure한다.
<p align="center"><img src="/assets/img/Software-Engineering/8장-4-K8s-orchestraion&networking/1-3-service&ingress.png" width="600"></p>

Cluster IP vs NodePort vs LoadBalancer vs Ingress
------
<p align="center"><img src="/assets/img/Software-Engineering/8장-4-K8s-orchestraion&networking/1-4-Service.png" width="600"></p>

* **service.yaml**
  - ClusterIP, NodePort, LoadBalancer의 세 가지 서비스 타입을 정의한다.
* **ingress.yaml**
  - Ingress 리소스를 정의하여 서비스 트래픽 라우팅 규칙을 설정한다.
* 외부 사용자는 퍼블릭 IP와 NodePort(36075)를 통해 노드로 트래픽을 보낸다. Ingress를 사용하여 여러 서비스에 대한 트래픽을 라우팅한다. LoadBalancer를 사용하여 클라우드 제공자의 NLB(Network Load Balancer)를 통해 트래픽을 분산시킨다.

3 Service Type Attributes
------
<p align="center"><img src="/assets/img/Software-Engineering/8장-4-K8s-orchestraion&networking/1-5-Service-Type.png" width="600"></p>

* Default type은 ClusterIP이다. ClusterIP service를 통해서는 cluster 내부에서만 pod 접근 가능하다(Internal Service).
* NodePort Service는 Static port 번호를 cluster에게 지정하여 외부에서 접근 하도록 한다. NodePort Service는 ClusterIP Service를 자동으로 생성하여 사용한다. 즉, ClusterIP Service를 외부 접근이 가능하도록 확장한 것이다.
* LoadBalancer Service는 nodePort Service를 확장한 것으로서, Cloud provicer가 제공하는 NLB를 사용한다. LoadBalancer Service가 자동으로 NodePort와 ClusterIP Service들을 생성한다. 즉, 내부 port번호를 제공하지 않도록 NodePort Service를 확장한 것이다.
* NodePort Service는 내부 test 용도로만 사용한다. Production에는 사용이 금지된다.

Ingress
------
&ensp;다음 그림은 Kubernetes 클러스터에서 Ingress를 통해 클라이언트 요청이 특정 서비스와 pod에 전달되는 과정을 설명한다.<br/>
<p align="center"><img src="/assets/img/Software-Engineering/8장-4-K8s-orchestraion&networking/1-6-Ingress.png" width="600"></p>

* Client -> Ingress
  - ingress.yaml
    + 클러스터 외부에서 들어오는 HTTP(S) 요청을 라우팅하는 규칙을 정의한다.
    + **host**: 특정 호스트네임에 대한 요청을 처리한다.
    + **paths**: 특정 경로('/')에 대한 요청을 처리하고, 해당 요청을 'microservice1-service'로 라우팅한다.
  - 클라이언트는 **microservice1.com** 호스트네임을 통해 요청을 보낸다.
  - Ingress는 이 요청을 받아, 정의된 규칙에 따라 적절한 서비스로 라우팅한다.
* Ingress -> Service
  - service-clusterIP.yaml
    + pod 그룹에 대한 네트워크 서비스를 정의하며, 로드 밸런싱을 통해 트래픽을 분산시킨다.
    + **selector**: **app:microservice1** 라벨을 가진 pod를 선택한다.
    + **ports**: servicePort 5000을 pod의 4000번 포트로 매핑한다.
  - Ingress는 **mircoservice1-service** 서비스로 요청을 전달한다.
  - 서비스는 로드 밸런싱을 통해 해당 요청을 적절한 pod로 분산시킨다.
  - Service는 selector가 match되는 모든 deploment(pod)를 접근할 수 있는 Endpoint로 등록한다.
* Service -> Pod
  - Deployment.yaml
    + pod의 배포와 관리를 정의한다.
    + **replicas**: 2개의 pod를 실행한다.
    + **template**: 배포할 pod의 탬플릿을 정의한다. **app: microservice1** 라벨을 가지고, 4000번 포트를 노출하는 컨테이너를 포함한다.
  - 서비스는 **app: microservice1** 라벨을 가진 pod를 선택하여 요청을 전달한다.
  - 각 pod는 지정된 포트(4000)에서 요청을 처리한다.

<p align="center"><img src="/assets/img/Software-Engineering/8장-4-K8s-orchestraion&networking/1-7-Ingress&NodePort$LoadBalancer.png" width="600"></p>

Kubernetes Networking
======

K8s pod network
------
<p align="center"><img src="/assets/img/Software-Engineering/8장-4-K8s-orchestraion&networking/1-8-pod.png" width="600"></p>

* Node A
  - Pod A
    + Pause 컨테이너가 eth0(10.1.1.1)을 가지고 있다.
    + Container1 및 Container2가 Pause 컨테이너를 통해 네트워크에 연결되었다.
  - Pod B
    + eth0(10.1.1.2)를 가지고 있다.
* Node B
  - Pod C
    + eth0(10.1.2.1)를 가지고 있다.
  - Pod D
    + eth0(10.1.2.2)를 가지고 있다.
* 네트워크 연결
  - 각 Pod는 veth페어(veth0, veth1)를 통해 Brige(L2 switch)와 연결되었다.
  - 노드 A의 Bridge는 10.1.1.0/24 서브넷에 속한다.
  - 노드 B의 Bridge는 10.1.2.0/24 서브넷에 속한다.
  - 두 노드 간의 통신은 VM 네트워크(10.100.0.0/24)를 통해 이루어진다.
* Router/Gateway
  - 목적지 서브넷(10.1.1.0/24 또는 10.1.2.0/24)에 대한 다음홉을 정의한다.
  - 10.1.1.0/24의 경우 10.100.0.2가 다음 홉이다.
  - 10.1.2.0/24의 경우 10.100.0.3이 다음 홉이다.

K8s service network via kube-proxy
------
&ensp;위 그림에 kube-proxy와 netfilter가 추가되었다. **kube-proxy**는 각 노드에서 네트워크 트래픽을 관리한다. **netfilter**는 패킷 필터링을 수행하여 네트워크 트래픽을 제어한다.<br/>
<p align="center"><img src="/assets/img/Software-Engineering/8장-4-K8s-orchestraion&networking/1-9-service.png" width="600"></p>

* Service
  - Pod replicas에 stable IP address와 DNS name을 제공한다.
  - Load Balancing
  - 서비스를 실제로 제공하는 pod replicas 앞 단에 reverse-proxy(혹은 load balaber)를 놓는 것이다.
  - Service 네트워크는 실제 어느 네트워크 device에 연결된 physical 또는 virtual 네트워크가 아니라 네트워크 전체의 IP table에 routing rule을 반영하여 실질적으로 네트워킹 기능을 하는 네트워크로 불릴 만한 것이다.
* Service IP(Cluster IP) - to - Pod IP mapping을 위해 control plane의 API server는 server, pod가 새로이 생기거나 제거되면 모든 노드의 kube-proxy에게 해당 Service IP(Cluster IP) - to - Pod IP mapping을 반영하도록 명령한다.
* Kube-proxy는 kernel 모듈인 iptables를 사용하여 해당 IP address mapping을 routing table에 반영하여 netfilter가 ip routing table을 참조하여 목적지 ip 주소를 translate 하여, 즉 ip routing에 있는 대로 목적지 서비스의 주소를 해당 pod의 pod ip주소로 변경하여 routing이 되도록 한다.
* Kube-Proxy는 K8s의 모든 worker 노드에 하나씩 장착된다.