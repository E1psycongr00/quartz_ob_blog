---
tags:
  - 완성
  - 솔루션
  - OS
  - Thread
  - Process
  - Synchronization
aliases: 
title: Java와 함께하는 Semaphore 사용 예제
date: 2024-01-17
---
작성 날짜: 2024-01-17
작성 시간: 01:33


----

## 문제 & 원인
공급자 & 소비자 구조의 자료를 구현하려고 한다. 

- 입력과 출력 모두 여러 쓰레드 요청 가능하며, 동기화가 되어야 한다.
- 데이터 입출력은 원형 큐 형태로 동작해야 한다.

여러 쓰레드가 하나의 버퍼에서 동작하게 하려면 상호 배제가 필요하다. 그 이유는 여러 쓰레드가 쓰고 읽는 과정에서 [[Race Condition]]]은 원치 않은 결과를 얻을 수 있기 때문이다. 그리고 입력 쓰레드와 출력 쓰레드 task를 따로 만든다면 읽고 쓰는 과정에서 실행 순서에 따라 쓰레드를 Block해야 할 수 있다. 그렇기 때문에 세마 포어를 쓰는 것이 좋을 것이다.

## 해결 방안
### 세마포어 정의하기
쓰레드를 제어하기 위해서 3개의 세마 포어가 필요하다.
1. full 세마포어
	- element 갯수
	- 입력을 호출 -> release
1. empty 세마포어
	- 비어있는 슬롯 갯수
	- 출력 -> release
1. mutex 세마포어
	- 상호 배제를 보장하기 위해 사용
	- 입/출력 시 Critical Section을 보호하기 위해 사용
### 입력
```java
public void produce(int item) throws InterruptedException {  
    empty.acquire(); // Wait for an empty slot  
  
    mutex.acquire(); // Enter Critical Section  
    buffer[in] = item;  
    in = (in + 1) % bufferSize;  
    mutex.release(); // Exit Critical Section  
  
    full.release(); // Signal that the slot is full  
  
}
```


위 코드는 다음과 같다.
- empty semaphore -> -1
- mutex -> critical section 보호에 사용
- full semaphore -> +1

당연히 입력을 했기 때문에 empty slot은 1 감소하며 full slot은 1 증가한다.

empty acquire 과정에서 만약 empty slot이 0이라면 => 가득 찻다면 thread를 block한다.
### 출력
```java
public int consume() throws InterruptedException {  
    full.acquire(); // Wait for a full slot  
  
    mutex.acquire(); // Enter Critical Section  
    int item = buffer[out];  
    out = (out + 1) % bufferSize;  
    mutex.release(); // Exit Critical Section  
  
    empty.release(); // Signal that the slot is empty  
    return item;  
}
```

위 코드는 다음과 같다.
- empty semaphore -> +1
- mutex -> mutual exclusion
- full semaphore -> -1

출력이기 때문에 empty slot은 1 증가하며 full slot은 1 감소한다.

여기서 full semaphore를 acquire하는데 full slot이 0이라면(비어 있다면) 해당 요청 쓰레드를 Wait 시킨다.


### 전체 코드
```java
import java.util.Arrays;  
import java.util.concurrent.Semaphore;  
  
public class ProducerConsumer {  
    private final int bufferSize;  
    private final int[] buffer;  
  
    private final Semaphore mutex;  
    private final Semaphore empty;  
    private final Semaphore full;  
  
    private int in = 0;  
    private int out = 0;  
  
    public ProducerConsumer(int bufferSize) {  
       this.bufferSize = bufferSize;  
       this.buffer = new int[bufferSize];  
       this.mutex = new Semaphore(1);  
       this.empty = new Semaphore(bufferSize);  
       this.full = new Semaphore(0);  
    }  
  
    public void produce(int item) throws InterruptedException {  
       empty.acquire(); // Wait for an empty slot  
  
       mutex.acquire(); // Enter Critical Section  
       buffer[in] = item;  
       in = (in + 1) % bufferSize;  
       mutex.release(); // Exit Critical Section  
  
       full.release(); // Signal that the slot is full  
  
    }  
  
    public int consume() throws InterruptedException {  
       full.acquire(); // Wait for a full slot  
  
       mutex.acquire(); // Enter Critical Section  
       int item = buffer[out];  
       out = (out + 1) % bufferSize;  
       mutex.release(); // Exit Critical Section  
  
       empty.release(); // Signal that the slot is empty  
       return item;  
    }  
  
    @Override  
    public String toString() {  
       return "ProducerConsumer{" +  
          "bufferSize=" + bufferSize +  
          ", buffer=" + Arrays.toString(buffer) +  
          ", mutex=" + mutex +  
          ", empty=" + empty +  
          ", full=" + full +  
          '}';  
    }  
}
```

### tryAcquire
semaphore 때문에 thread가 계속해서 대기 상태에 머무르는 게 부담스럽다면 타임 아웃 wait을 시도해 볼 수 있다. 이 때 사용하는 것이 tryAcquire이다. 

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
- [[Java 쓰레드 상태]]