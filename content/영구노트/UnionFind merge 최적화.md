---
tags:
  - 완성
  - 알고리즘
  - UnionFind
aliases: 
title: UnionFind merge 최적화
date: 2023-12-12
---
작성 날짜: 2023-12-12
작성 시간: 14:34


----
## 내용(Content)

### Simple UnionFind의 문제점

경로 최적화만 적용된 UnionFind의 문제점은 최악의 경우 find시 시간 복잡도가 O(N)이 걸릴 수도 있다. 다음과 같은 예제를 살펴보자


![[UnionFind예제2(draw).svg|400]]

2-3-4-5-6 으로 연결된 트리를 merge시 1 노드에 연결된다고 가정하자. 위의 노드의 경우 2,3,4,5,6 모두 탐색해가며 1에다가 업데이트한다. 그래서 O(N)의 시간복잡도를 소모하는 것이다. 그러나 항상 보통의 경우에는 이런 경우가 흔치 않기 때문에 거의 상수 복잡도가 된다. 그러나 특수한 상황의 경우 위와 같은 일이 꽤나 발생할 수도 있기 떄문에 최적화가 필요 할 수 있다.

### 균형 이진 트리를 적용하기

rank라는 것을 적용해서 큰 랭크쪽으로 붙이도록 하는 것이다. 랭크가 높다는 것이 depth가 크다는 의미이고, 큰 depth 쪽으로 낮은 depth들이 이동하도록 하는 것이다. 이런 경우에 최악의 경우에도 O(logN) 시작복잡도를 유지할 수 있다.


### code

```java
public class UnionFind {  
  
    private final int[] parents;  
    private final int[] ranks;  
  
    public UnionFind(int n) {  
       parents = new int[n + 1];  
       ranks = new int[n + 1];  
       for (int i = 0; i <= n; i++) {  
          parents[i] = i;  
       }  
    }  
  
    public int find(int x) {  
       if (parents[x] != x) {  
          parents[x] = find(parents[x]);  
       }  
       return parents[x];  
    }  
  
    public void merge(int x, int y) {  
       int rootX = find(x);  
       int rootY = find(y);  
       if (rootX == rootY) {  
          return;  
       }  
       if (ranks[rootX] < ranks[rootY]) {  
          parents[rootX] = rootY;  
       } else if (ranks[rootX] > ranks[rootY]) {  
          parents[rootY] = rootX;  
       } else {  
          parents[rootX] = rootY;  
          ranks[rootY]++;  
       }  
    }  
}
```

merge 부분을 살펴보면 merge부분이  rank에 따라 parents를 어떻게 업데이트 할지 결정된다. 이전 설명대로 랭크가  큰 쪽을 부모 root로 설정한다. 같은 경우에는 rank가 올라간다. rank == depth

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트










