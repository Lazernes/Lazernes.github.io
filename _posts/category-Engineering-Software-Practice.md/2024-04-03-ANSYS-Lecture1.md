---
title: "ANSYS Lecture1"
excerpt: "Getting Start with Cantilever Beam"

wirter: Myeongwoo Yoon
categories:
  - Engineering Software Practice
tags:
  - ANSYS

use_math: true
toc: true
toc_sticky: true
 
date: 2024-04-03
last_modified_at: 2024-04-03
---

<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-0-1.png"></p>

Create New Geometry
======
&ensp;Workbench의 Toolbox에서 **Static Structural**를 Project Schematic에 드래그한다. 이후, Geometry에서 SpaceClaim을 들어가 모델링을 한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-1-1.png"></p>

&ensp;위 옵션들 중에서 **Detail**에 들어가 **Dimension**을 이용하면 치수를 입력할 수 있다. 한 평면에 치수를 입력하고 새로운 평면에 치수를 입력하기 위해 **Select**를 사용해 원하는 평면을 선택하고 Dimension을 클릭한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-1-2.png"></p>

Model
======
&ensp;Model을 클릭해서 들어간다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-1.png"></p>

&ensp;Mesh Tab에 들어가서 Generate를 클릭해 자동으로 Mesh를 생성한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-2.png"></p>

&ensp;한 면에 Fixed 조건을 주고 다른 한 면에는 -y방향으로 1000N의 Force를 준다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-3.png"></p>

<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-4.png"></p>

&ensp;Solve를 클릭하고 Solution Information에 들어가면 해석이 완료된 정보를 확인할 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-5.png"></p>

&ensp;이후 Deformation Total, Strain von-Mises, Stress von-Mises를 클릭하고 다시 Solve를 하면 다음과 같이 결과를 볼 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-6.png"></p>

<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-7.png"></p>

<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-8.png"></p>

Changing Element Type
------
&ensp;Static Structural를 새로 만들고 다음과 같이 Geometry를 공유한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-9.png"></p>

&ensp;Mesh Tab에 들어가 Method를 클릭하고 3D모델을 클릭후 Apply를 하고 Definition에서 Method를 Tetrahedrons를 선택하고, Generate를 한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-10.png"></p>

<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-11.png"></p>

&ensp;Sizing을 클릭해 3D모델을 클릭후 Apply를 하고 Definition에서 Element Size를 0.01m로 입력해 수정할 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-12.png"></p>

<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-13.png"></p>

Local Pressure Load
------
&ensp;위 모델을 다음과 같이 수정해본다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-14.png"></p>

&ensp;새롭게 Static Structural을 생성한다. 이후 다음과 같이 Design Tab에서 Split를 이용해 다음과 같이 모델링을 한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-15.png"></p>

&ensp;이후 Mesh를 Generate하고 한 면을 Fixed하고 다음과 같이 Pressure를 100MPa를 준다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-16.png"></p>

&ensp;이후 Total Deformation, Equivalent Elastic Strain, Equivalent Stress를 구하면 다음과 같다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-17.png"></p>

<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-18.png"></p>

<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture1-2-19.png"></p>