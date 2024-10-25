---
tags:
  - 완성
  - JAVA
  - OS
  - Thread
aliases: 
date: 2024-04-24
title: Java Thread Pool
---
작성 날짜: 2024-04-24
작성 시간: 16:21


----
## 내용(Content)

### Java Thread Pool

>[!summary]
> 한번에 미리 쓰레드를 생성해두고 재활용하여 쓰레드 생성 소멸에 대한 오버헤드를 줄이고, 다수의 요청을 효율적으로 처리 가능

Thread 생성이 Process에 비해 가볍기는 하지만 매 번 여러 요청에 대해 쓰레드를 생성 소멸하는 것은 비용이 큰 일이다. 그 이유는 자바는 User Level Thread와 Kernel Level Thread가 1:1로 연결되어 있는 구조라 쓰레드를 위한 작업을 OS에 위임하게 되는데 OS에 동작을 위임하기 위해서 [[시스템 콜|System Call]]를 호출하고 유저 모드와 커널 모드를 오가는 비용과 생성, 소멸 비용이 꽤나 크게 발생하기 때문이다. 

이러한 퍼포먼스 문제 때문에 ThreadPool이 등장했다. 단어 의미 그대로 Thread Pool로 Thread를 미리 생성해서 Pool 안에서 재활용한다는 의미이다. System Call과 Context Switching은 여전히 발생하지만, 생성 및 수거 비용을 줄일 수 있다.

>[!info]
>자바에서 더욱더 효율적으로 쓰레드를 처리하기 위해 JDK21에서 Virtual Thread를 만들었다. CPU 버스트가 많은 작업은 효과적이지 않지만 IO 버스트가 많은 작업의 경우 효율적이다. 

### Thread Pool 원리

>[!summary]
>Thread Pool에 미리 많은 쓰레드를 만들어두고, 작업들을 Task Queue에서 관리하고, 사용하지 않는 쓰레드에 큐에서 꺼내서 적절하게 할당한다.

![[ThreadPool 동작원리(draw).svg]]

Application에서 Task 요청이 발생하면 Task Queue에 Task를 저장한다. 그리고 이를 Pool에서 비어있는 적절한 쓰레드에 Task를 할당한다.(이 과정에서 Context Switching 발생) 그리고 Task 작업이 완료되면 작업을 요청한 주체에게 결과를 알려준다.

### Java Thread Pool 사용법

java에서는 Thread Pool을 손쉽게 사용하기 위해 [[ExecutorService]]를 제공한다. 또는 필요 용도에 따라 **ForkJoinPool**을 사용하기도 한다. 

## 질문 & 확장

(없음)

## 출처(링크)

- [Thread pool - Wikipedia](https://en.wikipedia.org/wiki/Thread_pool)
- [[Java] Thread Pool 개념과 동작원리 (velog.io)](https://velog.io/@haero_kim/Java-Thread-Pool-%EA%B0%9C%EB%85%90%EA%B3%BC-%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC)
## 연결 노트

- [[OS 쓰레드|Kernel Level Thread]]
- [[유저 쓰레드|User Level Thread]]
- [[Kernel|커널]]








