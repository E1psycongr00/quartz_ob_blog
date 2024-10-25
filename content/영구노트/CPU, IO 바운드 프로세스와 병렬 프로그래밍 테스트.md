---
tags:
  - 완성
  - OS
  - Process
  - Async
  - Thread
aliases:
  - io bound & cpu bound test
title: CPU, IO 바운드 프로세스와 병렬 프로그래밍 테스트
date: 2024-01-09
---
작성 날짜: 2024-01-09
작성 시간: 11:13


----

## 문제 & 원인
IO Bound 그리고 CPU Bound 프로세스에는 어떤 병렬 프로그래밍이 효율적일지 테스트해본다.

## 해결 방안
병렬 프로그래밍은 비교 기준은 4가지로 정했다.

1. Sync
2. ASync
3. Multi Processing
4. Multi Threading

python 3.11 기준으로 테스트를 진행했는데 진행 결과는 다음과 같다. [깃허브 참고](https://github.com/E1psycongr00/multi_process_test)

![[Pasted image 20240110163734.png]]

#### CPU Bound 분석
cpu_bound 작업에서는 Multi Processing이 굉장히 높은 효율을 가졌고, multi-threading, async, sync 3개는 비슷한 결과가 나왔다. 

Multi Processing에 좋은 결과를 얻은 이유는 간단하다. 만든 프로세스들을 각각의 코어에 할당했기 때문에 병렬적으로 처리했기 때문이다. 

Multi Thread 방식 또한 결과가 좋아야 한다. 각각의 Thread를 코어에 할당한다면 빠른 속도로 처리해야 하기 때문이다. 위 테스트에서는 python GIL 때문에 속도가 나오지 않은 것으로 보인다.

ASync의 경우는 전혀 이득을 얻을 수 없었다. 그 이유는 이벤트 루프 기반으로 동작하더라도 CPU 위주의 작업은 대부분이 코어에 할당해서 연산 처리를 해야 하므로 미뤄서 처리해도 큰 이득이 없기 때문이다. 

#### IO Bound 분석
IO Bound 작업에서는 Async가 굉장히 빠르게 동작했다. Sync의 경우가 가장 오래 걸렸으며, Multi-Thread, Multi Thread 방식은 비슷비슷하게 동작했다.

Async가 굉장히 빠르게 동작했다. ASync로 http 요청하는 라이브러리를 사용했는데, 내가 짠 코드에 비해 최적화 될 가능성이 크기 때문일 수도 있다.  이론적으로 ASync가 빠르게 동작한 이유는 싱글 쓰레드에서 스레드를 관리할 필요 없이 비동기적으로 빠르게 동작하기 때문이다. 컨텍스트 스위칭 비용이 없으며 IO Bound 작업은 외부에서 들어오는 입출력 처리이기 떄문에 계산할 필요가 없다. 그래서 비동기적으로 처리하면 빠른 결과를 얻을 수 있다.

Multi-Threading과 Multi-Processig도 좋은 결과를 얻었다. ASync 만큼은 아니다.


### 결론
CPU Bound에서는 Multi-Threading과 Multi-Processing이 효과적이다. 이 때 Thread 또는 Process의 갯수는 (코어 갯수 + 1) 개 정도가 적당하다. cpu에 많은 작업이 소요되기 때문에 너무 많은 쓰레드는 컨텍스트 스위칭 비용만 증가할 뿐이기 때문이다.

IO Bound에서는 ASync, Multi-Threading, Multi-Processing 방식이 효과적이다. ASync 방식의 장점은 이벤트 루프 방식으로 동작하고 오래 걸리는 작업은 미뤄두고 다른 작업을 처리한다. 이 방식은 CPU 연산을 거의 처리하지 않는 IO Bound 작업에 효과적이다. 

Multi Threading이나 Processing 방식도 좋지만 코어가 아닌 다른 부분에서 병목이 발생하기 때문에 Thread 생성 기준이 따로 없으며, CPU Bound 작업에 비해 복잡하다. 

>[!caution] 주의
요즘 이벤트 루프 기반 비동기 방식이 유행하는 데 그 이유는 IO Bound에서 효과적으로 처리할 수 있기 때문이다. CPU Bound에서는 효과적이지 않음을 기억하자  
## 질문 & 확장

(없음)

## 출처(링크)
- https://github.com/E1psycongr00/multi_process_test

## 연결 노트
