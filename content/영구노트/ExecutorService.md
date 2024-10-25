---
tags:
  - 완성
  - JAVA
  - Thread
aliases: 
date: 2024-04-24
title: ExecutorService
---
작성 날짜: 2024-04-24
작성 시간: 17:50


----
## 내용(Content)

### Executorservice

>[!summary]
>java.util.concurrent에서 제공하고, 작업(Runnable, Callable) 등록 및 실행에 대한 책임과 역할을 수행하는 인터페이스이다.

우리가 사용하려는 ThreadPoolExecutor는 ExecutorService의 구현체이다. [[Java Thread Pool]]을 위해 사용됬다.

### Thread Pool 생성하기

Executors는 ThreadPool 구현체의 정적 팩토리 메서드를 제공한다. 그래서 보다 쉽게 인스턴스를 생성가능하다.

```java
ExecutorService executorService = Executors.newFixedThreadPool(10);
```

제공하는 팩토리 메서드를 분석해보자


#### Executors.newFixedThreadPool(int nThreads)

제한되지 않은 공유 큐에서 작동하는 고정된 수의 스레드를 재사용하는 스레드 풀을 만듭니다. 어떤 시점에서든 최대 nThreads 스레드는 활성 처리 작업이 됩니다. 모든 스레드가 활성 상태일 때 추가 작업이 제출되면 스레드를 사용할 수 있을 때까지 대기열에서 대기합니다. 종료 전 실행 중 오류로 인해 스레드가 종료되는 경우 후속 작업을 실행하는 데 필요한 경우 새 스레드가 그 자리를 대신합니다. 풀의 스레드는 명시적으로 shutdown 될 때까지 존재합니다.

#### Executors.newWorkStealingPool(int parallelism)

지정된 병렬 처리 수준을 지원하기에 충분한 스레드를 유지하고 경합을 줄이기 위해 여러 대기열을 사용할 수 있는 스레드 풀을 만듭니다. 병렬성 수준은 작업 처리에 적극적으로 참여하거나 참여할 수 있는 최대 스레드 수에 해당합니다. 실제 스레드 수는 동적으로 늘어나거나 줄어들 수 있습니다. 작업 도용 풀은 제출된 작업이 실행되는 순서를 보장하지 않습니다.

parallelism을 입력하지 않으면 **사용 가능한 프로세서 수**를 기준으로 스레드를 생성함

#### Executors.newSingleThreadExecutor()

제한되지 않은 대기열에서 작동하는 단일 작업자 스레드를 사용하는 실행자를 만듭니다. (단, 종료 전 실행 중 오류로 인해 이 단일 스레드가 종료되는 경우 후속 작업을 실행하는 데 필요한 경우 새 스레드가 그 자리를 대신하게 됩니다.) 작업은 순차적으로 실행되며 하나 이상의 작업이 활성화되지 않습니다. 언제든지. 동등한 newFixedThreadPool(1) 과 달리 반환된 실행기는 추가 스레드를 사용하도록 재구성할 수 없다는 것이 보장됩니다.

#### newCachedThreadPool()

필요에 따라 새 스레드를 생성하지만 사용 가능한 경우 이전에 생성된 스레드를 재사용하는 스레드 풀을 생성합니다. 이러한 풀은 일반적으로 많은 단기 비동기 작업을 실행하는 프로그램의 성능을 향상시킵니다. execute 호출은 가능한 경우 이전에 생성된 스레드를 재사용합니다. 기존 스레드를 사용할 수 없으면 새 스레드가 생성되어 풀에 추가됩니다. 60초 동안 사용되지 않은 스레드는 종료되고 캐시에서 제거됩니다. 따라서 오랫동안 유휴 상태를 유지하는 풀은 리소스를 소비하지 않습니다. 속성은 비슷하지만 세부 정보(예: 시간 제한 매개 변수)가 다른 풀은 ThreadPoolExecutor 생성자를 사용하여 생성할 수 있습니다.

#### Executors.newThreadPerTaskExecutor()

각 작업에 대해 새 스레드를 시작하는 실행자를 만듭니다. Executor가 생성하는 스레드 수에는 제한이 없습니다.
Executor에 제출된 작업의 보류 결과를 나타내는 Future 에서 cancel(true) 호출하면 작업을 실행하는 스레드가 interrupt 됩니다.

#### Executors.newVirtualThreadPerTaskExecutor()

각 작업에 대해 새로운 가상 스레드를 시작하는 실행자를 만듭니다. Executor가 생성하는 스레드 수에는 제한이 없습니다.

### ExecutorService 사용법

ExecutorService를 초기화한 후에 Task를 집어넣어 주면 된다. Callable, Runnable 모두 가능하다. 

invokeAll()를 하면 executorService에 넣은 Task를 자동으로 쓰레드에 할당하여 처리하고, Future를 반환한다. 값을 얻고 싶다면 Future를 통해서 얻으면 된다.

```java
@Test  
void test2() {  
    int taskNum = 10;  
    try (ExecutorService executorService = Executors.newFixedThreadPool(10)) {  
       List<CallableEx> callables = IntStream.range(0, taskNum)  
          .mapToObj(CallableEx::new)  
          .toList();  
  
       System.out.println("--- 작업 실행 ---");  
       List<Future<String>> futures = executorService.invokeAll(callables);  
       System.out.println("--- 작업 완료 ---");  
  
       System.out.println("결과 출력");  
       for (Future<String> future: futures) {  
          System.out.println(future.get());  
       }  
    } catch (InterruptedException | ExecutionException e) {  
       e.printStackTrace();  
    }  
}
```


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트

- [[Java Thread Pool]]


