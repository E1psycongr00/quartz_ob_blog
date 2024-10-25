---
tags:
  - 완성
  - OS
  - Process
  - Thread
  - Synchronization
aliases:
  - Spin Lock
  - 스핀 락
title: Spin Lock (스핀 락)
date: 2024-01-11
---
작성 날짜: 2024-01-11
작성 시간: 20:01


----
## 내용(Content)
### Spin Lock이란?
 Lock을 소유하고 있는 프로세스의 **Lock이 반환 될 때까지 대기 중인 다른 쓰레드가 계속 확인하고 기다리는 작업을 반복하는 것**을 SpinLock이라고 한다.

>[!info] Lock
>Lock이란 공유 자원을 특정 스레드가 사용하고 있을 때 다른 쓰레드는 해당 공유 자원에 접근하지 못하도록 막는 작업이다.

조금만 기달리면 바로 사용할 수 있는데 굳이 [[Process와 컨텍스트 스위칭|Process Context Switching]] 부하가 필요한가? 문제점을 바탕으로 만들어진 Lock으로 Critical Section에 진입이 불가능할 때 컨텍스트 스위칭을 하지 않고 잠시 루프를 돌리면서 재시도하는 것을 말한다.
### Spin Lock의 장/단점
>[!note] 장점
>- Lock의 획득이 빠르다.
>	- 지속해서 Lock를 무한루프를 돌면서 확인하기 때문이다.
>- Context Switching 비용을 줄일 수 있다.
>	- 문맥 교환 중엔 어떤 작업을 수행할 수 없다.

>[!note] 단점
>- Lock을 획득할 때까지 무한루프를 도는 작업 자체가 CPU 작업을 많이 잡아 먹는다.
>- busy waiting: 스핀락 획득을 위한 오버헤드(무한 루프를 돌면서 CPU 계속 사용)
>- Starvation: 특정 쓰레드가 오랜동안 해당 자원을 점유한다면 다른 쓰레드들이 계속해서 대기 상태에 빠질 수 있음


### Spin Lock의 적용

- 스레드나 프로세스의 경합 상황이 짧을 때 유용하다.(임계 구역에서 작업이 빠르게 이루어질 때)
	- 계속해서 락을 획득하기 위해서 무한루프를 돌며 확인하기 때문에 전환이 많다면 효과적이다.
- 여러 개의 CPU 코어가 존재할 때 용이함
	- 사용하지 않은 코어에서 스핀락을 통해 대기하다가 바로 락을 획득 가능하다.
- SpinLock 은 잘 사용하지 않는다.
- 단일 코어의 경우에는 스핀락은 위험하다.


### Spin Lock을 이해하기 위한 배경 지식

>[!info] Race Condition(경쟁 조건)
>여러 프로세스가 공통된 공유 자원을 활용할 때
>[[Race Condition]]

>[!info] Critical Section(임계 영역)
>여러 스레드 또는 프로세스가 공유 자원에 접근할 수 있는 코드 영역
>ex) Race Condition이 발생할 수 있는 곳으로 두 프로세스가 동시에 접근하면 안되는 구역
>```java
>public class Counter {
>	private int cnt;
>
>	//cnt에 접근하는 임계영역	
>	public synchronized void increase() {
>		cnt++;	
>	}
>}
>```





## 질문 & 확장

(없음)

## 출처(링크)
- https://hogwart-scholars.tistory.com/entry/OS-Spin-Lock-%EC%8A%A4%ED%95%80%EB%9D%BD%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90#Critical%20Section%20(%EC%9E%84%EA%B3%84%20%EC%98%81%EC%97%AD)-1

## 연결 노트










