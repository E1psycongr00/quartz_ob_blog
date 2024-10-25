---
tags:
  - 완성
  - CS
  - JAVA
  - Synchronization
aliases: 
title: Java Monitor
date: 2024-01-20
---
작성 날짜: 2024-01-20
작성 시간: 19:17


----
## 내용(Content)
### Java에서 Monitor
자바에서는 [[Monitor 동기화 메커니즘|Monitor]] 기능이 Object에 내장되어 있다. Object에는 Monitor를 활용하기 위해 3가지 메서드를 제공한다.

1. wait
2. notify == signal
3. notifyAll == broadcast

이 메서드들과 함께 상호배제를 보장하기 위해서 Synchronized 키워드와 함께 사용한다.

### Java Monitor (Synchronized 키워드)
Java Monitor 예제를 위해서 공급자-소비자 문제에서 동기화를 Monitor를 이용해 진행해보자

공급자-소비자 문제는 Bounded Buffer 문제이다. 하나의 버퍼를 두고 여러 쓰레드 입력과 여러 쓰레드 출력이 주어졌을 때 동기화 방법에 대한 문제이다.

[[Java와 함께하는 Semaphore 사용 예제]]를 보면 Bounded Buffer를 흥미롭게 해결한다. Java의 Monitor를 활용하면 더 쉽게 해결할 수 있다.


```java
public synchronized void produce(int item) throws InterruptedException {  
    while (count == FULL) {  
       wait();  
    }  
    buffer[count] = item;  
    count++;  
    notifyAll();  
}
```

```java
public int consume() throws InterruptedException {  
    int item;  
    synchronized (this) {  
       while (count == EMPTY) {  
          wait();  
       }  
       item = buffer[--count];  
       notifyAll();  
    }  
    return item;  
}
```

[[Monitor 동기화 메커니즘#모니터의 구현|Monitor 구현]] 을 살펴보면 while에 대한 부분은 이해할 수 있으므로 넘어가겠다. 다만 wait와 notify는 synchronized 블록 내부에서 동작하도록 해야 한다.

synchronized를 메서드 단위로 처리하면 메서드 전체가 Critical Section이 된다. 블록을 사용하면 일부분만 Critical Section으로 만들 수 있다.


>[!note] notifyAll() 메서드를 사용한 이유
> 1. notify()를 사용하면 쓰레드 기아 문제가 발생할 수 있기 때문이다. notify()는 하나의 쓰레드를 꺠우는데 JVM 스케줄링 또는 OS 스케줄링에 의존한다. 즉 쓰레드의 우선순위에 따라 특정 쓰레드가 꺠어나지 않을 수 있고 불필요하게 notify, wait 과정이 반복됨에 따라 불필요한 컨텍스트 스위칭으로 성능이 하락할 수 있다. 그렇기에 쓰레드를 모두 깨우는게 비효율적인거 같아도 notifyAll을 하는 것이 쓰레드에게 공평한 기회를 줄 수 있고 안전하다.
> 2. 하나의 조건 변수(monitor)에서 관리되기 때문이다. 모두 this Object에서 monitoring 되기 때문에 produce 메서드를 호출하는 쓰레드나 consume을 호출하는 쓰레드나 대기 쓰레드가 공유된다. 그렇기에 특정해서 소비자 쓰레드나 공급자 쓰레드를 깨울 수 없기 떄문에 notifyAll을 활용해서 모두 깨우고 확인시키는 것이다.

### Java Monitor(ReentranceLock과 Condition)
Synchronized의 단점은 특정한 쓰레드 별로 관리하기 어렵다는 것이다. 이를 위해 여러 Condition을 생성해서 특정 쓰레드를 대기 큐에 따로 담아 처리할 수 있는 방법이 있는데 그 방법이 ReentranceLock과 Condition을 이용하는 방법이다.

Lock 객체는 Synchronized의 Lock 기능을 대체하며 Condition은 모니터의 기능들을 대체한다. 이들의 분리 덕분에 하나의 객체에 대해서 여러 Condition을 두어  여러 종류의 대기 set을 만들 수 있다.

위에서 notifyAll을 사용할 수 밖에 없는 이유가 2번이 결정적인 이유인데 만약 공급자 쓰레드와 소비자 쓰레드를 구분해서 가져올 수 있다면 굳이 notifyAll를 호출할 필요가 없어지고 더 빠르게 동작할 수 있을 것이다. 예제를 살펴보자

```java
public BoundedBuffer2() {  
    this.buffer = new int[BUFFER_SIZE];  
    this.count = 0;  
    this.lock = new ReentrantLock();  
    this.notFull = lock.newCondition();  
    this.notEmpty = lock.newCondition();  
}
```

우선 모니터의 구현을 위해 lock과 두개의 Condition variable을 준비한다.
```java
public void produce(int item) throws InterruptedException {  
    lock.lock();  
    try {  
       while (count == FULL) {  
          notFull.await();  
       }  
       buffer[count] = item;  
       count++;  
       notEmpty.signal();  
    } finally {  
       lock.unlock();  
    }  
}
```


```java
public int consume() throws InterruptedException {  
    lock.lock();  
    try {  
       while (count == EMPTY) {  
          notEmpty.await();  
       }  
       int item = buffer[--count];  
       notFull.signal();  
       return item;  
    } finally {  
       lock.unlock();  
    }  
}
```

 notFull Condition에서는 produce task의 쓰레드를, notEmpty에서는 consume task를 보관한다. 여기서 signalAll() 이 아닌 signal()을 호출한 이유는 produce의 경우 signal로 consume task를 보관하고 있는 대기 큐의 쓰레드를 요청하고, consume은 반대로 요청한다. 이렇게 특정해서 쓰레드를 깨울 수 있기 때문에 불필요한 컨텍스트 스위칭이나 동기화 문제를 고려하지 않아도 된다.

>[!tip] java.util.concurrent.ArrayBlockingQueue 
>위 코드는 사실 ArrayBlockingQueue를 구현한 것이다. 

## 질문 & 확장

(없음)

## 출처(링크)
- https://hyunah-home.tistory.com/entry/Monitor%EB%9E%80
- https://coding-start.tistory.com/201
- https://aroundck.tistory.com/4531
## 연결 노트
- [[Monitor 동기화 메커니즘]]









