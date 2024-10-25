---
tags: 
  - 완성
  - JAVA
aliases: 
title: Java 시간 관련 라이브러리 다루기
date: 2023-11-30
---

작성 날짜: 2023-11-30
작성 시간: 16:12


----
## 내용(Content)

### LocalTime

#### 기본 사용법

LocalTime을 이용해서 시간 형태로 이루어진 문자열을 시계열 데이터로 파싱 가능하다.

```java
String log = "01:12:24";
LocalTime time = LocalTime.parse(log);
System.out.println(time);  
System.out.println(time.getHour());  
System.out.println(time.getMinute());  
System.out.println(time.getSecond());
```

plusMinutes, plusHours, plusSeconds를 활용해서 시, 분, 초를 바꿀 수 있다.

#### 초 단위로 변환 및 초를 시간 데이터로 파싱하기

```java
String log = "00:12:24";
LocalTime time = LocalTime.parse(log);

// LocalTime -> (int) seconds
int seconds = time.toSecondOfDay();

// (int) seconds -> LocalTime
LocalTime nowTime = LocalTime.ofSecondOfDay(seconds);
```


### Duration

Duration 객체를 활용하면 두 시간의 간격을 측정할 수 있고, 이를 second로 변환 가능하다.  이를 통해서 LocalTime 객체와 Duration을 활용해 시간 간격과 시간 데이터 변환을 쉽게 활용 가능하다.

```java
Duration duration = Duration.between(time, time.plusMinutes(10));  
long seconds = duration.getSeconds();  
LocalTime ofDay = LocalTime.ofSecondOfDay(seconds);  
```
## 질문 & 확장

(없음)

## 출처(링크)
- javadocs

## 연결 노트










