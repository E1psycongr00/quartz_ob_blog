---
tags:
  - 완성
  - 알고리즘
  - 이분탐색
aliases: 
title: Parametric Search 조건을 만족하는 최대값
date: 2024-02-16
---
작성 날짜: 2024-02-16
작성 시간: 21:19


----
## 내용(Content)
### Parametric Search Max
>[!summary] 
>주어진 조건에 이분탐색으로 대입해서 만족하는 최대값

매개 변수 탐색은 optimize(x) -> decision(x) 문제로 바꿔서 푸는 것이다. 결정화 문제는 `대입해서 그것이 조건을 만족하느냐` 이고 x를 대입해서 true, false로 나타내기 때문에 문제를 풀기 쉬워지는 장점이 있다.


>[!caution] 
>상황에 따라 삼분 탐색이나 정확하진 않지만 근사에서 탐색하는 등 여러 가지를 매개변수 탐색이라 할 수 있다. 매개변수의 핵심은 어떤 x를 대입해서 조건에 맞는 x를 구하는 것이다. 

### code
#### java

```java
public static long parametricSearchMax(long lo, long hi, LongPredicate condition) {  
    long result = 0;  
    while (lo <= hi) {  
       long mid = lo + (hi - lo) / 2;  
       if (condition.test(mid)) {  
          result = mid;  
          lo = mid + 1;  
       } else {  
          hi = mid + 1;  
       }  
    }  
    return result;  
}
```

>[!info]
>condition을 IntPredicate로 받은 이유는 Predicate로 받으면 boxing이 일어나기 때문에 매번 호출하는 알고리즘 특성상 성능에  안 좋을 수 있기 때문에 사용했다.

>[!info]
>함수형 객체를 사용한 이유는 람다는 지역변수를 캡처 할 수 있고, condition과 이분 탐색 로직 분리를 통해 코드가 한 가지 작업만 할 수 있도록 하고 다양한 컨디션에 대해서 쉽게 대처할 수 있다. 기존 이분 탐색과 거의 동일한 코드기 때문에 사용하는데 큰 어려움도 없다.

### 해석하기


![[lower_bound와 Parametric Search 예제(draw).svg|500]]


특정 경계를 기준으로 최대를 구한다는 것은 어떤 의미일까? 
오름차순의 데이터의 경우 

`x < target` 또는 `x <= target`

일 때 우리가 원하는 경계의 최대값을 구할 수 있다.
예를 들어 x < 5인 경우 target 5에 최대한 가까운 지점까지의 최대 인덱스(해)를 구할 수 있다.

>[!tip]
>Condition을 잡는 기준은 우리가 목표로 하는 경계가 상한이 되도록 정하는 것이다. 그러면 해당 조건에 부합하는 최대 해를 구할 수 있다.



### 결과 코드 예시

```java
System.out.println(parametricSearchMax(mid -> mid < 5, 0, 10)); // 5
```

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
- [[lower bound]]
- [[upper bound]]
- [[Parametric Search 조건을 만족하는 최소값]]








