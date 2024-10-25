---
tags:
  - 완성
  - IT
  - 알고리즘
  - PrefixSum
aliases: 
title: "1D array constant time query update"
date: 2023-11-03
---
작성 날짜: 2023-11-03
작성 시간: 11:43


----
## 내용(Content)

문제를 풀다보면 특정 범위를 동일한 상수만큼 업데이트하는 문제가 있다. 문제는 매번 업데이트 하는 구간이 매우 크고 업데이트를 많이 요청하다 보니 구간 업데이트를 시간 복잡도 $O(1)$ 에 해야하는 상황이 생긴다. 어떻게 하면 일차원 배열의 특정 구간 상수 업데이트를 최적화 할 수 있을까?

### 누적합을 이용해 시간 복잡도를 늦춘다

우리가 특정 구간의 합을 구할 때 미리 누적합을 구해서 원하는 구간은 누적합의 차로 계산했다. 이 아이디어를 이용하면 누적합할 때도 계산이 가능하다.

a에서 b구간까지 +1씩 update한다고 하면 다음과 같이 쓰면 된다

```java
a[b+1]--;
a[a]++;
```




## 질문 & 확장

(없음)

## 출처(링크)
- https://codeforces.com/blog/entry/86420

## 연결 노트
- [[부분합, 누적합]]
- [[영구노트/2D array constant time query update|2D array constant time query update]]









