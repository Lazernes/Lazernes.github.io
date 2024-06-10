---
title: "ANSYS Lecture4"
excerpt: "Analysis with 2-D Elements, Analysis with 1-D Elements"

wirter: Myeongwoo Yoon
categories:
  - Engineering Software Practice
tags:
  - ANSYS

use_math: true
toc: true
toc_sticky: true
 
date: 2024-04-06
last_modified_at: 2024-06-10
---

Analysis with 2-D Elements
======

Analysis with 2-D Elements-1
------
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-1-1.png"></p>

&ensp;먼저 다음과 같이 모델링을 진행한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-1-2.png"></p>

&ensp;Tools의 Mid-Surface를 클릭해 양 면을 적용한 뒤 Generate를 하면 다음과 같이 가운데 면만 생성됨을 볼 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-1-3.png"></p>

&ensp;Mesh를 생성하고 Display 탭의 Thick Shells and Beams를 클릭하면 원래의 모델링 대로 볼 수 있다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-1-4.png"></p>

&ensp;한 면에 Fixed를 주고 다른 한쪽면에는 Distributed Mass를 주어야 하는데, 이는 Geometry를 우클릭한 후 Insert, Distributed Mass를 클릭하여 주고, Environment 탭에서 Inertial에서 Standard Earth Gravity를 적용한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-1-5.png"></p>

&ensp;결과는 다음과 같다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-1-6.png"></p>

Analysis with 2-D Elements-2
------
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-1-7.png"></p>

&ensp;다음과 같이 모델링을 한 후 Mid-Surface를 이용해 다음과 같이 만든다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-1-8.png"></p>

&ensp;Mesh를 생성하고 아랫 두 Edges를 Fixed한 다음 윗 면에 Force를 주면 다음과 같다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-1-9.png"></p>

Analysis with 1-D Elements
======
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-2-1.png"></p>

&ensp;Create탭의 Point를 눌러 (0, 0, 0)과 (0, 0, 800)에 두 Point를 생성한다. 그리고 Concept에서 Lines From Points를 선택하고 두 점을 클릭해 다음과 같이 Line을 생성한다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-2-2.png"></p>

&ensp;Concept의 Cross Section의 I Section을 클릭한 뒤 Dimensions를 입력해준다.<br/>
&ensp;Mesh에서 Line Body의 Cross Section을 l1으로 선택해주고 Cross Section과 Thick Shells and Beams를 클릭하면 다음과 같다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-2-3.png"></p>

&ensp;Geometry에서 우클릭을 해 Point Mass를 한쪽 끝에 준 뒤, 다른 한 쪽은 Fixed를 시키고 -y방향으로 중력을 주고 Mesh의 size를 50mm로 설쟁해 Solve를 하면 다음과 같다.<br/>
<p align="center"><img src="/assets/img/공학소프트웨어실습/Ansys/Lecture4-2-4.png"></p>