---
title: "20장 기하공차"
excerpt: "Geometric Dimensioning and Tolerancing"

wirter: Myeongwoo Yoon
categories:
  - Mechanical Elements Design
tags:
  - Mechanical Engineering

use_math: true
toc: true
toc_sticky: true
 
date: 2023-12-15
last_modified_at: 2023-12-15
---

Dimensioning and Tolerancing Systems
======
　보통 좌표공차 시스템(Coordinate Dimensioning System)으로 했었지만 이것으로 부족해서 기하공차(Geometric Dimensioning and Tolerancing(GD&T))가 들어오게 되었다.<br/>
　Dimensioning는 치수, Tolerancing는 공차를 의미한다. $10 \pm 0.5$에서 10은 치수를 의미하고, 0.5는 공차를 의미한다.<br/>
　죄표공차 시스템은 전통적인 방법이고, 제품양이 많지 않을 경우 사용된다. $\pm$ 공차가 주로 사용된다. 하지만 이 좌표 공차는 다음과 같이 형상관리가 잘 되지 않는다.<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/1-1.png" width="300"></p>

　이를 해결하기 위해 기하 공차가 사용된다.

Definition of Geometric Dimensioning and Tolerancing(GD&T)
======
　기하공차는 이론적으로 완벽한 기하학적인 조건들을 정의한다. 그 주변에서 사이즈, 위치, 방향, 형상에 대한 허용이 되는 편차(공차)를 정의한다. 다음은 기하공차의 한 예이다.<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/2-1.png" width="400"></p>

　기하공차는 symbols, rules and definitions의 시스템을 포함한다. 이를 이용해 부품의 설겨, 제작, 품질관리를 정확하게 표현할 수 있다. 특히 품질이 높은 부품을 원할경우와 대량 생산을 할때 사용된다(interchangeable assembly(무엇을 골라도 조립이 됨)).

GD&T Standards
------
　표준을 통해 관리를 하기 위해 사용된다. ASME Y 14.5-2009 기하 공차는 미국에서 보편적으로 사용되며, 설계의도를 잘 표현할 수 있다. 국제 표준으로 International Organization of Standards(ISO)이 있는데 유럽에서 사용되며 부품의 측정을 위해 사용된다. 두 표준을 매우 비슷하다.

Features of a Part
------
　Feature(형체)은 명확하게 정의할 수 있는 파트의 물리적인 위치이다. 예를들어, hole, counternbore, pin, slot, surface, cylinder가 있다.<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/2-2.png" width="200"></p>

Feature of Size
------
　Feature of Size(FOS)는 사이즈를 정의할 수 있는 Feature이다. **서로 마주보고 있는 점** 사이의 거리로서 크기를 정의한다. 예를들어, hole, cylinder(pin), slot, tab이 있다.<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/2-3.png" width="400"></p>

　FOS에는 다음과 같이 분류할 수 있다.
|FOS|External|Internal|
|---|---|---|
|Cylinderical|pin|hole|
|width-type|tab|slot|

　Rule of Thumb(실용적인 규칙)은 버니어 캘리퍼로 측정할 수 있는 것이다.

Four Geometric Attributes of Features
------
　기하학적인 특징은 크기(Size), 위치(location), 방향(orientation), 형상(form) 4가지가 있고, 각각은 기하학적인 형상을 정의하기 위해 고려된다.<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/2-4.png" width="600"></p>

　Size는 $\pm$공차를 통해 표시되며, location, orientation, form은 기하공차의 심볼을 이용해 표시된다.<br/>

**Size**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/2-5.png" width="400"></p>

　FOS의 크기를 나타내며, 반대의 점을 통해 측정된다. 유일하게 $\pm$를 이용해 표시할 수 있다. location과 구별을 해야한다.<br/>

**Location**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/2-6.png" width="200"></p>

　A dimension that locates a feature with respect to an origin of measurement.<br/>

**Orientation**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/2-7.png" width="200"></p>

　A dimension that locates the angle of a feature of centerline of feature with to an origin of measurement(면과면 사이의 방향을 구함), 평행도, 직각도, 각도을 관리한다.<br/>

**Form**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/2-8.png" width="200"></p>

　Feaure의 모양의 완벽함을 표현한다. 평면도, 진직도, 진원도, 원통도를 포함한다.

Symbolic Language
------
　International language of symbols로 주석을 줄일 수 있다. 심볼의 종류는 다음과 같다.
* Geometric Characteristic
  - 기하공차(size, location, orientation and form)를 나타냄
  <p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/2-9.png" width="500"></p>
* Modifying
  - 치수 수식자
  <p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/2-10.png" width="500"></p>
  - 공차 수식자
  <p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/2-11.png" width="500"></p>

Datums
======
　Datum으로부터의 거리, 뱡향등 처럼 기하학적인 특성이정의되고 측정된다.<br/>
　Datum은 어떤 파트의 위치와 방향의 측정의 기준이 되는 면이다. 참고로 Size and form control do not require an origin for measurement, so are not referenced to datum(점과점, 면과면의 기준이 있으므로 필요가 없음).<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-1.png" width="700"></p>

Datum Reference Frame(DRF, 데이텀 좌표계)
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-2.png" width="700"></p>

Use of Datums
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-3.png" width="700"></p>

Immobilization of the Part
------
Immobilization: 움직이지 않게 고정<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-4.png" width="700"></p>

Order of Datums
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-5.png" width="700"></p>

Non-planar Datum Features
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-6.png" width="700"></p>

Actual Mating Envelope(AME)
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-7.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-8.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-9.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-10.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-11.png" width="700"></p>

Datum Feature Symbol
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-12.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-13.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-14.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/3-15.png" width="700"></p>

Controlling Geometric Tolerances
======

Tolerance Zones(공차역)
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/4-1.png" width="700"></p>

Limiting Material Conditions
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/4-2.png" width="700"></p>

Envelope Principle
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/4-3.png" width="700"></p>

Basic Dimensions
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/4-4.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/4-5.png" width="700"></p>

Feature Control Frames
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/4-6.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/4-7.png" width="700"></p>


Geometric Characteristic Definitions
======

Geometric Controls
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-1.png" width="700"></p>

Form Controls
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-2.png" width="700"></p>

**Straightness**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-3.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-4.png" width="700"></p>

**Flatness**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-5.png" width="700"></p>

**Circularity**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-6.png" width="700"></p>

**Cylindricity**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-7.png" width="700"></p>

Orientation Controls
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-8.png" width="700"></p>

**Angularity**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-9.png" width="700"></p>

**Perpendicularity**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-10.png" width="700"></p>

Profile Controls
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-11.png" width="700"></p>

**Profile of a Surface**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-12.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-13.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-14.png" width="700"></p>

Location Controls
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-15.png" width="700"></p>

**Position**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-16.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-17.png" width="700"></p>

**Concentricity and Symmetry**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-18.png" width="700"></p>

**Runout Controls**<br/>
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-19.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/5-20.png" width="700"></p>

Material Condition Modifiers
======
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/6-1.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/6-2.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/6-3.png" width="700"></p>

<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/6-4.png" width="700"></p>

Suggested Implementation Framework
------
<p align="center"><img src="/assets/img/기계요소설계/20장 기하공차/6-5.png" width="700"></p>

Practical Implementation
======