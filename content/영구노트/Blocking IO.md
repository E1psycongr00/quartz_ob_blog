---
tags:
  - 완성
  - OS
  - 네트워크
aliases:
  - Synchronous Blocking IO
title: Blocking IO
date: 224-01-26
---
작성 날짜: 2024-01-26
작성 시간: 17:12


----
## 내용(Content)
### I/O
>[!summary] I/O
>INPUT/OUTPUT, 데이터의 입출력의 의미를 가진다

**IO의 종류**
- 네트워크(socket)
- file
- pipe
- device

### Socket
>[!summary]- 소켓
>네트워크 통신은 두 컴퓨터 간의 소켓을 통해 이루어진다. 양측 모두 소켓을 열어야 통신이 가능하다.
>
>![[네트워크 소켓 통신(draw).svg|400]]

>[!summary]- 백엔드 서버의 소켓
>백엔드 서버에서는 네트워크 통신을 위해 여러 소켓이 존재하고 소켓을 열어야 다른 서버의 소켓을 통해 통신이 가능하다
>![[백엔드 서버 소켓(draw).svg|400]]

### Blocking IO 
>[!summar] Blocking IO == Synchronous Blocking IO
>I/O 작업을 요청한 프로세스/스레드가 요청 완료 전까지 Block됨
>
>![[Synchronous Blocking IO 과정(draw).svg|600]]

위의 그림과 같인 **Read**라는 시스템 콜을 호출하면 응답 전까지 Block되고 쓰레드는 아무런 역할을 하지 않고 Block 된다.

소켓 입장에서는 buffer의 상태에 따라 read 또는 write라는 시스템 콜을 요청한 쓰레드가 Blocked 될 수 있다. 소켓에는 sendBuffer와 receiveBuffer가 있는데 받는 소켓의 경우에는 recv 버퍼가 비어있다면 계속 대기하고, 소켓에 쓰는 요청을 보냈을 경우 writeBuffer가 가득 차있다면 대기한다.

>[!caution] 네트워크 통신에서 Blocking 모델의 한계
>Blocking IO는 프로세스가 시스템 콜을 호출하고 응답까지 대기하는데 그 동안 해당 쓰레드는 어떤 일도 처리할 수 없다.  application이 멈추지 않고 동시에 동작하기 위해서는 여러 쓰레드를 생성해야 되는데 쓰레드가 많아지면 Context Switching 횟수가 증가하고 효율이 떨어진다. 



## 질문 & 확장

(없음)

## 출처(링크)
- https://www.youtube.com/watch?v=mb-QHxVfmcs
- https://blog.naver.com/n_cloudplatform/222189669084
- https://velog.io/@octo__/BlockingNon-Blocking-IO-IO-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%86%B5%EC%A7%80-%EB%AA%A8%EB%8D%B8
## 연결 노트
- [[Non-Blocking IO]]









