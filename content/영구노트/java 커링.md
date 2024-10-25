---
tags:
  - 완성
  - JAVA
  - 커링
aliases:
  - java currying
date: 2024-03-26
title: java 커링
---
작성 날짜: 2024-03-26
작성 시간: 21:07


----
## 내용(Content)
### 커링
>[!summary]
>수학과 컴퓨터 과학에서 다중 인수를 가진 함수를 단일 인수를 가진 함수열로 바꾸는 것을 의미

$$
\begin{align}
x = f(a, b, c)  \\
h = g(a) \\
i = h(b) \\
x = i(c)
\end{align}
$$

### 람다와 커링
람다 함수를 활용하면 커링을 쉽게 표현할 수 있다.

```text
f(a, b, c) = a + b + c 를 쿼링
f(a)(b)(c)
f = a => b => c => a + b + c
```

function이 하나의 인자를 받으며 중첩된 형태로 존재하고 최종 재귀 끝에서 연산을 수행하는 것이다.

커링은 함수형 프로그래밍에 쉽게 응용될 수 있다. 예를 들면 java의 stream이나 배열을 한번에 처리할 때 map, filter, forEach등등 하나의 인수를 필요로 한다. 이때 이미 구현된 함수가 있다면 커링을 통해 인수 갯수를 원하는 대로 조작해서 사용가능하다. 이것이 불가능하다면 람다 함수를 이용하면 된다.

### java와 커링
#### Function 활용하기
```java
Function<Integer, Function<Integer, Integer>> f = x -> y -> x + y;  
Integer result = f.apply(1).apply(2);
```

가장 기본적인 방법으로 중첩 function을 이용해서 커링을 만든다. 해당 함수 객체를 호출하기 위해서 apply를 연속적으로 호출해서 커링을 한다.

단점은 많은 인수를 커링하면 앞에 선언한 코드들이 길어지는 단점이 있다.

#### bind util function 만들기

```java
static IntUnaryOperator bind(IntBinaryOperator f, int a) {  
    return b -> f.applyAsInt(a, b);  
}

// 사용 예시
IntBinaryOperator f = (a, b) -> a + b;  
IntUnaryOperator g = bind(f, 1);
```

이렇게 Function 대신에 적합한 function을 사용하면 박싱/언박싱 부분을 최적화 할 수 있다.
그러나 Function과 마찬가지로 많은 인수를 커링하면 선언한 코드들이 길어지는 단점이 있다.

#### Builder 패턴
사실 자바에서는 커링을 효율적으로 사용하기 위해 빌더 패턴을 활용한다.
예를 들어 생성자에 여러 인수를 넣는 경우, 인자가 많으면 복잡해지는데 이를 빌더 패턴을 이용하면 쉽게 커링이 가능하다. 물론 단순 연산에 적용하는 데는 거리가 멀기 때문에 생성자 정의 패턴 외에 사용하기에는 애매하다.

## 질문 & 확장

자바는 커링을 활용하기 보다는 람다 함수를 직접 이용해서 제어하는 것이 효과적일 수 있다. 우선 커링을 위해 따로 코드를 구현해야 하고, 자바에서 직접 지원하는 기능이 아니기 때문에 팀원과 같이 개발한다면 오히려 가독성이 떨어질 수 있다.

>[!caution] 그냥 람다 함수 쓰자
>자바에서 커링 신경쓰며 함수형 프로그래밍하기에는 고려해야 할 것이 많아 귀찮다.
## 출처(링크)
- https://stackoverflow.com/questions/38487755/can-java-lambdas-bind-methods-to-their-parameters
- https://www.golinuxcloud.com/java-currying-function-examples/#Example_1_Currying_function_to_multiply_two_numbers
- https://www.geeksforgeeks.org/currying-functions-in-java-with-examples/
## 연결 노트










