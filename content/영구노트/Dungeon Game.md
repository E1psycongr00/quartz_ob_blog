---
tags:
  - 완성
  - 알고리즘
  - DP
aliases: 
title: Dungeon Game
date: 2024-02-13
---
작성 날짜: 2024-02-13
작성 시간: 12:38


----

## 문제 & 원인
![[Pasted image 20240213124019.png]]

나이트가 (0, 0) 에서 (n-1, m-1) 좌표까지 가는데 - 발판을 가면 HP 가 깎이고 +발판을 밟으면 HP가 오른다. HP가 0이 되면 기사는 더 이상 움직일 수 없기 때문에 도착할 때까지 최소 1 이상의 목숨이 필요하다.

## 해결 방안
### Top-down DP
#### 해결 전략
재귀를 이용해서 시작 => 목표 지점까지 가고 재귀가 동작하면서 연산을 통해 기사가 갈 수 있는 최소한의 HP를 얻을 수 있도록 해보자. 그리고 문제를 단순화하기 위해서 1차원 배열을 정의하고 각 배열 별로 필요한 HP를 생각해보자


|  | 0 | 1 | 2 | 3 | 4 | 5 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 발판 | -2 | -3 | 4 | 1 | -2 | 3 |
| 필요한 HP | 6 | 4 | 1 | 2 | 3 | 1 |

기사에게 다음과 같은 발판이 주어졌을 때 필요한 HP를 어떻게 기록할 수 있을까?

쉬운 이해를 돕기 위해 코드로 작성해보겠다.

```js
plate = [-2, -3, 4, 1, -2, 3];
memo = [0 ,0, 0, 0, 0, 0]

function go(step) {
	if (step === plate.length - 1) {
		return Math.max(1, 1 - plate[step]); // 필요한 HP는 1 이상
	}
	if (memo[step]) {
		return memo[step]; // memo가 이미 존재하면 출력(memoization)
	}
	let myHp = go(step + 1) - plate[step]; // 필요한 HP
	memo[step] = Math.max(1, myHp); // 필요한 HP는 1 이상 + 기록
	return memo[step];
}
```

이 경우를 확장해서 2차원으로 확장하는 경우 문제 조건에 따라 고려해야 할 것은 다음과 같다.
- 오른쪽, 아래 2갈래로 이동한다면 주어진 배열 경계를 초과하는 경우에 대한 방어 코드
- 두 갈래 길이 있고 선택해야 한다면 필요한 HP는 최소가 되어야 함
- 선택한 갈래길이 최소가 되더라도 hp가 0이라면 그 길을 선택하되 필요한 hp는 1이다.

위 3개를 구현하면 답이다.


#### code
```js
/**
 * @param {number[][]} dungeon
 * @return {number}
 */
var calculateMinimumHP = function (dungeon) {
    const m = dungeon.length;
    const n = dungeon[0].length;
    const memo = Array.from(Array(m), () => Array(n).fill(0));
  
    function dp(i, j) {
        if (i == m - 1 && j == n - 1) {
            return Math.max(1, 1 - dungeon[i][j]);
        }
        if (i >= m || j >= n) {
            return Infinity;
        }
        if (memo[i][j]) {
            return memo[i][j];
        }

        const right = dp(i, j + 1);
        const down = dp(i + 1, j);
        const next = Math.min(right, down);
        memo[i][j] = Math.max(1, next - dungeon[i][j]);
        return memo[i][j];
    }

    return dp(0, 0);
};
```


```java
public class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        int n = dungeon.length;
        int m = dungeon[0].length;
        int memo[][] = new int[n][m];
        return dp(0, 0, memo, dungeon, n, m);
    }
  
    private int dp(int i, int j, int[][] memo, int[][] dungeon, int n, int m) {

        if (i == n - 1 && j == m - 1) {
            return Math.max(1, 1 - dungeon[i][j]);
        }
        if (i >= n || j >= m) {
            return Integer.MAX_VALUE;
        }
        if (memo[i][j] != 0) {
            return memo[i][j];
        }

        int right = dp(i, j + 1, memo, dungeon, n, m);
        int down = dp(i + 1, j, memo, dungeon, n, m);
        int min = Math.min(right, down) - dungeon[i][j];
        memo[i][j] = Math.max(1, min);
        return memo[i][j];
    }
}
```



## 질문 & 확장

(없음)

## 출처(링크)
- https://leetcode.com/problems/dungeon-game/description/

## 연결 노트
- [[cherry pick up]]
