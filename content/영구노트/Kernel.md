---
tags:
  - 완성
  - OS
aliases:
  - 커널
title: Kernel
date: 2024-01-25
---
작성 날짜: 2024-01-25
작성 시간: 12:25


----
## 내용(Content)
### 커널

>[!summary]
>커널은 운영체제의 핵심으로써 수행에 필요한 여러가지 서비스를 제공하고, CPU, 메모리 등의 리소스를 관리하는 역할을 수행한다.
>- 사용자가 SystemCall을 통해 컴퓨터 자원을 사용할수 있게 해주는 자원 관리자라고 할 수 있다.


커널은 시스템 자원 관리를 유저가 하지 못하게 막음으로써 시스템을 보호한다.

### 커널의 자원 관리
**커널이 관리하는 자원**
- Task(Process) Management: CPU를 추상적 자원 Task로 제공
- Memory Management: 메모리를 추상적 자원인 Page와 Segment로 제공
- File System: 디스크를 추상적 자원인 추상적 자원인 File로 제공
- Network Management: Network 장치를 추상적 자원인 Socket으로 제공
- Device Driver Management: 각종 외부 장치 접근 
- Interrupt Handling: 인터럽트 핸들러
- I/O Communication: 입출력 통신 관리



### 유저 모드와 커널 모드의 관계

![[user mode kernel mode interface(draw).svg|600]]

>[!note] 동작 과정
>1. 유저 모드에서 application을 실행한다
>2. 프로그램 실행 중에 인터럽트가 발생하거나 시스템 콜을 호출하면 user -> kernel
>3. 커널 모드에서 프로그램의 현재 CPU 상태를 저장함
>4. 커널이 [[인터럽트]]나 시스템 콜을 직접 처리 ==> CPU에서 커널 코드가 실행됨
>5. 커널은 처리가 완료되면 중단됬던 프로그램의 CPU 상태 복원
>6. 통제권을 다시 프로그램에게 반환 ==> kernel -> user

>[!note] 유저 모드
>유저 모드는 접근을 제한해두고 프로그램 자원에 함부로 침범하지 못하도록 해둔 mode이다. 간단하게 유저가 Application을 실행하는 것은 user mode 에서 일어난다고 이해하면 된다

## 질문 & 확장

(없음)

## 출처(링크)
- https://minkwon4.tistory.com/295
- https://www.youtube.com/watch?v=v30ilCpITnY&t=37s
## 연결 노트
- [[인터럽트]]
- [[시스템 콜]]
- [[인터럽트와 시스템 콜 동작 과정]]
- [[Process와 컨텍스트 스위칭]]








