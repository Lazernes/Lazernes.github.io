---
title: "FLUENT Pipe 2D"
excerpt: "Internal flow 2D pipe flow"

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
&ensp;먼저 다음과 같이 모델링을 진행한다. XY평면에 Sketching을 진행하고 Concept의 Surfaces From Sketches를 선택한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-1.png" width = "600"></p>

&ensp;이제 Mesh를 진행한다. 다음과 같이 4개의 Edges를 Number of Divisions 40, Bias Type을 선택 후 Bias Option을 smooth transition, Bias Growth Rate에 1.06을 입력한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-2.png" width = "600"></p>

&ensp;입출구를 선택한 후 Number of Divisions 40, Bias Type을 다음과 같이 선택하고 Bias Option을 Smooth Transition, Growth Rate 1.06을 입력한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-3.png" width = "600"></p>

&ensp;곡관 2부분은 Number of Divisions 120, Bias Type을 다음과 깉이 선택하고 Bias Option을 Smooth Transision, Growth Rate 1.0을 입력한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-4.png" width = "600"></p>

&ensp;이제 Generate Mesh를 하고 Face Meshing을 선택한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-1-5.png" width = "600"></p>

&ensp;입출구는 inlet과 outlet으로 이름을 변경해준다.

FLuent
======
&ensp;General 에서 Check를 눌러 Display 형상을 불러 올 수 있고, Check를 눌러 mesh에 오류가 없는지 확인한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-1.png" width = "600"></p>

&ensp;**Models**에서 Viscous를 선택하고 k-epsilon을 선택한다(대부분의 케이스에 적용되는 옵션). **Materials**에서 Fluid를 우클릭하고 New를 눌러 Create/Edit Materials 창을 연다. Fluent Database를 누른 뒤 water-liquid를 선택 후 Copy를 하고 Change/Create를 누르고 Close를 선택한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-2.png" width = "600"></p>

&ensp;**Cell Zone Conditions**의 Fluid, surface_body를 클릭하고 Material Name을 이전에 만든 water-liquid를 선택후 Apply한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-3.png" width = "600"></p>

&ensp;**Boundary Conditions**를 클릭하고 inlet의 Type을 velocity-inlet으로 설정하고 Velocity Magnitude에 0.65를 입력하고 Turbulence의 Specification Method에서 Intensity and Hydraulic Diameter을 선택하고 Hydraulic Diameter에 수력 직경 값 2를 입력해준다. outlet의 Type은 pressure-outlet으로 변경하고 inlet과 마찬가지로 Hydraulic Diameter를 입력해준다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-4.png" width = "600"></p>

&ensp;**Monitiors**에서 Residuals를 선택하고 Options에서 Print to Console과 Plot이 선택되어 있는지 확인하고 Equaions의 continuity에서 Absolute Criteria를 1e-05로 변경한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-5.png" width = "600"></p>

&ensp;**Initialization**에서 Initializarion Methods를 Standard Initialization으로 선택하고 Compute from에서 inlet을 설정하고 Initialize를 실행한다. **Run Calculation**에서 Check Case를 눌러 이상 유무를 확이하고 Number of Iterations에 1000을 입력한 뒤 Calculate를 눌러 실행한다.<br/>
&ensp;**Graphics**에서 Contours를 선택하고 Contours of에서 Pressure-Total Pressure 또는 Velocity-Velocity Magnitude를 선택한다. Surfaces에서 boundary, inlet, interior-surface_body, outlet을 선택한 뒤 Display를 선택한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-6.png" width = "600"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-7.png" width = "600"></p>

&ensp;**Graphics**에서 Vectors를 선택한다. Vectors of에서 Velocity-Velocity Magnitude를 선택한다. Surfaces에서 boundary, inlet, interior-surface_body, outlet을 선택한 뒤 Display를 선택한다. 유동흐름을 원할히 보기 위해 Scale과 Skip으로 조정한 뒤 Display를 선택한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-8.png" width = "600"></p>

&ensp;**Graphics**에서 Pathlines를 선택한다. Vectors of에서 Velocity-Velocity Magnitude를 선택하고 Surfaces에서 boundary, inlet, interior-surfece_body, outlet을 선택한 뒤 Display를 선택한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-9.png" width = "600"></p>

&ensp;**Plots**에서 XY Plot을 선택하고 Position on Y Axis를 선택 후 Plot direction을 X 0, Y 1로 변경한다. Velocity-Velocity Magnitude로 선택하고 Outlet 선택 후 Plot을 하면 출구 근처 속도를 확인할 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-10.png" width = "600"></p>

&ensp;**General**에서 Mesh의 Display를 눌러 mesh 형상을 다시 불러오고 Results의 Surface를 우클릭 수 New에서 Line Rake 선택 후 속도를 plot을 하기 위한 Line을 생성한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-11.png" width = "600"></p>

&ensp;**Vectors**를 더블클릭하고 Options에 Faces를 체크하고 Surfaces에서 boundary, inlet, outlet을 선택한후 Display를 하고 Vectors에서 생성한 line들 및 inlet, outlet들을 선택한 후 Display한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-12.png" width = "600"></p>

&ensp;**Reports**에서 Fluxes를 선택한 후 Option-Mass Flow Rate를 선택한다. Boundaries에서 inlet outlet을 선택하고 Compute를 한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-13.png" width = "600"></p>

* 이론적으로 직업 구한 유량 $\dot{m} = \rho V A = 998 * 0.65 * 2 = 1297.4$와 비슷함을 알 수 있다. 2D인 경우 단위 길이를 1m로 하므로 A = 2이다.

&ensp;**Reports**에서 Surface Integrals를 선택한다. Report Type에서 Mass-Weighted Average를 선택하고 Field Variable에서 Pressure-Total Pressure를 선택한다. Surfaces에서 inlet과 outlet을 선택한 후 Compute를 눌러 계산한다. 또한 곡선의 양 끝의 Line에서도 적용을 하면 다음과 같다. 다음 값들은 각 Line에서 베르누이 방정식의 계산 결과와 같다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-14.png" width = "600"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-15.png" width = "600"></p>

&ensp;다음과 같이 수두손실과 손실계수를 구할 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture3-2-16.png" width = "600"></p>