---
tags:
  - 완성
  - 알고리즘
  - 이분탐색
aliases: 
title: Parametric Search 조건을 만족하는 최소값
date: 2024-02-16
---
작성 날짜: 2024-02-16
작성 시간: 22:19


----
## 내용(Content)
### ParametricSearchMin

>[!summary]
>주어진 조건에 만족하는 최소 x를 찾는데 사용하는 이분 탐색 알고리즘. 경계보다 큰 값에서 가장 경계에 가까운 하한 값을 가지는 해를 구하는 알고리즘이라 할 수 있다.

### code
#### java
```java
public static long parametricSearchMin(long lo, long hi, LongPredicate condition) {  
    long result = 0;  
    while (lo <= hi) {  
       long mid = lo + (hi - lo) / 2;  
       if (condition.test(mid)) {  
          result = mid;  
          hi = mid - 1;  
       } else {  
          lo = mid + 1;  
       }  
    }  
    return result;  
}
```

>[!caution]
>값들이 오름차순으로 배치되어 있다면 고려할 수 있는 조건은 특정 인덱스의 값이 크거나 같아야한다.

### 해석하기

![[lower_bound와 Parametric Search 예제(draw).svg|500]]

위 그림을 살펴보자 오름차순으로 배치되어 있는 값중에서 최소값을 구한다는 것은 False 영역에서 오른쪽에 왼쪽으로 타고 오면서 경계값보단 크면서 False 영역 중 가장 작은 최소 해를 구하는 것이다. True 영역에서 최소값을 구하는 것은 말이 되지 않는다. 사실상 $-\infty$ 이기 때문이다. 

특정 영역에서 초록색 구간을 정의하기 위해서는 다음과 같이 조건을 정의하면 된다.

`x > target` 또는 `x >= target`


### 결과 코드 예시
```java
System.out.println(parametricSearchMin(mid -> mid >= 5, 0, 10)); // 5
```
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
- [[Parametric Search 조건을 만족하는 최대값]]
- [[lower bound]]
- [[upper bound]]










