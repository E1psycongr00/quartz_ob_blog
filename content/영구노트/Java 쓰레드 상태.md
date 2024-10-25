---
tags:
  - 완성
  - JAVA
  - Thread
aliases:
  - Java Thread State
title: Java 쓰레드 상태
date: 2024-01-19
---
작성 날짜: 2024-01-19
작성 시간: 11:51


----
## 내용(Content)
### Java Thread State 종류
자바 쓰레드에는 크게 4가지 종류의 쓰레드 상태가 존재한다

- 객체 쓰레드 생성
- 실행 대기
- 일시 정지
- 종료

그리고 4개에서 일시 정지 상태를 세부적으로 나누면 6가지의 상태가 존재하게 된다

```java
public enum State {
	NEW,
	RUNNABLE,
	BLOCKED,
	WATING,
	TIMED_WATING,
	TERMINATED
}
```

| 상태        | 상수         | 역할                                                                                    |
| ----------- | ------------ | --------------------------------------------------------------------------------------- |
| 쓰레드 생성 | NEW          | 쓰레드가 생성되었으나 시작되지 않은 상태                                                |
| 실행 대기   | RUNNABLE     | JVM에서 실행 중인 쓰레드지만 아직 OS로부터 프로세서 자원을 할당 받지 못해 기다리는 상태 |
| 일시 정지   | BLOCKED      | 모니터 잠금을 기다리며 차단된 상태                                               |
|             | WATING       | 다른 스레드가 특정 작업을 수행하기를 무한정 기다리는 상태                                                                                        |
|             | TIMED_WATING | 지정된 대기 시간이 있는 쓰레드                                                                                        |
| 종료            | TERMINATED             | 쓰레드의 실행이 완료되고 종료된 쓰레드                                                                                        |

>[!tip] WATING
>Object.wait나 Thread.join시 해당 상태가 된다. wait의 경우 notify에 의해 깨어나길 희망하고 있으며, join의 경우 쓰레드가 종료되기를 기다린다.


### Java Thread 상태 전이도
![[쓰레드 상태 전이도(draw).svg|700]]


## 질문 & 확장

(없음)

## 출처(링크)
- https://jongwoon.tistory.com/14
- https://m.blog.naver.com/qbxlvnf11/220921178603

## 연결 노트
- [[JVM 구조]]








