---
tags:
  - 완성
  - OS
  - Process
  - CPU
  - Synchronization
aliases:
  - 데드락
  - 교착상태
title: DeadLock
date: 2024-01-22
---
작성 날짜: 2024-01-22
작성 시간: 14:15


----
## 내용(Content)
### 데드락 정의
>[!summary] Dead Lock
>두 개이상의 프로세스 혹은 쓰레드가 서로가 가진 리소스를 기다리는 상태 
### 데드락의 발생 조건
>[!summary] 데드락의 4가지 조건
>1. Mutual exclusion
>2. Hold and Wait
>3. No preemption
>4. Circular Wait

>[!note] Mutual exclusion
>상호 배제, 하나의 프로세스 또는 쓰레드만 해당 자원에 접근 가능도록 하는 것

>[!note] Hold And Wait
>프로세스가 이미 하나의 리소스를 취득한 상태(Hold)에서 다른 프로세스가 사용하고 있는 리소스를 추가로 기다린다.

>[!note] No Preemption
>리소스 반환은 그 리소스를 점유한 자만이 반환할 수 있다

>[!note] Circular Wait
>프로세스들이 순환하는 형태로 기다린다

### 그림을 통해 데드락 조건을 이해하기
![[Dead Lock 예시2(draw).svg|600]]

도로 교차점에 1,2,3,4 가 있다. 이 때 1,2,3,4 에는 오직 하나의 차만이 있을 수 있다. 이를 통해 **상호 배제 조건**이 완성됬다.

After를 보면 모든 차량이 이미 1,2,3,4위에 올라와 있다. 그리고 예를 들어 우측의 2번에 올라와 있는 차량이 1번으로 가기 위해 1번 차량이 비키길 기다리고 있다. 이러한 상황은** Hold And Wait**이다.

차들이 기다리는데 문제는 그 자리를 점유하고 있는 차가 비켜야만 다른 차량이 지나갈 수 있다. 이러한 상태를** No Preemption**이라 한다.

모든 차들이 순환하는 형태로 대기한다. 이러한 작업 때문에 서로가 어떤 일도 하지 못한다. 이를 **Circular Wait**이라 한다.

이 4가지 조건이 모두 만족해야 교착 상태에 빠진다.


## 질문 & 확장

(없음)

## 출처(링크)
- https://www.youtube.com/watch?v=ESXCSNGFVto&list=PLcXyemr8ZeoQOtSUjwaer0VMJSMfa-9G-&index=7&t=2s

## 연결 노트
- [[Race Condition]]
- [[OS에서 데드락 문제 해결하기|OS에서 DeadLock 문제 해결하기]]
- [[프로그래밍으로 데드락 해결하기|Programming으로 DeadLock 해결하기]]








