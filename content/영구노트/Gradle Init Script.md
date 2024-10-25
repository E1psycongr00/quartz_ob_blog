---
tags: 
  - 완성
  - Gradle
aliases: 
title: Gradle Init Script
date: 2023-10-16
---
작성 날짜: 2023-10-16
작성 시간: 21:20


----
## 내용(Content)

Initialization Script는 Gradle의 다른 스크립트와 유사하지만 다른 특징이 있다. 바로 Gradle의 build가 실행되기 전에 Script를 실행한다는 점이다. 그래서 Initialization Script를 작성시 알아야 할 특징 및 목적에 대해서 알아보자

1. 시스템 전역 설정에 사용한다.(ex. 커스텀 플러그인 찾을 위치 설정)
2. 현재 환경에 따른 프로퍼티 설정, 개발 장비, 지속적 통합과 같은 프로퍼티 값 바꿔치기가 같은 경우
3. 빌드에 필요한 개발자의 개인 정보
4. 장비 전용 설정, JDK 위치 등
5. 빌드 리스너를 등록하여 Gradle 빌드 이벤트를 받는다.
6. buildSrc에는 접근할 수 없다


### 사용 방법

우선 init script를 작성한다

**build.gradle.kts**

```kotlin
// build.gradle.kts

repositories {
    mavenCentral()
}

tasks.register("showRepos") {
    val repositoryNames = repositories.map { it.name }
    doLast {
        println("All repos:")
        println(repositoryNames)
    }
}
```

**init.gradle.kts**

```kotlin
// init.gradle.kts

allprojects {
    repositories {
        mavenLocal()
    }
}
```

그리고 실행하는 방법은 다음과 같다.

```
> gradle --init-script init.gradle.kts -q showRepos
All repos:
[MavenLocal, MavenRepo]
```

그 이외의 자세한 사용 방법은 [Gradle 초기화 스크립트](https://docs.gradle.org/current/userguide/init_scripts.html)를 참고하자
## 질문 & 확장

(없음)

## 출처(링크)
- https://kwonnam.pe.kr/wiki/gradle/init_scripts
- https://docs.gradle.org/current/userguide/init_scripts.html

## 연결 노트










