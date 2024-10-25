---
tags:
  - 완성
  - OS
  - Go
  - Thread
aliases:
  - 고루틴
  - GoRoutine
title: GoRoutine 고루틴 소개
date: 2023-12-17
---
작성 날짜: 2023-12-17
작성 시간: 20:03


----
## 내용(Content)
### Golang

Go 언어는 뛰어난 동시성을 지원하는 언어로 꼽힌다. 다른 프로그램 언어에서는 쓰레드를 활용해서 동시성을 지원하지만 GoLang에서는 고루틴을 활용하여 동시성 프로그래밍을 개발한다.  고루틴에 대해서 알아보자

### 고루틴(Goroutine)이란

>[!info] goroutine by go
>“lightweight thread managed by the Go runtime”

Go 런타임에서 관리되는 경량 쓰레드이다. 경량 쓰레드 개념은 OS의 쓰레드 개념과 달리 논리적인 가상 쓰레드이며, 기존의 OS 쓰레드보다 가볍고 비동기적인 동시성 처리 작업을 위해 만들어졌다. Go 런타임시 쓰레드가 생성되며, Go 종료시 모두 소멸되기 때문에 Go Runtime 경량 쓰레드라고 불린다.

### 고루틴과 쓰레드의 차이점

| 분류     | 고루틴                                              | 스레드                              |
| -------- | --------------------------------------------------- | ----------------------------------- |
| 크기     | 매우 작은 메모리 크기(2kb)                          | 큰 메모리 크기(1MB)                 |
| 스케줄러 | Go Runtime Scheduler(M:N 모델)                      | OS 스케줄러(1:1)                    |
| 동시성   | 수천 ~ 수백만개의 고루틴 생성                       | 고루틴에 훨씬 적은 수의 스레드 생성 |
| 블로킹   | 다른 고루틴이 실행될 수 있도록 빠른 컨텍스트 스위칭 | 블로킹 연산 수행 도중, 해당 스레드 사용 불가                                    |

#### 적은 메모리 소비

고루틴에서는 많은 메모리가 필요로 하지 않는다.

![[Pasted image 20231217201103.png]]

고루틴 생성시 대략 2KB의 스택 메모리 공간만 필요하나 쓰레드의 경우에는 쓰레드 Guard Page 때문에 1MB 이상의 메모리 공간이 필요하다

>[!info] Guard page
>스레드와 스레드 간 메모리 보호 역할을 수행

이것이 의미하는 것은 고루틴은 서버 요청 하나당 하나의 고루틴을 생성하지만, 요청 1당 1쓰레드를 생성하는 다른 언어에 비해 Out of Memory가 발생할 확률이 매우 적다. Java나 C++에서는 이런 문제 때문에 Thread Pool을 활용하여 쓰레드 비용 문제를 해결하려고 노력한다.

#### 생성 소멸 비용
쓰레드는 OS에 Resource를 요청하고 다 사용하고 나면 반환해야 하기 때문에 큰 생성, 소멸 비용이 들어간다. Thread Pool을 활용해서 기존 쓰레드를 재활용하면 이런 문제를 해결할 수 있지만 고루틴의 경우 Go 런타임 내에서 보다 적은 생성, 소멸 비용이 든다. 

#### Context Switching 비용
컨텍스트 스위칭으로 인해 다른 프로세스 또는 스레드로 전환되어 실행될 때 비용이 발생한다.

PCB의 경우

|정보|설명|
|---|---|
|Process Id|프로세스의 고유 번호 (PID)|
|Process state|준비, 대기, 실행 등의 상태|
|Program counter|다음 실행될 프로세스의 포인터 (PC)|
|Register information|CPU 레지스터 관련 정보|
|Scheduling information|스케줄링 및 프로세스 우선 순위 정보|
|Memory related information|자원 할당 정보|
|Accounting information|CPU 사용 시간, 실제 사용 시간, 실행 ID 정보|
|Status information related to I/I|입출력 상태 정보|


TCB의 경우

| 정보                          | 설명                            |
| --------------------------- | ----------------------------- |
| Thread Id                   | 스레드의 고유 번호 (TID)              |
| Stack pointer               | 프로세스에서 스레드의 스택을 가르키는 포인터 (SP) |
| Program counter             | 프로세스에서 다음 실행될 스레드의 포인터 (PC)   |
| Thread state                | 준비, 대기, 실행 등의 스레드 상태          |
| Thread register information | 스레드의 레지스터 정보                  |
| PCB pointer                 | PCB에 대한 포인터                   |

고루틴의 경우

|정보|설명|
|---|---|
|Program counter|고루틴 인터럽트 후 복원하기 위한 프로그램 카운터(PC)로 고루틴 내부에 저장|
|Stack Pointer|고루틴의 스택을 가르키는 포인터 (SP)|
|DX|CPU 레지스터 중 [DX](https://hongci.tistory.com/21)|

TCB가 전달해야 할 정보가 PCB에 적지만 고루틴의 경우 Context Switching 전달하는 정보가 매우 적다. 쓰레드간 Context Switching 비용보다 저렴하기 때문에 부담이 적다.


### 고루틴의 스레드 활용
고루틴은 user-level 방식과 kernel-level 방식을 혼용해서 사용하고 있다. (M:N) 고루틴(User-lv) - OS 스레드(kernel lv)를 M:N으로 매핑하는 구조로 사용되어 Context Switching의 비용 문제와 멀티코어의 장점 모두 가지게 된다.

유저 레벨과 커널 레벨의 쓰레드에 대한 내용은 [[Kernel Level Thread vs User Level Thread]] 를 참고하자

## 질문 & 확장

(없음)

## 출처(링크)
- https://velog.io/@khsb2012/go-goroutine
- https://velog.io/@kineo2k/%EA%B3%A0%EB%A3%A8%ED%8B%B4%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81%EB%90%98%EB%8A%94%EA%B0%80#gmp-%EA%B5%AC%EC%A1%B0%EC%B2%B4
- https://velog.io/@snake7667/Go-%EA%B3%A0%EB%A3%A8%ED%8B%B4Goroutine%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C#%EA%B3%A0%EB%A3%A8%ED%8B%B4%EC%9D%B4-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81%EB%90%98%EB%8A%94%EA%B0%80

## 연결 노트
- [[Kernel Level Thread vs User Level Thread]]









