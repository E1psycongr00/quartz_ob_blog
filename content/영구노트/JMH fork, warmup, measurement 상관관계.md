---
tags:
  - 완성
  - JAVA
  - BenchMark
  - JMH
aliases: 
date: 2024-05-13
title: JMH fork, warmup, measurement
---
작성 날짜: 2024-05-13
작성 시간: 16:52


----
## 내용(Content)

### Fork

>[!summary]
> Fork수의 설정은 벤치마크할 별도의 JVM 를 OS 프로세스에 할당해서 실행한다.

새로운 JVM을 OS에 할당해서 실행하기 때문에 완전히 독립적인 환경에서 안정성있게 테스트 가능하다. Fork 수가 높을 수록 안정성이 높아지지만 테스트 시간이 길어지기 때문에 적은 수의 Fork를 지정하는 것을 추천한다.

코드 내에서는 @Fork 어노테이션을 이용해 설정 가능하다.
Gradle에서는 jmh gradle option에 fork를 지정해주면 된다.

```kotlin
jmh {  
    fork = 1  
}
```
### Warmup

>[!summary]
> 벤치마크가 실행되기 이전에 시스템이 안정화하고 일관성을 높인다.


Warmup은 준비 운동과 같이 생각하면 된다. Warmup을 통해 얻게 되는 것은 다음과 같다.

- 파일을 열거나 실행할 때, Prime 디렉토리 및 파일 캐시
- 더티 페이지(dirty pages)를 기록하고 사용 가능한 풀(free pool)에 페이지를 추가하여 가상 메모리 시스템을 프라이밍
- 공유 클래스 캐시를 프라임 (JVM이이를 지원하는 경우)

이러한 작업들이 벤치마크의 일관성을 향상시킨다.

Warmup 포크가 실제로 도움이 될 수 있을지 없을지 예측하기는 어렵다. 하지만 벤치마크 테스트 이전에 안정성을 높여주기 때문에 적은 수의 반복은 괜찮다고 생각한다.

@Warmup 어노테이션을 사용하고 gradle에서는 다음과 같이 설정 가능하다.

```kotlin
jmh {  
    warmupIterations = 1  
}
```

### Measurement

>[!summary]
>실제 반복할 테스트 수

여러번 반복하면 테스트의 신뢰성을 향상시킬 수 있다. 예를 들면 항상 테스트를 하면 예측과 다르게 편차가 발생하는데 JMH에서는 ERROR SCORE로 표현하며, 테스트 수가 많을수록 더 정확하게 예측이 가능하다.

@Measurement 어노테이션을 이용해서 iteration 속성을 설정하면 된다.

```java
@Measurement(iterations=5)
```

gradle에선 다음과 같이 설정한다

```kotlin
jmh {  
    iterations = 2  
}
```

## 질문 & 확장

(없음)

## 출처(링크)

- [java 자바 - JMH에서 워밍업 포크가 유용한 이유는 무엇입니까? - 스택 오버플로 (stackoverflow.com)](https://stackoverflow.com/questions/74062856/why-are-warmup-forks-useful-in-jmh)

## 연결 노트

- [[JMH|Java MicroBenchmark Harness]]








