---
tags:
  - 완성
  - Gradle
aliases: 
title: Why Gradle
date: 2023-10-05
---
작성 날짜: 2023-10-05
작성 시간: 17:55


----
## 내용(Content)
### Gradle이란
2012년에 출시된 Groovy 기반의 오픈소스  빌드 도구로(최근엔 Kotlin도 지원), 거의 대부분 타입의 소프트웨어를 빌드할 수 있는 빌드 자동화 도구이다.

안드로이드 표준 빌드 시스템으로 채택되었다.

### 빌드 자동화 도구 필요성
- 많은 라이브러리를 자동으로 추가하고 관리
- 손쉬운 라이브러리 버전 동기화 필요성
- jar를 직접 다운로드 받는 것의 보안상 위험성

### 왜 Gradle 인가
- 풍부한 커뮤니티 플러그인 생태계를 가지고 있다.
- Groovy, Kotlin 언어를 활용해 손쉽게 빌드 스크립트를 쓸 수 있다.
- Custom Task를 쉽게 작성할 수 있다.
- 손쉽게 확장 가능하다.
- incremental builds, 빌드 캐시, 병렬 실행과 같은 최적화의 이점을 활용할 수 있다.

이 중에 크게 3가지로 간략하게 요약하면 3가지 이유로 쓰인다고 할 수 있다.

1. 프로젝트 설정 주입(Configuration Injection)
2. 멀티 프로젝트 빌드
3. 빠른 빌드 속도

#### 1. 프로젝트 설정 주입
```groovy
dependencies {
	implementation 'org.springframework.boot::spring-boot-starter'
	testImplementation 'org.springframework.boot::spring-boot-starter-test'
}
```

project별로 주입되는 설정을 다르게 할 수 있는 장점이 있음

#### 2. 멀티 프로젝트 빌드
프로젝트를 진행하다보면 하나의 repository에 여러개의 하위 프로젝트를 진행하는 경우가 있을 수 있다.
만약 따로 프로젝트를 구성한다면 중복되는 코드가 많아질 수 있다. 그러나 gradle의 멀티 프로젝트를 활용하면 공통 프로젝트는 같이 쓰고 다른 부분만 구현해서 빌드가 가능하다.


#### 3. 빠른 빌드 속도
**점진적 빌드**
- 마지막 빌드 후에 변경된 부분만 빌드
- 변경되지 않은 부분은 캐시 결과를 검색해 재사용
- 태스크 입출력 혹은 변경하지 않은 부분은 빌드하지 않음

**빌드 캐시**
- 라이브러리 의존성을 캐시로 저장한 후에 이전에 다운로드한 라이브러리를 재사용한다

**데몬 프로세스**
- 다음 빌드 작업을 위해 백그라운드에 대기하는 프로세스
- Gradle로 빌드한 결과물을 메모리에 보관한다
- 이로 인해 한번 빌드된 프로젝트는 다음에 빌드될 때 매우 적은 시간이 소요된다.


### 지원되는 언어 및 프레임워크
![[Pasted image 20231005181313.png]]


### 호환되는 IDE
![[Pasted image 20231005181345.png]]




## 질문 & 확장

- 데몬 프로세스는 백그라운드 프로세스로 forward 프로세스와 다르게 background에서 실행되는 프로세스라 빌드 결과물을 캐싱할 수 있는 거겠지?
- 빌드와 컴파일에 대해 헷갈리면 공부하는게 좋을 것 같아
## 출처(링크)
- https://gradle.org/
- https://docs.gradle.org/current/userguide/userguide.html?_gl=1*15v27hw*_ga*MTk1ODI1ODcyMy4xNjk2NDk2MDgx*_ga_7W7NC6YNPT*MTY5NjQ5NjA4MS4xLjEuMTY5NjQ5NjM3Ni4xOS4wLjA.#why_gradle
- https://www.youtube.com/watch?v=ntOH2bWLWQs
- https://www.youtube.com/watch?v=V4knLFDG-ZM
## 연결 노트

- [[컴파일(compile)과 빌드(build) 개념]]
- [[빌드 관리 도구 필요성]]
- [[Maven vs Gradle]]




