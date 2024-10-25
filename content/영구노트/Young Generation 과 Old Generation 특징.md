---
tags:
  - 완성
  - JAVA
  - GC
aliases: 
title: Young Generation 과 Old Generation 특징
date: 2023-10-11
---
작성 날짜: 2023-10-11
작성 시간: 17:26


----
## 내용(Content)
### Young 영역과 Old 영역 특징

JVM의 Heap 영역에는 처음 설계될 때, 다음의 2가지를 전제(Weak Generation Hypothesis)로 설계되었다.

- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
- 오래된 객체에서 새로운 객체 참조는 매우 적게 존재한다.

대부분의 **객체는 일회성이며, 메모리에 오랫동안 남는 경우는 드물다.** 이 가설 조건을 최대한으로 활용하기 위해서 HotSpot VM에서는 크게 2개로 물리적 공간을 나눴다. 둘로 나눈 공간이 Young과 Old 영역이다. 

![[Pasted image 20231011171224.png]]

- Young 영역(Young Generation):
	- 새롭게 생성된 객체가 할당되는 영역이다.
	- 대부분의 객체는 금방 Unreachable한 상태가 되기 때문에 많은 객체가 Young Area에서 사라진다.
	- Young 영역에서 발생하는 가비지 컬렉션을 Minor GC라 부른다.

- Old 영역(Old Generation)
	- Young 영역에서 Rechable한 상태를 유지하고 살아남은 객체가 복사되는 영역이다.
	- Young 영역보다 크게 할당되기 때문에 GC가 적게 발생한다.(GC는 공간에 데이터가 가득차면 발생)
	- Old 영역에서 일어나는 가비지 컬렉션을 Major GC라고 부른다.

Old 영역이 Young 영역보다 크게 할당되는 이유는 큰 데이터들은 바로 Old에 할당되기 때문이다.
Young 영역에는 수명이 짧은 객체만 할당되기 때문에 크게 할당될 이유가 없다.

그러나 Old 영역의 객체가 Young 영역의 객체를 참조하는 경우가 있을 수 있는데 이런 경우를 대비해서 Old 영역에는 512 byte의 Chunk(덩어리)로 되어 있는 카드 테이블을 제공한다.

![[Pasted image 20231011172103.png]]
이런 테이블이 도입된 이유는 간단하다. young 객체가 GC될 때 참조된 객체인지 판별하려고 모든 Old 영역의 객체를 탐색하는 것은 비효율적이기 때문이다. index 역할을 한다고 보면 된다.


## 질문 & 확장

(없음)

## 출처(링크)
- https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html
- https://mangkyu.tistory.com/118
- https://d2.naver.com/helloworld/329631
- https://d2.naver.com/helloworld/1329

## 연결 노트
- [[Minor GC(Young) vs Major GC(Old)]]









