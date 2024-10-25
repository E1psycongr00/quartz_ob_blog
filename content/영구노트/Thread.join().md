---
tags:
  - 완성
  - JAVA
  - Thread
aliases: 
title: Thread.join()
date: 2024-01-29
---
작성 날짜: 2024-01-29
작성 시간: 12:37


----
## 내용(Content)
### join 소개
![[Pasted image 20240129123850.png]]

>[!summary] Thread.join()
>자바에서는 Thread가 종료되기를 기다릴 때 Join()을 사용할 수 있다.

예를 들어 Thread A가 Thread B를 호출할 때 어떤 작업을 완료하고 기다려야 할 때가 있다. 이럴 때 A가 B의 Join을 호출하면 A는 B가 종료될 때까지 기다린다. 


### 예시

#### 테스트에 사용될 쓰레드 코드

```java
private static class MyThread extends Thread {  
    @Override  
    public void run() {  
       System.out.println(LocalTime.now() + " Thread is started");  
       try {  
          Thread.sleep(3000);  
       } catch (InterruptedException e) {  
          System.out.println(LocalTime.now() + " Thread is interrupted");  
       }  
       System.out.println(LocalTime.now() + " Thread is running");  
    }  
}
```
#### join을 사용하지 않는 경우
```java
public static void main(String[] args) {  
    Thread thread = new MyThread();  
    thread.start();  
  
    System.out.println(LocalTime.now() + " alive: " + thread.isAlive());  
}
```

![[Pasted image 20240129124652.png]]

join을 사용하지 않는 경우 별도로 동작하는 MyThread의 동작을 기다리지 않는다. 그래서 결국  Main Thread에서는 MyThread가 동작하고 있는 도중에 alive을 체크한다.

#### Join을 사용한 경우
```java
public static void main(String[] args) throws InterruptedException {  
    Thread thread = new MyThread();  
    thread.start();  
  
    thread.join();  
    System.out.println(LocalTime.now() + " alive: " + thread.isAlive());  
}
```

![[Pasted image 20240129124911.png]]

MyThread가 끝날 때까지 Main Thread을 wait 상태로 만들고 MyThread 작업이 끝나면 Main을 깨워서 다시 진행한다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://codechacha.com/ko/java-thread-join/

## 연결 노트
- [[유저 쓰레드]]
- [[Java 쓰레드 상태]]









