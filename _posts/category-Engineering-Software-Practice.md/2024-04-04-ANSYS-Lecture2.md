---
title: "ANSYS Lecture2"
excerpt: "Mesh Control, Symmetry Model, Load and Constraint in Cylindrical Coordinate"

wirter: Myeongwoo Yoon
categories:
  - Engineering Software Practice
tags:
  - ANSYS

use_math: true
toc: true
toc_sticky: true
 
date: 2024-04-04
last_modified_at: 2024-06-06
---

Mesh Control
======

Mesh Refinement
------

Practice
------
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-1-1.png"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-1-2.png"></p>

&ensp;먼저 직사각형 2개를 그린다음 Trim Away로 겹친 선을 지운다. 그리고 Filet Radius = 5mm, Extrude 30mm symmetrically(대칭적으로)로 모델링을 한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-1-3.png"></p>

&ensp;Model에 들어가 Mesh Tab에 들어가 Method를 클릭하고 3D모델을 Apply하고 Definition에서 Method를 Tetrahedrons를 선택하고 Element Size를 7mm로 Mesh를 생성한다. 그리고 Filet Radius를 한 곳에 3mm로 좀더 촘촘한 Mesh를 생성한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-1-4.png"></p>

&ensp;이제 Bottom face에 Fixed를 하고 윗 면에는 x방향으로 2000N의 힘을 주고 결과를보면 다음과 같다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-1-5.png"></p>

Symmetry Model
======
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-2-1.png"></p>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-2-2.png"></p>

&ensp;다음과 같이 모델링을 한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-2-3.png"></p>

&ensp;Cast iron으로 만들어졌기 때문에 Engineering Data에 들어가 Engineering Data Sources Tab에서 General Materials를 누르고 Gray Cast Iron을 Add한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-2-4.png"></p>

&ensp;이제 Model에 들어가 Material을 Gray Cast Iron으로 바꿔준다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-2-5.png"></p>

&ensp;Elements Type을 Tetrahedron, Element Size를 8mm로 Mesh를 생성한다.<br/>
&ensp;이제 y방향으로 -10000N으로 Bearing Force와 Bottom에 Fixed를 준다. 한 단면을 보고싶어서 **Section Plane**을 선택하여 단면을 잘라 볼 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-2-6.png"></p>

Symmetry Analysis
------

Make Symmetry modeling
------
&ensp;이제 모델링을 통해 다음과 같이 절반으로 자른다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-2-7.png"></p>

&ensp;위와 같은 Bearing load등을 주면 다음과 같이 출력된다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture2-2-8.png"></p>

Load and Constraint in Cylindrical Coordinate
======
