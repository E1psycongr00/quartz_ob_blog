---
tags:
  - 완성
  - JAVA
aliases: 
date: 2024-10-07
title: Record Pattern
---
작성 날짜: 2024-10-07
작성 시간: 11:56


----
## 내용(Content)

### Record Pattern

>[!summary]
>Record 타입이라면, instanceof 를 이용해 타입을 분해해서 사용할 수 있다.

점점 java가 데이터 지향으로 가면서 데이터를 쉽게 접근하고 쓰기 위해 새로운 문법이 추가되었다.

java 16에서 도입된 instanceof 문법은 캐스팅할 필요 없이 instanceof으로 타입을 체크하면 해당 타입을 손 쉽게 사용할 수 있었다. Record Pattern은 여기서 더 나아가 Record 타입이라면 타입 접근자를 자동으로 호출해서 개발자가 쉽게 접근해서 활용할 수 있다. 마치 객체를 분해한 것과 같은 느낌을 준다.

### Record Pattern 이전의 코드

```java
private static void print(Object obj) {
	if (obj instanceof Point p) {
		int x = p.x();
		int y = p.y();
		System.out.println(x + y);
	}
}
```

위 코드는 obj가 Point 객체라면 Point 객체 p로 선언하고 이를 if 문 블록 안에서 지역 변수로써 활용 할 수 있다. 

### Record Pattern 코드

```java
private static void print(Object obj) {
	if (obj instanceof Point(int x, int y)) {
		System.out.println(x + y);
	}
}
```

record처럼 객체를 선언하면 Point Record의 객체의 변수를 가져와서 if문 블록 내에서 사용가능하다. if 문 안에서 사용 가능한 객체 분해 할당과 같은 문법이다.

## 질문 & 확장

여전히 js나 python의 객체 분해 할당과는 약간 거리가 멀지만 Java 진영에서 타입 안정성과 함께 데이터를 쉽게 처리하는 방법을 많이 고민하고 있는 것 같다.

## 출처(링크)

- https://mangkyu.tistory.com/308
- 
## 연결 노트










