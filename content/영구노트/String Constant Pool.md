---
tags:
  - 완성
  - JAVA
aliases: 
title: String Constant Pool
date: 2023-10-08
---

작성 날짜: 2023-10-08
작성 시간: 20:09


----
## 내용(Content)

문자열 리터럴을 저장하는 독립된 공간 영역을 String Constant Pool이라 부른다. 또는 String Pool이라 부른다.  기본적으로 String Constant Pool에 있는 데이터는 GC 대상이 되지 않는다.

선언은 다음과 같다.
```java
String stringConstantPool = "hello world";
String simpleObject = new String("hello world");
```

첫번째와 두번째는 서로 다른 공간에 저장된다. 두번쨰의 경우 단순 인스턴스 생성이기에 heap에 생성된다. 첫번째의 경우도 heap이긴 하나 heap 안에 String Constant Pool에 저장된다.

![[string constant pool(draw).svg]]

string constant pool의 경우 중복되는 경우 단순히 재사용하기 때문에 new를 이용한 임시 객체에 비해 훨씬 효율적이다. 

new는 사용하지 말자
## 질문 & 확장


(없음)

## 출처(링크)
- https://deveric.tistory.com/123

## 연결 노트










