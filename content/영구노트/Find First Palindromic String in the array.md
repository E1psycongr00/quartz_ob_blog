---
tags:
  - 완성
  - 팰린드롬
  - 투포인터
  - String
  - 알고리즘
aliases: 
title: ind First Palindromic String in the array
date: 2024-02-13
---
작성 날짜: 2024-02-13
작성 시간: 10:37


----

## 문제 & 원인
![[Pasted image 20240213103947.png]]
팰린드롬을 판별하라

## 해결 방안
### 투포인터
>[!summary] 
>좌우 끝에 포인터를 안쪽으로 좁히면서 같아지는 지점까지 탐색한다.

```js
const isPalindrome = word => {
    let firstPt = 0;
    let lastPt = word.length - 1;
    while (firstPt < lastPt) {
        if (word[firstPt] != word[lastPt]) {
            return false;
        }
        firstPt++;
        lastPt--;
    }
    return true;
};
```

###  문자열 방법
>[!summary]
>reverse한 문자열 == 기존 문자열


#### javascript
js의 문자열 뒤집는 방법은 다음과 같다.

```js
function reverseString(str) {
    return str.split("").reverse().join("");
}
```


#### java
```java
private static String reverseString(String str) {  
    return new StringBuilder(str).reverse().toString();       
}
```

## 질문 & 확장

(없음)

## 출처(링크)
- https://leetcode.com/problems/find-first-palindromic-string-in-the-array/description/

## 연결 노트
