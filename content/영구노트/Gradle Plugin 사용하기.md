---
tags:
  - 완성
  - Gradle
aliases: 
title: Gradle Plugin 사용하기
date: 2023-10-06
---
작성 날짜: 2023-10-06
작성 시간: 19:13


----
## 내용(Content)

Gradle은 의도적으로 자동화를 제공하지 않는다.  Java Code Compile과 같은 유용한 기능은 모두 플러그인에 의해 추가된다. 

### 플러그인의 기능

플러그인을 사용하면 프로젝트의 기능을 확장할 수 있다. 예를 들면 다음과 같은 작업을 수행 가능하다.

- Gradle 모델 확장
- 규칙에 따른 프로젝트 구성
- 특정 configuration 적용(add organizational repositories or enforce standards)

프로젝트 빌드 스크립트 대신에 플러그인을 사용하면 여러 이점을 또한 얻을 수 있다.

- 스크립트의 재사용성을 높이고 여러 프로젝트에 유사한 로직을 짜야하는 불필요함을 줄일 수 있다.
- 보다 높은 수준의 모듈화를 통해 유지/보수를 더 간단히 할 수 있다.
- command logic을 캡슐화하고 빌드 스크립트를 보다 명확하게 설계 가능하다.


### 플러그인 참고

Gradle 사용자를 위한 다양한 플러그인을 제공하는 [플러그인 개발자 커뮤니티](https://plugins.gradle.org/?_gl=1*px7vbx*_ga*MTk1ODI1ODcyMy4xNjk2NDk2MDgx*_ga_7W7NC6YNPT*MTY5NzQ1NTUwNC42LjEuMTY5NzQ2MDA5Mi4yOC4wLjA.) 참고



## 질문 & 확장

(없음)

## 출처(링크)
- https://docs.gradle.org/current/userguide/plugins.html

## 연결 노트
- [[Intellij와 Kotlin DSL과 함께하는 멀티 모듈 빌드해보기]]









