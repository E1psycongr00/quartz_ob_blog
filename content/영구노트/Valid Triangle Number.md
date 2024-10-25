---
tags:
  - 완성
  - 알고리즘
  - 이분탐색
  - LineSweep
  - 둘레
  - 수학
aliases: 
title: Valid Triangle Number
date: 2024-02-15
---
작성 날짜: 2024-02-15
작성 시간: 22:59


----

## 문제 & 원인
![[Pasted image 20240216150439.png]]

nums 배열이 주어졌을 때 각 숫자(선분)을 이용하여 삼각형을 만들 수 있는 셋의 순서쌍이 최대 몇 개까지 가능한가?
## 해결 방안
### 이분 탐색
#### 해결 전략
데이터 제한이 1000개이고 $O(N^3)$이면 통과할 수도 있지만 Brute Force만으로는 통과 가능할수도, 또는 안될 수도 있다. 이럴 때 마지막 하나의 탐색을 이분 탐색을 이용하면 쉽게 구할 수 있다.  3Sum 문제와 비슷한 접근 방법이다.

i,j 2개를 무식하게 선회하되 k는 lower_bound를 이용해서 구한다. 그 이후 `(j, k)의 범위에 몇 개의 index가 있느냐 => k - j - 1`

#### code
```js
/**

 * @param {number[]} nums
 * @return {number}
 */
var triangleNumber = function(nums) {
    nums.sort((a, b) => a - b);
    let count = 0;
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            let target = nums[i] + nums[j];
            let k = lowerbound(nums, j + 1, nums.length, target);
            count += (k - j - 1 || 0);
        }
    }
    return count;
};


const lowerbound = (nums, low, high, target) => {
    let lo = low;
    let hi = high;
    
    while (lo < hi) {
        let mid = Math.floor(lo + (hi - lo) / 2);
        if (nums[mid] < target) {
            lo = mid + 1;
        } else {
            hi = mid;
        }
    }
    return lo;
}
```


### Line Sweep
#### 해결 전략
그냥 라인을 쓱 훑어 지나가면서 조건에 맞는지 확인하는 것이다. 조건은 i,j가 선택한 값의 합이 k의 값보다 작아야 한다.

i,j를 무식하게 돌고 k는 i,j 상황에 따라 k가 조건을 만족한다면 만족하지 않을 때까지 loop를 돌아 k를 증가시키고 그 이후 count를 업데이트한다. k가 매번 j 시작 위치부터 돌 필요가 없기 때문에 j,k는 마치 투 포인터 처럼 동작하며 시간 복잡도는 $O(N^2)$이다.


#### code
```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var triangleNumber = function(nums) {
    let count = 0;
    nums.sort((a, b) => a - b);
    for (let i = 0; i < nums.length; i++) {
        let k = i + 2;
        for (let j = i + 1; j < nums.length && nums[i] != 0; j++) {
            while (k < nums.length && nums[i] + nums[j] > nums[k]) {
                k++;
            }
            count += k - j - 1;
        }
    }
    return count;
};
```

## 질문 & 확장

(없음)

## 출처(링크)
- https://leetcode.com/problems/valid-triangle-number/description/

## 연결 노트
- [[Find Polygon With Lagest Perimeter]]