---
tags:
  - 완성
  - OS
  - Synchronization
aliases:
  - 뮤텍스
  - Mutex
title: 뮤텍스(Mutex)
date: 2024-01-11
---
작성 날짜: 2024-01-11
작성 시간: 21:47


----
## 내용(Content)
### 뮤텍스의 의미
Mutex는 Mutual(상호간의) + Exclusion(제외, 배제)의 합성어이다. 단어 의미 그대로 **공유 자원(Critical Section)에 한번에 하나의 프로세스만 접근하도록 하는 동기화 메커니즘**이다.

이렇게 하는 이유는 공유 자원을 여럿이 접근할 때 발생하는 [[Race Condition]] 상황을 방지하기 위해서 사용한다.


### 뮤텍스 동작 방식
1. Shared Resource에 접근하려면 Mutex Lock을 획득해야 한다.(Lock을 소유하는 주체는 실행중인 Process 또는 Thread 이다)
2. 이미 다른 프로세스/스레드가 사용중이면 Lock이 되고, unlock 될 때까지 다른 프로세스/스레드의 접근을 막는다.
3. 리소스의 사용이 끝나면 unlock 상태로 전환되고, 다른 프로세스가 접근이 가능하다.(Lock 획득)


## 질문 & 확장

(없음)

## 출처(링크)
- https://taeyoungcoding.tistory.com/346#:~:text=Mutex%EB%9E%80%20'Mutual%20Exclusion'%EC%9D%98%20%EC%95%BD%EC%9E%90%EB%A1%9C%2C%20%ED%95%9C%20%EB%B2%88%EC%97%90%20%ED%95%98%EB%82%98%EC%9D%98,%ED%95%A0%20%EB%95%8C%20%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94%20%EA%B2%BD%EC%9F%81%20%EC%A1%B0%EA%B1%B4%EC%9D%84%20%EB%B0%A9%EC%A7%80%ED%95%98%EB%8A%94%EB%8D%B0%20%EC%82%AC%EC%9A%A9%EB%90%9C%EB%8B%A4.


## 연결 노트
- [[Spin Lock (스핀 락)|Spin Lock]]
- [[Java와 함께하는 Mutex 사용 예제]]
- [[Mutex와 binary semaphore 같지 않다]]










