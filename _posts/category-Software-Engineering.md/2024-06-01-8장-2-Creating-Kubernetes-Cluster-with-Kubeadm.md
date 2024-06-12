---
title: "Creating Kubernetes Cluster with Kubeadm"
excerpt: "Creating Kubernetes Cluster with Kubeadm"

wirter: Myeongwoo Yoon
categories:
  - Software Engineering
tags:
  - Kubernetes

toc: true
toc_sticky: true
use_math: true 

date: 2024-06-01
last_modified_at: 2024-06-02
---

Installing k8s with kubeadmin
======
&ensp;**kubeadmin**은 쿠버네티스 클러스터 생성을 위한 "빠른 경로"의 모범 사례로 kubeadm init 및 kubeadm join을 제공하도록 만들어진 도구이다. Kubeadm은 실행 가능한 최소 클러스터를 시작하고 실행하는 데 필요한 작업을 수행한다.

TCP ports in K8s cluster deployment
======
&ensp;네트워크 경계가 엄격한 환경에서 쿠버네티스를 실행할 때, 쿠버네티스 구성 요소에서 사용하는 포트와 프로토콜(TCP)을 알고 있는 것이 유용한다.
* Control Plane(Master Node)
  - 6443: Kubernetes API server, Used by All
  - 2379~2380: etcd server client API, Used by kube-apiserver, etcd
  - 10250: Kubelet API, Used by Self, Control plane
  - 10259: Kube-scheduler, Used by Self
  - 10257: Kube-controller-manager, Used by Self
* Worker Nodes
  - 10250: Kubelet API, Used by Self, Control plane
  - 30000~32767: NodePort Services, Used by All

실습
======
&ensp;Procedures to deploy K8s cluster on bare machines
* Add Kubernetes Repository
* Install kubelet, kubeadm, kubectl and docker
* Enable and start the kubelet and docker service
* Create Cluster with kubeadm
* Setup Pod network
* Join Worker Nodes
* Test the cluster by creating a test pod

&ensp;실습을 하기 위해 control plane 기계로는 AWS EC2 ubuntu 24.04 t2.medium, 즉 CPU core 2개 짜리를 사용하고, worker nodes는 AWS EC2 ubuntu 24.04 t2.micro를 사용한다. Kubeadm 도구를 사용하여 Cluster를 만드는 작업은 네가지로 나눌 수 있다.
* Kubeadm 도구를 설치하기 전에 Linux OS에 준비해야 하는 작업
* 원하는 container runtime을 선정하여 설치하는 작업
* Kubeadm 도구(kubeadm, kubectl, kubelet)를 설치하는 작업
  - 위 세 작업은 control plane과 worker 노드들 모두에게 해야하는 작업이다.
* 원하는 Pod network addon을 선정하여 설치하는 작업
  - control plane과 worker nodes 사이에 Pod 네트워크를 설치하여 통신할 수 있도록 kubernetes network addon을 설치해야하는데 container runtime과 마찬가지로 원하는 network addon을 선정하여 설치해야한다.
  - network addon은 control plane에만 설치해도 kubeadm init 명령으로 control plane을 설정하고 worker nodes에서 kubeadm join 명령을 수행하면 kubernetes cluster가 생성되면서 control plane이 worker nodes를 인식하여 Pod network 통신에 필요한 모든 것을 수행하므로 worker nodes에는 network addon을 설치할 필요는 없다.
* control plane으로 사용할 기계에서 sudo kubeadmin init 명령을 수행하고 worker 노드들에서 각각 sudo kubeadm join 명령을 실행하여 cluster를 생성한다.

&ensp;실습 전에 먼저 Inbound rules를 설정해준다.<br/>
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-1-Inbound-rules.png" width="600"></p>

&ensp;실습에서는 모든 포트를 다 열지만, 실제에서는 필요한 포트만 열어서 사용한다.

Linux OS에 준비해야 하는 작업
------
&ensp;대부분의 명령이 root 권한이 필요하므로 "sudo su" 명령을 실행하여 root로 작업을 수행하는 것이 편하기는 하지만 control plane에 프로그램들을 설치 후 kubeadm init 명령을 실행한 후 일반 user 권한으로 실행해야 하는 명령들이 있어 일반 user 권한으로 설치한다.<br/>
&ensp;작업을 하면서 control plane 기계인지 worker 기계인지 헷갈리지 않기 위해서 control plane과 worker nodes에 다음 명령를 수행한다.
* **sudo hostnamectl set-hostname control-plane-fe**
* **sudo hostnamectl set-hostname worker-1**, **sudo hostnamectl set-hostname worker-2**

<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-2.png" width="800"></p>

&ensp;이후 **exec bash**명령을 실행하여 reboot 없이 기계이름을 변경한다.<br/>
&ensp;container는 in-memory 에서 계속 유지되어야 하는 프로세스이므로 pod안 container들 실행을 감시하고 관리하는 kubelet container는 메모리 스왑이 벌어지면 동작하지 않고 에러 종료를 한다. 스왑을 disable시켜야 하므로 **sudo swapoff -a**명령을 실행하지만, 이번 실습에서는 "/etc/fstab" 파일에 **#/swap.img none swap ws 0 0**한 줄을 추가한다(\# 코멘트 처리는 boot 시간에 Linux 기계가 swap을 하지 못하도록 막는 라인). 이번 실습에서는 한번도 swap 해 본적도 없고 swap이 벌어질 일을 실행하지 않으므로 지장이 없지면 실제 production 환경에서는 반드시 해야하는 사항이다.
* **sudo vi /etc/fstab**
  - **#/swap.img none swap ws 0 0**
* **free -h**
  - swapoff가 되었는지 확인
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-3.png" width="800"></p>

&ensp;Kubernetes 관련 소프트웨어 모듈들이 자주 업그레이드 되며 관련 dependency 들도 update가 빈번하므로 다음 명령들이 구체적인 모듈 설치에 앞에 권장된다.
* **sudo apt-get update**
* **sudo apt-get upgrade -y**

&ensp;Pod가 Node에서 실행될 수 있도록 클러스터의 각 노드에 **컨테이너 런타임**을 설치해야 한다.
* **cat \<\<EOF \| sudo tee /etc/modules-load.d/k8s.conf**
  - cat \<\<EOF : EOF까지 입력되는 내용을 표준 입력으로 읽어들인다.
  - 표준 입력으로 받은 내용을 '/etc/modules-load.d/k8s.conf' 파일에 저정한다.
* **overlay**
* **br_netfilter**
* **EOF**

&ensp;위는 k8s.conf 파일에 loadable 커널 모듈 overlay와 br_netfilter를 등록하는 명령으로서, /etc/modules-load.d/k8s.conf 파일에 등록되어 있는 모듈들은 기계가 booting할 때 자동 적재한다.<br/>
&ensp;이후 등록한 overlay 모듈과 br_netfilter 모듈을 kernel에 적재하도록 명령한다.
* **sudo modprobe overlay**
  - modprobe 프로그램은 loadable 커널 모듈을 적재하거나 remove하는 명령
  - overlay 커널 모듈을 즉시 로드한다. 컨테이너 오버레이 파일 시스템에 사용된다.
* **sudo modprobe br_netfilter**
  - br_netfilter 커널 모듈을 즉시 로드한다. Linux 브리지 네트워크와 관련된 네트워크 필터링에 사용된다.

&ensp;iptable 파라미터를 설정한다. 이 설정들은 Linux 커널의 네트워크 설정을 조정하여 Kubernetes가 올바르게 작동할 수 있도록 한다.
* **cat \<\<EOF \| sudo tee /etc/sysctl.d/k8s.conf**
* **net.bridge.bridge-nf-call-iptables = 1**
  - 브릿지된 네트워크 트래픽이 iptables를 통해 필터링되도록 한다.
  - Kubernetes는 pod 네트워크 통신을 관리하기 위해 iptables를 사용하므로, 이 설정이 필요한다.
* **net.bridge.bridge-nf-call-ip6tables = 1**
  - IPv6 네트워크 트래픽이 ip6tables를 통해 필터링되도록 한다.
  - IPv4와 유사하게, Kubernetes는 IPv6 통신을 관리하기 위해 이 설정을 필요로 할 수 있다.
* **net.ipv4.ip_forward = 1**
  - IP 포워딩을 활성화해 Kubernets 클러스터 내의 네트워크 통신이 원할하게 이루어지게 한다.
  - IP forwarding: 패킷이 호스트를통해 다른 네트워크로 라우팅될 수 있도록 하는 기능
* **EOF**
* **sudo sysctl --system**
  - 이 명령은 기계를 reboot 하지 않고 커널에 새롭게 로딩된 드라이버, 파라미터를 반영하라는 것이다.
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-4.png" width="800"></p>

&ensp;Ubuntu 24.04는 ufw(Uncomplicated File Wall)이 default로 장착되어 있어 ufw를 disable하는 것이 필요하다.
* **sudo systemctl stop ufw**
* **sudo systemctl disable ufw**
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-5.png" width="800"></p>

Kubeadm도구(kubeadm, kubectl, kubelet)를 설치하는 작업
------
&ensp;**kubeadm**는 클러스터 구축을 위해 사용하는 도구, **kubelet**은 Pod의 시작, 관리를 위해 사용되는 컨테이너 프로세스, **kubectl**은 사용자의 Command line 도구로서 control plane의 API server와 통신하여 app을 deploy하거나 클러스터를 조사, 관리하는 명령을 수행할 수 있다. 자칫, control plane에 kubelet을 설치할 필요가 없을 것으로 착각할 수 있으나, control plane과 worker 노드 모두에 kubernetes architecture의 모든 구성 요소들은 container로 구현되어 있어 control plane도 kubelet가 설치되어야 한다.
* **sudo mkdir -p 755 /etc/apt/keyrings**
  - -p: 이미 존재하면 mkdir 하지 않고 없으면 directory를 만드는 옵션
  - 755: 소유자는 읽기, 쓰기, 실행 권한을, 그룹과 다른 사용자들은 읽기 및 실행 권한을 가진다.
* curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key \| sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
* echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' \| sudo tee /etc/apt/sources.list.d/kubernetes.list
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-6.png" width="800"></p>

* sudo apt-get update
* sudo apt-get install -y kubelet kubeadm kubectl
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-7.png" width="800"></p>

* sudo apt-mark hold kubelet kubeadm kubectl
* kubeadm version
* kubelet --version
* kubectl version
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-8.png" width="800"></p>

원하는 container runtime을 선정하여 설치하는 작업
------
* sudo apt-get install -y gnupg2 software-properties-common
* curl -fsSL https://download.docker.com/linux/ubuntu/gpg \| sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker-archive-keyring.gpg
* sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
* sudo apt-get update
* sudo apt-get install -y containerd.io
* sudo mkdir -p 755 /etc/containerd
* sudo containerd config default\|sudo tee /etc/containerd/config.toml
* sudo vi /etc/containerd/config.toml
  - SystemdCgroup = true 로 변경한다.
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-9.png" width="800"></p>

* sudo systemctl restart containerd
* sudo systemctl enable containerd
* systemctl status containerd
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-10.png" width="800"></p>

&ensp;위 과정까지 worker nodes에서 다시 실행한다.
* lsmod \| grep br_netfilter
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-11.png" width="800"></p>

sudo kubeadmin init, sudo kubeadm join
------
&ensp;다음 과정은 control plane에서 실행한다.
* sudo kubeadm init  --pod-network-cidr=192.168.0.0/16
* mkdir -p $HOME/.kube
* sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
* sudo chown $(id -u):$(id -g) $HOME/.kube/config
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-12.png" width="800"></p>

&ensp;이후 worker-2(순서를 잘못함...)에서 다음 명령을 실행한다.
* sudo kubeadm join 172.31.12.111:6443 --token o5e2cf.fi7q8sywmu596xdr \
        --discovery-token-ca-cert-hash sha256:6e8064b3035167fba989c239f7ee59c181a6d4d2a1f3f4533765e712a3aa0f73
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-13.png" width="800"></p>

&ensp;위 명령을 실행한 후, control plane에서 다음 명령을 수행하면 worker node가 join한 것을 확인할 수 있다.
* kubectl get nodes
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-14.png" width="800"></p>

* kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/tigera-operator.yaml
* curl https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/custom-resources.yaml -O
* kubectl create -f custom-resources.yaml
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-15.png" width="800"></p>

* kubectl get pods -n kube-system
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-16.png" width="800"></p>

* kubectl get nodes
  - NotReady였던 STATUS가 Ready로 바꿔었음을 알 수 있다.
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-17.png" width="800"></p>

&ensp;다시 worker-1에서 다음 명령을 실행한다.
* sudo kubeadm join 172.31.12.111:6443 --token o5e2cf.fi7q8sywmu596xdr \
        --discovery-token-ca-cert-hash sha256:6e8064b3035167fba989c239f7ee59c181a6d4d2a1f3f4533765e712a3aa0f73
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-18.png" width="800"></p>

&ensp;그리고 control plane에서 다음 명령을 실행하면 worker-1이 join되었음을 볼 수 있다.
* kubectl get nodes
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-19.png" width="800"></p>

* 시간이 흐른 후 다시 위 명령을 실행하면 worker-1의 STATUS가 Ready가 됨을 확인할 수 있고 **kubectl get pods -n kube-system**을 실행하면 worker-1의 NAME이 추가됨을 알 수 있다.
<p align="center"><img src="/assets/img/Software-Engineering/8장-2-Creating-Kubernetes-Cluster-with-Kubeadm/3-20.png" width="800"></p>

종료
------
&ensp;우선 control plane에서 worker nodes를 drain한다.
* kubectl drain worker-1 --delete-emptydir-data --force --ignore-daemonsets
* kubectl drain worker-2 --delete-emptydir-data --force --ignore-daemonsets

&ensp;worker nodes에서 다음 명령을 수행한다.
* sudo kubeadm reset
* sudo iptables -F && sudo iptables -t nat -F && sudo iptables -t mangle -F && sudo sudo iptables -X
* sudo apt-get install ipvsadm
* sudo ipvsadm -C

&ensp;다시 control plane에서 수행한다.
* kubectl delete node worker-1
* kubectl delete node worker-2
* sudo kubeadm reset
* sudo rm -rf /etc/cni/net.d
* sudo iptables -F && sudo iptables -t nat -F && sudo iptables -t mangle -F && sudo sudo iptables -X
* rm -f $HOME/.kube/config

&ensp;이제 control plane, worker nodes에서 **sudo reboot**를 수행한다.

Kubeadm
======
&ensp;Kubernetes에서 제공하는 기본적인 도구이며 kubernetes 클러스터를 구축하기 위한 다양한 기능을 제공한다.
* kubeadm init: control plane을 초기화하는 명령
* kubeadm join: worker 노드를 초기화하고 cluster에 가입시킴
* kubeadm token: control plane과 worker 노드들 사이의 인증
* kubeadm reset: kubeadm init과 kubeadm join을 reset