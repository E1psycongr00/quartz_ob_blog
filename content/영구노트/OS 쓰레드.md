---
tags:
  - 완성
  - OS
  - Thread
aliases:
  - 커널 쓰레드
  - 커널 레벨 쓰레드
  - Kernel Level Thread
  - KLT
title: OS 쓰레드
date: 2024-01-21
---
작성 날짜: 2024-01-21
작성 시간: 23:30


----
## 내용(Content)
### 커널 레벨 스레드(Kernel Level Thread)
>[!summary] 커널 쓰레드 == OS 쓰레드
>- OS 커널 레벨 에서 생성되고 관리되는 쓰레드이다.  
>- CPU에서 실행 단위이며 CPU 스케줄링을 바로 이 OS 쓰레드를 활용한다.


커널 스레드는 OS에서 지원하는 스레드 기능으로 구현된다. 커널이 스레드의 생성 및 스케줄링을 관리한다. 스레드가 시스템 호출로 중단되더라도, 커널은 프로세스 내의 다른 스레드를 중단시키지 않고 계속 실행시킨다. 그러나 사용자 스레드에 비해 느리다.

<br></br>

>[!note] OS 쓰레드가 느린 이유
>컨텍스트 스위칭 시에 여러 커널 코드가 CPU에서 실행되기 때문에 CPU 리소스를 잡아 먹는다.  그리고 다시 커널 모드에서 User 모드로 전환되는 데 이런 비용 때문에 느리다. 


## 질문 & 확장

(없음)

## 출처(링크)
- https://kspsd.tistory.com/50
- https://www.youtube.com/watch?v=vorIqiLM7jc&t=326s
- https://velog.io/@khsb2012/go-goroutine#%EA%B3%A0%EB%A3%A8%ED%8B%B4%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%8B%A4%ED%96%89%EB%90%A0%EA%B9%8C
- [[ Linux Kernel ] 12. Kernel Thread(커널 스레드) (tistory.com)](https://coder-in-war.tistory.com/entry/Embedded-19-Linux-Kernel-Kernel-Thread%EC%BB%A4%EB%84%90-%EC%8A%A4%EB%A0%88%EB%93%9C)

## 연결 노트

- [[유저 쓰레드]]
- [[Kernel Level Thread vs User Level Thread]]
- [[인터럽트와 시스템 콜 동작 과정]]
- [[Process와 컨텍스트 스위칭|Process Context Switching]]