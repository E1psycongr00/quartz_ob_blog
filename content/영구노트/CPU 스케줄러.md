---
tags:
  - 완성
  - OS
  - Process
  - CPU
aliases:
  - CPU Scheduler
title: CPU 스케줄러
date: 2024-01-16
---
작성 날짜: 2024-01-16
작성 시간: 19:45


----
## 내용(Content)
### CPU Scheduler
CPU 스케줄러는 CPU에서 실행될 프로세스를 선택하는 역할을 수행한다.

![[프로세스 상태 전이도(draw).svg|700]]

자세히 알고 싶으면 [[프로세스 상태 전이]] 보자

조금 더 그림을 통해 자세히 설명하면 CPU 스케줄러는 Ready 상태의 프로세스가 들어있는 Ready Queue에서 다음에 실행해야 할 프로세스를 선택하는 역할을 수행한다.

### Dispatcher
ready에서 프로세스가 running 상태로 바뀌는 것을 Dispatch라고 하는데 이때 Dispatcher가 여러 역할을 수행한다.

- Context Switching
- Kernel -> User mode 전환

>[!summary] dispatcher
>선택된 프로세스에게 CPU를 할당하는 역할

### 선점/비선점 스케줄링
>[!note] 선점 스케줄링
>스케줄링이 적극적으로 개입해서 끝까지 끝나지 않았음에도 불구하고 강제로 프로세스 상태를 전환한다.
>
>특징
>- 적극적
>- 강제적
>- 빠른 응답성
>- 데이터 일관성 문제
>
>

>[!summary] 비선점 스케줄링
> 비선점 스케줄링 방식의 특징은 크게 3가지다
> - 신사적
> 	- OS 입장에서 CPU가 Thread를 끝까지 쓸 때까지 기다려줌
> - 협력적(Cooperative)
> 	- 프로세스가 스스로 양보해서 다른 프로세스를 실행하도록 함 => 프로그램의 협력으로 상태를 관리함
> - 느린 응답성
> 	- 신사적의 연장선으로서 CPU가 Thread를 끝까지 쓸때까지 기다리기 때문에 프로세스 시작은 했지만 실행되지 못하는 경우가 생긴다. 이것은 사용자 입장에서 느린 응답성으로 느낄 수 있다.

### 스케줄링 알고리즘
- FCFS(First Come First Serve)
- SJF(Shortest Job First)
- SRTF(Shortest Remaining Time First)
- Priority
- RR(Round Robin)
- MultiLevel Queue
## 질문 & 확장

(없음)

## 출처(링크)
- https://www.youtube.com/watch?v=LgEY4ghpTJI

## 연결 노트
- [[CPU 스케줄링 알고리즘]]









