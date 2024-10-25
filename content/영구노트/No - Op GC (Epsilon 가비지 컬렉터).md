---
tags:
  - 완성
  - JAVA
  - GC
aliases: 
title: No - Op GC (Epsilon 가비지 컬렉터)
date: 2023-10-04
---
작성 날짜: 2023-10-04
작성 시간: 13:22


----
## 내용(Content)

Java 11에서부터 가장 낮은 GC 오버헤드를 약속하는 Epsilon 이라는 No Op GC를 도입했다. 

```java
class MemoryPolluter { 
	static final int MEGABYTE_IN_BYTES = 1024 * 1024;
	static final int ITERATION_COUNT = 1024 * 10;
	
	static void main(String[] args) { 
		System.out.println("Starting pollution"); 
		for (int i = 0; i < ITERATION_COUNT; i++) { 
			byte[] array = new byte[MEGABYTE_IN_BYTES]; 
		} 
		System.out.println("Terminating");
	}
}
```

위 코드는 1 MB 배열을 생성하고 이를 10240번 반복하므로 10GB 메모리를 할당한다는 의미이다. 이 정도면 heap이 보관할 수 있는 메모리를 초과할 수 있다.

이를 표준 GC를 돌려서 확인하면 예외 발생 없이 잘 동작한다. 그러나 다음과 같은 명령어를 사용해보자

```
-XX:+UnlockExperimentalVMOptions -XX:+UseEpsilonGC
```

그럼 
```
Terminating due to java.lang.OutOfMemoryError: Java heap space
```

다음과 같은 예외가 발생할 것이다.

### Epsilon 동작 방식

[JEP318](https://openjdk.org/jeps/318) 을 살펴보면 Epsilon에 대한 설명은 다음과 같다.

> Epsilon은 메모리 할당을 처리하지만 실제 메모리 회수 메커니즘을 구현하지 않았습니다. 그래서 사용 가능한 Java Heap을 모두 사용하면 Application이 종료됩니다.


이것 때문에 Epsilon GC를 사용하면 Out Of Memory 에러가 발생하는 것이다.

### 메모리를 회수하지 못하는 GC를 사용하는 이유

- 성능 시험
- 메모리 압력 테스트
- VM 인터페이스 테스트
- 수명이 매우 짧은 직업
- 최종 지연 시간 개선
- 최종 처리량 개선

주로 우리가 만든 API를 제한된 heap 안에서 테스트하기 좋다. 실제 Application에서는 사용해서는 안되는 GC다.

## 질문 & 확장

(없음)

## 출처(링크)
- https://www.baeldung.com/jvm-epsilon-gc-garbage-collector

## 연결 노트
- [[Garbage Collection(가비지 컬렉션) 소개]]









