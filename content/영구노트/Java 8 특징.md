---
tags:
  - 완성
  - JAVA
aliases: 
title: Java 8 특징
date: 2023-10-03
---
작성 날짜: 2023-10-03
작성 시간: 15:04


----
## 내용(Content)
자바 8은 대규모 릴리즈이고 자세한 내용은 [자바8 릴리즈](https://www.oracle.com/java/technologies/javase/8-whats-new.html) 를 참고하면 된다. 그 중 핵심은 람다, Collection stream, Optional이 있다. 이것의 자세한 내용은 주제를 넘어서기 때문에 간단하게 소개하겠다.

### Lambda
8 이전에 새로운 Runnable을 인스턴스할 때 다음과 같이 사용해야 했다.
```java
Runnable runnable = new Runnable() {
	@Override
	public void run() {
		System.out.println("Hello world !");
	}
}
```

람다를 사용하면 다음과 같이 쉽게 사용 가능하다.

```java
Runnable runnable = () -> System.out.println("Hello world two!");
```

### 컬렉션 & 스트림
기존에는 for - loop으로 작성해야 했지만 이제는 간단하게 List를 초기화할 수 있다.

```java
List<String> list = Arrays.asList("hello", "world");
```

또한 컬렉션을 stream을 이용하여 가독성있게 처리할 수 있다.

```java
list.stream()
	.filter(name -> name.startsWith("f"))
	.map(String::toUpperCase())
	.sorted()
	.forEach(System.out::println);
```


## 질문 & 확장

- Optional도 간략하게 소개하는 게 낫지 않으려나?
- 람다의 심화적인 활용도 궁금하군

## 출처(링크)
- https://www.marcobehler.com/guides/a-guide-to-java-versions-and-features
- https://www.oracle.com/java/technologies/javase/8-whats-new.html
## 연결 노트
- [[JRE 와 JDK 의 차이]]
- [[Java8 이전 버전 별 특징]]









