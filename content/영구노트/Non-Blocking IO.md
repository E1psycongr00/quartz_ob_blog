---
tags:
  - 완성
  - OS
  - 네트워크
aliases:
  - Synchronous non-blocking IO
title: Non-Blocking IO
date: 2024-01-28
---
작성 날짜: 2024-01-28
작성 시간: 09:25


----
## 내용(Content)
### Non-Block IO
>[!summary] Non-Block IO == Synchronous non-blocking IO
>프로세스/스레드를 Block 시키지 않고 요청에 대한 현재 상태를 즉시 리턴해서 User Process가 이어서 다른 일을 수행할 수 있는 메커니즘
>
>![[non-blocking Io(draw).svg|600]]

Blocked IO는 [[Kernel]]에서 read 동작시 [[시스템 콜]]을 요청한 Thread가 Block되지만 Non-Blocked IO의 경우 Block되지 않고 즉시 현재 상태를 리턴 후 Thread는 계속해서 동작한다. 차이점은 어떤 방법에 의해서 response을 응답 받은 사실을 알아내고 그 이후 다시 [[시스템 콜]]을 요청해 커널로부터 데이터를 받아온다.

커널이 IO 작업이 끝났는지 확인하는 작업은 지속적으로 확인한다(polling).

>[!note] 커널의 현재 상태 반환
>시스템 콜 요청 작업을 완료하지 못했다면 -1을 반환하고 에러 코드를 같이 반환한다.
>이 때 에러코드는 일반적으로 (EGAIN, EWOULDBLOCK)을 반환한다.

>[!caution] 폴링의 한계
>폴링은 주기적으로 시스템 콜을 계속해서 요청하는 작업이다. 이것의 문제점은 주기가 너무 짧다면 의미 없는 커널 응답 반환에 리소스를 낭비하게 되고 폴링 주기가 너무 길다면 실제 데이터는 준비되었는데 사용하지 못하는 응답시간에 문제가 생길 수 있다.

>[!caution] 네트워크 통신에서 non-blocking IO 모델의 한계
>[[Blocking IO]] 모델이나 non-blocking IO 모델이나 문제점은 동기적으로 동작한다는 점이다. IO 처리 동안 다른 작업을 처리할 수 있어서 Blocking IO 보다 리소스를 효율적으로 사용할 수 있어도 결국엔  필요한 데이터를 받아야 하기 때문에 동기화 과정에서 대기하게 되고 여전히 쓰레드가 동작하지 않을 수 있다. 그렇기에 요청마다 매번 새로운 프로세스/쓰레드를 생성하게 되고 IPC/동기화 문제([[뮤텍스(Mutex)|뮤텍스]], [[세마포어]]) 등 복잡한 동기화 이슈를 발생할 수 있다.

## 질문 & 확장

(없음)

## 출처(링크)
- https://www.youtube.com/watch?v=mb-QHxVfmcs
- https://blog.naver.com/n_cloudplatform/222189669084
- https://blog.naver.com/n_cloudplatform/222189669084
## 연결 노트
- [[시스템 콜]]
- [[인터럽트와 시스템 콜 동작 과정]]
- [[Kernel|커널]]
- [[IO Multiplexing]]
- [[Blocking IO]]
- [[뮤텍스(Mutex)|뮤텍스]]
- [[세마포어]]







