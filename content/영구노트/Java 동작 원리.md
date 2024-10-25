---
tags:
  - 완성
  - JAVA
aliases: 
title: Java 동작 원리
date: 2023-10-06
---


작성 날짜: 2023-10-06
작성 시간: 19:14


----
## 내용(Content)

### 동작 흐름

![[자바 동작 과정(draw).svg|800]]

동작 순서는 크게 컴파일과정과 런타임 과정로 나눌 수 있다.

**컴파일 과정**
javac 컴파일러를 이용해 java source code를 byte code로 컴파일한다. class 파일형식으로 되어 있고 클래스 안에는 class path와 jvm이 클래스를 찾고 관리할 수 있도록 여러가지 메타 데이터를 가지고 있다.

**런타임 과정**
Class Loader가 클래스 파일을 받고 이 과정에서 Loading, Linking, Initialization을 한다.

#### Loading
클래스 또는 인터페이스 생성은 해당 클래스의 필드, 메소드, 런타임 상수 풀 등 클래스가 가지고 있는 바이트 코드를 찾고, JVM 메소드 영역안(Runtime Data Area 안에 있음)에 구성하는 것을 의미한다.

#### Linking
링크는 3단계를 거친다
- Verification(검증): 로딩된 바이트 코드가 JVM 명세를 따르는지 검증
- Prepare(준비): 정적 필드를 각 유형의 초기값으로 초기화하는 과정
	- ex) int type-> 0, reference type -> null
- Resolution(해결): 클래스 안의 Constant Pool 안에 있는 Symbolic Reference를 고정된 주소값으로 바꾸는 과정이다. 

#### Initialization
클래스 초기화 함수를 실행한다. 클래스에 작성된 static 초기화 함수를 모두 합쳐 한꺼번에 실행한다. 초기화 과정은 링킹 과정이 끝나고 단 한번만 이루어진다.



## 질문 & 확장

(없음)

## 출처(링크)
- https://junroot.github.io/programming/Java%EC%9D%98-%EB%B9%8C%EB%93%9C%EC%99%80-%EB%B0%B0%ED%8F%AC/
- https://kingofbackend.tistory.com/123#article-2--java-compiler-with-binary-code,-byte-code
## 연결 노트
-  [[Class Loader]]
- [[JVM 구조]]
-  [[영구노트/String Constant Pool|String Constant Pool]]








