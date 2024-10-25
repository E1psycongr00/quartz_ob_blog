---
tags:
  - 완성
  - 알고리즘
  - DP
aliases:
title: cherry pick up
date: 2024-02-11
---
작성 날짜: 2024-02-11
작성 시간: 22:47


----

## 문제 & 원인
cherry pickup 문제는 격자형 grid 판에 체리가 갈 수 없는 가시 덤불이 있고, 시작(0, 0)부터 목표 지점(n-1, n-1)까지 갔다가 다시 원점으로 돌아올 때 최대한 많이 체리를 가져 오면 몇 개를 가져올 수 있는지 묻는 문제이다.

![[Pasted image 20240212171223.png]]

목표지점으로 갈때는 우측 또는 아래로만 이용해서 가고, 다시 되돌아 갈때는 왼쪽, 위 이렇게 2 방향만을 이용해 원점으로 돌아간다.
## 해결 방안
### 4D Top down으로 해결하기

#### 문제 분석
갔다가 되돌아 오는 것은 어떻게 보면 두 사람이 동시에 출발해서 목표지점으로 도달하고 이때 두 사람이 채집한 체리양을 모두 합하는 것과 위 문제는 동일하다. 그렇다면 이렇게 해석 가능하다.

P1(r1, c1), P2(r2, c2)가 오른쪽과 아래쪽으로 가면서 p1, p2가 합해서 채집할 수 있는 최대의 cheery 양

**cherrypickUpMax(int r1, int c1, int r2, int c2) -> return max 체리양**

이 된다.

재밌는 점은 p1, p2의 현재 좌표가 다음에 갈 좌표와 chery pick간의 관계가 있다는 것이다.

top-down 방식에 의하면 0,0,0,0 에서 시작해 각각 p1과 p2를 아래 오른쪽으로 옮기면서(4가지 경우의 수) n-1,n-1,n-1,n-1에 도달할 때 최대 cherry 값을 구하면 된다.

#### code

```python
class Solution:
    INF = 10 ** 9
    BLOCKED = -1

    def cherryPickup(self, grid: List[List[int]]) -> int:
        n = len(grid)
  
        def out_of_bound(r1, c1, r2, c2):
            return r1 == n or r2 == n or c1 == n or c2 == n
            
        def is_arrive_target_point(r1, c1, r2, c2):
            return r1 == n - 1 and c1 == n - 1 and r2 == n - 1 and c2 == n - 1

        @cache
        def countPickupCherry(r1, c1, r2, c2):
            if out_of_bound(r1, c1, r2, c2):
                return -self.INF
            if grid[r1][c1] == self.BLOCKED or grid[r2][c2] == self.BLOCKED:
                return -self.INF
            if is_arrive_target_point(r1, c1, r2, c2):
                return grid[r1][c1]
  

            cherries = grid[r1][c1] if r1 == r2 and c1 == c2 else grid[r1][c1] + grid[r2][c2]

            return cherries + max(
                countPickupCherry(r1 + 1, c1, r2 + 1, c2),
                countPickupCherry(r1 + 1, c1, r2, c2 + 1),
                countPickupCherry(r1, c1 + 1, r2 + 1, c2),
                countPickupCherry(r1, c1 + 1, r2, c2 + 1)
            )
  
        return max(0, countPickupCherry(0, 0, 0, 0))
```

## 질문 & 확장

(없음)

## 출처(링크)
- [cherry pickup 문제 사이트](https://leetcode.com/problems/cherry-pickup/submissions/1172320912/)
- [cherry pickup 2 문제 사이트](https://leetcode.com/problems/cherry-pickup-ii/)

## 연결 노트
- [[Dungeon Game]]
- [[Paths in matrix whose sum is divisible by K]]