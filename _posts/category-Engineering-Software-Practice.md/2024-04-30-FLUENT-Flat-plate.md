---
title: "FLUENT Flat plate"
excerpt: "External flow: Flat plate"

wirter: Myeongwoo Yoon
categories:
  - Engineering Software Practice
tags:
  - FLUENT

use_math: true
toc: true
toc_sticky: true
 
date: 2024-04-30
last_modified_at: 2024-06-08
---

<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-0-1.png" width = "600"></p>

Laminar
======
&ensp;먼저 **Design Modeler**를 실행 시킨 후 XY Plane을 선택하여 1m X 1m Plate를 생성시킨다. 이후 **Concept** 툴에서 Surfaces From Sketches를 클릭한 뒤, Sketch1에 적용을 하면 다음과 같다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-1.png"></p>

&ensp;이제 **Mesh**를 실행 시키고 위, 아래, 왼쪽, 오른쪽 변을 각가 top, plate, inflow, outflow로 설정한다. 이는 향후 과정에 있어서 각 파트의 구분을 위해서 사용자 편의상 필요한 부분이다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-2.png"></p>

&ensp;Mesh를 우클릭을 한 뒤, Insert, Sizing을 선택하고 Details에서 Scope의 Geometry를 inflow로 설정하고 Definition의 Number of Divisions와 Advanced의 Bias Type, Bias Option, Bias Growth Rate를 다음과 같이 설정한다. outflow, top, plate도 각각 다음과 같이 설정한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-3.png"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-4.png"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-5.png"></p>

&ensp;Mesh Generate를 하면, 자동격자생성 기능을 이용했기 때문에 격자가 균일하지 않음을 확인할 수 있다. 그러므로 Mesh를 우클릭하고 Insert, Face Meshing을 하여 Apply를 한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-6.png"></p>

&ensp;**Setup**을 실행하는데, Options에서 Double Precision을 체크하고 실행한다. General에서 Mesh에서 Display를 눌러 Mesh형상을 불러 올 수 있다(시작하면 바로 보인다). Mesh에서 Check를 눌러 Mesh에 오류가 없는지 확인한다(메쉬 정보 확인 가능하다). Unit에서 원하는 단위를 설정할 수 있다. Scale에서 형상 및 격자생성과 별개로 크기 조절이 가능하다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-7.png"></p>

&ensp;**Models**에서 Viscous를 선택하고 Laminar로 설정한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-8.png"></p>

&ensp;**Materials**에서 Fluid, air를 클릭하고 Density와 Viscosity를 constant(비압축성)로 선택한 후 적용한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-9.png"></p>

&ensp;**Boundary Conditions**에서 inflow를 선택하고, Type을 velocity-inlet으로 변경하고, Velocity Magnitude를 0.5 m/s로 설정한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-10.png"></p>

&ensp;outflow는 Type을 pressure-outlet으로 변경하고 아무변경없이 OK를 클릭한다. plate와 top은 각각 Type을 wall과 symmetry로 변경한다.<br/>
&ensp;**Solution**의 **Methods**에서 Pressure-Velocity Coupling 항목은 기본값인 SIMPLE로 설정하고 **Solution Initialization**은 다음과 같이 설정 후 Initialize를 클릭한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-11.png"></p>

&ensp;**Residuals**에서는 continuity, x-velocity, y-velocity 모두 0.001로 설정한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-12.png" width = "600"></p>

&ensp;**Run Calculation**에서 Check Case를 클릭하고 Number of Iterations를 10000으로 설정 후 Calculate를 클릭한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-13.png"></p>

&ensp;계산이 완료되면 **Results**에서 XY Plot을 클릭하고 Solution SY Plot에서 다음과 같이 설정한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-14.png" width = "600"></p>

&ensp;Axes의 Solution XY Plot에서 Axes를 클릭하고 Axes 설정도 다음과 같이 설정한 후, Apply를 클릭 후, Save/Plot을 클릭한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-15.png" width = "600"></p>

&ensp;그러면 다음과 같이 그래프가 생성된다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-16.png"></p>

* 층류경계층 엄밀해인 Blasius Equation 경계층 두께 0.027과 비교적 정확히 일치힌다.

&ensp;이제 다시 XY Plot을 클릭하고 다음과 같이 Solution XY Plot과 Axes를 설정해준다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-17.png" width = "600"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-18.png" width = "600"></p>

&ensp;그러면 다음과 같이 그래프가 생성된다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-19.png"></p>

&ensp;이 결과들을 엑셀 Data로 받기위해 다음과 같이 Solution XY Plot을 설정해준다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-20.png"></p>

&ensp;이 Data를 엑셀에서 다음과 같이 Blasius Equation과 비교할 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-1-21.png" width = "600"></p>

Turbulent
======
&ensp;Laminar 경우와 그대로 Double Precision을 선택해 Start를 한다. Check를 눌러 Mesh에 오류가 없는지 확인한다.<br/>
&ensp;**Models**에서 Viscous를 선택해 다음과 같이 Model을 k-epsilon, k-epsilon Model을 Realizable, Near-Wall Treatment를 Enhanced Wall Treatment를 선택한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-2-1.png" width = "600"></p>

&ensp;또한 **Models**에서 Energy Equation 박스를 체크한다. **Materials**에서 Fluid, air를 더블클릭하고, Density 항복을 ideal-gas, 열전도율(Thermal Conductivity)를 $9.4505\times10^{-4}$, 점성계수(Viscosity)를 $6.667\times10^{-7}$로 설정한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-2-2.png" width = "600"></p>

&ensp;**Boundary Conditions**에서 inflow의 Type을 Velocity-inlet, Velocity Magnitude를 1m/s, Specification Method 항목에서 Intensity and Viscosity Ratio를 선택하고, Turbulent Intensity, Turbulent Viscosity Ratio를 각각 1로 입력하고, Thermal을 353K로 설정한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-2-3.png" width = "600"></p>

&ensp;outflow의 Type을 pressure-outlet으로 변경 후 아무 변경없이 OK를 클릭한다. plate의 Type을 wall으로 변경하고 Thermal의 Thermal Conditions를 Temperature을 선택하고 Temperature에 413을 입력한다. top의 type은 symmetry로 변경한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-2-4.png" width = "600"></p>

&ensp;**Solution**의 Methods, Pressure-Velocity Coupling의 Scheme을 SIMPLE로, Spatial Discretization의 Density, Momentum, Turbulent Kinetic Energy, Turbulent Dissipation Rate, Energy를 Second Order Upwind로 변경한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-2-5.png"></p>

&ensp;**Initialization**의 Initialization Methods를 Standard Initialization, inflow로 Residual은 다음과 같이 설정을 한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-2-6.png" width = "600"></p>

&ensp;**Run Calculation**에서 Check Case를 클릭하고, Number of Iterations 항목을 10000으로 설정 후 Calculate를 클릭한다. **Result**의 XY Plot을 클릭해 Solution XY Plot과 Axes를 다음과 같이 설정한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-2-7.png"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-2-8.png"></p>

&ensp;Plot 결과, 경계층 두께는 대략 2.00e-02로 이론값에 근사한다. 이후 다시 Solution XY Plot과 Axes를 다음과 같이 설정한 후 열전도율을 구한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-2-9.png"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-2-10.png"></p>

&ensp;Excel로 다음과 같이 FLUENT k-e Model, Seban-Doughty Experiment, Reynolds Correlation을 비교하면 다음과 같다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Fluent/Lecture2-2-11.png" width = "600"></p>