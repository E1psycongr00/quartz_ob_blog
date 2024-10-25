---
tags:
  - 완성
  - Gradle
aliases: 
title: Gradle Build Life Cycle
date: 2023-10-17
---
작성 날짜: 2023-10-17
작성 시간: 20:22


----
## 내용(Content)

Gradle의 핵심은 의존성 기반의 프로그래밍 언어이다. Gradle의 용어로 이것은 작업과 작업 간의 종속성을 정의 할 수 있음을 의미한다.

Gradle은  종속성 순서대로 작업을 수행하고 단 한번만 작업이 수행함을 보장한다. 작업을 실행하기 이전에 종속성 그래프가 빌드된다.

이 때 빌드 스크립트는 종속성 그래프를 구현하게 된다. 

![[gradle life cycle(draw).svg]]

> **Initialization**
> Gradle은 멀티 프로젝트를 빌드를 지원한다. 초기화 단계에서 Gradle은 참여할 프로젝트를 결정하고 이러한 각 프로젝트들에 대한 Project 인스턴스를 생성한다.

> **Configuration**
> 초기화 단계에서 생성된 Project 인스턴스를 구성한다. 빌드에 참여하게 된 모든 프로젝트의 빌드 스크립트가 실행된다.

> **Execution**
> 구성 단계에서 생성된 Task의 하위 집합을 결정한다. 하위 집합은 현재 디렉토리와 Gradle 명령어에 넘겨진 인수에 의해 결정된다. 그리고 선택된 Task를 실행하게 된다.


### settings.gradle

gradle의 설정 파일 역할을 한다. 이 설정 파일은 build의 초기화 단계에서 실행된다. 멀티 프로젝트의 경우 빌드에 참여할 프로젝트를 정의해야 하기 때문에 root 프로젝트에 settings.gradle 파일이 반드시 있어야 한다. 반면 단일 프로젝트의 경우 이 설정 파일은 선택사항이 된다.


### doLast(), doFirst()

task를 정의할 때 doLast, doFirst를 한번쯤 본 사람들이 있을 것이다. doLast와 doFirst는 블럭 내부의 로직이 execution phase에 동작하도록 보장해준다. 

그래서 task 로직을 짤 때 라이프 사이클을 생각하면서 짜는 것이 좋다.

## 질문 & 확장

(없음)

## 출처(링크)

- https://monny.tistory.com/237


## 연결 노트
- [[Gradle의 doLast()를 언제 써야 할까]]









