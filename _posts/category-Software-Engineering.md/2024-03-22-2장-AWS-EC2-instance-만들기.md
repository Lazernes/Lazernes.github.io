---
title: "AWS EC2 instance 만들기"
excerpt: "AWS EC2 instance 만들어 SSH로 연결하여 사용하기"

wirter: Myeongwoo Yoon
categories:
  - Software Engineering
tags:
  - AWS

toc: true
toc_sticky: true
use_math: true 

date: 2024-03-22
last_modified_at: 2023-03-22
---

&ensp;https://aws.amazon.com/ko/에 접속하여 AWS 계정을 생성한다.

EC2 서비스 이용하기
======
&ensp;**인스턴스 시작**을 눌러 새로운 인스턴스를 만든다. Ubuntu 최신판 LTS 버전을 선택한다. 이때, 프리티어가 이용할 수 있는 운영체제를 선택한다. 인스턴스 유형도 기본으로 선택된 것으로 진행한다(Free Tier). 이후 키페이를 생성한다. 해당 키는 EC2 인스턴스에 접속하는데 사용되므로 다운로드 후 보관한다.<br/>

인스턴스 환경 준비 하기
======
&ensp;**sudo apt-get update** 명령어를 사용하여 우분투 업데이트를 먼저 진행하여 설치 오류를 방지할 수 있다.