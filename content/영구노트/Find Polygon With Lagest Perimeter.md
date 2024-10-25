---
tags:
  - 완성
  - 알고리즘
  - 수학
  - 다각형
  - 둘레
  - Greedy
aliases:
title: Find Polygon With Lagest Perimeter
date: 2024-02-15
---
작성 날짜: 2024-02-15
작성 시간: 22:50


----

## 문제 & 원인
![[Pasted image 20240215225125.png]]

nums라는 배열이 주어 졌을 때 만들 수 있는 최대 길이의 다각형을 구하여라
## 해결 방안
### 탐욕법

#### 해결 전략
- 선택한 선분은 3개 이상이여야 한다.
- 작은것부터 선택한 선분의 합이 새로 추가할 큰 선분보다 커야 한다.

![[선분들의 다각형 생성 조건 (draw).svg|500]]

#### Code

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var largestPerimeter = function(nums) {
    nums.sort((a, b) => a - b);
    let polygon = nums[0] + nums[1];
    let result = -1;
    for (let i = 2; i < nums.length; i++) {
        if (polygon > nums[i]) {
            result = polygon + nums[i];
        }
        polygon += nums[i];
    }
    return result;
};
```

## 질문 & 확장

(없음)

## 출처(링크)
- https://leetcode.com/problems/find-polygon-with-the-largest-perimeter/description/

## 연결 노트
