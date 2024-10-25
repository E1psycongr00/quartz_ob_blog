---
tags:
  - 완성
  - JAVA
  - OS
  - Process
  - Synchronization
aliases: 
title: Java와 함께하는 Mutex 사용 예제
date: 2024-01-16
---
작성 날짜: 2024-01-16
작성 시간: 20:28


----

## 문제&원인
### 동시에 여러 쓰레드가 공유 자원에 접근할 때 문제점 예시
```java
public class SequentialGenerator {  
    private int currentValue = 0;  
  
    public int getNextSequence() {  
       currentValue++;  
       return currentValue;  
    }  
}
```

getNextSequence 메서드를 호출할 때마다 내부적으로 공유 자원이 올라가는 클래스를 하나 정의하자. 이것을 이용해 여러 쓰레드가 해당 임계영역을 접근하는 테스트 코드를 짜보자

```java
class SequentialGeneratorTest {  
  
    @RepeatedTest(3)  
    @DisplayName("race condition이 주어졌을 때 안전하지 않은 SequenceGenerator 사용시 예측하지 못한 결과가 나온다")  
    void shouldUnExpectedResultWhenUseUnsafeSequenceGenerator() throws ExecutionException, InterruptedException {  
       SequentialGenerator generator = new SequentialGenerator();  
       Set<Integer> uniqueSequences = getUniqueSequences(generator, 1000);  
       assertEquals(1000, uniqueSequences.size());  
    }  
  
    private Set<Integer> getUniqueSequences(SequentialGenerator generator, int count) throws  
       ExecutionException,  
       InterruptedException {  
       ExecutorService executor = Executors.newFixedThreadPool(3);  
       Set<Integer> uniqueSequences = new LinkedHashSet<>();  
       List<Future<Integer>> futures = new ArrayList<>();  
  
       for (int i = 0; i < count; i++) {  
          futures.add(executor.submit(generator::getNextSequence));  
       }  
  
       for (Future<Integer> future : futures) {  
          uniqueSequences.add(future.get());  
       }  
       executor.awaitTermination(1, TimeUnit.SECONDS);  
       executor.shutdown();  
  
       return uniqueSequences;  
    }  
}
```

위의 코드를 실행하면 다음과 같은 결과가 나온다

![[Pasted image 20240116213307.png]]
## 내용(Content)
### 동기화가 필요하다
#### Mutex
[[뮤텍스(Mutex)]] 참고하자

다중 스레드 방식을 사용하다보면 동시에 여러 스레드가 공유 리소스에 액세스하면 안되는 상황이 발생할 수 있다. 상호 배제를 위해서 Mutex를 사용한다.

### Synchronized 키워드 사용하기
자바에선 Synchronized를 활용해서 임계 영역에 접근하는 것을 제한 시킬 수 있다. 내부적으로 Mutex 기법으로 동작한다.

```java
public class SynchronizedSequentialGenerator extends SequentialGenerator {  
    @Override  
    public synchronized int getNextSequence() {  
       return super.getNextSequence();  
    }  
}
```

기존의 SequentialGenerator를 상속 받고 genNextSequence를 synchronized로 선언하면 된다.

![[Pasted image 20240116213900.png]]

동기화가 잘 적용된 것을 알 수 있다.

### ReentrantLock 사용
Synchronized 키워드보다 좀 더 세부적으로 lock을 정의 할 수 있다. 이 클래스를 활용하면 임계영역을 보다 세부 조정할 수 있다.

```java
public class LockSequenceGenerator extends SequentialGenerator {  
  
    private final Lock lock = new ReentrantLock();  
  
    @Override  
    public int getNextSequence() {  
       lock.lock();  
       try {  
          return super.getNextSequence();  
       } finally {  
          lock.unlock();  
       }  
    }  
}
```

![[Pasted image 20240116214428.png]]

### Semaphore 사용
Semaphore를 사용해서 Mutex와 같이 상호 배제를 보장하도록 할 수 있다. Semaphore는 시그널 방식이기도 하지만 1로 설정하여 binary Semaphore로 활용하면 Mutex 효과를 낼 수 있다.

```java

```
## 질문 & 확장

(없음)

## 출처(링크)
- https://taeyoungcoding.tistory.com/346#:~:text=Mutex%EB%9E%80%20'Mutual%20Exclusion'%EC%9D%98%20%EC%95%BD%EC%9E%90%EB%A1%9C%2C%20%ED%95%9C%20%EB%B2%88%EC%97%90%20%ED%95%98%EB%82%98%EC%9D%98,%ED%95%A0%20%EB%95%8C%20%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94%20%EA%B2%BD%EC%9F%81%20%EC%A1%B0%EA%B1%B4%EC%9D%84%20%EB%B0%A9%EC%A7%80%ED%95%98%EB%8A%94%EB%8D%B0%20%EC%82%AC%EC%9A%A9%EB%90%9C%EB%8B%A4.
- https://www.baeldung.com/java-mutex#bd-overview
## 연결 노트










