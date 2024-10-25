---
tags:
  - 완성
  - JAVA
  - 테스트
  - BenchMark
  - JMH
aliases:
  - Java MicroBenchmark Harness
  - Java Micro Benchmark
date: 2024-05-03
title: JMH
---
작성 날짜: 2024-05-03
작성 시간: 19:33


----
## 내용(Content)

### JMH

>[!summary]
>Java 및 JVM을 대상으로 하는 기타 언어로 작성된 벤치마크를 구축하고, 실행 및 분석을 위한 Java 하네스다.

보통 API를 테스트할 때 NGrinder나 Locust, K6와 같은  부하 테스트 도구를 많이 사용한다. 그러나 학습 또는 내가 구현한 코드들이 성능이 좋은지 안 좋은지 class 또는 메서드 단위로 테스트하고 싶은데 매번 API를 호출해서 테스트하는 것은 비용이 클 수도 있다. JMH(Java Micro Benchmark Harness)는 보다 간편하게 벤치마크 수 있기 때문에 단위 성능 테스트를 원한다면 JMH는 좋은 선택이 될 수 있다.  

>[!info] 하네스
>하네스란 문맥에 따라 2가지 의미를 지원한다.
>1. Test Harness: 소프트웨어 테스트를 자동화하기 위한 프레임워크. 테스트 케이스를 실행하고 보고하는 역할을 한다. 따라서 테스트 하네스는 테스트를 더 쉽고 효율적이게 만들어주는 도구라 할 수 있다.
>2. Harness in the context of framework or libraries: 프레임워크나 라이브러리 에서 하네스란 특정 기능이나 서비스를 돕는 코드 또는 도구를 의미한다. 종종 복잡한 시스템을 더 쉽게 사용할 수 있도록 도와주는 역할을 수행한다.

### JMH 설치 방법

Gradle 기준으로 테스트해봤기 때문에 Gradle 기준으로 진행하겠다.

```kotlin
// build.gradle.kts
plugins {
	id("java")
	id("me.champeau.jmh") version "0.7.2" // id 플러그인 추가
}

dependencies {
	// ~~~~ 테스트 의존성 추가
	jmh ("org.openjdk.jmh:jmh-core:1.36")  
	jmh ("org.openjdk.jmh:jmh-generator-annprocess:1.36")  

	// this is the line that solves the missing /META-INF/BenchmarkList error  
	jmhAnnotationProcessor("org.openjdk.jmh:jmh-generator-annprocess:1.36")
}

// JMH Gradle Task 옵션 설정
jmh {  
    iterations = 3  
    warmupIterations = 3  
    fork = 1  
}
```

version이나 설정의 자세한 정보는 https://github.com/melix/jmh-gradle-plugin 를 참고하자.

### JMH 디렉토리 구성하기

JMH는 src/main이 아닌 src/jmh/java, src/jmh/resources로 구성한다. 

![[Pasted image 20240503212106.png]]

### 벤치마크 코드 짜기

테스트를 위한 Sum이라는 유틸 클래스를 만들어본다.

```java
// src/main/java/org/example/Sum.java

import java.util.stream.IntStream;  
  
public class Sum {  
    public static int forSum(int maxNum) {  
       int sum = 0;  
       for (int i = 1; i <= maxNum; i++) {  
          sum += i;  
       }  
       return sum;  
    }  
  
    public static int streamSum(int maxNum) {  
       return IntStream.rangeClosed(0, maxNum).sum();  
    }  
}
```

위 클래스는 forSum과 streamSum 두 개의 메서드를 만들었다. 이제 두 메서드의 코드를 테스트하는 코드를 짜보자


```java
@State(Scope.Benchmark)  
public class SumBenchMark {  
  
    private static final int MAX_NUM = 1000;  
  
    @Benchmark  
    public void forSum(Blackhole bh) {  
       bh.consume(Sum.forSum(MAX_NUM));  
    }  
  
    @Benchmark  
    public void streamSum(Blackhole bh) {  
       bh.consume(Sum.streamSum(MAX_NUM));  
    }  
}
```

>[!info] Blackhole 클래스
>Blackhole 클래스는 JMH에서 제공하는 클래스로, JVM의 Dead Code Elimination(DCE)를 방지하는 역할을 수행한다. 예를 들어 JVM이 최적화를 위해 DCE를 수행하면 특정 연산에서 사용되지 않는 연산을 완전히 제거해버릴 수 있다. 이러한 과정 때문에 연산의 성능을 측정하는 벤치마크 입장에서는 테스트 결과가 왜곡될 수 있다. 그래서 테스트시 `Blackhole.consume()` 을 사용해서 벤치마크 테스트를 수행한다.
>
>

>[!info] 상수 폴딩
>JVM의 Jit Compiler는 최적화를 위해 상수 폴딩을 할 때가 있다. 예를 들면 `Math.log(8)` code는 컴파일러가 2.0794415416798357로 대체할 가능성이 매우 높다. 만약 상수 폴딩까지 최적화까지 고려해서 벤치마크 테스트를 해야한다면 https://www.baeldung.com/java-microbenchmark-harness 의 7. Constant folding 파트를 참고해보자.

### 벤치마크 실행하기

intellij를 사용하고 있다면 gradle 태스크에서 눌러서 실행할 수도 있고, 아니면 terminal에 `gradlew jmh`로 실행할 수 있다.

![[Pasted image 20240503213817.png]]

실행하면 실행 결과는 다음과 같다.

![[Pasted image 20240503213856.png]]

Default 벤치마크 모드가 Throughput(처리량)이기 때문에 Score에 초당 연산 숫자가 표시된다. 처리량이니 당연시 Score 숫자가 클수록 성능이 좋음을 뜻한다. 신기한 점은 for문이 빠를줄 알았는데 IntStream이 조금 더 높은 Score를 보인다. 큰 차이는 아닌 것 같다.

### 어노테이션 활용하기

처음에 `build.gradle.kts`에서 jmh 태스크에 대한 옵션을 설정해줬는데 벤치마크 테스트 코드에 어노테이션으로 적용 가능하다. **환경 설정의 우선순위는 테스트코드에 적용된 어노테이션이다.**






## 질문 & 확장

(없음)

## 출처(링크)


- https://www.baeldung.com/java-microbenchmark-harness
- https://pompitzz.github.io/blog/Java/jmh_and_asyncProfiler.html#%E1%84%87%E1%85%A6%E1%86%AB%E1%84%8E%E1%85%B5%E1%84%86%E1%85%A1%E1%84%8F%E1%85%B3%E1%84%92%E1%85%A1%E1%86%AF-%E1%84%92%E1%85%A1%E1%86%B7%E1%84%89%E1%85%AE-%E1%84%8C%E1%85%A1%E1%86%A8%E1%84%89%E1%85%A5%E1%86%BC
- https://pompitzz.github.io/blog/Java/jmh_and_asyncProfiler.html#jmh-%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC
- https://stackoverflow.com/questions/38056899/jmh-unable-to-find-the-resource-meta-inf-benchmarklist

## 연결 노트

- [[JMH fork, warmup, measurement 상관관계]]









