---
tags: 
  - 완성
  - IT
  - Gradle
aliases: 
title: Intellij와 Kotlin DSL과 함께하는 멀티 모듈 빌드해보기
date: 2023-10-16
---
작성 날짜: 2023-10-16
작성 시간: 23:43


----
## 내용(Content)

Gradle과 intellij를 활용하면 MultiModule를 쉽게 설계 할 수 있다. 실제 실습을 통해 멀티 모듈 적용 방법 알아보자

### 멀티 모듈 만들기

new 탭에서 new module 만들기를 들어가면 다음과 같은 화면을 볼 수 있다.


![[Pasted image 20231017104637.png]]

여기서 모듈을 생성하면 자동으로 setting.gradle.kts에 다음과 같이 등록된다.

![[Pasted image 20231017105157.png]]
IDE가 똑똑하게도 자동으로 입력해주는데 include("app")을 통해서 app이라고 만들어둔 module에 자동으로 등록된다.

이를 등록하지 않으면 만들어둔 서브 모듈의 app을 인식하지 못하므로 반드시 include("앱이름")을 통해 등록해줘야 한다.

그 다음 app module 디렉토리에 build.gradle.kts를 생성해보자

```kotlin
plugins {  
    id("application")  
}  
  
  
  
application {  
    mainClass = "org.example.AppMain"  
}
```

다음과 같이 작성하면 intellij gradle 태스크에 다음과 같이 생성된다.

![[Pasted image 20231017110425.png]]

application으로 클래스 경로를 지정해줬기 때문에 실행하면 자동으로 해당 클래스 경로를 실행한다.

![[Pasted image 20231017110504.png]]

## 질문 & 확장

(없음)

## 출처(링크)
- https://docs.gradle.org/current/userguide/multi_project_builds.html

## 연결 노트










