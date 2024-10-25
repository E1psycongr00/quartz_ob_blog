---
tags:
  - 완성
  - JAVA
  - Validation
aliases: 
title: Java Bean Validation 소개
date: 2023-11-10
---
작성 날짜: 2023-11-10
작성 시간: 09:42


----
## 내용(Content)

### Java Bean Validation이란

Java Bean Validation은 validator와 어노테이션을 활용해 필요한 유효성 검사를 편리하게 처리하기 위한 라이브러리이다. 자바 표준 스펙(JSR 380)으로, 어노테이션을 사용하여 객체의 필드에 대한 규칙을 정의하고 이를 검증할 수 있는 도구를 제공한다.

### 구성

Java Bean Validation은 Jakarata Bean Validation 이라는 명세와 이를 구현한 Hibernate Validator를 제공한다. 즉, 어노테이션과 해당 어노테이션을 활용해서 validation을 처리하는 구현체가 서로 분리되어 있다.


### 의존성 등록

```groovy
implementation('org.hibernate.validator:hibernate-validator:6.1.2.Final')  
implementation('org.glassfish:jakarta.el:3.0.3')
```

## 질문 & 확장

(없음)

## 출처(링크)
- https://kapentaz.github.io/java/Java-Bean-Validation-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90/#

## 연결 노트



