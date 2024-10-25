---
tags:
  - 완성
  - JAVA
aliases: 
title: Java17 Switch 문법
date: 2023-10-21
---


작성 날짜: 2023-10-21
작성 시간: 00:18


----
## 내용(Content)
### Java17 Switch Expression

>[!summary] java17의 switch 문 특징
>- switch문을 통해 결과 값을 반환 가능하다.
>- break없이 사용 가능한 -> 를 활용한 문법이 존재한다.

**기존 코드**
```java
String x = "MON";  
switch (x) {  
    case "MON":  
       System.out.println("월요일");  
       break;    
   case "TUE":  
       System.out.println("화요일");  
       break;    
   case "WED":  
       System.out.println("수요일");  
       break;    
   case "THU":  
       System.out.println("목요일");  
       break;    
   case "FRI":  
       System.out.println("금요일");  
       break;    
   case "SAT":  
       System.out.println("토요일");  
       break;    
   case "SUN":  
       System.out.println("일요일");  
       break;    
   default:  
       throw new IllegalStateException("Unexpected value: " + x);  
}
```

굉장히 코드가 길고 비효율적이다. 이를 java 12, 13을 거치면서 switch문이 많이 개선됬다.

```java
String x = "MON";  
String value = switch (x) {  
    case "MON" -> "월요일";  
    case "TUE" -> "화요일";  
    case "WED" -> "수요일";  
    case "THU" -> "목요일";  
    case "FRI" -> "금요일";  
    case "SAT" -> "토요일";  
    case "SUN" -> "일요일";  
    default -> throw new IllegalStateException("Unexpected value: " + x);  
};  
System.out.println(value);
```

스위치를 그룹핑해서 처리 할 수도 있다.

```java
String day = "MON";  
String result = switch (day) {  
    case "MON", "TUES", "WED", "TURS", "FRI" -> "workday";  
    case "SAT", "SUN" -> "holiday";  
    default -> throw new IllegalArgumentException("exception");  
};
```

만약 여러가지 처리를 해야해서 block문으로 작성하는 경우 yield로 리턴값을 줄 수 있다.

```java
String result = switch (food) {  
    case "pizza" -> {  
       System.out.println("pizza is trash");  
       yield "pizza is trash";  
    }  
    default -> throw new IllegalArgumentException("exception");  
};
```


## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










