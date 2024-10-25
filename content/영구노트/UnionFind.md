---
tags:
  - 완성
  - 알고리즘
  - UnionFind
aliases: 
title: UnionFind
date: 2023-12-12
---
작성 날짜: 2023-12-12
작성 시간: 14:21


----
## 내용(Content)

### UnionFind란

UnionFind 는 Union + Find가 결합된 단어로 노드를 연결하고 그룹을 지어서 해당 그룹의 조상을 찾는데 사용한다. 

![[UnionFind예시1(draw).svg]]

예를 들어 0,1,2,3,4,5 노드가 존재하고 1,2,3 노드와 4,5 노드가 서로 그룹 지어져 있다면 UnionFind의 내부 데이터에서는 \[0,1,1,1,4,4] 이런 식으로 저장되게 된다.

### UnionFind 활용

공통 조상 활용 및 사이클 여부를 판단을 UnionFind를 이용하면 쉽게 할 수 있다.

### 주의할 점
UnionFind에서는 node 공통 조상을 접근할 때 find를 활용해야 하는 것이다.  특정 상황에서 find 없이 임의대로 진행해도 일부 테스트 케이스에서는 통과하는 경우가 생길 수도 있는데 find를 호출하지 않으면 경로 압축이 제대로 이루어지지 않아 잘못된 값을 받을 수 있다.

### Code

대부분의 코딩 테스트에서는 Simple UnionFind 코드로 충분하다

```java
public class UnionFind {  
    private final int[] parents;  
    public UnionFind(int n) {  
       parents = new int[n + 1];  
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
       x = find(x);  
       y = find(y);  
       if (x != y) {  
          parents[y] = x;  
       }  
    }  
}
```

경로 압축을 하기 때문에 빠른 성능을 기대할 수 있다.

## 질문 & 확장

- merge를 더욱 최적화 할 수 있다고 들었는데 과연?
## 출처(링크)


## 연결 노트










