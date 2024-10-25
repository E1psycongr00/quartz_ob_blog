---
tags:
  - 완성
  - Gradle
aliases: 
title: Gradle의 doLast()를 언제 써야 할까
date: 2023-10-19
---
작성 날짜: 2023-10-19
작성 시간: 12:25


----

## 문제 & 원인

레퍼런스나 따른 코드를 보면 doLast(), doFirst()를 쓸 때도 있고 안 쓸 때도 있다. 이것을 왜 쓰는 걸까?

## 해결 방안

gradle의 라이프 사이클과 관련이 있다. gradle은 초기화, 구성, 실행 이렇게 3단계를 거치는데 만약 아무것도 없이 그냥 task를 작성하면 구성 단계에서 호출된다. 그래서 task를 짤 때는 일종에 rule이 존재한다.

```groovy
task myTask {
    dependsOn 'anotherTask'
    // more configuration code...
}
```

그냥 작성하는 경우는 단순 task 정의 및 task의 의존 관계를 설정할 때 사용한다. 그러나 task에 나만의 비즈니스 로직을 추가하고 싶다면 다음과 같이 정의 하자.

```groovy
task myTask {
    doLast {
        // code to copy files, run tests etc.
    }
}
```

만약 로직이 시스템 속성 값 로직을 처리해야 한다면 configure phase에서 처리하도록 할 수도 있다. 이때는 doLast를 사용하지 않으면 된다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://velog.io/@jeongyunsung/Gradle%EB%86%80%EC%9D%B43-Custom-Task#task-1
- https://docs.gradle.org/current/userguide/build_lifecycle.html
## 연결 노트
- [[Gradle Build Life Cycle]]