---
title: "ANSYS Lecture3"
excerpt: "Contact Analysis, Bearing Housing Analysis"

wirter: Myeongwoo Yoon
categories:
  - Engineering Software Practice
tags:
  - ANSYS

use_math: true
toc: true
toc_sticky: true
 
date: 2024-04-05
last_modified_at: 2024-06-07
---

Contact Analysis
======
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture3-1-1.png"></p>

&ensp;먼저 아래판을 먼저 모델링을 하고, 다음과 같이 ZY평면에서 Pull 옵션을 No merge를 선택하고 모델링을 한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture3-1-2.png"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture3-1-3.png"></p>

&ensp;Mesh를 짜기 전에, **Connections**, **Insert**, **Manual Contact Region**을 선택한다.
* Contact Bodies를 위 beam의 아랫면을 선택한다.
* Target Bodies를 아래 beam의 윗면을 선택한다.
* Type을 Bounded로 설정한다.
* Behavior를 Auto Asymmetric을 선택한다.

&ensp;이제 Tetrahedrons로 0.01m로 Mesh를 짜고, 양 면에 Fix를 주고, 윗 면에 10000N을 주고 결과를 보면 다음과 같다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture3-1-4.png"></p>

Bearing Housing Analysis
======
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture3-2-1.png"></p>

&ensp;다음과같이 모델링을 한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture3-2-2.png"></p>

&ensp;Housing의 Material을 Cast Iron으로, Bearing의 Material을 Copper로 설정하고 다음과 같이 Contacts를 설정한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture3-2-3.png"></p>

&ensp;Mesh를 짜기위해 다음과 같이 8개의 Edges를 선택하고 Element Size를 10mm로 설정하고 Mesh를 생성하면 방금 선택한 Edges가 더 촘촘히 짜졌음을 알 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture3-2-4.png"></p>

&ensp;이제 Bearing Load와 constraints를 주고 결과를 보면 다음과 같다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture3-2-5.png"></p>