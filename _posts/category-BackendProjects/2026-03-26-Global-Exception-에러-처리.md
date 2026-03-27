---
title: "Global Exception 에러 처리"
excerpt: "Global Exception으로 에러 처리"

wirter: Myeongwoo Yoon
categories:
  - BackendProjects
tags:
  - Java
  - SpringBoot

toc: true
toc_sticky: true
use_math: true
 
date: 2026-03-26
last_modified_at: 2026-03-26
---

&ensp;**Global Exception Handler**는 @ControllerAdvice 또는 @RestControllerAdvice 와 @ExceptionHandler 어노테이션을 기반으로 Controller 내에서 발생하는 에러에 대해서 해당 Handler에서 캐치하여 오류를 발생시키지 않고 응답 메시지로 Client에게 전달해 주는 기능을 의미한다.<br/>
&ensp;이는 HTTP Status 코드로 정상코드가 아닌 `오류코드`로 반환하였을 시 실제 에러가 발생하기에 이를 위해 중간에 GlobalException을 통해 Exception 발생 시에도 HTTP Status 코드로 `정상 코드`를 보내고 커스텀한 코드를 보냄으로써 실제 Client 내에서 이를 처리할 수 있게 돋기 위해서 사용한다.
