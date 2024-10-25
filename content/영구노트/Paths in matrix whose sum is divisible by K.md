---
tags:
  - 완성
  - 솔루션
  - 알고리즘
  - DP
aliases: 
title: Paths in matrix whose sum is divisible by K
date: 2024-02-13
---
작성 날짜: 2024-02-13
작성 시간: 17:43


----

## 문제 & 원인
![[Pasted image 20240213174626.png]]

(0, 0) 지점부터 목표 지점까지 경로들의 모든 합을 K로 나눠떨어지면 0이 되는 모든 경로의 수를 구하여라
## 해결 방안
### top-down 3DP
#### 해결 전략

DP로 쓰기 위해 3차원을 활용하려고 한다. 

**3차원**
- y: y 
- x x 좌표
- value: % 연산을 이용한 k의 나머지 값

value % k를 인자로 받는 이유는 value % k == 0이라면 나누어 떨어지기 때문이다.


#### code
```js
/**
 * @param {number[][]} grid
 * @param {number} k
 * @return {number}
 */
var numberOfPaths = function (grid, k) {
    const m = grid.length;
    const n = grid[0].length;
    const memo = Array.from({ length: m }, () =>
        Array.from({ length: n }, () => Array(k).fill(-1))
    );
    const mod = 1e9 + 7;
  
    function dp(y, x, value) {
        if (y >= m || x >= n) {
            return 0;
        }
        if (y == m - 1 && x == n - 1) {
            return (value + grid[y][x]) % k == 0 ? 1 : 0;
        }
        if (memo[y][x][value] != -1) {
            return memo[y][x][value];
        }

        let ans = 0;
        ans = (ans + dp(y + 1, x, (value + grid[y][x]) % k)) % mod;
        ans = (ans + dp(y, x + 1, (value + grid[y][x]) % k)) % mod;
        memo[y][x][value] = ans;
        return memo[y][x][value];
    }

    return dp(0, 0, 0);
};
```

## 질문 & 확장

(없음)

## 출처(링크)
- https://leetcode.com/problems/paths-in-matrix-whose-sum-is-divisible-by-k/

## 연결 노트
- [[cherry pick up]]
- [[Dungeon Game]]