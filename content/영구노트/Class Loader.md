---
tags:
  - 완성
  - JAVA
aliases: 
title: Class Loader
date: 2023-10-08
---
작성 날짜: 2023-10-08
작성 시간: 20:08


----
## 내용(Content)
### 클래스 로더란
클래스 로더는 클래스 파일의 바이트 코드를 읽어 Method Area의 런타임 데이터 영역으로 가져온다. 

![[Pasted image 20231010151555.png]]


### Bootstrap Class Loader
- 네이티브 코드로 작성되어 있으며 JVM이 내장되어 있다.
- JVM이 시작될 때 실행되며, package 처럼 JVM 실행에 필요한 클래스들을 로딩한다.

### Platform Class Loader(Extension Class Loader)
- java.lang.ClassLoader의 인스턴스로 Java SE platform API 등 자바에서 기본적으로 제공해주는 클래스를 로딩할 때 사용된다.
- Bootstrap Class Loader 클래스를 부모로 가진다.
- 기존에는 Extension Class Loader 였으나 모듈 시스템이 도입된 이후 Platform Class Loader로 바뀜

### System Class Loader(Application Class Loader)
- 유저가 작성한 클래스를 로딩할 때 사용된다.
- ClassPath에 명시된 경로를 통해 클래스를 찾는다.
- 부모는 Platform Class Loader이다.
- 기존에는 Application Class Loader였으나 System Class Loader로 수정됨


### Class Loader 특징

#### 위임 모델
클래스 로더는 기본적으로 위임 모델을 가진다. 자신에게 클래스 로딩 요청이 들어왔을 때, 부모에게 클래스 요청을 위임하며, 부모 클래스가 찾지 못하는 경우 본인이 탐색을 수행한다.

#### 가시성 원칙
상위 클래스 로더에서 탐색한 클래스가 하위 클래스 로더에서도 표시가 되지만, 하위 클래스에 의해 로딩된 클래스는 상위 클래스에서 표시하지 않는다.

#### 고유성 원칙
고유성 속성은 클래스가 고유하고, 클래스가 반복되지 않도록 보장한다. 상위 클래스 로더가 찾지 못한다면, 현재 클래스 로더가 스스로 클래스를 찾으려 시도한다.


## 질문 & 확장

- Java Platform SE API가 정확하게 뭘까 표준 API인가?
- Class Loader는 하위 클래스에서 class 탐색을 진행하지만, 그 이전에 부모에게 위임해서 class를 처리하고, 처리하지 못하는 경우, 해당 로더에서 탐색을 진행하는 군


## 출처(링크)
- https://www.baeldung.com/java-classloaders
- https://www.geeksforgeeks.org/classloader-in-java/
- https://velog.io/@sgwon1996/JAVA%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC%EC%99%80-JVM-%EA%B5%AC%EC%A1%B0#class-loader
- https://tecoble.techcourse.co.kr/post/2021-07-15-jvm-classloader/

## 연결 노트
- [[Class Loader 메서드]]
- [[영구노트/String Constant Pool]]









