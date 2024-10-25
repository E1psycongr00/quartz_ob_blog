---
tags:
  - 완성
  - 클린코드
  - JS
aliases:
  - nullish
  - nullish coalescing operator
title: nullish 병합 연산자
date: 2024-02-07
---
작성 날짜: 2024-02-07
작성 시간: 22:27


----
## 내용(Content)
### nullish coalescing operator
>[!summary] null 병합 연산자
>변수가 **null** 또는 **undefined** 일 때 값을 대체할 때 사용한다.

`x = a ?? b ` 이것을 이전 문법으로 표현하면 다음과 같다.

```js
x = !(a == null || a == undefined) ? a : b;
```

a == null 또는 a == undefined 일 때 b를 리턴하고 그렇지 않으면 a 를 리턴해라 라는 뜻이다.

null 병합 연산자는 [[단축 평가 계산|short circuit evaluation]]과 비슷하긴 하나 조금 다르다.

단축평가의 null은 Falsy하면 다음으로 넘어가는데   Falsy는 null, undefined 이외에도 많기 때문이다.

$$
nullish \quad \in \quad \mid\mid
$$
```js
let a = 0;
a ?? 10   // result: 0
a || 10  // result: 10
```
## 질문 & 확장

(없음)

## 출처(링크)
- https://ko.javascript.info/nullish-coalescing-operator

## 연결 노트
- [[Truthy or Falsy]]









