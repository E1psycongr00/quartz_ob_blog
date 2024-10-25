---
tags:
  - 완성
  - JAVA
  - JPA
aliases: 
title: data jpa repository 응답 타입 유연하게 쓰기
date: 2023-10-26
---
작성 날짜: 2023-10-26
작성 시간: 19:21


----

## 문제 & 원인

Hello라는 객체의 엔티티가 있다고 하자. 

우리는 data jpa repository에서 [[data jpa Projection 인터페이스|closed projection]]을 활용해 정보를 가져오는데 만약 여러 projection이 있고 이를 비슷한 네이밍 규칙을 활용해 spring data repository를 작성할 수 있는 방법이 있을까?


## 해결 방안

### 객체 리턴 타입을 메서드 네임에 명시하기

만약 Hello 엔티티와 HelloInfo라는 projection 두 가지 타입을 name을 활용해 데이터를 가져오고 싶다고 가정하자. 

그렇다면 이런 네이밍 규칙을 활용해 작성하면 된다

(find 또는 count exist) (리턴 타입) by ~~~

예시 코드를 살펴보자

```java
public interface HelloRepository extends JpaRepository<Hello, UUID> {  
    Optional<HelloInfo> findHelloInfoByName(String name);  
  
    Optional<Hello> findHelloByName(String name);  
  
}
```

테스트 코드를 돌려보면 잘 동작한다.

![[Pasted image 20231026193154.png]]


![[Pasted image 20231026193345.png]]


### 제네릭을 활용하기
```java
<T> Optional<T> findByName(String name, Class<T> type);
```

위와 같이 인자 타입과 classType을 같이 넘긴다. 그러면 다음과 같은 결과를 얻을 수 있다.

![[Pasted image 20231026220731.png]]


## 질문 & 확장

(없음)

## 출처(링크)
- https://velog.io/@max9106/JPA-Projection
- https://dncjf64.tistory.com/358
## 연결 노트
- [[data jpa Projection 인터페이스]]
- [[JDK Proxy(동적 Proxy)]]
