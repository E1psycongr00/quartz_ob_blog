---
tags:
  - 완성
  - OS
  - Process
  - Thread
  - Synchronization
aliases:
  - 경쟁 조건
title: Race Condition
date: 2024-01-10
---
작성 날짜: 2024-01-10
작성 시간: 19:45



----
## 내용(Content)
### 정의
여러 프로세스/스레드가 동시에 같은 데이터를 조작할 때 타이밍이나 접근 순서에 따라 결과가 달라질 수 있는 상황을 말한다

### race condition이 가지는 문제점
race condition은 3가지 문제에 직면한다.
- Mutual exclusion
- deadlock
- starvation

>[!info] Mutual exclusion(상호 배제)
>두 개 이상의 프로세스가 공용 데이터에 동시에 접근하는 것을 막는 방법


>[!info] deadlock
>상호 배제(동시에 공유 자원에 접근)시에 교착 상태에 빠질 수 있다.
>![[Dead Lock 예시(draw).svg|300]]

>[!info] 교착상태
>어떤 프로세스가 자원을 요청 했을 때 그 시각에 그 자원을 사용할 수 없는 상황이 발생할 수 있고 그 때는 프로세스가 대기 상태로 들어 간다. 이 때 대기 상태로 들어간 프로세스들이 실행 상태로 변경 될 수 없을 때 교착 상태라 한다.

>[!info] Starvation (기아 상태)
>특정 프로세스의 우선 순위가 낮아서 계속해서 CPU와 메모리로부터 자원을 할당 받지 못하는 문제
## 질문 & 확장

(없음)

## 출처(링크)
- https://www.youtube.com/watch?v=vp0Gckz3z64
- https://iredays.tistory.com/125
- https://ko.wikipedia.org/wiki/%EA%B5%90%EC%B0%A9_%EC%83%81%ED%83%9C
- https://velog.io/@woga1999/Starvation-%EA%B8%B0%EC%95%84-%EC%83%81%ED%83%9C-%EB%9E%80
## 연결 노트
- [[DeadLock]]









