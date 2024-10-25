---
tags:
  - 완성
  - OS
  - Synchronization
  - Process
  - CPU
aliases: 
title: Mutex와 binary semaphore 같지 않다
date: 2024-01-17
---
작성 날짜: 2024-01-17
작성 시간: 01:31


----
## 내용(Content)
기본이 궁금하다면 [[뮤텍스(Mutex)|뮤텍스]]와 [[세마포어]]를 참고하자.


### 공통점
Mutex와 Binary Semaphore는 둘 다 상호 배제 특성을 가진다.  Mutex의 경우 Lock/UnLock 이라는 2가지 상태를 통해 쓰레드의 접근을 허용할지 결정하고, Semaphore의 경우, binary라면 signal 메커니즘이 사실상 boolean과 같기 때문에 동일한 효과를 볼 수 있다.

### 차이점
Mutex와 Binary Semaphore의 차이점이 있다면 내부 동작 방식이 다르다. 

Mutex는  Priority Inheritance를 가지지만 Semaphore는 그런 특성을 가지지 않는다. 그 이유는 Mutex는 Lock을 소유한 자가 UnLock을 한다. 이 때문에 해당 쓰레드의 우선순위를 올려주면 Context Switching과 쓰레드가 대기 상태가 되는 횟수를 줄일 수 있다. 하지만 Semaphore의 경우 외부에 다른 쓰레드 task의 signal을 통해 semaphore 허용 쓰레드를 확보해야 하는데 이를 알 수 없기 때문에 우선순위를 조정한다고 큰 효과를 보긴 어렵기 때문이다.


>[!info] Priority Inheritance
>우선순위가 높은 T1 thread, 우선순위가 낮은 T2 thread가 있을 때 T2가 Lock을 소유하면 T1은 Block되므로 대기 해야 한다. 이러한 문제 때문에 Lock을 소유한 T2 쓰레드의 우선순위를 높여 먼저 실행시켜 불필요한 컨텍스트 스위칭이나 스레드 대기 상태를 줄일 수 있다. 이것을 **Priority Inheritance**라 한다.


### 결론
Mutex != Binary Semaphore

그러나 개념적으로는 동일하게 사용할 수 있다. 그래서 Mutex는 상호 배제만이 필요하다면 사용하고, 복잡한 task 순서 동기화가 필요하다면 Semaphore를 활용하자. 이 때 상호 배제로 binary semaphore를 사용할 수도 있다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://www.youtube.com/watch?v=gTkvX2Awj6g&t=807s

## 연결 노트
- [[뮤텍스(Mutex)|Mutex]]
- [[세마포어|Semaphore]]









