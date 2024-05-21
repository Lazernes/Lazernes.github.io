---
title: "Kubernetes"
excerpt: "Kubernetes"

wirter: Myeongwoo Yoon
categories:
  - Software Engineering
tags:
  - Kubernetes

toc: true
toc_sticky: true
use_math: true 

date: 2024-05-16
last_modified_at: 2024-05-16
---

Kubernetes intro
======

Official Definition of Kubernetes
------
&ensp;**k8s** 또는 **kube**라고도 하는 **Kubernetes**는 컨테이너화된 애플리케이션의 배포, 관리 및 확장을 예약하고 자동화하기 위한 컨테이너 오케스트레이션(Container Orchestraion) 플랫폼이다. 수많은 container들이 이질적인 deployment 환경에서 실행할 수 있도록 돕는 도구로, 수요에 따라 동적으로 container의 수를 증감하고 실직적으로는 Cloud Service Provider의 CDN에서 실행한다.<br/>
&ensp;**Container orchestration**은 모든 앱은 microservice들을 Container로 만들어서 배포한다. 매우 많은 수의 복제된 container들이 배포되는데 수백, 수천 개의 container들을 동적으로 관리하는 것이 여렵기 때문에 Orchestraion 도구가 필수적이다.<br/>
&ensp;즉, 컨테이너 오케스트레이션은 대규모 애플리케이션을 배포할 수 있도록 컨테이너의 네티워킹 및 관리를 자동화하는 프로세스이다. 애플리케이션이 성장하고 복잡해짐에 따라 마이크로서비스 아키텍처에는 수백 또는 수천 개의 컨테이너가 있을 수 있다. 컨테이너 오케스트레이션 도구는 프로비저닝과 스케줄링에서 배포 및 삭제에 이르는 전체 수명 주기를 자동화하여 컨테이너 인프라 관리를 간소화하는 것을 목표로 한다.
* High availability(No downtime in service)
* Scalability(high service throughput against high demands)
* Disaster recovery(backup & restore)
