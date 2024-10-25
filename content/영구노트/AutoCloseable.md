---
tags:
  - 완성
  - JAVA
aliases: 
date: 2024-04-24
title: AutoCloseable
---
작성 날짜: 2024-04-24
작성 시간: 18:47


----
## 내용(Content)


### AutoCloseable

>[!summary]
>try with resource 에서 자동으로 자원을 반환할 때 AutoCloseable interface를 통해 동작한다.

예를 들어 ExecutorService를 사용한다고 가정하자. ExecuterService를 다 사용하고 나면 자원을 반환하기 위해 shutdown() 메서드를 호출해줘야 한다. 그렇지 않으면 ExecutorService에서 생성된 쓰레드가 메모리를 잡아먹고 이로 인해 메모리 누수가 발생할 수 있기 때문이다.

예제 코드는 다음과 같다.

```java
@Test  
void test1() throws InterruptedException, ExecutionException {  
    int tasksNum = 10;  
    ExecutorService executorService = null;  
    try {  
       executorService = Executors.newFixedThreadPool(10);
       // ~~ 동작 코드
    } finally {  
       if (executorService != null) {  
          executorService.shutdown();  
       }  
    }  
}
```

ExecutorService interface는 AutoCloseable을 지원한다. 그래서 해당 코드를 try with resource 문을 통해 리팩토링 가능하다.

```java
@Test  
void test2() {  
    int taskNum = 10;  
    try (ExecutorService executorService = Executors.newFixedThreadPool(10)) {  
    // 동작 코드
    }
}
```

try () 안에 executorService를 초기화해주기만 하면 try 문 종료후에 AutoCloseable에서 구현한 close()를 알아서 호출한다.

>[!tip] 
**AutoCloseable은 외부와 어떤 자원을 연결하거나 메모리를 할당했을 때 제거해줘야 하는 경우에 구현하면 된다.**

## 질문 & 확장

(없음)

## 출처(링크)
- [AutoCloseable 클래스 — 나의 작은 개발자 (tistory.com)](https://almond0115.tistory.com/entry/AutoCloseable-%ED%81%B4%EB%9E%98%EC%8A%A4)

## 연결 노트










