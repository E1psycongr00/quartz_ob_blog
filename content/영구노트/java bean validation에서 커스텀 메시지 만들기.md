---
tags:
  - 완성
  - JAVA
  - Validation
aliases: 
title: java bean validation에서 커스텀 메시지 만들기
date: 2023-11-10
---
작성 날짜: 2023-11-10
작성 시간: 12:31


----
## 내용(Content)

### Message 직접 작성하기

```java
@Positive(message = "양수만 가능합니다")  
private final int productNo;
```


이렇게 직접 어노테이션에 속성 message를 줄 수 있다.

단점은 매번 이렇게 message를 작성하는 것은 불필요한 중복 코드를 작성하게 될 가능성이 크다.

### MessageTemplate 활용하기

ContraitViolation 객체 속성을 살펴보면

```text
ConstraintViolationImpl{interpolatedMessage='과거 또는 현재의 날짜여야 합니다', propertyPath=salStartAt, rootBeanClass=class backjoon.Product, messageTemplate='{javax.validation.constraints.PastOrPresent.message}
```


이렇게 messageTemplate이 있음을 알 수 있다. 이 때 참조하는 template을 직접 수정해도 되고, 내가 지정한 template을 등록해줘도 된다.

MessageTemplate은 **ValidationMessages.properties** 을 참조해서 InterpolatedMessage를 만든다.

#### 내가 새로 경로 지정하고 등록하기

```properties
//ValidationMessages_ko.properties

com.backjoon.Product.Positive.message=양수만 가능하다
```


```java
@Positive(message = "{com.backjoon.Product.Positive.message}")  
private final int productNo;
```


#### 기존에 지정된 경로에서 MessageTemplate 수정하기

```properties
//ValidationMessages_ko.properties

javax.validation.constraints.Positive.message=양수만 가능하다
```

이렇게 작성하면 기존의 Validation에 설정된 message를 덮어 쓴다
## 질문 & 확장

(없음)

## 출처(링크)
- https://kapentaz.github.io/java/Java-Bean-Validation-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90/#
- https://docs.spring.io/spring-framework/reference/core/validation/beanvalidation.html

## 연결 노트










