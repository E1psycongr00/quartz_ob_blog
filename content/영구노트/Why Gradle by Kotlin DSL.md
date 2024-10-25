---
tags:
  - 완성
  - Gradle
  - Kotlin
aliases: 
title: Why Gradle by Kotlin DSL
date: 2023-10-05
---

작성 날짜: 2023-10-05
작성 시간: 18:08


----
## 내용(Content)

### IDE를 통해 정적 분석 가능

정적 분석이 가능하다는 말은 실행 전에 문법 오류 및 자동 완성이 가능하다는 의미이다. IDE를 통해 정적 분석을 한다면 얻을 수 있는 이득은 다음과 같다.

- 코드 자동 완성
- 오류 코드 강조
- 빠른 문서 보기 기능
- 리팩터링

**제공하는 IDE**

| IDE            | 프로젝트 불러오기 | 문법 강조 | 편집 지원 |
| -------------- |:-----------------:|:---------:|:---------:|
| Intellij IDEA  |         O         |     O     |     O     |
| Android Studio |         O         |     O     |     O     |
| Eclipse IDE    |         O         |     O     |     X     |
| VS Code        |         O         |     O     |     X     |
| Visual Studio  |         O         |     X     |     X     |



### Groovy DSL과 Kotlin DSL 확장자 차이
Gradle은 Groovy DSL과 Kotlin DSL을 제공한다. 해당 스크립트 파일 확장자는 약간 다르다.

Groovy DSL
-  `.gradle` `build.gradle`, `settings.gradle`

Kotlin DSL
- `.gradle.kts` `build.gradle.kts`, `settings.gradle.kts`




## 질문 & 확장

(없음)

## 출처(링크)
- https://blog.jetbrains.com/ko/kotlin/2023/05/kotlin-dsl-is-the-default-for-new-gradle-builds/
- https://techblog.woowahan.com/2625/
- https://docs.gradle.org/current/userguide/kotlin_dsl.html#sec:plugins_resolution_strategy
## 연결 노트










