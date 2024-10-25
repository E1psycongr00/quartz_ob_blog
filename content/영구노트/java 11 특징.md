---
tags:
  - 완성
  - IT
  - JAVA
aliases: 
title: java 11 특징
date: 2023-10-04
---

작성 날짜: 2023-10-04
작성 시간: 13:06


----
## 내용(Content)
2018년 9월 25일 ~ 2023년 9월,  연장 지원은 2026년 9월에 종료

### 문자열 & 파일
```java
"Marco".isBlank();
"Mar\nco".lines();
"Marco  ".strip();

Path path = Files.writeString(Files.createTempFile("helloworld", ".txt"), "Hi, my name is!");
String s = Files.readString(path);
```

### 소스 파일 실행
```bash
ubuntu@DESKTOP-168M0IF:~$ java MyScript.java
```

### 람다 매개변수에 대한 지역 변수 추론
```java
(var firstName, var lastName) -> firstName + lastName;
```

### HttpClient
java9버전의 HttpClient가 더 이상 preview 버전이 아니게 됩니다.

### 기타
- Flight Recorder
- No-OP-GC

### 라이선스의 변화
Java SE 11부터 Oracle JDK의 독점 기능이 Open JDK에 이식된다. 이것이 의미하는 것은  Oracle JDK와 Open JDK가 사실상 거의 동일해진다는 것이다. 2019년부터 1월부터 Oracle이 제공하는 모든 Oracle JDK가 유료화된다. 이 때부터 많은 기업이 Oracle JDK을 버리고 Open JDK로 갈아탔다. 대표적인 예로는 AZul의 Zulu JDK가 있는데 이는 Oracle의 TCK 인증을 받은 구현체이다.

## 질문 & 확장

- TCK란 정확히 무엇인가?
- TCK를 인증 받은 Open JDK를 되도록 사용하도록 해야겠군
- Oracle JDK와 Open JDK의 차이점과 둘 간의 역사를 알고 싶다
- Flight Recorder는 뭐지...
- 엡실론 가비지 컬렉터도 복잡한데
- JDK의 라이센스에 대해서도 공부할 가치가 있을지도 몰라

## 출처(링크)
- https://www.marcobehler.com/guides/a-guide-to-java-versions-and-features
- https://namu.wiki/w/Java/%EB%B2%84%EC%A0%84#s-14

## 연결 노트
- [[Java 배포판(distribution) (Oracle JDK vs Open JDK)]]
- [[무료와 독점 JDK 모음]]
- [[No - Op GC (Epsilon 가비지 컬렉터)]]
- [[Project Jigsaw]]
- [[자바 9 ~ 11 이전까지 과정]]











