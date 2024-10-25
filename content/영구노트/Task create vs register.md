---
tags:
  - 완성
  - Gradle
aliases: 
title: Task create vs register
date: 2023-10-19
---
작성 날짜: 2023-10-19
작성 시간: 16:31


----
## 내용(Content)

### Create와 Register

create와 register 모두 task를 등록하는데 사용한다. 그러나 둘 간의 약간의 차이점이 있다.

> **create**
> 이 메서드는 즉시 태스크를 생성하고 구성한다.  Gradle 빌드 수명 주기의 구성 단계에서 발생하기 때문에 모든 태스크가 실제로 필요하지 않더라도 매번 생성된다.

> **register**
> register는 지연 생성, 지연 구성을 사용하여 태스크를 생성한다. register의 장점은 매번 task를 초기화하는 것이 아닌 호출할 때 초기화한다. 이 때문에 필요하지 않으면 생성하지 않으므로 성능 향샹을 기대할 수 있다.


### 테스트를 통해 비교
```kotlin
tasks.register("myTask") {  
    description = "나의 태스크"  
    doFirst {  
        println("hello first")  
    }  
    println("run")  
    doLast {  
        println("hello last")  
    }  
}  
  
tasks.create("myTask2") {  
    description = "task2"  
    doFirst {  
        println("hello first2")  
    }  
    println("run2")  
    doLast {  
        println("hello last2")  
    }  
}
```

2개의 task가 있는데 하나는 register라 작성되어 있고 하나는 create로 작성되어 있다. 생성된 gradle task를 인텔리제이에서 확인하면 다음과 같다.

![[Pasted image 20231019170514.png]]

이제 register로 등록된 myTask를 실행해보자

![[Pasted image 20231019170613.png]]

다음과 같이 myTask2의 run2가 실행된다. 그 이유는 configure phase에서 create 선언한 task는 무조건 생성되어 동작하기 때문에 run2와 run1 모두 실행된 것이다.

myTask2를 실행해보자

![[Pasted image 20231019170743.png]]

myTask의 configure은 실행되지 않았음을 알 수 있다. register는 호출 전까지 초기화하지 않기 때문이다.

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










