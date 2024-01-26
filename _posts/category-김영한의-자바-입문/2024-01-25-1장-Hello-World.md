---
title: "1장 Hello World"
excerpt: "Hello World"

wirter: Myeongwoo Yoon
categories:
  - 김영한의 자바 입문
tags:
  - Java

toc: true
toc_sticky: true
 
date: 2024-01-25
last_modified_at: 2024-01-25
---

개발 환경 설정
======

인텔리제이 실행하기
------
　다음과 같이 Intellij에서 New Project를 만든다.<br/>
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/1-1.png" width="500"></p>

　왼쪽에 삼각형 버튼을 누르면 실행이 된다.
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/1-2.png"></p>

<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/1-3.png" width="300"></p>

다운로드 소스 코드 실행 방법
======
　**File**에서 **New**, **Project from Existing Sources...**을 클릭하고, 원하는 폴더를 선택하면 다음과 같은 화면이 나온다.<br/>
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/2-1.png" width="500"></p>

　이후 계속 **Next**를 선텍하면, 실행된다.

자바 프로그램 실행
======
　**src**에서 **New**, **Java Class**를 하면 다음과 같은 화면이 나온다.<br/>
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/3-1.png" width="300"></p>

　HelloJava, **Java**로 새로운 파일을 생성하면 다음과 같이 생성된다.<br/>
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/3-2.png" width="400"></p>

```java
public class HelloJava {  // HelloJava 클래스의 범위 시작

    public static void main(String[] args) {  // main() 메서드의 범위 시작
        System.out.println("hello java");
    } // main() 매서드의 범위 끝
} // HelloJava 클래스의 범위 끝
```

　위 코드를 생행하면 콘솔창에 다음과 같이 생행된다.<br/>
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/3-3.png" width="400"></p>

　위 코드를 살펴보자.
* HelloJava, String처럼 Java는 대소문자를 구분한다. 파일의 첫 문자는 대문자를 사용한다.
* **public class HelloJava**
  - HelloJava를 클래스라 한다.
  - 파일명과 클래스 이름이 같아야 한다.
  - {} 블록을 사용해서 클래스의 시작과 끝을 나타낸다.
* **public static void main(String[] args)**
  - main을 메서드라 하고, 자바는 main(String[] args) 메서드를 찾아서 프로그램을 시작한다.
  - {} 블록을 사용해서 클래스의 시작과 끝을 나타낸다.
* **System.out.println("hello java");**
  - System.out.println()은 값을 콘솔에 출력하는 기능이다.
  - 자바는 문자열을 사용할 때 "을 사용한다.
  - 자바는 세미클론으로 문장을 구분한다.

주석
======
　주석의 종류는 **//**와, **/*...*/**가 있다.<br/>
　psvm을 입력하고 엔터를 누르면, **public static void main(String[] args)**를 자동으로 만들어 준다. sout은 **System.out.println();**을 자동으로 만들어 준다.<br/>

자바란?
======

자바 표준 스펙
------
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/4-1.jpeg" width="500"></p>

　자바는 표준 스펙과 구현으로 나눌 수 있다.
* 자바 표준 스펙
  - 자바는 이렇게 만들어야 한다는 설계도이며, 문서이다.
  - 이 표준 스펙을 기반으로 여러 회사에서 실제 작동하는 자바를 만든다.
  - 자바 표준 스펙은 자바 커뮤니티 프로세스(JCP)를 통해 관리된다.
* 다양한 자바 구현
  - 여러 회사에서 자바 표준 스펙에 맞추어 실제 작동하는 자바 프로그램을 개발한다.
  - 각각 장단점이 있다. 예를 들어, Amazon Corretto는 AWS에 최적화 되어 있다.
  - 각 회사들은 대부분 윈도우, MAC, 리눅스 같이 다양한 OS에서 작동하는 버전의 자바도 함께 제공한다.

컴파일과 실행
------
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/4-2.jpeg" width="500"></p>

　자바 프로그램은 컴파일과 실행 단계를 거친다.
* Hello.java와 같은 자바 소스 코드를 개발자가 작성한다.
* 자바 컴파일러를 사용해서 소스 코드를 컴파일 한다.
  - 자바가 제공하는 javac라는 프로그램을 사용한다.
  - .java를 .class 파일이 생성된다.
  - 자바 소스 코드를 바이트코드로 변환하며 자바 가상 머신에서 더 빠르게 실행될 수 있게 최적화하고 문법 오류도 검출한다.
* 자바 프로그램을 실행한다.
  - 자바가 제공하는 java라는 프로그램을 사용한다.
  - 자바 가상 머신(JVM)이 실행되면서 프로그램이 작동한다.

IDE와 자바
------
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/5-1.jpeg" width="500"></p>

　인텔리제이는 내부에 자바를 편리하게 설치하고 관리할 수 있는 기능을 제공한다. 이 기능을 사용하면 인텔리제이를 통해 자바를 편리하게 다운로드 받고 실행할 수 있다.<br/>
　인텔리제이를 통한 자바 컴파일, 실행 과정은 다음과 같다.<br/>
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/5-2.jpeg" width="500"></p>

* 컴파일
  - 자바 코드를 컴파일 하려면 javac라는 프로그램을 직접 사용해야 하는데, 인텔리제이는 자바 코드를 실행할 때 이 과정을 자동으로 처리해준다.
  - 인텔리제이 화면에서 프로젝트에 있는 out 폴더에 가보면 컴파일된 .class 파일이 있는 것을 확인할 수 있다.
* 실행
  - 자바를 실행하려면 java라는 프로그램을 사용해야 한다. 이때 컴파일된 .class 파일을 지정해주면 된다.
* 인텔리제이에서 자바 코드를 실행하면 컴파일과 실행을 모두 한번에 처리한다.
* 인텔리제이 덕분에 매우 편리하게 자바 프로그램을 개발하고, 학습할 수 있다.

자바와 운영체제 독립성
------
　일반적인 프로그램은 다음과 같이 작동한다.<br/>
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/5-3.jpeg" width="500"></p>

　일반적인 프로그램은 다른 운영체제에서 실행할 수 없다. 예를 들어, 윈도우 프로그램은 MAC이나 리눅스에서 작동하지 않는다. 왜냐하면 윈도우 프로그램은 윈도우 OS가 사용하는 명령어들로 구성되어 있기 때문이다. 해당 명령어는 다른 OS와 호환되지 않는다.<br/>
　자바 프로그램은 다음과 같이 작동한다.<br/>
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/5-4.jpeg" width="500"></p>

　자바 프로그램은 자바가 설치된 모든 OS에서 실행할 수 있다. 자바 개발자는 특정 OS에 맞추어 개발을 하지 않아도 된다. 자바 개발자는 자바에 맞추어 개발하면 된다. OS 호환성 문제는 자바가 해결한다. Hello.class와 같이 컴파일된 자바 파일은 모든 자바 환경에서 실행할 수 있다. 윈도우 자바는 윈도우 OS가 사용하는 명령어들로 구성되어 있다. MAC이나 리눅스 자바도 본인의 OS가 사용하는 명령어들로 구성되어 있다. 개발자는 각 OS에 맞도록 자바를 설치하기만 하면 된다.<br/>
　자바 개발과 운영 환경은 다음과 같다.<br/>
<p align="center"><img src="/assets/img/김영한의 자바 입문/1장 Hello Wolrd/5-5.jpeg" width="500"></p>

　개발할 때 자바와 서버에서 실행할 때 다른 자바를 사용할 수 있다. 개발자들은 개발의 편의를 위해서 윈도우나 MAC OS를 주로 사용한다. 서버는 주로 리눅스를 사용한다. 만약 AWS를 사용한다면 Amazon Corretto 자바를 AWS 리눅스 서버에 설치하면 된다. 자바의 운영체제 독립성 덕분에 각각의 환경에 맞추어 자바를 설치하는 것이 가능하다.