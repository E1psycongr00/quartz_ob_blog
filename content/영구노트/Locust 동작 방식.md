---
tags:
  - 완성
  - Locust
  - 부하테스트
aliases: 
title: Locust 동작 방식
date: 2023-12-01
---
작성 날짜: 2023-12-01
작성 시간: 17:24


----
## 내용(Content)
### 동작 과정

![[Pasted image 20231201172508.png]]
https://mungiyo.tistory.com/44 에서 가져온 이미지

Locust에서 부하테스트를 위해서 멀티 쓰레드 형식이 아닌 co-routine 방식을 사용한다.

>[!info] co-routine vs thread
>- 동시성 처리 측면에서 코루틴은 하나의 스레드 내에서 함수를 호출하기 때문에, context-switching 비용과 메모리를 절약할 수 있다.

크게 4가지를 설정한다.

1. Number of Users: task를 실행하는 User 클래스 인스턴스 수
2. Spwn rate: 초당 생성할 유저의 수
3. Host: 부하테스트 Host
4. RunTime: 테스트 실행 시간

### 결과 분석

- 특정 시점에서는 User가 늘어도 RPS가 늘지 않는다
	- task가 서버에 요청할 떄 response를 받지 않으면 task가 종료되지 않기 때문에 RT + wait_time 시간 만큼 blocking된다
- User가 늘어나도 RPS는 유지되고 RT가 증가되지 않는다면 부하 테스트를 실시하는 테스트 서버 측의 리소스 한계를 의심해야함
- RPS는 유지되고 RT가 증가하면 API서버 쪽에서 병목이 발생하는 것이 아닌지 의심해야 한다.

![[Pasted image 20231202185516.png]]


## 질문 & 확장
- 위의 부하 테스트를 상세 분석이 가능할까?

## 출처(링크)
- https://mungiyo.tistory.com/44

## 연결 노트
- [[부하 테스트 개념]]









