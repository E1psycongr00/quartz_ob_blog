---
tags:
  - 완성
  - OS
  - 네트워크
aliases:
  - Asynchronous Blocking IO
  - IO 멀티플렉싱
title: IO Multiplexing
date: 2024-01-28
---
작성 날짜: 2024-01-28
작성 시간: 10:17


----
## 내용(Content)
### IO 멀티플렉싱
>[!summary] IO multiplexing == Asynchronous Blocking IO
>하나의 프로세스가 관심있는 여러 IO 작업을 동시에 모니터링하고 한번에 처리하는 메커니즘
>![[IO multiplexing(draw).svg|600]]

멀티 플렉싱 방식은 동기적으로 동작한다. select() 함수 호출 시에 block되어 대기하고 kenel에서 결과 값이 완료되었다는 callback 신호가 오면 유저 프로세스는 자신의 Buffer로 해당 데이터를 복사해온다. 사실 내부적으로 select는 loop를 돌리며 대기하는 방식을 취한다. select()와 같은 함수를 이용해 여러 IO를 한번에 관리하기 때문에 Asynchronous한 방식으로 보지만 실제 I/O 동작은 Synchronous하기 때문에 개발자와 학자 사이에서도 이 주제에 대해 여러 토론을 한다.

>[!faq] 시스템 콜 요청에 대해서 곧바로 응답은 non-blocking 아닌가?
>맞는 말이다. 그러나 multiplexing은 multiplexing 함수를 호출한 시점에서 봐야 한다. 위 그림의 경우 select() 함수를 호출한 시점에 block이 되고 이 과정에서 멀티플렉싱에 대한 메인 처리를 수행하기 때문에 Blocking IO인 것이다.

>[!faq] read 요청에 대한 non-blocking 작업을 하는 이유
>select() 결과에 따라 system call을 호출해서 non-blocking 요소를 없앨 수 있다. 그러나 데이터가 **checksum** 문제로 폐기되는 일부 상황의 경우에는 select() 함수가 어떤 FD를 읽다가 Block이 될 수 있다. 이러한 상황에 대비해 데이터를 줄 수 없는 경우 errorcode를 즉시 리턴하도록 설계하여 안정성을 높일 수 있다.

>[!note] checksum
>데이터의 무결성을 검증하기 위해서 탐지하는 간단한 오류 탐지 기술. 일반적인 checksum 알고리즘에는 CRC, SHA-1, SHA-256, MD-5등이 있다. 만약 오류가 발생한 경우 재전송을 요청하거나 수정하는 등의 조치를 취한다.

### 멀티 플렉싱의 종류
IO 관점에서 멀티 플렉싱은 하나의 프로세스가 여러 파일을 관리하는 기법으로 설명할 수 있다. server-client 환경이라면 하나의 server에서 여러 socket 즉, 파일을 관리하여 여러 client가 접속할 수 있게 구성되기 때문이다.

프로세스에서 특정 파일에 접근할 때 [[파일 디스크립터]]를 사용하게 되는데, 해당 FD를 어떻게 활용하는지가 IO multiplexing의 핵심이 된다. 여기서 커널 응답에 대해 어떤 방식으로 대기하느냐에 따라 select, poll, epoll(linux), kqueue(bsd), iocp(windows)로 나뉜다.


>[!note] 파일
>프로세스가 [[Kernel|커널]]에 진입할 수 있도록 중간에 이어주는 역할을 하는 인터페이스

#### Select
target [[파일 디스크립터|FD]]를 배열에 쭉 넣어서 하나하나 순차 검색하는 방식이다. 따라서 FD가 늘어날수록 느려지며, 시간 복잡도는 $O(N)$ 이다. 고정된 단일 비트 테이블을 사용하여 관리할 수 있는 총 FD는 1024개로 제한되어 있다.

FD는 배열 형태의 구조체로 담고 있는 FD_SET 형태로 사용되는데 이 때 비트가 변경됨에 따라 FD의 변경을 감지하게 된다

![[Pasted image 20240128165823.png]]

그렇기에 FD_SET을 모두 탐색해야 데이터 변화를 알 수 있다.

#### Poll
Select에서는 관리할 수 있는 최대 FD 갯수가 1024개로 제한되어 있던 반면에 select와 달리 무한개의 FD를 검사할 수 있다. 그리고 매번 최대 갯수의 loop를 도는 Select와 달리 pool은 현재 FD의 갯수 nfds에 따라 loop를 돌 수 있도록 구현하여 select보다 효율적이다. 그러나 여전히 모두 순차 검색을 통해 FD의 변경상태를 확인하므로 여전히 $O(N)$ 의 시간 복잡도를 가지고 있기 때문에 FD와 큰 차이가 있지는 않다. FD가 무제한인 차이만 있을 뿐..

#### epoll(linux)
Linux kernel 2.5.44에서 처음 도입되었다. poll()과 같이 FD의 수는 무제한이지만 select, poll과 달리 kernel이 직접 FD를 관리하고 상태가 바뀌면 알려준다. 이 떄문에 루프를 돌 필요가 없고, 변화가 감지된 FD 갯수가 아닌 목록 자체를 반환해주기 때문에 대상 파일을 추가 탐색할 필요 없이 효과적이다. 

epoll은 2가지 방식이 존재한다.
- Level Triggered
	- ![[level triggered(draw).svg]]
	- 특정 준위 상태가 유지되는 동안 감지
	- 입력 buffer에 데이터가 남아있는 동안 계속해서 이벤트에 등록된다.
	- select, poll은 Level-Trigger만 지원
- Edge Triggered
	- ![[Edge Triggered(draw).svg]]
	- 특정 준위가 변화하는 시점에 감지
	- 입력 buffer가 데이터에 수신된 상황에만 한 번 이벤트를 등록한다. 이후 추가적인 설정은 하지 않는다.


## 질문 & 확장

(없음)

## 출처(링크)
- https://velog.io/@octo__/BlockingNon-Blocking-IO-IO-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%86%B5%EC%A7%80-%EB%AA%A8%EB%8D%B8
- https://blog.naver.com/n_cloudplatform/222189669084
- https://www.youtube.com/watch?v=mb-QHxVfmcs
## 연결 노트
- [[Non-Blocking IO]]
- [[Blocking IO]]
- [[운영 체제(OS)|운영체제]]
- [[Kernel]]
- [[파일 디스크립터]]






