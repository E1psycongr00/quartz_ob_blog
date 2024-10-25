---
tags:
  - 완성
  - OS
  - CS
  - Synchronization
aliases:
  - Monitor
  - 모니터
title: Monitor 동기화 메커니즘
date: 2024-01-20
---
작성 날짜: 2024-01-20
작성 시간: 15:31


----
## 내용(Content)
### Monitor 소개
>[!summary] Monitor란
>Monotor는 mutual exclusion과 cooperation을 가진 동기화 메커니즘이다


>[!info] cooperation
>wait-set을 사용하여 스레드가 특정 조건을 충족할 때까지 기다리는 기술

[[세마포어|Semaphore]]와 비교해서 설명하자면 동기화를 위해 Semaphore를 사용하면 쓰레드 허용 갯수등 여러 부분에서 자유롭게 쓰레드 동기화를 조절할 수 있다. 그러나 자유도가 높다는 것은 큰 단점도 존재하는데 우선 여러 쓰레드를 허용 가능하기 때문에 잘못 사용하면 mutual exclusion을 충족하지 못해 의도치 않은 동기화 문제가 발생할 수 있다.

이에 Monitor는 객체 단위로 구성된 프로그램으로 공유 데이터 멤버 변수와 그 변수들을 조작할 수 있는 함수들을 포함한다. 이를 통해 모니터 내에서는 한 번에 하나의 쓰레드만이 접근 가능하기 떄문에 semaphore를 활용할 때와 다르게 직접 동기화 제약 조건을 명시적으로 작성할 필요가 없기 때문에 간단하게 이용할 수 있다.

### Monitor 구성요소
- mutex
- condition variable

>[!info] Condition Variable(조건 변수)
>조건이 충족되길 기다리는 쓰레드들이 대기 상태로 머무는 곳이다. 조건이 충족되면 쓰레드를 깨우고 그렇지 않으면 대기 상태로 만들어 보관한다.
>![[Pasted image 20240120162424.png]]
>이는 자바의 모니터의 한 예로 위 그림을 보면 조건 변수의 의미를 이해 가능하다

### 모니터의 구현
```text
acquire(m);      // 모니터 락 취득
while(!p) {      // 조건 확인
	wait(m, cv); // 조건이 충족되지 않으면 wait
}

// ~~~ other code

signal(cv2);     // 또는 broadcast(cv2) cv1 == cv2 일 수 있다
release(m);      // 모니터 락 반환
```

>[!tip]- code 설명
>1. acquire를 통해 락을 취득한다는 것은 monitor의 상호배제 특성을 위해서 그렇다. release 역시 마찬가지다
>2. if 문 대신에 while문을 이용 조건을 확인하는 이유는 [[Spurious Wake up]] 문제가 발생할 수 있기 때문이다. 조건에 맞지 않게 깨어난 쓰레드를 계속해서 조건을 확인시켜줄 필요가 있기 떄문에 while문 사용한다.
>3. signal은 쓰레드 동작이 끝나고 이제 남은 대기 큐에 있는 쓰레드를 깨우기 위해 사용된다.
>4. 1번 설명과 같다



## 질문 & 확장

(없음)

## 출처(링크)
- https://www.baeldung.com/cs/monitor
- https://hyunah-home.tistory.com/entry/Monitor%EB%9E%80
- https://www.youtube.com/watch?v=Dms1oBmRAlo&t=83s
- https://coding-start.tistory.com/201
- https://www.youtube.com/watch?v=yWprp019_n4
## 연결 노트
- [[세마포어]]
- [[Java Monitor]]









