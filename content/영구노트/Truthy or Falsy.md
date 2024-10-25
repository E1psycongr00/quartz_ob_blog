---
tags:
  - 완성
  - JS
aliases: 
title: Truthy or Falsy
date: 2024-02-07
---
작성 날짜: 2024-02-07
작성 시간: 21:18


----
## 내용(Content)
### Truthy
>[!summary] Truthy
>참으로 평가되는 것

```js
if (true)
if ({})
if ([])
if (42)
if ("0")
if ("false")
if (new Date())
if (-42)
if (12n)
if (3.14)
if (-3.14)
if (Infinity)
if (-Infinity)

```

true인 것이나 실체가 있는 것들은 Truthy하다고 하고 if 조건문의 컨디션으로 들어가는 경우 참으로 반환된다.

### Falsy
>[!summary] Falsy
>거짓으로 평가되는 것


```js
if (false) {
  // Not reachable
}

if (null) {
  // Not reachable
}

if (undefined) {
  // Not reachable
}

if (0) {
  // Not reachable
}

if (-0) {
  // Not reachable
}

if (0n) {
  // Not reachable
}

if (NaN) {
  // Not reachable
}

if ("") {
  // Not reachable
}

```

null, undefined 같은 실체가 없거나 0이거나 문자열이 빈경우 또는 NaN 의 경우 Falsy한 값이다.

뭔가 없거나 비어있다면 Falsy함을 알 수 있다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://developer.mozilla.org/en-US/docs/Glossary/Truthy

## 연결 노트
- [[단축 평가 계산]]
- [[nullish 병합 연산자]]









