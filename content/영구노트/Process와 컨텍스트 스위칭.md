---
tags:
  - 완성
  - OS
  - Process
  - Thread
aliases:
  - Process Context Switching
  - PCS
title: Process와 컨텍스트 스위칭
date: 2024-01-05
---
작성 날짜: 2024-01-05
작성 시간: 20:08


----
## 내용(Content)
### Process Context Switching
멀티 태스킹시 일어나는 현상이다. 하나의 코어에서 여러 프로세스를 실행하려면 다른 프로세스로 전환을 해야 한다. 이 일련의 과정을 프로세스 컨텍스트 스위칭이라 한다. [[CPU의 작업 처리 방식 (병렬성과 동시성)|CPU 동시성]]
과도 관련이 깊다.

### Process Context Switching 과정
![[Pasted image 20240110175922.png]]

1. CPU는 P1을 실행한다.
2. Interrupt 또는 System Call이 발생
	- P1 상태를 PCB1에 저장한다.
	- PCB2로 부터 P2 상태를 로드한다.
3. P2를 실행한다.
4. Interrupt 또는 System Call이 발생
	- P2 상태를 PCB2에 저장한다
	- PCB1으로 부터 P1 상태를 로드한다
5. P1을 실행한다

>[!info] idle & executing
>CPU 동작 상태를 의미한다
>idle ==> 대기
>executing ==> 실행
>

>[!info] PCB
>[[PCB(Process Controll Block)|PCB]]는 프로세스에 대한 정보를 가지고 있는 임시 저장소이다. 프로세스 생성시 생성되며 수명을 함께 한다. 


### OverHead
컨텍스트 스위칭은 빠른 동시성을 제공하지만, 실행되는 프로세스의 변경 과정에서 프로세스 상태나 여러 정보들을 저장하고 불러오는 작업 때문에 프로세스 작업 외에 추가로 CPU의 자원을 소모한다.

프로세스 컨텍스트 스위칭은 다음과 같은 부분에서 오버헤드가 발생한다.

![[PCS 오버헤드(draw).svg|700]]


## 질문 & 확장

(없음)

## 출처(링크)
- https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%E2%9A%94%EF%B8%8F-%EC%93%B0%EB%A0%88%EB%93%9C-%EC%B0%A8%EC%9D%B4#%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%9D%98_%EC%9E%90%EC%9B%90_%EA%B5%AC%EC%A1%B0


## 연결 노트
- [[PCB(Process Controll Block)]]
- [[CPU의 작업 처리 방식 (병렬성과 동시성)]]
- [[Kernel]]
- [[인터럽트와 시스템 콜 동작 과정]]






