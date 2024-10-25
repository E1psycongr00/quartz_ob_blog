---
tags:
  - 완성
  - 솔루션
  - 알고리즘
  - UnionFind
aliases: 
title: UnionFind 그룹별 갯수 구하기
date: 2023-12-12
---
작성 날짜: 2023-12-12
작성 시간: 16:18


----

## 문제 & 원인
보통 Counter를 만들때는 HashMap을 많이 이용하곤 한다. 그러나 컬렉션 타입의 단점은 Boxing 타입을 넣는다는 것인데 Boxing 타입을 만들고 +1씩 계속 업데이트하는 과정에서 Boxing/UnBoxing이 일어나 횟수가 10,000회 이상으로 매우 커지게 되면 성능 이슈가 발생한다.

UnionFind의 그룹별 갯수를 구하되, 박싱/언박싱으로 발생하는 성능 이슈를 최소화하고 싶다. 이런 경우 어떻게 해야 할까?
## 해결 방안

UnionFind는 노드 번호 별로 일차원 배열에 저장하고, 이를 활용해 노드를 연결하고 공통 조상을 정하게 된다. 이것을 카운트할 때 find(int x) 메서드를 활용하는데 이것이 리턴하는 것이 int이기 때문에 배열을 충분히 활용할 수 있다. 배열의 index: find(x) 로 매핑하고 value값은 counting해주면 된다.

```java
public int[] counts() {
	int[] counts = new int[parents.length];
	for (int i = 0; i < parents.length; i++) {
		int root = find(i);
		counts[root]++;
	}
	return counts;
}
```

생성하는데 O(N)의 시간복잡도가 소모되며, int[] 타입이기 업데이트 및 접근이 굉장히 빠르다.

## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트











