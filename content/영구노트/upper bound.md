---
tags:
  - 완성
  - 알고리즘
  - 이분탐색
  - 스니펫
aliases: 
title: upper bound
date: 2024-02-16
---
작성 날짜: 2024-02-16
작성 시간: 10:54


----
## 내용(Content)
### upper bound

>[!summary] upper bound
>특정 값보다 바로 큰 값의 위치를 이분 탐색을 통해 찾는 알고리즘(target index + 1, 값이 중복된 경우 중복된 값의 마지막 index + 1)

![[Upper Bound 동작 과정(draw).svg|600]]

>[!caution] lower bound와 upper bound 관계
>code 상으로는 low가 움직일 조건이  `<, <=` 차이 밖에 없다. lower_bound의 여러가지 특징을 이해하려면 이렇게 이해하는 것도 괜찮다
>target보다 작지만 target에 가장 가까운 값 + 1 == (target이 존재한다면) target (존재하지 않으면 target보다 큰 값)
### Code

#### java
```java
public static int upperBound(int[] nums, int lowIn, int highEx, int target) {  
    while (lowIn < highEx) {  
       int mid = lowIn + (highEx - lowIn) / 2;  
       if (nums[mid] <= target) {  
          lowIn = mid + 1;  
       } else {  
          highEx = mid;  
       }  
    }  
    return lowIn;  
}
```


**테스트 코드**
```java
@Nested  
@DisplayName("upperbound 메소드 테스트")  
class UpperBoundTest {  
    @Test  
    @DisplayName("값이 중복되는 영역이 있을 경우 최초 target의 인덱스를 반환한다")  
    void testUpperBoundWithDuplicateValues() {  
       int[] nums = {1, 3, 3, 5, 7};  
       int target = 3;  
       int expected = 3;  
       assertEquals(expected, Utils.upperBound(nums, 0, nums.length, target));  
    }  
  
    @Test  
    @DisplayName("모든 요소보다 target이 큰 경우 최초 입력의 high만큼 반환한다")  
    void testUpperBoundWithTargetGreaterThanAllElements() {  
       int[] nums = {1, 3, 5, 7};  
       int target = 10;  
       int expected = nums.length;  
       assertEquals(expected, Utils.upperBound(nums, 0, nums.length, target));  
    }  
  
    @Test  
    @DisplayName("모든 요소가 target 보다 작은 경우 0을 반환한다")  
    void testUpperBoundWithTargetLessThanAllElements() {  
       int[] nums = {1, 3, 5, 7};  
       int target = 0;  
       int expected = 0;  
       assertEquals(expected, Utils.upperBound(nums, 0, nums.length, target));  
    }  
  
    @Test  
    @DisplayName("target이 존재하지 않고 특정 숫자 사이에 끼어 있다면 큰 값을 반환한다")  
    void testUpperBoundWithTargetBetweenValues() {  
       int[] nums = {1, 3, 5, 7};  
       int target = 4;  
       int expected = 2;  
       assertEquals(expected, Utils.upperBound(nums, 0, nums.length, target));  
    }  
}
```

![[Pasted image 20240216114147.png]]


```js
function lowerbound(nums, lo, hi, target) {
	while (lo < hi) {
		let mid = Math.floor(lo + (hi - lo) / 2);
		if (nums[mid] <= target) {
			lo = mid + 1;
		} else {
			hi = mid;
		}
	}
	return lo;
}
```
## 질문 & 확장

(없음)

## 출처(링크)


## 연결 노트
- [[이분 탐색]]
- [[lower bound]]









