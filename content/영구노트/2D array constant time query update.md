---
tags:
  - 완성
  - 알고리즘
  - PrefixSum
aliases: 
title: 2D array constant time query update
date: 2023-11-04
---
작성 날짜: 2023-11-04
작성 시간: 13:45


----
## 내용(Content)

1차원 constant update에 비해 조금 더 복잡하다. 알아보자

### 원리
2D 누적합의 공식을 활용한다.

$$ S(x2, y2) = S(x2, y1-1) + S(x1-1, y2) - S(x1-1, y1-1) + a(x1-1, y1-1) $$
위 식에서 영감을 얻어서 2차원 쿼리 업데이트를 할 수 있다.

예를 들어 (x1, y1) -> (x2, y2) 까지 4각형 구간을 +1씩 업데이트 한다고 가정하자

그러면 우리가 업데이트 해야 할 값은 4개이다
1.  S(x1, y1)++
2. S(x2+1, y1)--
3. (x1, y2+1)--
4. (x2+1, y2+1)++

다음 그림은 (1,1) (2, 2) 구간 사각형을 모두 1로 업데이트하는 쿼리를 만드는 과정이다.

![[2d array constant value query (draw).svg| 700]]

### Code
```java
// 쿼리 초기화
int[][] upd = new int[rows+1][cols+1];
for (int[] query : queries) {
	int y1 = query[0];
	int x1 = query[1];
	int y2 = query[2];
	int x2 = query[3];
	
	upd[y1][x1]++;
	upd[y1][x2+1]--;
	upd[y2+1][x1]--;
	upd[y2+1][x2+1]++;
}

// 누적합을 이용해 쿼리 생성하기
for (int i = 0; i < rows; i++) {
	for (int j = 1; j < cols; j++) {
		upd[i][j] = upd[i][j-1] + upd[i][j];
	}
}

for (int j = 0; j < cols; j++) {
	for (int i = 1; i < rows; i++) {
		upd[i][j] = upd[i-1][j] + upd[i][j];
	}
}

```
## 질문 & 확장

(없음)

## 출처(링크)
- https://codeforces.com/blog/entry/86420

## 연결 노트
- [[영구노트/1D array constant time query update|1D array constant time query update]]









