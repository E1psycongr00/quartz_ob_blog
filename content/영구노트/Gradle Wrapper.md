---
tags:
  - 완성
  - Gradle
  - 빌드
aliases: 
title: Gradle Wrapper
date: 2023-10-16
---
작성 날짜: 2023-10-16
작성 시간: 21:07


----
## 내용(Content)

Gradle을 이용해 build할 때 가장 추천하는 방법은 Gradle Wrapper를 이용하는 것이다. 
Wrapper는 선언된 버전을 호출하고 필요한 경우 미리 다운로드하는 스크립트이기 때문이다. 결과적으로 개발자는 Gradle 프로젝트를 신속하게 시작하고 실행할 수 있다.

![[Pasted image 20231016211159.png]]

Gradle Wrappe를 이용해 얻을 수 있는 이점
- 보다 안정적이고 강력한 빌드를 위해 특정 Gradle 버전에서 프로젝트를 표준화한다.
- 다양한 사용자를 위해 `Gradle 버전 프로비저닝`은 간단한 Wrapper 정의 버전으로 수정된다.
- 다양한 실행환경(IDE)을 위한 `Gradle 버전 프로비저닝`은 간단한 Wrapper 정의 변경으로 쉽게 수행 가능하다.

> **버전 프로비저닝**
> 특정 소프트웨어, 애플리케이션 또는 시스템의 특정 버전을 설정하고 관리하는 프로세스를 의미
> 소프트웨어 개발 및 관리에서 중요한 역할을 수행하며, 새로운 버전의 소프트웨어를 배포하고 업데이트를 수행하며, 이전 버전의 호환성을 유지하거나 특정 버전을 선택적으로 사용하는 방법을 포함한다.

## 질문 & 확장

(없음)

## 출처(링크)
- https://docs.gradle.org/current/userguide/gradle_wrapper.html
- https://junilhwang.github.io/TIL/Gradle/GradleWrapper/#build-gradle-%E1%84%8C%E1%85%A1%E1%86%A8%E1%84%89%E1%85%A5%E1%86%BC

## 연결 노트










