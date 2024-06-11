---
title: "FLUENT Pipe 3D"
excerpt: "Internal flow 3D pipe flow"

wirter: Myeongwoo Yoon
categories:
  - Engineering Software Practice
tags:
  - FLUENT

use_math: true
toc: true
toc_sticky: true
 
date: 2024-05-01
last_modified_at: 2024-06-10
---

<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-0-1.png" width = "600"></p>

Modeling & Meshing
======
&ensp;다음과 같이 끝나는 점이 원점에 오도록 파이프의 중심이 되는 선을 그린다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-1.png" width = "600"></p>

&ensp;YZ평면에서 Diameter 0.5m원을 그린다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-2.png" width = "600"></p>

&ensp;Sweep을 클릭한 후, Profile에 Sketch2, Path에 Sketch1을 각각 선택하고 Apply를하고 Generate한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-3.png" width = "600"></p>

&ensp;inlet, outlet, body를 Create Named Selection을 이용해 입력한다. 이제 Mesh를 생성하고 Quality의 Smoothing을 High로 변경한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-4.png" width = "600"></p>

&ensp;Mesh를 우클릭을 한 후 Insert-Inflation을 클릭하고 Geometry는 다음과 같이 2개의 Face들을 설정한다. Boundary는 벽면 1Edge를 설정한다. Inflation Option에서 Smooth Transition을 설정하고 Maximum Layer 6으로 설정한다. Growth Rate는 1.4로 설정한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-5.png" width = "600"></p>

&ensp;Mesh의 Sizing에서 전체 Body를 선택하고 Type은 Elemetn Size를 선택한다. Element Size는 0.05m로 설정하고 Behavior는 Soft로 Generate를 한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-6.png" width = "600"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-7.png" width = "600"></p>

FLuent
======
&ensp;Mesh에서 Check를 클륵하면 하단에 Checking mesh라고 나온다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-1.png" width = "600"></p>

&ensp;난류 유동으로 해석하기 위해 K-epsilon 모델로 변경한다. Material에서 Water-liquid를 선택해 만든다. **Cell Zone Conditions**에서 Type이 fluid인지 확인하고 Edit을 눌러 water-liquid로 변경한다. **Boundary Conditions**에서 inlet을 선택 후 Velocity-inlet에서 Velocity Magnitude 0.65, Intensity and Hydraulic Diameter에서 수력 직경을 0.5로 설정한다. outlet은 Pressure-outlet으로, 수력직경을 0.5로 설정한다.<br/>
&ensp;**Initialization**에서 Standard Initialization, inlet으로 초기화를 하고, **Run Calculation**에서 Number of Iteration을 1000으로하고 Calculation을 한다.<br/>
&ensp;**Contours**를 선택하고 Pressure-Total Pressure를 선택하고 Display하고 싶은 Surface를 선택한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-2.png" width = "600"></p>

&ensp;**Vectors**에서 Velocity-Velocity Magnitude를 선택하고 Display하고 싶은 Surface를 선택한다. Vector를 좀더 잘 보기위해 Scale과 Skip을 설정한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-3.png" width = "600"></p>

&ensp;**Vectors**에서 Draw Mesh를 선택하고 Options에서 Edges-Outline을 Surface에서 boundary, inlet, outlet을 클릭하여 Display한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-4.png" width = "600"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-5.png" width = "600"></p>

&ensp;**Surface**의 Plane에서 다음과 같이 ZX Plane에서 Y=2를 입력 후 평면을 하나 만든다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-6.png" width = "600"></p>

&ensp;또한, Method를 Point and Normal로 하고 Select with Mouse를 이용해 다음과 같이 평면의 위치를 설정하고, Normal에서 IX, IY, IZ를 1, 0, 0으로 설정한다. 그리고 추가로 XY Plane, Z=0으로 하나 더 만든다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-7.png" width = "600"></p>

&ensp;이제 **Vectors**에서 생성한 Plane을 선택하면 다음과 같이 볼 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-8.png" width = "600"></p>

&ensp;또한 다음과 같이 inlet과 outlet에서 vectors를 볼 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-9.png" width = "600"></p>

&ensp;다음과 같이 질량유량을 확인할 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-10.png" width = "600"></p>

&ensp;다음과 같이 압력을 확인할 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-11.png" width = "600"></p>

&ensp;다음과 같이 손실 계수를 구할 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-11.png" width = "600"></p>