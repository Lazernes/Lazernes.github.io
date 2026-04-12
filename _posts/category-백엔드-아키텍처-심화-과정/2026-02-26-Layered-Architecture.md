---
title: "Layered Architecture"
excerpt: "계층형 아키텍처의 구조와 역할 분리"

wirter: Myeongwoo Yoon
categories:
  - 백엔드 아키텍처 심화 과정
tags:
  - Spring
  - Java

toc: true
toc_sticky: true
 
date: 2026-02-26
last_modified_at: 2026-02-26
---

&ensp;백엔드 개발을 공부하다 보면 `Controller, Service, Repository` 구조를 아주 자연스럽게 접하게 된다. 하지만 많은 경우 이 구조를 "그냥 스프링에서 원래 이렇게 하는 것" 정도로 받아들이곤 한다.  
&ensp;이번 글에서는 Layered Architecture를 단순한 코드 배치 방식으로만 보지 않고, 왜 이런 구조가 등장했는지, 그리고 이 구조가 앞으로 DDD나 MSA 같은 더 큰 아키텍처로 나아가기 위한 어떤 출발점이 되는지를 함께 정리해보려 한다.

아키텍처란 무엇인가
======

&ensp;아키텍처라는 단어를 들으면 다소 어렵고 거창하게 느껴질 수 있다. 하지만 사실 우리가 Spring Boot 프로젝트를 만들고, Controller를 작성하고, Service를 호출하고, Repository를 통해 데이터를 저장하는 순간 이미 그 코드에는 하나의 구조가 생긴다.<br/> 
&ensp;즉, 아키텍처란 거대한 시스템에서만 등장하는 개념이 아니라, **코드가 어떤 구조로 배치되고 서로 어떤 관계를 맺는지**를 설명하는 방식이라고 볼 수 있다.<br/>
&ensp;조금 더 구체적으로 말하면 아키텍처는 다음과 같은 것들을 결정한다.
* 어떤 코드가 어디에 위치하는가
* 코드들이 서로 어떻게 소통하는가
* 변경이 발생했을 때 영향이 어디까지 퍼지는가

&ensp;결국 아키텍처는 단순히 "예쁘게 정리된 구조"가 아니라, **변경에 얼마나 잘 버틸 수 있는 시스템을 만들 것인가**에 대한 설계라고 볼 수 있다.

왜 아키텍처를 고민하게 되었을까
======

&ensp;초기 인터넷 시대의 애플리케이션은 지금처럼 복잡하지 않았다. 단순히 HTML을 출력하고, 사용자의 요청을 간단히 처리하는 수준이 많았다.  
&ensp;하지만 인터넷이 발전하고 서비스가 점점 복잡해지면서, 처음에는 단순하게 나열되어 있던 코드에 점점 더 많은 변경이 쌓이기 시작했다. 그러면서 구조를 고민하지 않은 시스템은 여러 문제를 드러내기 시작했다.
* 코드가 길어지며 복잡도가 증가한다
* 변경이 필요할 때 전체 흐름을 파악하는 데 시간이 오래 걸린다
* 수정 과정에서 예상하지 못한 오류가 발생한다
* 테스트와 배포 시간이 점점 길어진다
* 생산성이 떨어진다
* 담당자가 바뀌면 왜 그렇게 작성되었는지 맥락이 사라진다
* 기존 코드를 건드리기 두려워 중복 코드를 새로 작성하게 된다
* 결국 기술 부채가 쌓이고 변경이 어려운 코드가 된다

&ensp;이런 문제는 과거의 오래된 시스템에서만 나타나는 것이 아니다. 처음에는 작게 시작한 서비스도 기능이 늘어나고, 사용자 수가 늘어나고, 새로운 요구사항이 추가되면서 동일한 문제를 반복해서 겪게 된다. 즉, **아키텍처는 규모가 커진 뒤에 고민하는 것이 아니라, 변화가 시작되는 순간부터 중요해지는 문제**이다.

왜 계층형 구조가 필요했을까
======

요청은 한 방향으로 흐른다
------

&ensp;대부분의 시스템은 다음과 같은 순서로 동작한다.
1. 사용자로부터 입력을 받는다
2. 입력값에 따라 필요한 규칙과 정책을 적용한다
3. 처리 결과를 저장하거나 조회한다

&ensp;이 흐름을 보면 요청 처리는 자연스럽게 **한 방향**으로 이동한다. 즉, 화면에서 입력을 받고, 그 입력을 처리하고, 데이터를 저장하거나 읽어오는 흐름이다. 이런 특성 때문에 코드를 계층별로 나누는 방식이 등장하게 되었다.

서로 다른 이유로 변경되는 코드를 분리한다
------

&ensp;계층을 나누는 가장 큰 이유는 각 부분이 **서로 다른 이유로 변경되기 때문**이다.
* 사용자와 소통하는 코드  
  - 화면, 요청 형식, 응답 형식이 바뀔 때 변경된다
* 비즈니스 규칙을 처리하는 코드  
  - 정책, 규칙, 요구사항이 바뀔 때 변경된다
* 데이터를 저장하고 조회하는 코드  
  - DB 종류, 쿼리 방식, 저장소 기술이 바뀔 때 변경된다

&ensp;예를 들어 DB를 MySQL에서 MongoDB로 바꾼다고 해서 할인 정책이 바뀌어야 하는 것은 아니다. 또 모바일 앱이 추가된다고 해서 결제 규칙이 달라지는 것도 아니다.<br/>  
&ensp;그런데 구조가 섞여 있으면 이런 변경이 서로 영향을 주기 시작한다. 그래서 변경 이유가 다른 코드들을 분리하는 것이 중요하다.

익숙한 형태: Controller, Service, Repository
------

&ensp;이러한 생각이 정리되면서 오늘날 우리가 익숙하게 사용하는 구조가 만들어졌다.
* **Controller** : 사용자 요청을 받고 응답을 반환하는 계층
* **Service** : 비즈니스 규칙과 로직을 처리하는 계층
* **Repository** : 데이터를 저장하고 조회하는 계층

&ensp;즉, Layered Architecture는 단순히 폴더를 나누는 방식이 아니라, **역할이 다른 코드들을 책임 기준으로 분리하는 구조**라고 이해하는 것이 더 정확하다.

핵심 규칙: 의존성은 한 방향이어야 한다
======

&ensp;계층형 구조에서 가장 중요한 규칙은 **의존성의 방향이 한 방향이어야 한다는 것**이다. 상위 계층은 하위 계층을 알지만, 하위 계층은 상위 계층을 몰라야 한다.
* Controller는 Service를 알 수 있다
* Service는 Repository를 알 수 있다
* 하지만 Repository는 Service를 몰라야 한다
* Service는 Controller를 몰라야 한다

&ensp;이 규칙이 중요한 이유는, 하위 계층이 상위 계층에 의존하기 시작하면 구조가 뒤엉키기 때문이다. 한 곳의 변경이 다른 곳으로 연쇄적으로 퍼지고, 결국 계층 분리의 의미가 사라진다.

의존성 방향이 깨졌을 때의 문제
======

잘못된 예시
------

```java
@Service
public class OrderService {

    public OrderResponse createOrder(HttpServletRequest request) {
        String userId = request.getHeader("X-User-Id");
        String clientIp = request.getRemoteAddr();

        // 비즈니스 로직 처리...
    }
}
```

&ensp;이 코드는 Service 계층이 HttpServletRequest를 직접 다루고 있다. 즉, Service가 HTTP라는 특정 통신 방식에 의존하게 된다.<br/>
&ensp;이렇게 되면 다음과 같은 문제가 생긴다.
* Service가 비즈니스 로직만 담당하지 못한다
* HTTP 요청이 아닌 다른 환경에서 재사용하기 어려워진다
* gRPC, Kafka Consumer, Scheduler 같은 다른 진입점에서 같은 로직을 쓰기 어렵다
* 테스트를 할 때도 HttpServletRequest를 준비해야 해서 번거롭다

&ensp;즉, Service가 Presentation 계층의 개념까지 알아버리면, 구조가 단단해지는 것이 아니라 오히려 특정 환경에 묶여버린다.

올바른 예시
------

```java
@RestController
public class OrderController {

    private final OrderService orderService;

    @PostMapping("/orders")
    public ResponseEntity<OrderResponse> createOrder(
            @RequestHeader("X-User-Id") String userId,
            @RequestBody OrderRequest request) {

        // Controller에서 HTTP 요소를 꺼내서 Service에 전달
        OrderResponse response = orderService.createOrder(userId, request);
        return ResponseEntity.ok(response);
    }
}

// Service는 순수한 비즈니스 로직만 담당
@Service
public class OrderService {

    public OrderResponse createOrder(String userId, OrderRequest request) {
        // HTTP에 대해 전혀 모르는 상태
        // 비즈니스 로직 처리...
    }
}
```

&ensp;이 구조에서는 Controller가 HTTP와 관련된 요소를 해석하고, Service에는 순수한 값만 전달한다. 그 결과 Service는 HTTP를 몰라도 되고, 비즈니스 로직에만 집중할 수 있다. 이렇게 되면 같은 Service를 여러 환경에서 재사용할 수 있다.
```
REST Controller  ──→ OrderService.createOrder()
gRPC Service     ──→ OrderService.createOrder()
Kafka Consumer   ──→ OrderService.createOrder()
Scheduler        ──→ OrderService.createOrder()
```

&ensp;즉, 의존성 방향을 지킨다는 것은 단순한 규칙 준수가 아니라, 재사용성, 테스트 용이성, 확장성을 확보하는 일이다.

패키지 구조로 살펴보는 Layered Architecture
======

기본적인 계층 중심 패키지 구조
------

```
src/main/java/com/example/shop
│
├── controller/
├── service/
├── repository/
├── entity/
└── dto/
```

&ensp;이 구조는 가장 전형적인 Layered Architecture 패키지 구조이다. 각 계층의 역할이 폴더 단위로 명확하게 드러나기 때문에, 처음 구조를 익히는 단계에서는 이해하기 쉽고 관리하기 편하다. 장점은 다음과 같다.
* 역할별 책임이 명확하다
* 프로젝트를 처음 보는 사람도 구조를 빠르게 이해할 수 있다
* 작은 규모의 프로젝트에서는 충분히 효율적이다

도메인 중심 패키지 구조
------

```
src/main/java/com/example/shop
│
├── order/
│   ├── controller/
│   ├── service/
│   ├── repository/
│   ├── entity/
│   └── dto/
│
├── product/
│   ├── controller/
│   ├── service/
│   ├── repository/
│   ├── entity/
│   └── dto/
│
└── user/
    ├── controller/
    ├── service/
    ├── repository/
    ├── entity/
    └── dto/
```

&ensp;시스템 규모가 커지면 계층 기준으로만 패키지를 나누는 것보다, order, product, user처럼 도메인 기준으로 묶는 구조가 더 유리해질 수 있다. 장점은 다음과 같다.
* 특정 도메인 관련 파일을 한곳에서 찾기 쉽다
* 도메인 경계를 더 명확하게 볼 수 있다
* 시스템이 커질수록 유지보수성이 좋아질 수 있다

&ensp;즉, 작은 프로젝트에서는 계층 중심 구조가 익숙하고 편리할 수 있지만, 도메인이 커지고 팀 단위 협업이 많아질수록 도메인 중심 구조가 더 적합할 수 있다.

Layered Architecture의 목표
======

&ensp;Layered Architecture의 핵심 목표는 각 계층이 자기 책임에만 집중하고, 다른 계층의 변경으로부터 보호받는 구조를 만드는 것이라고 정리할 수 있다. 이 말을 조금 더 풀어보면 결국 핵심은 변경 가능성이다.
* 표현 계층을 REST API에서 GraphQL로 바꿀 수 있고
* 데이터 접근 계층을 JPA에서 MyBatis로 바꿀 수 있고
* 이런 변화가 비즈니스 로직에 영향을 주지 않아야 한다

&ensp;이 상태가 바로 이상적인 계층 분리 상태라고 할 수 있다.

결국 기억해야 할 것
------

1. 각 계층은 자신의 역할과 책임에 집중해야 한다
2. 서로 다른 이유로 변경되는 코드들을 분리해야 한다
3. 의존성의 방향은 위에서 아래로만 흐르게 해야 한다
4. 기술이 바뀌어도 비즈니스 로직이 흔들리지 않는 구조를 지향해야 한다

Layered Architecture는 끝이 아니라 시작
======

&ensp;Layered Architecture는 현대 백엔드 개발의 출발점에 가깝다. 이 구조를 이해하면, 이후 더 큰 구조로 나아가는 흐름도 자연스럽게 이해할 수 있다.

```
[현재 위치]
모놀리식 + Layered Architecture
│
├── 도메인 기반 설계 (DDD)
│   비즈니스 영역별로 경계를 나누는 방법
│
├── MSA (Microservices Architecture)
│   나뉜 경계를 독립적인 서비스로 분리하는 방법
│
└── 그리고 이를 운영하기 위한 기술들
    ├── Redis
    ├── Kafka
    └── Container
```

&ensp;즉, Layered Architecture의 핵심 원칙인 "서로 다른 이유로 변경되는 것들을 분리하고, 의존성의 방향을 지키는 것"은 DDD, MSA, 분산 시스템 기술의 기본 전제가 된다.

아키텍처와 소프트웨어의 본질
======

&ensp;소프트웨어는 한 번 만들고 끝나는 것이 아니다. 계속해서 요구사항이 바뀌고, 기능이 추가되고, 버그가 수정되고, 팀원이 바뀌며, 외부 기술도 달라진다. 즉, 소프트웨어의 본질은 "완성"보다 변화에 가깝다. 그래서 아키텍처의 본질적인 목적도 결국 하나로 모인다. 아키텍처의 목적은 변경에 유연한 시스템을 만드는 것이다.<br/>
&ensp;정리하면, Layered Architecture는 단순히 Controller, Service, Repository를 나누는 문법적 규칙이 아니다. 그보다 더 중요한 것은, 변경 이유가 다른 코드들을 분리하고, 의존성의 방향을 통제하여, 시스템이 커져도 유연하게 유지될 수 있도록 만드는 사고방식이라는 점이다.