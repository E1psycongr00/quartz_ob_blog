---
tags:
  - 완성
  - JAVA
aliases: 
title: instanceof 패턴 매칭
date: 2023-10-21
---
작성 날짜: 2023-10-21
작성 시간: 00:36


----
## 내용(Content)

기존의 instanceof는 casting을 활용해 형변환해줘야 한다.
```java
Object x = "1";  
if (x instanceof String) {  
    String result = ((String)x).concat("2");  
    System.out.println(result);  
}
```

그러나 jav16에 오면서 형변환 없이 쉽게 사용하도록 문법이 개선되었다.

```java
Object x = "1";  
if (x instanceof String s) {  
    String result = s.concat("2");  
    System.out.println(result);  
}
```

이렇게 뒤에 valuable name을 할당하면 캐스팅 없이 해당 name을 이용해 해당 타입의 메서드를 사용할 수 있다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://velog.io/@gkskaks1004/Java-16%EC%97%90%EC%84%9C-instanceof-%EC%97%B0%EC%82%B0%EC%9E%90%EC%97%90-%EB%8C%80%ED%95%9C-pattern-matching

## 연결 노트










